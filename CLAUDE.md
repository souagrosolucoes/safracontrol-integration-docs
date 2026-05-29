# SafraControl — Documentação de Integração

Site de documentação estático para a API de integração do SafraControl.

## Stack

- **Redoc standalone** (CDN) — renderiza a referência da API a partir de uma URL de spec
- **HTML + CSS puro** — sem framework, sem build step
- **`@redocly/cli`** — apenas para linting local da spec (`npm run lint`)
- **GitHub Pages** — hospedagem via GitHub Actions

## Estrutura

```
├── index.html                      # Referência da API (Redoc viewer)
├── docs/
│   └── fluxos/
│       └── criacao-pedido.html     # Guia de fluxo (HTML com topnav)
├── .github/workflows/deploy.yml    # Deploy automático no GitHub Pages
├── redocly.yaml                    # Config do Redocly CLI
└── package.json
```

## URL da API

```
https://safracontrol.api.stg.services.souagrosolucoes.com.br/integration/api-json
```

O `index.html` carrega essa URL por padrão. Aceita spec alternativa via query param `?spec=<url>`.

## Como adicionar uma nova página de fluxo

### 1. Criar o arquivo em `docs/fluxos/nome-do-fluxo.html`

Copie o bloco `<head>` e `#topnav` de `docs/fluxos/criacao-pedido.html` integralmente. Ele contém todos os estilos necessários para o layout e a barra de navegação.

Os `href` dentro de `docs/fluxos/` apontam para a raiz com `../../`:

```html
<nav id="topnav">
  <a class="brand" href="../../index.html">SafraControl Integração</a>
  <div class="nav-links">
    <a class="nav-item" href="../../index.html">Referência da API</a>
    <div class="nav-separator"></div>
    <a class="nav-item" href="criacao-pedido.html">Fluxo: Criação de Pedido</a>
    <!-- adicione o link da nova página aqui, com class="active" -->
    <a class="nav-item active" href="nome-do-fluxo.html">
      Novo Fluxo <span class="nav-badge">GUIA</span>
    </a>
  </div>
</nav>
```

### 2. Adicionar o link no `index.html`

No bloco `<div class="nav-links">` do `index.html`:

```html
<a class="nav-item" href="docs/fluxos/nome-do-fluxo.html">
  Novo Fluxo <span class="nav-badge">GUIA</span>
</a>
```

### 3. Adicionar o link nas outras páginas de fluxo

Abra cada arquivo em `docs/fluxos/` e repita o passo 2 no `#topnav` deles, sem o `class="active"`.

## O que NÃO fazer

- **Não criar páginas de fluxo em Markdown** — o Redocly CLI (`preview-docs`) não suporta páginas customizadas no sidebar; isso é uma feature do Redocly Developer Portal (produto separado, pago). Use HTML.
- **Não usar `npm run dev` como `redocly preview-docs`** — o dev server é `npx serve`, que serve o `index.html` estático. O `redocly preview-docs` tem sua própria UI que ignora o `index.html`.
- **Não criar uma pasta `openapi/` com spec local** — a spec é consumida diretamente da URL remota. Não há cópia local.
- **Não adicionar etapa de build no workflow** — o site é 100% estático; o deploy faz upload direto da raiz do repositório.

## Componentes de estilo disponíveis no topnav

Classe `nav-badge`: badge colorido ao lado do label do link.

```html
<span class="nav-badge">GUIA</span>
```

Classe `nav-separator`: divisor vertical entre grupos de links.

```html
<div class="nav-separator"></div>
```

Classe `active` no `nav-item`: destaca o link da página atual.

```html
<a class="nav-item active" href="...">Link atual</a>
```

## Componentes de estilo disponíveis nas páginas de fluxo

Copiados de `criacao-pedido.html`, prontos para reutilizar:

| Classe | Uso |
|---|---|
| `.step-card` | Card numerado de passo a passo |
| `.field-table` | Tabela de campos com colunas tipo/obrigatório |
| `.code-block` | Bloco de código escuro com syntax highlight manual |
| `.alert.alert-info` | Caixa informativa azul |
| `.alert.alert-warn` | Caixa de atenção amarela |
| `.alert.alert-tip` | Caixa de dica verde |
| `.diagram` | Container para diagrama ASCII |
| `.sub-steps` | Grid de cards de sub-etapas |
| `.tag-req` / `.tag-opt` | Badge vermelho/cinza obrigatório/opcional |
