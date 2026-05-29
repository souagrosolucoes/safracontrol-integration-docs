# Guia rápido

Tenha a API funcionando em menos de 5 minutos.

## Pré-requisitos

- Credenciais de acesso (`client_id` e `client_secret`) — solicite ao time de integrações
- Qualquer cliente HTTP (curl, Postman, Insomnia, etc.)

## Passo 1 — Gerar token

```bash
curl -X POST https://sandbox.api.exemplo.com/v1/auth/token \
  -H "Content-Type: application/json" \
  -d '{
    "client_id": "SEU_CLIENT_ID",
    "client_secret": "SEU_CLIENT_SECRET"
  }'
```

Guarde o `access_token` da resposta.

## Passo 2 — Fazer sua primeira requisição

```bash
curl https://sandbox.api.exemplo.com/v1/users \
  -H "Authorization: Bearer SEU_TOKEN_AQUI"
```

## Próximos passos

- Entenda como [renovar tokens automaticamente](./fluxos/autenticacao.md)
- Veja como funciona a [paginação](./fluxos/paginacao.md)
