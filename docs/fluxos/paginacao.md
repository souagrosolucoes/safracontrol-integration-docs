# Paginação

Todos os endpoints que retornam listas usam paginação baseada em **página e limite**.

## Parâmetros

| Parâmetro | Tipo | Padrão | Máximo | Descrição |
|---|---|---|---|---|
| `page` | integer | `1` | — | Número da página |
| `limit` | integer | `20` | `100` | Itens por página |

## Exemplo de requisição

```bash
curl "https://api.exemplo.com/v1/users?page=2&limit=50" \
  -H "Authorization: Bearer SEU_TOKEN"
```

## Estrutura da resposta

```json
{
  "data": [ ... ],
  "pagination": {
    "page": 2,
    "limit": 50,
    "total": 143
  }
}
```

O campo `total` indica o número total de registros — use para calcular
quantas páginas existem: `Math.ceil(total / limit)`.

## Iterando todas as páginas

```javascript
async function fetchAllUsers() {
  const allUsers = [];
  let page = 1;
  const limit = 100;

  while (true) {
    const res = await fetch(
      `https://api.exemplo.com/v1/users?page=${page}&limit=${limit}`,
      { headers: { Authorization: `Bearer ${await getToken()}` } }
    );
    const { data, pagination } = await res.json();

    allUsers.push(...data);

    const totalPages = Math.ceil(pagination.total / limit);
    if (page >= totalPages) break;
    page++;
  }

  return allUsers;
}
```

## Boas práticas

- Use `limit=100` para minimizar o número de requisições ao buscar muitos registros
- Não faça requisições paralelas em páginas sequenciais — respeite o rate limit
- Armazene o `total` da primeira resposta para exibir progresso ao usuário
