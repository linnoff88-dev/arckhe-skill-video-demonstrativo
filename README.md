# skill-video-demonstrativo

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


> ðŸŒ **Curso navegÃ¡vel (GitHub Pages):** https://inematds.github.io/skill-video-demonstrativo/
> â€” landing + 3 trilhas / 10 mÃ³dulos no formato INEMA.CLUB, com a Skill em `.zip` pra baixar.
> TambÃ©m listado no portal [inema.club](https://inema.club).

Projeto de desenvolvimento da skill **`video-demonstrativo`** â€” gera vÃ­deos de
demonstraÃ§Ã£o (walkthrough) de uma aplicaÃ§Ã£o web a partir do link do app: navega o app de
verdade, captura as telas reais passo a passo e monta um vÃ­deo narrado com moldura de
navegador, cursor animado, destaque/zoom e CTA do INEMA.CLUB.

Ã‰ a irmÃ£ da skill `video-explicativo`: aquela **explica um conceito** (motion graphics);
esta **mostra um app real sendo usado**.

**VersÃ£o da Skill:** `v1.1.0` Â· esquema `v1.yy.xxx` (yy = recurso, xxx = bug) Â· histÃ³rico em [`video-demonstrativo/CHANGELOG.md`](video-demonstrativo/CHANGELOG.md).

## Estrutura
```
skill-video-demonstrativo/
â”œâ”€â”€ README.md                      (este arquivo)
â””â”€â”€ video-demonstrativo/           (a skill instalÃ¡vel â€” AUTO-CONTIDA)
    â”œâ”€â”€ SKILL.md
    â”œâ”€â”€ references/
    â”‚   â”œâ”€â”€ pipeline.md            captura â†’ narraÃ§Ã£o â†’ composiÃ§Ã£o â†’ render
    â”‚   â”œâ”€â”€ cursor-and-chrome.md   mapeamento de coordenadas, cursor, moldura, zoom
    â”‚   â”œâ”€â”€ gotchas.md             armadilhas de captura/cursor/layout
    â”‚   â””â”€â”€ house-style.md         paleta dark premium (embutida)
    â”œâ”€â”€ assets/
    â”‚   â””â”€â”€ fonts/                 Sora / Inter / JetBrains Mono (.woff2 + fonts.css)
    â””â”€â”€ scripts/
        â”œâ”€â”€ capture.mjs            dirige o agent-browser â†’ shots + steps.json
        â”œâ”€â”€ actions.example.json   entrada do capture.mjs (URL, viewport, passos)
        â”œâ”€â”€ steps.example.json     saÃ­da de exemplo
        â”œâ”€â”€ composition-template.mjs  gerador (lÃª steps.json, mede WAVs, monta o vÃ­deo)
        â”œâ”€â”€ narration-template.sh  gera os WAVs Kokoro a partir do steps.json
        â””â”€â”€ fetch-fonts.mjs        (opcional) baixa as fontes, caso prefira nÃ£o copiar
```

> **Auto-contida:** a skill traz as prÃ³prias fontes e house style. NÃ£o depende de nenhum
> outro projeto, repositÃ³rio ou skill pra funcionar.

## Como instalar a skill
TrÃªs opÃ§Ãµes. Em todas, reinicie a sessÃ£o depois â€” a skill dispara com pedidos como
"vÃ­deo de demonstraÃ§Ã£o", "demo do app", "walkthrough", "tutorial em vÃ­deo da
ferramenta", ou ao dar um link/localhost.

**A) A partir do repositÃ³rio (clone):**
```bash
git clone git@github.com:inematds/skill-video-demonstrativo.git
cp -r skill-video-demonstrativo/video-demonstrativo ~/.claude/skills/   # global
# ou cp -r .../video-demonstrativo <repo>/.claude/skills/               # sÃ³ num projeto
```

**B) A partir do pacote `.zip`** (versionado na raiz do repo):
```bash
unzip video-demonstrativo.zip -d ~/.claude/skills/    # extrai a pasta video-demonstrativo/
```

**C) Symlink** (pra desenvolver â€” ediÃ§Ã£o no repo reflete na hora):
```bash
ln -s "$(pwd)/video-demonstrativo" ~/.claude/skills/video-demonstrativo
```

