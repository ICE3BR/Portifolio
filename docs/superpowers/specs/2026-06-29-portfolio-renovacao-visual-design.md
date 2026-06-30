# Design Spec — Renovação Visual do Portfólio (Opção B)

**Data:** 2026-06-29  
**Repositório:** `C:\Users\gusta\Downloads\Portifolio` → GitHub Pages em `me.gaqtech.dev`  
**Stack:** HTML/CSS/JS estático (sem build step, sem framework)  
**Objetivo:** Renovar visualmente o portfólio, corrigir bugs invisíveis e adicionar skills de front-end modernas (React/Next.js/TS/Tailwind) — contexto: candidatura à vaga de Front-End Jr na MagaluPay.

---

## Escopo

Renovação **sem troca de stack** (permanece HTML/CSS/JS + Bootstrap 4 + jQuery). O site continua hospedado no GitHub Pages. Nenhuma funcionalidade é removida — apenas corrigida, polida ou adicionada.

---

## 1. Hero

### Problema
Todo o conteúdo (nome + typing effect + nav + 3 ícones sociais) está concentrado no canto esquerdo-inferior da viewport com metade direita completamente vazia.

### Solução
- Dividir o hero em **2 colunas CSS** (`display: flex; align-items: center`):
  - **Esquerda (60%):** conteúdo atual (nome, subtitle, social links) + novo botão CTA
  - **Direita (40%):** elemento decorativo — bloco de código com syntax highlighting em verde/branco
- Centralizar o bloco esquerdo **verticalmente** na viewport (`min-height: 100vh`)
- **Botão CTA:** `→ Ver Projetos` com `href="#portfolio"` e scroll suave, estilizado com borda verde (`border: 2px solid #12d640`), hover com fundo verde e texto escuro
- **Elemento decorativo (direita):** `<pre>` estilizado com fundo semitransparente (`rgba(9,32,58,0.8)`), borda esquerda `#12d640`, fonte monospace, simulando um trecho de código Python/JS relevante ao perfil. Sem animação pesada — apenas um cursor piscando na última linha.
- Em mobile (`< 768px`): coluna direita some, conteúdo esquerdo ocupa 100% centralizado

---

## 2. Bugs e Correções Técnicas

### 2a. AOS — Animações de entrada
- **Problema:** `data-aos="fade-up"` usado em vários elementos mas a biblioteca AOS nunca é importada.
- **Solução:** Adicionar no `<head>`:
  ```html
  <link rel="stylesheet" href="https://unpkg.com/aos@2.3.4/dist/aos.css">
  ```
  E antes de `</body>`:
  ```html
  <script src="https://unpkg.com/aos@2.3.4/dist/aos.js"></script>
  <script>AOS.init({ duration: 600, once: true });</script>
  ```

### 2b. Google Analytics / GTM
- **Contexto:** O portfólio é um fork editado — o código de rastreamento do autor original foi removido. O código GTM/GA4 atual (`GTM-MHM8F4J9`) é do próprio Gustavo mas está mal configurado (GTM Container ID usado como GA4 Measurement ID).
- **Solução no plano de implementação:** Deixar um placeholder comentado no `index.html` com instruções claras para o usuário inserir seu GA4 Measurement ID (`G-XXXXXXXXXX`) e GTM Container ID (`GTM-XXXXXXXX`) corretos após configurar as propriedades no Google Analytics e Google Tag Manager. A implementação técnica dos snippets (posição correta no `<head>` e `<body>`) será feita corretamente no código, mas os IDs reais ficam pendentes de configuração pelo usuário.

### 2c. Logos quebradas nas Skills
- **Problema:** Logos do Git, CSS e outros carregam de CDNs instáveis (vectorlogo.zone, Wikimedia), causando falhas intermitentes.
- **Solução:** Substituir todas as URLs de logo por `https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/` — CDN altamente confiável e com cobertura completa de ícones de tecnologia.

### 2d. Performance — Lazy loading
- Adicionar `loading="lazy"` em todas as `<img>` das seções de projetos, certificações e educação.

### 2e. SEO / Open Graph
- Adicionar `<meta property="og:image" content="https://me.gaqtech.dev/assets/img/profile_2026.jpeg">` no `<head>` para preview correto ao compartilhar o link.

