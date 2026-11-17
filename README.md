[![CI - Validação do Portfólio](https://github.com/RenanFelliphe/portfolio/actions/workflows/ci.yml/badge.svg)](https://github.com/RenanFelliphe/portfolio  /actions/workflows/ci.yml)
[![CD - Deploy no GitHub Pages](https://github.com/RenanFelliphe/portfolio/actions/workflows/deploy.yml/badge.svg?branch=main)](https://github.com/RenanFelliphe/portfolio/actions/workflows/deploy.yml)

# Portfólio Profissional — Renan Felliphe Moura

Site de portfólio publicado automaticamente no GitHub Pages por uma esteira CI/CD no GitHub Actions.

🔗 **Site no ar:** https://github.com/RenanFelliphe/portfolio

## Estrutura

```
.
├── .github/workflows/
│   ├── ci.yml        # CI: valida todo Pull Request para a main
│   └── deploy.yml    # CD: publica no GitHub Pages a cada push na main
├── images/           # imagens do site (todas < 500KB)
├── index.html
├── style.css
├── .htmlhintrc       # regras do linter de HTML
└── package.json      # dependências: htmlhint e linkinator
```

## Pipeline de CI (`ci.yml`)

Dispara somente em **Pull Request para a `main`**:

| Job | O que faz |
|---|---|
| Estrutura e Segurança | 1º verifica se `index.html` existe na raiz (falha imediata se não existir); bloqueia arquivos > 500KB com `find`; procura `TODO`, `FIXME` e termos sensíveis no HTML/CSS/JS |
| Linter (Node 18 / Node 20) | Roda o HTMLHint em duas versões do Node com `strategy: matrix` |
| Verificação de Links e Imagens | Linkinator confere todos os `href` e `src` (caminhos locais e URLs externas) |
| CI Aprovado | Consolida o resultado — é o check obrigatório da proteção de branch |

## Pipeline de CD (`deploy.yml`)

Dispara a cada **push na `main`** (ou seja, após o merge do PR). Monta a pasta `_site` só com os arquivos do site e publica no GitHub Pages usando as permissões `pages: write` e `id-token: write`. Se o deploy falhar, o job `notificar-falha` envia aviso no Discord e/ou por e-mail.

## Rodando localmente

```bash
npm install
npm run lint    # linter de HTML
npm run links   # verificação de links
```
