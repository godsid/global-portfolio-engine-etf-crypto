# 📊 System Diagrams - Global Portfolio Engine Data Fetch

**Project:** global-portfolio-engine-etf-crypto  
**Last Updated:** 2026-05-27  
**Version:** v50.0

---

## 1. High-Level Architecture

```mermaid
graph TD
    A[User Input] --> B[Data Fetcher]
    B --> C{Market Type}
    C -->|ETF| D[Yahoo Finance API]
    C -->|Crypto| E[Binance API]
    C -->|Thai| F[Yahoo Finance .BK]
    
    D --> G[Data Parser]
    E --> G
    F --> G
    
    G --> H[Data Store]
    H --> I[Strategy Engine]
    I --> J[Visualization]
    
    H --> K[Cache System]
    K --> D
    K --> E
    K --> F
```

**Description:**  
ระบบรับ input จาก user (symbols, date range) → ส่งไปยัง Data Fetcher → ตรวจสอบประเภทตลาด → เรียก API ที่เหมาะสม → Parse ข้อมูล → เก็บใน Data Store → ใช้ใน Strategy Engine → แสดงผลบน Visualization

---

## 2. Detailed Fetch Flow

```mermaid
flowchart TD
    Start[Fetch Button Clicked] --> Validate[Validate Inputs]
    Validate -->|Invalid| Error[Show Error Message]
    Validate -->|Valid| Determine[Determine Market Type]
    
    Determine -->|ETF| FetchETF[Yahoo Finance API]
    Determine -->|Crypto| FetchCrypto[Binance API]
    Determine -->|Thai| FetchThai[Yahoo Finance .BK suffix]
    
    FetchETF --> CheckCache1{Cache Exists?}
    FetchCrypto --> CheckCache2{Cache Exists?}
    FetchThai --> CheckCache3{Cache Exists?}
    
    CheckCache1 -->|Yes| UseCache1[✓ Use Cached Data]
    CheckCache1 -->|No| NewFetch1[API Call → Parse → Store Cache]
    CheckCache2 -->|Yes| UseCache2[✓ Use Cached Data]
    CheckCache2 -->|No| NewFetch2[API Call → Parse → Store Cache]
    CheckCache3 -->|Yes| UseCache3[✓ Use Cached Data]
    CheckCache3 -->|No| NewFetch3[API Call → Parse → Store Cache]
    
    UseCache1 --> Parse[Parse Data]
    UseCache2 --> Parse
    UseCache3 --> Parse
    NewFetch1 --> Parse
    NewFetch2 --> Parse
    NewFetch3 --> Parse
    
    Parse --> Liquidity[Apply Liquidity Filter]
    Liquidity --> UpdateUI[Update UI & Health Monitor]
    UpdateUI --> EnableRun[✓ Enable Run Button]
    
    Error --> End[× Display Error]
    EnableRun --> End[✓ Complete]
```

**Flow Details:**

| Step | Description |
|------|-------------|
| Validate | ตรวจสอบ symbols และ date range ว่าถูกต้อง |
| Determine | เช็คว่าเป็น ETF / Crypto / Thai market |
| Fetch | เรียก API ที่เหมาะสม |
| Cache Check | ตรวจสอบว่ามี cache ที่ยังไม่หมดอายุหรือไม่ |
| Parse | แปลงข้อมูล OHLCV ให้อยู่ในรูปแบบที่ต้องการ |
| Liquidity | คำนวณ ADV และกรอง assets ที่มีสภาพคล่องต่ำ |
| Update UI | แสดงข้อมูลใน health monitor |
| Enable Run | เปิดปุ่ม Run ให้ใช้งานได้ |

---

## 3. Cache Strategy

```mermaid
sequenceDiagram
    participant U as User
    participant F as Fetcher
    participant C as Cache (localStorage)
    participant A as Yahoo/Binance API
    
    U->>F: Fetch Data (symbol, dateRange)
    F->>C: Check Cache Key<br/>`cache_${symbol}_${startDate}_${endDate}`
    
    alt Cache Hit & Valid (within TTL)
        C-->>F: Return Cached Data
        F-->>U: Display Data (⚡ Fast: ~10ms)
        
    else Cache Miss
        F->>A: API Request
        A-->>F: OHLCV JSON Data
        F->>F: Parse & Validate Data
        F->>C: Store with TTL<br/>`timestamp: Date.now()`
        F-->>U: Display Data (🐢 Slow: ~500ms-2s)
        
    else Cache Expired (TTL > 24h)
        F->>A: API Request (Refresh)
        A-->>F: Fresh OHLCV Data
        F->>C: Update Cache
        F-->>U: Display Fresh Data
    end
```

**Cache Configuration:**

```javascript
const CACHE_CONFIG = {
    // TTL (Time To Live) - 24 hours
    TTL: 24 * 60 * 60 * 1000,
    
    // Max cache size (5MB)
    MAX_SIZE: 5 * 1024 * 1024,
    
    // Auto-refresh threshold (1 hour before expiry)
    REFRESH_THRESHOLD: 60 * 60 * 1000,
    
    // Cache key format
    KEY_FORMAT: 'gpe_cache_{symbol}_{startDate}_{endDate}'
};
```

