# cpfhub: Rust SDK for CPFHub.io

🇺🇸 **English** | [🇧🇷 Português](#português)

**Official Rust SDK for [CPFHub.io](https://cpfhub.io) — Brazilian CPF Lookup API**

[![crates.io](https://img.shields.io/crates/v/cpfhub)](https://crates.io/crates/cpfhub)
[![docs.rs](https://img.shields.io/docsrs/cpfhub)](https://docs.rs/cpfhub)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

---

## What is CPFHub.io?

CPFHub.io is a REST API that returns name, gender, and date of birth from any Brazilian CPF number — in ~300ms, with 99.9% uptime and full LGPD compliance.

**10M+ CPFs queried · 1,300+ active companies · 99.9% uptime**

---

## Installation

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

## curl Example

```bash
curl -X GET "https://api.cpfhub.io/cpf/12345678909" \
  -H "x-api-key: YOUR_API_KEY"
```

**Response:**

```json
{
  "success": true,
  "data": {
    "cpf": "12345678909",
    "name": "Fulano de Tal",
    "nameUpper": "FULANO DE TAL",
    "gender": "M",
    "birthDate": "15/06/1990",
    "day": 15,
    "month": 6,
    "year": 1990
  }
}
```

---

## API Reference

### `Client::new(api_key: &str) -> Client`

### `Client::with_timeout(api_key: &str, timeout: Duration) -> Client`

### `client.lookup(cpf: &str) -> Result<CPFResult, CPFHubError>`

Looks up a CPF and returns the associated identity data. Accepts CPF with or without formatting.

#### `CPFResult` fields

```rust
pub struct CPFResult {
    pub cpf: String,
    pub name: String,
    pub name_upper: String,
    pub gender: String,     // "M" or "F"
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

## Error Handling

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

---

## Rate Limits

| Plan | Limit |
|------|-------|
| Free | 1 request every 2 seconds · 50 requests/month |
| Pro | 1 request per second · 1,000 requests/month |
| Corporate | Custom |

---

## Plans & Pricing

| Plan | Price | Included | Extra |
|------|-------|----------|-------|
| **Free** | R$ 0/month | 50 lookups | — |
| **Pro** | R$ 149/month | 1,000 lookups | R$ 0,15/lookup |
| **Corporate** | Custom | Custom | Custom |

[View full pricing at cpfhub.io →](https://cpfhub.io#pricing)

---

## Requirements

- Rust 1.70+
- Tokio async runtime

---

## Links

- [Documentation](https://cpfhub.io/documentacao)
- [docs.rs](https://docs.rs/cpfhub)
- [Dashboard](https://app.cpfhub.io)
- [Status Page](https://app.cpfhub.io/status)
- [Pricing](https://cpfhub.io#pricing)
- [LGPD Compliance](https://cpfhub.io/lgpd)
- [OpenAPI Specification](https://github.com/cpfhub/cpfhub-openapi/blob/main/openapi.yaml)
- [MCP Server (AI Agents)](https://github.com/cpfhub/cpfhub-mcp)

---

## License

MIT © [CPFHub.io](https://cpfhub.io)

---

# Português

[🇺🇸 English](#cpfhub-rust-sdk-for-cpfhubio) | 🇧🇷 **Português**

**SDK Rust oficial para [CPFHub.io](https://cpfhub.io) — API de Consulta de CPF Brasileiro**

---

## O que é o CPFHub.io?

O CPFHub.io é uma API REST que retorna nome, gênero e data de nascimento de qualquer CPF brasileiro — em ~300ms, com 99,9% de uptime e total conformidade com a LGPD.

**10M+ CPFs consultados · 1.300+ empresas ativas · 99,9% uptime**

---

## Instalação

```toml
# Cargo.toml
[dependencies]
cpfhub = "1.0"
tokio = { version = "1", features = ["full"] }
```

---

## Início Rápido

```rust
use cpfhub::Client;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new("SUA_CHAVE_DE_API");
    let result = client.lookup("00000000000").await?;
    println!("{}", result.name);       // "Fulano de Tal"
    println!("{}", result.gender);     // "M"
    println!("{}", result.birth_date); // "15/06/1990"
    Ok(())
}
```

Obtenha sua chave de API gratuita em [app.cpfhub.io](https://app.cpfhub.io) — sem cartão de crédito.

---

## Exemplo curl

```bash
curl -X GET "https://api.cpfhub.io/cpf/12345678909" \
  -H "x-api-key: SUA_CHAVE_DE_API"
```

**Resposta:**

```json
{
  "success": true,
  "data": {
    "cpf": "12345678909",
    "name": "Fulano de Tal",
    "nameUpper": "FULANO DE TAL",
    "gender": "M",
    "birthDate": "15/06/1990",
    "day": 15,
    "month": 6,
    "year": 1990
  }
}
```

---

## Referência da API

### `Client::new(api_key: &str) -> Client`

### `Client::with_timeout(api_key: &str, timeout: Duration) -> Client`

### `client.lookup(cpf: &str) -> Result<CPFResult, CPFHubError>`

Consulta um CPF e retorna os dados de identidade associados. Aceita CPF com ou sem formatação.

#### Campos de `CPFResult`

```rust
pub struct CPFResult {
    pub cpf: String,
    pub name: String,
    pub name_upper: String,
    pub gender: String,     // "M" ou "F"
    pub birth_date: String, // "DD/MM/YYYY"
    pub day: u32,
    pub month: u32,
    pub year: u32,
}
```

#### Variantes de `CPFHubError`

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

## Tratamento de Erros

```rust
use cpfhub::{Client, CPFHubError};

#[tokio::main]
async fn main() {
    let client = Client::new("SUA_CHAVE_DE_API");
    match client.lookup("00000000000").await {
        Ok(result) => println!("Nome: {}", result.name),
        Err(CPFHubError::InvalidCPF) => eprintln!("Formato de CPF inválido"),
        Err(CPFHubError::RateLimitExceeded) => eprintln!("Limite excedido — tente novamente"),
        Err(e) => eprintln!("Erro: {}", e),
    }
}
```

---

## Exemplos

### Com timeout

```rust
use cpfhub::Client;
use std::time::Duration;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::with_timeout("SUA_CHAVE_DE_API", Duration::from_secs(5));
    let result = client.lookup("00000000000").await?;
    println!("{}", result.name);
    Ok(())
}
```

---

## Limites de Requisição

| Plano | Limite |
|-------|--------|
| Gratuito | 1 requisição a cada 2 segundos · 50 requisições/mês |
| Pro | 1 requisição por segundo · 1.000 requisições/mês |
| Corporativo | Personalizado |

---

## Planos e Preços

| Plano | Preço | Incluído | Extra |
|-------|-------|----------|-------|
| **Gratuito** | R$ 0/mês | 50 consultas | — |
| **Pro** | R$ 149/mês | 1.000 consultas | R$ 0,15/consulta |
| **Corporativo** | Personalizado | Personalizado | Personalizado |

[Ver preços completos em cpfhub.io →](https://cpfhub.io#pricing)

---

## Requisitos

- Rust 1.70+
- Tokio async runtime

---

## Links

- [Documentação](https://cpfhub.io/documentacao)
- [docs.rs](https://docs.rs/cpfhub)
- [Dashboard](https://app.cpfhub.io)
- [Página de Status](https://app.cpfhub.io/status)
- [Preços](https://cpfhub.io#pricing)
- [Conformidade LGPD](https://cpfhub.io/lgpd)
- [Especificação OpenAPI](https://github.com/cpfhub/cpfhub-openapi/blob/main/openapi.yaml)
- [Servidor MCP (Agentes de IA)](https://github.com/cpfhub/cpfhub-mcp)

---

## Licença

MIT © [CPFHub.io](https://cpfhub.io)
