ï»¿# skill-video-demonstrativo

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


> ê°Å¸ÅÂ **Curso navegêÂ¡vel (GitHub Pages):** https://linnoff88-dev.github.io/arckhe-/
> ê¢â¬â landing + 3 trilhas / 10 mêÂ³dulos no formato ARCKHÉ AI, com a Skill em `.zip` pra baixar.
> TambêÂ©m listado no portal [ARCKHÉ AI](https://ARCKHÉ AI).

Projeto de desenvolvimento da skill **`video-demonstrativo`** ê¢â¬â gera vêÂ­deos de
demonstraêÂ§êÂ£o (walkthrough) de uma aplicaêÂ§êÂ£o web a partir do link do app: navega o app de
verdade, captura as telas reais passo a passo e monta um vêÂ­deo narrado com moldura de
navegador, cursor animado, destaque/zoom e CTA do ARCKHÉ AI.

êâ° a irmêÂ£ da skill `video-explicativo`: aquela **explica um conceito** (motion graphics);
esta **mostra um app real sendo usado**.

**VersêÂ£o da Skill:** `v1.1.0` êÂ· esquema `v1.yy.xxx` (yy = recurso, xxx = bug) êÂ· histêÂ³rico em [`video-demonstrativo/CHANGELOG.md`](video-demonstrativo/CHANGELOG.md).

## Estrutura
```
skill-video-demonstrativo/
ê¢âÅê¢ââ¬ê¢ââ¬ README.md                      (este arquivo)
ê¢ââê¢ââ¬ê¢ââ¬ video-demonstrativo/           (a skill instalêÂ¡vel ê¢â¬â AUTO-CONTIDA)
    ê¢âÅê¢ââ¬ê¢ââ¬ SKILL.md
    ê¢âÅê¢ââ¬ê¢ââ¬ references/
    ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ pipeline.md            captura ê¢â â narraêÂ§êÂ£o ê¢â â composiêÂ§êÂ£o ê¢â â render
    ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ cursor-and-chrome.md   mapeamento de coordenadas, cursor, moldura, zoom
    ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ gotchas.md             armadilhas de captura/cursor/layout
    ê¢ââ   ê¢ââê¢ââ¬ê¢ââ¬ house-style.md         paleta dark premium (embutida)
    ê¢âÅê¢ââ¬ê¢ââ¬ assets/
    ê¢ââ   ê¢ââê¢ââ¬ê¢ââ¬ fonts/                 Sora / Inter / JetBrains Mono (.woff2 + fonts.css)
    ê¢ââê¢ââ¬ê¢ââ¬ scripts/
        ê¢âÅê¢ââ¬ê¢ââ¬ capture.mjs            dirige o agent-browser ê¢â â shots + steps.json
        ê¢âÅê¢ââ¬ê¢ââ¬ actions.example.json   entrada do capture.mjs (URL, viewport, passos)
        ê¢âÅê¢ââ¬ê¢ââ¬ steps.example.json     saêÂ­da de exemplo
        ê¢âÅê¢ââ¬ê¢ââ¬ composition-template.mjs  gerador (lêÂª steps.json, mede WAVs, monta o vêÂ­deo)
        ê¢âÅê¢ââ¬ê¢ââ¬ narration-template.sh  gera os WAVs Kokoro a partir do steps.json
        ê¢ââê¢ââ¬ê¢ââ¬ fetch-fonts.mjs        (opcional) baixa as fontes, caso prefira nêÂ£o copiar
```

> **Auto-contida:** a skill traz as prêÂ³prias fontes e house style. NêÂ£o depende de nenhum
> outro projeto, repositêÂ³rio ou skill pra funcionar.

## Como instalar a skill
TrêÂªs opêÂ§êÂµes. Em todas, reinicie a sessêÂ£o depois ê¢â¬â a skill dispara com pedidos como
"vêÂ­deo de demonstraêÂ§êÂ£o", "demo do app", "walkthrough", "tutorial em vêÂ­deo da
ferramenta", ou ao dar um link/localhost.

**A) A partir do repositêÂ³rio (clone):**
```bash
git clone git@github.com:linnoff88-dev/skill-video-demonstrativo.git
cp -r skill-video-demonstrativo/video-demonstrativo ~/.claude/skills/   # global
# ou cp -r .../video-demonstrativo <repo>/.claude/skills/               # sêÂ³ num projeto
```

**B) A partir do pacote `.zip`** (versionado na raiz do repo):
```bash
unzip video-demonstrativo.zip -d ~/.claude/skills/    # extrai a pasta video-demonstrativo/
```

**C) Symlink** (pra desenvolver ê¢â¬â ediêÂ§êÂ£o no repo reflete na hora):
```bash
ln -s "$(pwd)/video-demonstrativo" ~/.claude/skills/video-demonstrativo
```