### 2f. PWA — Web App Manifest
- Criar `manifest.json` na raiz com `name`, `short_name`, `icons`, `theme_color` (#010e1b), `background_color`, `display: standalone`
- Adicionar `<link rel="manifest" href="/manifest.json">` no `<head>`
- Habilita "Adicionar à tela inicial" em mobile

---

## 3. Habilidades — Conteúdo

### Adições na categoria "Frameworks & Bibliotecas"
Nível **Intermediário** (adicionar ao lado de Django/Bootstrap/Scikit-learn):
- React.js
- Next.js
- TypeScript
- Tailwind CSS

Justificativa: são exatamente as tecnologias exigidas pela vaga MagaluPay e serão demonstradas no projeto-demo subsequente.

---

## 4. Certificações — Diferenciação Visual

### Problema
3 cards de certificação com a logo DIO idêntica — impossível distinguir sem hover.

### Solução
- O nome do curso (`<h4>` dentro de `.portfolio-info`) já existe no HTML mas fica oculto até hover
- Adicionar uma **cor de borda superior** diferente por tema de certificação diretamente na estrutura dos cards:
  - Azure AI → azul (`#0078d4`)
  - Banco de Dados → laranja (`#e8672c`)
  - Machine Learning → verde (`#12d640`)
- Texto do curso visível **abaixo da logo** (fora do hover), em fonte pequena

---

## 5. Visual Geral — Polish

### 5a. Hover nos cards de projeto
- Adicionar ao `.portfolio-wrap`:
  ```css
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  ```
  No `:hover`:
  ```css
  transform: translateY(-6px);
  box-shadow: 0 12px 30px rgba(18, 214, 64, 0.15);
  ```

### 5b. Section titles
- Estilo atual: `SOBRE ——` (texto + linha horizontal simples)
- Proposta: manter o padrão atual mas adicionar `border-left: 3px solid #12d640` + `padding-left: 10px` no `h2` das section-titles — consistente com o estilo já usado nos tiers de habilidades

### 5c. Navbar blur (quando sticky)
- Quando a navbar passa para o topo ao rolar, adicionar:
  ```css
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  background-color: rgba(1, 14, 27, 0.85);
  ```
  Efeito "glass" moderno sem mudar a cor base.

### 5d. Scroll-to-top button
- Botão fixo no canto inferior direito (`position: fixed; bottom: 24px; right: 24px`)
- Aparece após 300px de scroll (`opacity: 0` → `opacity: 1` com transição)
- Ícone: seta para cima (`↑` ou ícone boxicons)
- Estilo: círculo `48px`, fundo `#12d640`, ícone escuro
- Ao clicar: scroll suave para o topo via `window.scrollTo({ top: 0, behavior: 'smooth' })`

### 5e. Remoção de elementos depreciados
- Substituir todas as ocorrências de `<center>` por `<div class="text-center">` (Bootstrap)
- Mover o bloco `<style>` inline das skills do `index.html` para `assets/css/style.css`

---

## 6. O que NÃO muda

- Identidade visual: cores (`#010e1b`, `#12d640`, `#09203a`), fontes (Open Sans/Raleway/Poppins), logo
- Estrutura de navegação e seções
- Conteúdo textual (bio, experiência, projetos)
- Stack tecnológico (Bootstrap 4, jQuery, Isotope, Venobox, Typed.js)
- Imagens dos projetos (stock photos até screenshots reais estarem disponíveis)
- Hospedagem GitHub Pages / domínio `me.gaqtech.dev`

---

## Arquivos Afetados

| Arquivo | Tipo de mudança |
|---|---|
| `index.html` | Hero (layout + CTA), AOS import, GA4 fix, og:image, manifest link, lazy loading, `<center>` → `text-center`, mover `<style>` inline, skills (novos itens + logos), certificações (cores), scroll-to-top (HTML + JS) |
| `assets/css/style.css` | Hero 2-col layout, decorative code block, CTA button, card hover, section title border, navbar blur, scroll-to-top button |
| `manifest.json` | Novo arquivo na raiz |

---

## Fora do escopo deste spec

- Reescrita em React/Next.js (Opção C — descartada para o portfólio)
- Screenshots reais dos projetos (pendente do usuário)
- Formulário de contato funcional (requer backend)
- Projeto-demo na Vercel (spec separado, próxima etapa)
