# DAY22: Configuration — "No Hardcoded Values"

---

## The anecdote: "A control room"

Your server has knobs and dials: port number, database path, rate limit, log level. These change between your laptop, staging, and production.

You don't recompile the code for each environment. You read configuration at startup.

```
Development:  PORT=8080  DB_PATH=./dev.db  LOG_LEVEL=debug
Production:   PORT=80    DB_PATH=/data/db  LOG_LEVEL=info
```

---

## Environment variables

```go
port := os.Getenv("PORT")
if port == "" {
    port = "8080" // default
}
```

`os.Getenv` returns empty string if not set. Always provide defaults.

## Command-line flags

```go
var port = flag.Int("port", 8080, "HTTP server port")
var dbPath = flag.String("db", "./url_shortener.db", "path to SQLite database")

flag.Parse() // must call before using flags
```

Run with: `go run main.go -port=9090 -db=/tmp/prod.db`

---

## A config struct

```go
type Config struct {
    Port        int
    DatabasePath string
    LogLevel    string
    BaseURL     string
}

func LoadConfig() Config {
    return Config{
        Port:         getEnvInt("PORT", 8080),
        DatabasePath: getEnv("DB_PATH", "./url_shortener.db"),
        LogLevel:     getEnv("LOG_LEVEL", "info"),
        BaseURL:      getEnv("BASE_URL", "http://localhost:8080"),
    }
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

---

## Using config in the URL shortener

```go
func main() {
    cfg := config.LoadConfig()

    db, _ := sql.Open("sqlite3", cfg.DatabasePath)
    repo := sqlrepo.NewSQLRepository(db)
    svc := service.NewService(repo)

    mux := http.NewServeMux()
    // ... register handlers

    log.Printf("Starting server on :%d", cfg.Port)
    http.ListenAndServe(fmt.Sprintf(":%d", cfg.Port), mux)
}
```

---

## 12-factor app config

From the [12-factor app manifesto](https://12factor.net/config):

> Store config in the environment.
> Never hardcode config as constants in the code.

```go
// ❌ Bad: hardcoded
const port = 8080

// ✅ Good: from environment
port := getEnv("PORT", "8080")
```

---

## Senior mindset: "Fail fast at startup"

Validate all config at startup, not when the first request arrives:

```go
func LoadConfig() (Config, error) {
    cfg := Config{...}

    if cfg.Port < 1024 || cfg.Port > 65535 {
        return Config{}, fmt.Errorf("invalid port: %d", cfg.Port)
    }
    if cfg.DatabasePath == "" {
        return Config{}, errors.New("DB_PATH is required")
    }

    return cfg, nil
}
```

A server that starts with bad config is safer than one that crashes at 3 AM.

---

## Summary

| Method | Use case | Example |
|--------|----------|---------|
| `os.Getenv` | Environment variables | `PORT=8080` |
| `flag.String/Int` | CLI flags | `-port=8080` |
| Config struct | Organized config | `cfg.Port`, `cfg.DBPath` |

Open DAY22/DRILL.md.