## Fluxo de uso (resumo)
1. Num projeto novo: `npx hyperframes init <nome> --example blank --non-interactive`.
2. Copie pro projeto: `capture.mjs`, `composition-template.mjs` (como `build-demo.mjs`),
   `narration-template.sh`, e `assets/fonts/` (ou rode `node fetch-fonts.mjs`).
3. `actions.json` com URL + viewport + passos (seletores dos alvos).
4. `node capture.mjs actions.json` â†’ `assets/shots/*.png` + `steps.json`.
5. `bash narration-template.sh` â†’ WAVs (voz `pf_dora`).
6. `node build-demo.mjs` â†’ `index.html`.
7. `npx hyperframes lint` + `inspect` â†’ render `--quality high`.

## Modos no roadmap
- **v1** (esta skill, default): screenshots estÃ¡ticos + cursor/zoom animados.
- **v2**: mais automaÃ§Ã£o via `actions.json`/`steps.json` (jÃ¡ embutido no capture.mjs).
- **v3**: gravaÃ§Ã£o de tela real (`agent-browser record`) com narraÃ§Ã£o/zoom por cima â€”
  ainda nÃ£o implementado nesta skill.

## Validado em apps reais
- **App simples** (geraÃ§Ã£o de imagem): fluxo de 7 passos, ~42s â€” primeiro POC.
- **App complexo** (suÃ­te de dublagem com IA): tutorial ponta a ponta de **14 passos, 2:08**
  (configurar â†’ analisar â†’ aprovar â†’ dublar â†’ resultado). Provou a skill em pÃ¡gina longa,
  input controlado por React e fluxo assÃ­ncrono multi-estado.
- O `composition-template.mjs` roda end-to-end a partir de um `steps.json` com **0 erros de
  lint e 0 problemas de layout**, medindo os WAVs sozinho.

> Os vÃ­deos de exemplo foram gerados fora desta pasta e **nÃ£o fazem parte da skill** â€” ela
> Ã© independente e gera tudo do zero em qualquer projeto.

## Backlog de melhorias (descobertas nas demos reais â€” pra trabalhar)
Prioridade alta primeiro. Cada item Ã© uma evoluÃ§Ã£o concreta do `capture.mjs`/template:

1. **Scroll na captura (pÃ¡ginas longas).** O `capture.mjs` hoje nÃ£o rola a pÃ¡gina; nas
   telas do inemaVOX tive que dirigir o agent-browser na mÃ£o (scrollIntoView por seÃ§Ã£o).
   â†’ Adicionar aÃ§Ã£o `scroll`/`scrollTo` e, por passo, `scrollIntoView({block})` antes do
   screenshot. As bounding boxes jÃ¡ sÃ£o viewport-relative, entÃ£o batem com o shot daquele
   scroll.
2. **Inputs controlados por framework (React/Vue).** `eval .value` mostra o texto mas NÃƒO
   dispara o estado (botÃµes continuam disabled). O `capture.mjs` deveria usar o `fill`
   nativo do Playwright (via `agent-browser fill @ref`) em vez de setar `.value`.
3. **Esperar condiÃ§Ã£o em vez de tempo fixo.** Trocar `do:{type:"wait",ms}` por um
   `waitFor` (texto/seletor/â€œstatus mudouâ€). Fluxos assÃ­ncronos (ex.: `waiting_approval`,
   etapas de pipeline, geraÃ§Ã£o que leva minutos) precisam disso â€” usei um poll externo.
4. **Capturar fluxos multi-estado** (analisar â†’ aprovar â†’ dublar â†’ concluÃ­do) como
   sub-passos nomeados, com poll de conclusÃ£o embutido (hoje foi script `poll-job*.sh` Ã  mÃ£o).
5. **9:16 / Shorts.** Telas de app sÃ£o landscape; o template sÃ³ faz 16:9. Avaliar
   reenquadre (zoom/pan na regiÃ£o ativa) pra gerar vertical.
6. **v3 â€” gravaÃ§Ã£o de tela real** (`agent-browser record`) com narraÃ§Ã£o/zoom por cima,
   pra apps com muito movimento.
7. **Polimento de cursor**: caminho em curva (nÃ£o linear) e â€œdigitaÃ§Ã£oâ€ (typewriter) ao
   preencher campos, pra ficar mais natural.

> PrÃ³xima sessÃ£o: abrir aqui e atacar o item 1 (scroll no capture.mjs) â€” Ã© o que mais
> economiza trabalho manual. Detalhes do que jÃ¡ funciona em `video-demonstrativo/references/`.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>