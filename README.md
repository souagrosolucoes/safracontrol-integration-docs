# SafraControl — Documentação de Integração

Site de documentação estático para a API de integração do SafraControl. Combina a referência completa dos endpoints (via Redoc) com páginas customizadas que explicam fluxos de integração.

## Estrutura do projeto

```
├── index.html                        # Referência da API (Redoc)
├── docs/
│   └── fluxos/
│       └── criacao-pedido.html       # Guia: fluxo de criação de pedido
├── .github/
│   └── workflows/
│       └── deploy.yml                # Deploy automático no GitHub Pages
├── redocly.yaml                      # Configuração do Redocly CLI (lint)
└── package.json
```

## Rodando localmente

```bash
npm install
npm run dev
```

Acesse [http://localhost:8080](http://localhost:8080).

O `index.html` carrega a spec da API diretamente da URL de staging. Para apontar para outra spec, use o parâmetro `?spec=`:

```
http://localhost:8080?spec=https://outra-url.com/api-json
```

## Adicionando uma nova página de fluxo

### 1. Crie o arquivo HTML

Crie `docs/fluxos/nome-do-fluxo.html` copiando o cabeçalho de `docs/fluxos/criacao-pedido.html` (bloco `#topnav` e os estilos associados).

Atenção ao caminho relativo dos links — dentro de `docs/fluxos/` aponte para a raiz assim:

```html
<a class="brand" href="../../index.html">SafraControl Integração</a>
<a class="nav-item" href="../../index.html">Referência da API</a>
```

### 2. Adicione o link no cabeçalho do `index.html`

Localize o bloco `<div class="nav-links">` e acrescente um novo item:

```html
<a class="nav-item" href="docs/fluxos/nome-do-fluxo.html">
  Novo Fluxo <span class="nav-badge">GUIA</span>
</a>
```

### 3. Adicione o mesmo link nas outras páginas de fluxo

Abra cada arquivo em `docs/fluxos/` e repita o passo 2 no bloco `#topnav` deles.

## Deploy no GitHub Pages

O deploy é automático via GitHub Actions a cada push na branch `main`.

**Configuração inicial (feita uma única vez no GitHub):**

1. Vá em **Settings → Pages** do repositório
2. Em **Source**, selecione **GitHub Actions**
3. O próximo push na `main` já publica o site

O workflow em `.github/workflows/deploy.yml` simplesmente faz o upload da raiz do repositório como site estático — não há etapa de build.

> **Atenção com CORS:** o `index.html` busca a spec da API diretamente no browser. Se o servidor da API não retornar o header `Access-Control-Allow-Origin: *`, o Redoc não conseguirá carregar a spec a partir do domínio do GitHub Pages.

## Scripts disponíveis

| Comando | O que faz |
|---|---|
| `npm run dev` | Sobe servidor estático na porta 8080 |
| `npm run lint` | Valida a spec da API com o Redocly CLI |
| `npm run build` | Copia os arquivos para `dist/` |
