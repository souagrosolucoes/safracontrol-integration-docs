# Fluxo de autenticação

A API usa **OAuth 2.0 Client Credentials**. Toda requisição autenticada
precisa de um `Bearer Token` no header `Authorization`.

## Como funciona

```
Seu servidor               API
     │                      │
     │── POST /auth/token ──▶│
     │◀── { access_token } ──│
     │                      │
     │── GET /users ─────────▶│
     │   Authorization: Bearer│
     │◀── { data: [...] } ───│
```

## Obtendo credenciais

Acesse o painel e crie um par de credenciais em **Configurações → Integrações**.
Você receberá um `client_id` e um `client_secret`.

> ⚠️ Nunca exponha o `client_secret` no frontend. Use sempre a partir do seu backend.

## Gerando o token

```bash
curl -X POST https://api.exemplo.com/v1/auth/token \
  -H "Content-Type: application/json" \
  -d '{
    "client_id": "app_abc123",
    "client_secret": "secret_xyz789"
  }'
```

Resposta:

```json
{
  "access_token": "eyJhbGci...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

## Usando o token

Inclua o token em todas as requisições autenticadas:

```bash
curl https://api.exemplo.com/v1/users \
  -H "Authorization: Bearer eyJhbGci..."
```

## Renovação automática

O token expira em **1 hora**. Recomendamos armazenar o token em memória
e renovar antes do vencimento:

```javascript
// Node.js
let cachedToken = null;
let tokenExpiry = null;

async function getToken() {
  if (cachedToken && Date.now() < tokenExpiry - 60_000) {
    return cachedToken; // ainda válido
  }

  const res = await fetch('https://api.exemplo.com/v1/auth/token', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      client_id: process.env.API_CLIENT_ID,
      client_secret: process.env.API_CLIENT_SECRET,
    }),
  });

  const data = await res.json();
  cachedToken = data.access_token;
  tokenExpiry = Date.now() + data.expires_in * 1000;
  return cachedToken;
}
```

## Erros comuns

| Código | Motivo | Solução |
|---|---|---|
| `401 UNAUTHORIZED` | Token ausente ou inválido | Verifique o header `Authorization` |
| `401 TOKEN_EXPIRED` | Token expirado | Gere um novo token |
| `403 FORBIDDEN` | Sem permissão para o recurso | Verifique o escopo das suas credenciais |
