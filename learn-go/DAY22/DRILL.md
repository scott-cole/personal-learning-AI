# DAY22: Drill — "Add Config to the URL Shortener"

Add a config package to the URL shortener and wire it up.

## Structure

```
DAY22/
├── config/
│   └── config.go        → Config struct + LoadConfig
├── main.go              → uses config
├── storage/
│   └── memory.go
└── service/
    └── service.go
```

## config/config.go

```go
package config

import (
    "errors"
    "fmt"
    "os"
    "strconv"
)

type Config struct {
    Port     int
    DBPath   string
    BaseURL  string
    LogLevel string
}

func Load() (Config, error) {
    cfg := Config{
        Port:     getEnvInt("PORT", 8080),
        DBPath:   getEnv("DB_PATH", "./url_shortener.db"),
        BaseURL:  getEnv("BASE_URL", "http://localhost:8080"),
        LogLevel: getEnv("LOG_LEVEL", "info"),
    }

    if cfg.Port < 1 || cfg.Port > 65535 {
        return Config{}, fmt.Errorf("invalid port: %d", cfg.Port)
    }
    if cfg.DBPath == "" {
        return Config{}, errors.New("DB_PATH is required")
    }

    return cfg, nil
}

func getEnv(key, defaultVal string) string {
    if val := os.Getenv(key); val != "" {
        return val
    }
    return defaultVal
}

func getEnvInt(key string, defaultVal int) int {
    val := os.Getenv(key)
    if val == "" {
        return defaultVal
    }
    i, err := strconv.Atoi(val)
    if err != nil {
        return defaultVal
    }
    return i
}
```

## main.go

```go
package main

import (
    "fmt"
    "log"
    "net/http"
    "learn-go/DAY22/config"
    "learn-go/DAY22/service"
    "learn-go/DAY22/storage"
)

func main() {
    cfg, err := config.Load()
    if err != nil {
        log.Fatalf("invalid config: %v", err)
    }

    repo := storage.NewMemoryRepository()
    svc := service.NewService(repo)

    mux := http.NewServeMux()
    mux.HandleFunc("POST /shorten", func(w http.ResponseWriter, r *http.Request) {
        // ... handler code
    })
    mux.HandleFunc("GET /{code}", func(w http.ResponseWriter, r *http.Request) {
        // ... handler code
    })

    addr := fmt.Sprintf(":%d", cfg.Port)
    log.Printf("Starting server on %s (base: %s)", addr, cfg.BaseURL)
    log.Fatal(http.ListenAndServe(addr, mux))
}
```

## Run it

```bash
cd ~/Dev/learn-go/DAY22
go mod init learn-go/DAY22

# With defaults
go run main.go

# With custom config
PORT=9090 DB_PATH=/tmp/prod.db go run main.go
```

## Test the config validation

```bash
PORT=99999 go run main.go
# Expected: invalid config: invalid port: 99999
```
