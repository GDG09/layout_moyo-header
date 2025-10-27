## Objetivo rápido

Ajude a completar/ajustar o layout do projeto "Moyo header" (página estática). O foco principal é produzir um `header` fiel ao mock do Figma e compatível com os testes automatizados (pixel-perfect e visuais).

## Arquitetura e arquivos-chave

- Frontend estático simples: HTML + CSS em `src/`.
  - `src/index.html` — ponto de entrada HTML. O `header` deve usar tags semânticas (`<header>`, `<nav>`, `<ul>`, `<li>`, `<a>`).
  - `src/style.css` — estilos do projeto.
  - `src/images/` — ativos (logo, etc.).
- Scripts e CI:
  - `package.json` usa `mate-scripts` para comandos principais: `npm start`, `npm test`, `npm run build`, `npm run deploy`.
  - GitHub Actions workflow em `.github/workflows/test.yml` roda `npm install` e `npm test` em PRs.
- Regressão visual:
  - `backstopConfig.js` define cenários para BackstopJS; tests esperam elementos e atributos específicos:
    - cena `selectors: ['header']`
    - cena `selectors: ['nav']`
    - cena que usa `data-qa="hover"` e `hoverSelector` (espera hover visual)
    - cena que busca `a.is-active` (linha azul via pseudo-elemento)

## Padrões e requisitos específicos do projeto (importantes)

- Visual / testes:
  - O layout deve ser "pixel perfect" em relação ao mock do Figma. Pequenas discrepâncias podem falhar nos testes automatizados.
  - A fonte deve ser Roboto — apenas a variante roman com peso 500 (medium). Veja `readme.md` — seguir exatamente; caso contrário, os testes automáticos podem falhar.

- HTML/CSS conventions:
  - Use tags semânticas: `<header>`, `<nav>`, `<ul>`, `<li>` e `<a>`.
  - O logo é um link com uma `<img>` dentro e NÃO faz parte do `<nav>`.
  - Nav links não devem ter padding; use height para controlar a área clicável e alinhar verticalmente o texto.
  - Não use `gap` em flex containers — os testes não aceitam; use `margin` para espaçamentos entre itens.
  - Não adicione margem antes do primeiro `<li>` nem depois do último `<li>`.
  - A cor azul usada no projeto deve estar em uma variável CSS (p.ex. `--moyo-blue`) — o checklist exige isso.
  - O link ativo deve ter a classe `is-active` e a linha azul abaixo deve ser implementada com `::after` posicionado em relação ao link.
  - O quarto link (texto: "Laptops & computers") deve ter `data-qa="hover"` para o cenário de hover.

## Como os testes e o CI verificam o projeto

- `npm test` (executado pelo workflow) roda os linters (Prettier + mate-scripts lint) e os testes (que incluem BackstopJS). Portanto:
  - Garanta que `npm run format` não altera arquivos (rodar localmente antes de PR).
  - Backstop captura telas em viewports 1024px e 1200px (veja `backstopConfig.js`).

## Exemplos concretos retirados do repositório

 - Ativar link com pseudo-elemento:
   - Exemplo HTML:

```html
&lt;a href=&quot;#&quot; class=&quot;nav__link is-active&quot;&gt;Home&lt;/a&gt;
```

   - Exemplo CSS:

```css
.nav__link.is-active::after {
  content: '';
  position: absolute;
  /* ... */
}
```
- Atributo para hover:
  - HTML: `<a data-qa="hover">Laptops & computers</a>` — utilizado por Backstop (`hoverSelector`).
- Scripts importantes em `package.json`:
  - `npm test` — faz `npm run lint` e executa os testes visuais.
  - `npm run build` / `npm run deploy` — via `mate-scripts` (usualmente para deploy do GitHub Pages).

## O que evitar / observações

- Não altere a estrutura de `backstopConfig.js` nem os nomes de `data-qa` ou da classe `is-active` — os testes dependem desses nomes exatos.
- Evite usar propriedades CSS incomuns ou hacks que alterem tamanhos de fonte/line-height de forma não-padrão (o README desencoraja valores estranhos vindos direto do Figma).

## Onde procurar quando precisar de mais contexto

- `readme.md` — requisitos do layout e checklist de submissão.
- `checklist.md` — regras de estilo e formatação exigidas pelos testes.
- `.stylelintrc.js`, `.linthtmlrc.json`, `.bemlintrc.json` — regras de lint que o CI aplica.

## Comunicação com o revisor / PR

- Ao abrir PR, inclua os links de DEMO e TEST REPORT (veja `readme.md` instruções para substituir `<your_account>`). Mencione se você alterou o nome de classes/atributos usados em testes (evitar isso).

---
Se algo aqui estiver incompleto ou se você quiser que eu adicione exemplos de código (HTML/CSS) mais detalhados, diga o que acrescentar e eu ajusto o arquivo.