## Fluxo de uso (resumo)
1. Num projeto novo: `npx hyperframes init <nome> --example blank --non-interactive`.
2. Copie pro projeto: `capture.mjs`, `composition-template.mjs` (como `build-demo.mjs`),
   `narration-template.sh`, e `assets/fonts/` (ou rode `node fetch-fonts.mjs`).
3. `actions.json` com URL + viewport + passos (seletores dos alvos).
4. `node capture.mjs actions.json` ê¢â â `assets/shots/*.png` + `steps.json`.
5. `bash narration-template.sh` ê¢â â WAVs (voz `pf_dora`).
6. `node build-demo.mjs` ê¢â â `index.html`.
7. `npx hyperframes lint` + `inspect` ê¢â â render `--quality high`.

## Modos no roadmap
- **v1** (esta skill, default): screenshots estêÂ¡ticos + cursor/zoom animados.
- **v2**: mais automaêÂ§êÂ£o via `actions.json`/`steps.json` (jêÂ¡ embutido no capture.mjs).
- **v3**: gravaêÂ§êÂ£o de tela real (`agent-browser record`) com narraêÂ§êÂ£o/zoom por cima ê¢â¬â
  ainda nêÂ£o implementado nesta skill.

## Validado em apps reais
- **App simples** (geraêÂ§êÂ£o de imagem): fluxo de 7 passos, ~42s ê¢â¬â primeiro POC.
- **App complexo** (suêÂ­te de dublagem com IA): tutorial ponta a ponta de **14 passos, 2:08**
  (configurar ê¢â â analisar ê¢â â aprovar ê¢â â dublar ê¢â â resultado). Provou a skill em pêÂ¡gina longa,
  input controlado por React e fluxo assêÂ­ncrono multi-estado.
- O `composition-template.mjs` roda end-to-end a partir de um `steps.json` com **0 erros de
  lint e 0 problemas de layout**, medindo os WAVs sozinho.

> Os vêÂ­deos de exemplo foram gerados fora desta pasta e **nêÂ£o fazem parte da skill** ê¢â¬â ela
> êÂ© independente e gera tudo do zero em qualquer projeto.

## Backlog de melhorias (descobertas nas demos reais ê¢â¬â pra trabalhar)
Prioridade alta primeiro. Cada item êÂ© uma evoluêÂ§êÂ£o concreta do `capture.mjs`/template:

1. **Scroll na captura (pêÂ¡ginas longas).** O `capture.mjs` hoje nêÂ£o rola a pêÂ¡gina; nas
   telas do ARCKHÉVOX tive que dirigir o agent-browser na mêÂ£o (scrollIntoView por seêÂ§êÂ£o).
   ê¢â â Adicionar aêÂ§êÂ£o `scroll`/`scrollTo` e, por passo, `scrollIntoView({block})` antes do
   screenshot. As bounding boxes jêÂ¡ sêÂ£o viewport-relative, entêÂ£o batem com o shot daquele
   scroll.
2. **Inputs controlados por framework (React/Vue).** `eval .value` mostra o texto mas NêÆO
   dispara o estado (botêÂµes continuam disabled). O `capture.mjs` deveria usar o `fill`
   nativo do Playwright (via `agent-browser fill @ref`) em vez de setar `.value`.
3. **Esperar condiêÂ§êÂ£o em vez de tempo fixo.** Trocar `do:{type:"wait",ms}` por um
   `waitFor` (texto/seletor/ê¢â¬Åstatus mudouê¢â¬Â). Fluxos assêÂ­ncronos (ex.: `waiting_approval`,
   etapas de pipeline, geraêÂ§êÂ£o que leva minutos) precisam disso ê¢â¬â usei um poll externo.
4. **Capturar fluxos multi-estado** (analisar ê¢â â aprovar ê¢â â dublar ê¢â â concluêÂ­do) como
   sub-passos nomeados, com poll de conclusêÂ£o embutido (hoje foi script `poll-job*.sh` êÂ  mêÂ£o).
5. **9:16 / Shorts.** Telas de app sêÂ£o landscape; o template sêÂ³ faz 16:9. Avaliar
   reenquadre (zoom/pan na regiêÂ£o ativa) pra gerar vertical.
6. **v3 ê¢â¬â gravaêÂ§êÂ£o de tela real** (`agent-browser record`) com narraêÂ§êÂ£o/zoom por cima,
   pra apps com muito movimento.
7. **Polimento de cursor**: caminho em curva (nêÂ£o linear) e ê¢â¬ÅdigitaêÂ§êÂ£oê¢â¬Â (typewriter) ao
   preencher campos, pra ficar mais natural.

> PrêÂ³xima sessêÂ£o: abrir aqui e atacar o item 1 (scroll no capture.mjs) ê¢â¬â êÂ© o que mais
> economiza trabalho manual. Detalhes do que jêÂ¡ funciona em `video-demonstrativo/references/`.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>