**Cache Key Example:**
```
gpe_cache_SPY_2024-01-01_2024-12-31
gpe_cache_BTCUSDT_2024-01-01_2024-12-31
gpe_cache_SET.BK_2024-01-01_2024-12-31
```

---

## 4. Error Handling & Retry Logic

```mermaid
flowchart TD
    API[API Call] --> Success{Success?}
    
    Success -->|Yes| Store[✓ Store Data to Cache]
    Success -->|No| CheckRetry{Retry < 3?}
    
    CheckRetry -->|Yes| Wait[Wait 1s → Retry]
    Wait --> API
    
    CheckRetry -->|No| CheckFallback{Fallback Cache<br/>Available?}
    
    CheckFallback -->|Yes| UseFallback[⚠️ Use Old Cache<br/>Show Warning]
    CheckFallback -->|No| ShowError[❌ Show Error Message<br/>Suggest Manual Upload]
    
    Store --> Complete[✓ Complete - Enable Run]
    UseFallback --> Complete
    ShowError --> End
    
    subgraph Retry_Loop ["Retry Logic"]
        Wait -.-> API
    end
```

**Retry Configuration:**

```javascript
const RETRY_CONFIG = {
    MAX_RETRIES: 3,
    RETRY_DELAY: 1000, // 1 second
    BACKOFF_MULTIPLIER: 2, // Exponential backoff
    TIMEOUT: 10000 // 10 seconds per request
};

// Retry schedule: 1s → 2s → 4s
```

**Error Messages:**

| Error | Message |
|-------|---------|
| Network Error | "ไม่สามารถเชื่อมต่ออินเทอร์เน็ต กรุณาตรวจสอบการเชื่อมต่อ" |
| Rate Limit | "เกินขีดจำกัดการเรียก API กรุณารอสักครู่" |
| Symbol Not Found | "ไม่พบข้อมูลสำหรับ {symbol} กรุณาตรวจสอบชื่อ" |
| Invalid Date | "รูปแบบวันที่ไม่ถูกต้อง กรุณาเลือกวันที่ใหม่" |
| Cache Failed | "เกิดข้อผิดพลาดในการบันทึก cache ข้อมูลจะถูกดึงใหม่" |

---

## 5. Thai Market Data Flow

```mermaid
flowchart LR
    subgraph Thai_Fetch
        A[User Input<br/>e.g., PTT] --> B{Symbol<br/>Format?}
        B -->|Plain| C[Add .BK suffix]
        B -->|Has .BK| D[Keep as is]
        C --> E[Yahoo Finance<br/>^XE:SYMBOL.BK]
        D --> E
        E --> F{HTTP<br/>Success?}
        F -->|Yes| G[Parse OHLCV<br/>Convert Currency<br/>THB → USD]
        F -->|No| H[Show Error]
        G --> I[Store in Cache<br/>with TH metadata]
    end
    
    subgraph Quick_Add
        J[SET Index] --> B
        K[PTT] --> B
        L[CPF] --> B
        M[BDMS] --> B
    end
    
    subgraph Default_Settings
        N[252 trading days]
        O[THB 1M daily volume filter]
        P[0.15% trade fee]
        Q[1.00% frontend fee]
    end
```

**Thai Market Symbol Mapping:**

| User Input | Full Symbol | Yahoo URL |
|------------|-------------|-----------|
| SET | ^SET.BK | https://finance.yahoo.com/quote/^SET.BK |
| SET50 | ^SET50.BK | https://finance.yahoo.com/quote/^SET50.BK |
| PTT | PTT.BK | https://finance.yahoo.com/quote/PTT.BK |
| CPF | CPF.BK | https://finance.yahoo.com/quote/CPF.BK |
| BDMS | BDMS.BK | https://finance.yahoo.com/quote/BDMS.BK |

---

## 6. Data Flow Summary

```mermaid
graph LR
    subgraph Input
        S1[User adds symbols]
        S2[Select market type]
        S3[Set date range]
    end
    
    subgraph Process
        P1[Validate inputs]
        P2[Check cache]
        P3[Fetch from API]
        P4[Parse OHLCV]
        P5[Calculate ADV]
        P6[Apply liquidity filter]
    end
    
    subgraph Output
        O1[Update asset list]
        O2[Show health monitor]
        O3[Enable run button]
        O4[Display charts]
    end
    
    S1 --> P1
    S2 --> P1
    S3 --> P1
    P1 --> P2
    P2 -->|Cache hit| O1
    P2 -->|Cache miss| P3
    P3 --> P4
    P4 --> P5
    P5 --> P6
    P6 --> O1
```

---

## 📝 Notes

- **Cache TTL:** 24 ชั่วโมง (สามารถปรับได้ใน Config)
- **Retry:** สูงสุด 3 ครั้ง พร้อม exponential backoff
- **Thai Market:** ใช้ Yahoo Finance กับ .BK suffix
- **Data Format:** OHLCV (Open, High, Low, Close, Volume)

---

**Last Updated:** 2026-05-27  
**Status:** ✅ Complete