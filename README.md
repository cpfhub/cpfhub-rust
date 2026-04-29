# cpfhub

**Official Rust SDK for [CPFHub.io](https://cpfhub.io) — Brazilian CPF Lookup API**

> SDK oficial Rust para a [CPFHub.io](https://cpfhub.io) — API de consulta de CPF

[![crates.io](https://img.shields.io/crates/v/cpfhub)](https://crates.io/crates/cpfhub)
[![docs.rs](https://img.shields.io/docsrs/cpfhub)](https://docs.rs/cpfhub)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

---

## What is CPFHub.io?

CPFHub.io is a REST API that returns name, gender, and date of birth from any Brazilian CPF number — in ~300ms, with 99.9% uptime, and full LGPD compliance.

**10M+ CPFs queried · 1,300+ active companies · 99.9% uptime**

---

## Installation / Instalação

```toml
# Cargo.toml
[dependencies]
cpfhub = "1.0"
tokio = { version = "1", features = ["full"] }
```

---

## Quick Start

```rust
use cpfhub::Client;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new("YOUR_API_KEY");

    let result = client.lookup("00000000000").await?;

    println!("{}", result.name);       // "Fulano de Tal"
    println!("{}", result.gender);     // "M"
    println!("{}", result.birth_date); // "15/06/1990"

    Ok(())
}
```

Get your free API key at [app.cpfhub.io](https://app.cpfhub.io) — no credit card required.

---

## API Reference

### `Client::new(api_key: &str) -> Client`

### `Client::with_timeout(api_key: &str, timeout: Duration) -> Client`

### `client.lookup(cpf: &str) -> Result<CPFResult, CPFHubError>`

Accepts CPF with or without formatting (`000.000.000-00` or `00000000000`).

#### `CPFResult` fields

```rust
pub struct CPFResult {
    pub cpf: String,
    pub name: String,
    pub name_upper: String,
    pub gender: String,   // "M" or "F"
    pub birth_date: String, // "DD/MM/YYYY"
    pub day: u32,
    pub month: u32,
    pub year: u32,
}
```

#### `CPFHubError` variants

```rust
pub enum CPFHubError {
    InvalidCPF,        // 400
    Unauthorized,      // 401
    NotFound,          // 404
    RateLimitExceeded, // 429
    ServerError,       // 500
    Unavailable,       // 503
    RequestError(reqwest::Error),
}
```

---

## Examples

### With timeout

```rust
use cpfhub::Client;
use std::time::Duration;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::with_timeout("YOUR_API_KEY", Duration::from_secs(5));
    let result = client.lookup("00000000000").await?;
    println!("{}", result.name);
    Ok(())
}
```

### Error handling

```rust
use cpfhub::{Client, CPFHubError};

#[tokio::main]
async fn main() {
    let client = Client::new("YOUR_API_KEY");

    match client.lookup("00000000000").await {
        Ok(result) => println!("Name: {}", result.name),
        Err(CPFHubError::InvalidCPF) => eprintln!("Invalid CPF format"),
        Err(CPFHubError::RateLimitExceeded) => eprintln!("Rate limit — try again shortly"),
        Err(e) => eprintln!("Error: {}", e),
    }
}
```

---

## Rate Limits / Limites

| Plan | Limit |
|------|-------|
| Free | 1 req/2s · 50/month |
| Pro | 1 req/s · 1,000/month |
| Corporate | Custom |

---

## Requirements / Requisitos

- Rust 1.70+
- Tokio async runtime

---

## Links

- [Documentation / Documentação](https://cpfhub.io/documentacao)
- [docs.rs](https://docs.rs/cpfhub)
- [Dashboard](https://app.cpfhub.io)

---

## License / Licença

MIT © [CPFHub.io](https://cpfhub.io)
