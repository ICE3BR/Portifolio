# Renovação Visual do Portfólio — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Renovar visualmente o portfólio em `me.gaqtech.dev` — corrigindo bugs, modernizando o visual e adicionando skills de front-end (React/Next.js/TS/Tailwind) alinhadas à candidatura MagaluPay.

**Architecture:** Site estático HTML/CSS/JS sem build step. Todas as mudanças tocam diretamente `index.html`, `assets/css/style.css` e um novo `manifest.json`. Nenhum bundler, nenhum framework, nenhum package.json.

**Tech Stack:** HTML5, CSS3 (Bootstrap 4 + custom), JavaScript vanilla, jQuery, devicons CDN, AOS library

## Global Constraints

- Manter identidade visual: `#010e1b` (fundo), `#12d640` (verde acento), `#09203a` (card bg), `#9e7f25` (dourado)
- Não remover nenhuma seção ou conteúdo existente
- Não alterar a estrutura de navegação
- Bootstrap 4 permanece (não migrar para Bootstrap 5)
- Todas as imagens de projeto permanecem como stock photos (screenshots reais são pendência do usuário)
- GA4/GTM: deixar placeholder comentado — não configurar IDs reais (usuário fará isso depois)
- Deploy via `git push origin master` (GitHub Pages serve da raiz do master)

---

## File Map

| Arquivo | O que muda |
|---|---|
| `index.html` | Hero estrutura (2-col + decorativo + CTA), AOS imports, manifest link, og:image, lazy loading, GA placeholder, `<center>` → `text-center`, mover `<style>` inline, logos skills, novos skills, certificações (bordas coloridas), scroll-to-top (HTML+JS) |
| `assets/css/style.css` | Hero 2-col layout, código decorativo, CTA btn, navbar blur, section title border-left, card hover, scroll-to-top btn, skills block (movido do HTML) |
| `manifest.json` | Novo — PWA básico |

---

## Task 1: Limpeza de código (mover CSS inline + remover `<center>`)

**Files:**
- Modify: `index.html`
- Modify: `assets/css/style.css`

**O que faz:** Move o bloco `<style>` das skills do `index.html` para `style.css` e substitui todas as ocorrências de `<center>` por `<div class="text-center">`. Isso é fundação para as tarefas de CSS que vêm depois.

- [ ] **Step 1: Abrir `index.html` e localizar o bloco `<style>` inline**

Está entre as linhas 491–508 (seção `#skills`). Contém as classes `.skill-group`, `.skill-tier`, `.skill-tier-label`, `.skill-tier-logos` e suas variantes `.adv`, `.int`, `.bas`.

- [ ] **Step 2: Copiar o bloco CSS para `assets/css/style.css`**

Adicionar ao final de `style.css`:

```css
/*--------------------------------------------------------------
# Skills — Tier Layout
--------------------------------------------------------------*/
.skill-group { margin-top: 12px; display: flex; flex-direction: column; gap: 8px; }
.skill-tier { display: flex; align-items: center; gap: 12px; border-radius: 8px; padding: 10px 14px; }
.skill-tier.adv { background: rgba(18,214,64,0.07);  border: 1px solid rgba(18,214,64,0.2); }
.skill-tier.int { background: rgba(255,193,7,0.07);  border: 1px solid rgba(255,193,7,0.2); }
.skill-tier.bas { background: rgba(108,117,125,0.07); border: 1px solid rgba(108,117,125,0.2); }
.skill-tier-label { min-width: 110px; flex-shrink: 0; font-size: 0.75rem; font-weight: 700; text-transform: uppercase; letter-spacing: 0.6px; padding-left: 8px; white-space: nowrap; }
.skill-tier-label.adv { color: #12d640; border-left: 3px solid #12d640; }
.skill-tier-label.int { color: #ffc107; border-left: 3px solid #ffc107; }
.skill-tier-label.bas { color: #6c757d; border-left: 3px solid #6c757d; }
.skill-tier-logos { display: flex; flex-wrap: wrap; align-items: center; gap: 6px; min-width: 0; overflow: hidden; }
.skill-tier-logos img { height: 40px; max-width: 130px; object-fit: contain; flex-shrink: 0; }
@media (max-width: 575px) {
  .skill-tier { flex-direction: column; align-items: flex-start; gap: 4px; }
  .skill-tier-label { min-width: unset; }
  .skill-tier-logos img { height: 32px; max-width: 100px; }
}
```

- [ ] **Step 3: Remover o bloco `<style>` do `index.html`**

Deletar as linhas 491–508 (o bloco `<style>` completo antes de `<section id="skills"`).

- [ ] **Step 4: Substituir `<center>` por `<div class="text-center">`**

Localizar todas as ocorrências de `<center>` na seção `#portfolio` (são as tags de título dos cards de projeto) e substituir:

```html
<!-- ANTES -->
<center><h4>IA Data Analysis</h4></center>

<!-- DEPOIS -->
<div class="text-center"><h4>IA Data Analysis</h4></div>
```

Fazer o mesmo para todos os 11 cards de projeto (IA Data Analysis, Chatbot IA, Downloader de Vídeos, Gerador de QR Code, Busca CEP, Processador de Imagens, Analisador de Documentos, Monitor de Preços, Legal-HUB, Calculadora Previdenciária, LLM Translate Extension).

- [ ] **Step 5: Verificar no browser**

Abrir `index.html` localmente (duplo clique ou Live Server no VS Code). Conferir:
- A seção Habilidades ainda exibe as tiers corretamente
- Os títulos dos cards de projeto ainda estão centralizados

- [ ] **Step 6: Commit**

```bash
git add index.html assets/css/style.css
git commit -m "refactor: mover CSS inline para style.css e remover tags center depreciadas"
```

---

## Task 2: Correções técnicas (AOS, lazy loading, og:image, manifest, GA placeholder)

**Files:**
- Modify: `index.html`
- Create: `manifest.json`

- [ ] **Step 1: Adicionar AOS library no `<head>` do `index.html`**

Após a linha `<link href="assets/css/style.css" rel="stylesheet">`, adicionar:

```html
<!-- AOS — Animate On Scroll -->
<link rel="stylesheet" href="https://unpkg.com/aos@2.3.4/dist/aos.css">
```

- [ ] **Step 2: Adicionar og:image e manifest link no `<head>`**

Após as meta tags Open Graph existentes, adicionar:

```html
<meta property="og:image" content="https://me.gaqtech.dev/assets/img/profile_2026.jpeg">
<link rel="manifest" href="/manifest.json">
<meta name="theme-color" content="#010e1b">
```

- [ ] **Step 3: Substituir o bloco GA/GTM por um placeholder comentado**

Localizar os blocos Google Tag Manager e gtag.js existentes no `<head>` e substituir por:

```html
<!-- ============================================================
  Google Analytics & Tag Manager
  AÇÃO PENDENTE: Configure suas propriedades em:
    - Google Analytics: https://analytics.google.com
    - Google Tag Manager: https://tagmanager.google.com
  Depois substitua este comentário pelos snippets corretos.
  
  Snippet GTM (substituir GTM-XXXXXXXX pelo seu Container ID):
  <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
  new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
  j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
  'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
  })(window,document,'script','dataLayer','GTM-XXXXXXXX');</script>
  
  Snippet GA4 direto (alternativa ao GTM, substituir G-XXXXXXXXXX pelo Measurement ID):
  <script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'G-XXXXXXXXXX');
  </script>
============================================================ -->
```

E substituir o bloco `<noscript>` do GTM logo após o `<body>` por:

```html
<!-- GTM noscript: descomentar após configurar o Container ID acima
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-XXXXXXXX"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
-->
```

- [ ] **Step 4: Adicionar `loading="lazy"` nas imagens**

Localizar todas as `<img>` dentro das seções `#portfolio` (projeto cards) e `#education` (certificações e logos de educação) e adicionar o atributo `loading="lazy"`. Exemplos:

```html
<!-- Cards de projeto -->
<img src="assets/img/project/gan.jpg" class="img-fluid" alt="" loading="lazy">

<!-- Logos de certificação -->
<img src="assets/img/certification/dio_logo.png" class="img-fluid" alt="DIO" loading="lazy">

<!-- Logos de educação -->
<img src="assets/img/education/logo-anhanguera_horizontal.webp" class="img-fluid" alt="Universidade Anhanguera" loading="lazy">
```

NÃO adicionar `loading="lazy"` na foto de perfil (`assets/img/profile_2026.jpeg`) — ela está above the fold.

- [ ] **Step 5: Inicializar AOS antes de `</body>`**

Localizar os scripts de vendor ao final do `index.html`. Após o último `<script>` existente (antes de `</body>`), adicionar:

```html
<!-- AOS Init -->
<script src="https://unpkg.com/aos@2.3.4/dist/aos.js"></script>
<script>AOS.init({ duration: 600, once: true, offset: 50 });</script>
```

- [ ] **Step 6: Criar `manifest.json` na raiz do projeto**

```json
{
  "name": "Gustavo Quinup — Portfólio",
  "short_name": "GAQ Dev",
  "description": "Portfólio de Gustavo Quinup, Engenheiro de Computação especializado em IA, Cloud e Dados.",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#010e1b",
  "theme_color": "#010e1b",
  "icons": [
    {
      "src": "/favicon.png",
      "sizes": "any",
      "type": "image/png"
    }
  ]
}
```

- [ ] **Step 7: Verificar no browser**

Abrir `index.html`. Conferir:
- As animações `data-aos="fade-up"` aparecem ao scrollar (elementos entram com fade)
- DevTools → Application → Manifest: exibe o manifest sem erros
- DevTools → Network: imagens de projeto carregam com `lazy` (aparecem ao scrollar)

- [ ] **Step 8: Commit**

```bash
git add index.html manifest.json
git commit -m "fix: adicionar AOS, og:image, manifest PWA, lazy loading e placeholder GA/GTM"
```

---

## Task 3: Skills — corrigir logos (devicons) e adicionar React/Next/TS/Tailwind

**Files:**
- Modify: `index.html` (seção `#skills`)

**Nota:** Devicons CDN (`cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/`) é altamente confiável e cobre todos os ícones necessários. Alguns logos no fundo escuro precisam da variante `original` (colorida) ou `plain-wordmark` (sem fundo).

- [ ] **Step 1: Substituir logos da categoria "Linguagens & Dados"**

```html
<!-- Avançado -->
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/python/python-original-wordmark.svg" alt="Python">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/mysql/mysql-original-wordmark.svg" alt="MySQL/MariaDB">

<!-- Intermediário -->
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/javascript/javascript-original.svg" alt="JavaScript">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/html5/html5-original-wordmark.svg" alt="HTML5">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/css3/css3-original-wordmark.svg" alt="CSS3">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/postgresql/postgresql-original-wordmark.svg" alt="PostgreSQL">

<!-- Básico -->
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/mongodb/mongodb-original-wordmark.svg" alt="MongoDB">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/dotnetcore/dotnetcore-original.svg" alt=".NET">
```

- [ ] **Step 2: Substituir logos da categoria "Tecnologias & Ferramentas"**

```html
<!-- Avançado -->
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/git/git-original-wordmark.svg" alt="Git">
<img src="assets/img/openai-vector.svg" alt="OpenAI">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/amazonwebservices/amazonwebservices-original-wordmark.svg" alt="AWS">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/linux/linux-original.svg" alt="Linux">

<!-- Intermediário -->
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/docker/docker-original-wordmark.svg" alt="Docker">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/azure/azure-original-wordmark.svg" alt="Azure">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/jupyter/jupyter-original-wordmark.svg" alt="Jupyter">

<!-- Básico -->
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/googlecloud/googlecloud-original-wordmark.svg" alt="Google Cloud">
```

- [ ] **Step 3: Substituir logos + adicionar React/Next/TS/Tailwind na categoria "Frameworks & Bibliotecas"**

```html
<!-- Avançado -->
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/fastapi/fastapi-original-wordmark.svg" alt="FastAPI" style="max-width:50px; height:44px;">

<!-- Intermediário (substitui logos existentes + adiciona os 4 novos) -->
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/django/django-plain-wordmark.svg" alt="Django">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/bootstrap/bootstrap-original-wordmark.svg" alt="Bootstrap">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/scikitlearn/scikitlearn-original.svg" alt="Scikit-learn" style="max-width:70px;">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/react/react-original-wordmark.svg" alt="React.js">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/nextjs/nextjs-original-wordmark.svg" alt="Next.js">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/typescript/typescript-original.svg" alt="TypeScript">
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/tailwindcss/tailwindcss-original-wordmark.svg" alt="Tailwind CSS">

<!-- Básico -->
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/nodejs/nodejs-original-wordmark.svg" alt="Node.js">
```

**Atenção:** Next.js tem ícone preto por padrão (`nextjs-original-wordmark`). Se ficar invisível no fundo escuro, adicionar `style="filter: invert(1);"` no `<img>`.

- [ ] **Step 4: Verificar no browser**

Conferir a seção Habilidades:
- Todas as logos carregam sem ícones quebrados
- React, Next.js, TypeScript e Tailwind aparecem no nível Intermediário
- Git, CSS3, MongoDB visíveis (eram os que quebravam antes)
- Next.js visível no fundo escuro (ajustar `filter: invert(1)` se necessário)

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: corrigir logos via devicons CDN e adicionar React/Next.js/TypeScript/Tailwind nas habilidades"
```

---

## Task 4: Hero — layout 2 colunas + elemento decorativo + CTA

**Files:**
- Modify: `index.html` (seção `#header`)
- Modify: `assets/css/style.css`

**Contexto CSS existente:**
- `#header` já tem `display: flex; align-items: center; height: 100vh`
- `#header .container` no breakpoint `max-width: 992px` recebe `flex-direction: column` — precisamos sobrescrever isso acima de 992px

- [ ] **Step 1: Adicionar CSS do hero 2-col em `style.css`**

Adicionar após o bloco `#header` existente (após a linha do `@media (max-width: 992px)` do header):

```css
/*--------------------------------------------------------------
# Hero — 2-column layout
--------------------------------------------------------------*/
@media (min-width: 993px) {
  #header .container {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    gap: 2rem;
  }
}

.hero-content {
  flex: 0 0 55%;
  max-width: 55%;
}

.hero-decorative {
  flex: 0 0 40%;
  max-width: 40%;
}

@media (max-width: 992px) {
  .hero-content {
    flex: 0 0 100%;
    max-width: 100%;
  }
  .hero-decorative {
    display: none;
  }
}

/* CTA Button */
.hero-cta {
  display: inline-block;
  margin-top: 24px;
  padding: 10px 28px;
  border: 2px solid #12d640;
  color: #12d640;
  font-weight: 600;
  font-size: 0.9rem;
  border-radius: 4px;
  text-decoration: none;
  transition: background 0.25s ease, color 0.25s ease;
  letter-spacing: 0.5px;
  font-family: "Poppins", sans-serif;
}

.hero-cta:hover {
  background: #12d640;
  color: #010e1b;
  text-decoration: none;
}

/* Decorative code block */
.hero-code-block {
  background: rgba(9, 32, 58, 0.85);
  border: 1px solid rgba(18, 214, 64, 0.2);
  border-radius: 10px;
  overflow: hidden;
  font-family: 'Courier New', Courier, monospace;
  font-size: 0.82rem;
  line-height: 1.75;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
}

.hero-code-dots {
  display: flex;
  gap: 6px;
  padding: 10px 14px;
  background: rgba(0, 0, 0, 0.25);
  border-bottom: 1px solid rgba(18, 214, 64, 0.1);
}

.hero-code-dots span {
  width: 12px;
  height: 12px;
  border-radius: 50%;
}
.hero-code-dots span:nth-child(1) { background: #ff5f57; }
.hero-code-dots span:nth-child(2) { background: #ffbd2e; }
.hero-code-dots span:nth-child(3) { background: #28ca41; }

.hero-code-content {
  margin: 0;
  padding: 20px 24px;
  color: #cdd9e5;
  white-space: pre;
  overflow-x: auto;
  background: transparent;
  border: none;
}

.hero-code-content .c  { color: #6e8898; }
.hero-code-content .k  { color: #f97583; }
.hero-code-content .fn { color: #b392f0; }
.hero-code-content .s  { color: #9ecbff; }
.hero-code-content .o  { color: #12d640; }

.code-cursor {
  display: inline-block;
  width: 8px;
  height: 1em;
  background: #12d640;
  vertical-align: text-bottom;
  margin-left: 1px;
  animation: blink-cursor 1s step-end infinite;
}

@keyframes blink-cursor {
  0%, 100% { opacity: 1; }
  50%       { opacity: 0; }
}
```

- [ ] **Step 2: Reestruturar o HTML do `#header` em `index.html`**

Substituir o conteúdo atual do `<header id="header">`:

```html
<!-- ======= Header ======= -->
<header id="header" class="header-tops">
  <div class="container">

    <!-- Coluna esquerda: conteúdo -->
    <div class="hero-content">
      <h1><a href="index.html">Gustavo Quinup</a></h1>
      <h2 style="color:#fff">Sou <span class="typing" style="color:#12D640"></span></h2>

      <nav class="nav-menu d-none d-lg-block">
        <ul>
          <li class="active"><a href="#header"><span>Início</span></a></li>
          <li><a href="#about"><span>Sobre</span></a></li>
          <li><a href="#education"><span>Formação</span></a></li>
          <li><a href="#experience"><span>Experiência</span></a></li>
          <li><a href="#portfolio"><span>Projetos</span></a></li>
          <li><a href="#skills"><span>Habilidades</span></a></li>
          <li><a href="https://drive.google.com/file/d/1g14usUos8zd0Ma1ZNJJjPpqKmOqmipvI/view?usp=sharing" target="_blank"><span>Currículo</span></a></li>
          <li><a href="#contacts"><span>Contato</span></a></li>
        </ul>
      </nav>

      <div class="social-links">
        <a href="https://www.linkedin.com/in/gustavoaquinup2005/" target="_blank" class="linkedin"><i class="bx bxl-linkedin"></i></a>
        <a href="https://github.com/ICE3BR" target="_blank" class="github"><i class="bx bxl-github"></i></a>
        <a href="mailto:gustavoaquinup@gmail.com" target="_blank" class="google"><i class="bx bxl-google"></i></a>
      </div>

      <a href="#portfolio" class="hero-cta">→ Ver Projetos</a>
    </div>

    <!-- Coluna direita: elemento decorativo -->
    <div class="hero-decorative d-none d-lg-block">
      <div class="hero-code-block">
        <div class="hero-code-dots">
          <span></span><span></span><span></span>
        </div>
        <pre class="hero-code-content"><code><span class="c"># Gustavo Quinup — Dev</span>
<span class="k">from</span> skills <span class="k">import</span> React, Python, AI

<span class="k">def</span> <span class="fn">build_solution</span>(problem):
    tools <span class="o">=</span> [React, Python, AI]
    <span class="k">return</span> <span class="fn">optimize</span>(problem, tools)

result <span class="o">=</span> <span class="fn">build_solution</span>(
    <span class="s">"MagaluPay"</span>
)
<span class="c"># → solution deployed ✓</span><span class="code-cursor"></span></code></pre>
      </div>
    </div>

  </div>
</header><!-- End Header -->
```

- [ ] **Step 3: Verificar no browser — desktop**

Abrir `index.html` com janela larga (>993px). Conferir:
- Hero exibe 2 colunas: conteúdo à esquerda, código decorativo à direita
- O bloco de código tem os 3 pontos coloridos (vermelho/amarelo/verde) no topo
- Cursor piscando no final do código
- Botão "→ Ver Projetos" aparece abaixo dos ícones sociais
- Ao clicar no botão, scrollar suavemente até a seção Projetos

- [ ] **Step 4: Verificar no browser — mobile**

Reduzir a janela para menos de 993px (ou usar DevTools → Toggle device). Conferir:
- Coluna direita (código) some
- Conteúdo esquerdo ocupa 100% da largura
- Nome, typing effect, nav mobile e social links intactos

- [ ] **Step 5: Commit**

```bash
git add index.html assets/css/style.css
git commit -m "feat: hero com layout 2 colunas, bloco de código decorativo e botão CTA"
```

---

## Task 5: Visual polish (navbar blur, section titles, card hover, scroll-to-top)

**Files:**
- Modify: `assets/css/style.css`
- Modify: `index.html` (scroll-to-top button HTML + JS)

- [ ] **Step 1: Adicionar navbar blur em `style.css`**

Localizar a seção `/* Desktop Navigation */` (ou o seletor `.sticky-top` / `.header-scrolled` se existir no JS). Verificar em `assets/js/main.js` qual classe é adicionada à navbar ao fazer scroll — tipicamente `.header-scrolled`.

Adicionar em `style.css`:

```css
/*--------------------------------------------------------------
# Navbar — Blur when sticky/scrolled
--------------------------------------------------------------*/
.header-scrolled {
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  background-color: rgba(1, 14, 27, 0.88) !important;
  box-shadow: 0 2px 20px rgba(0, 0, 0, 0.4);
}
```

Se a classe usada for diferente (verificar `main.js`), ajustar o seletor acima.

- [ ] **Step 2: Adicionar border-left nos section titles em `style.css`**

```css
/*--------------------------------------------------------------
# Section Titles — border-left accent
--------------------------------------------------------------*/
.section-title h2 {
  border-left: 3px solid #12d640;
  padding-left: 10px;
}
```

- [ ] **Step 3: Adicionar hover nos cards de projeto em `style.css`**

Localizar qualquer regra existente para `.portfolio-wrap` e adicionar/complementar:

```css
/*--------------------------------------------------------------
# Portfolio Cards — hover lift
--------------------------------------------------------------*/
.portfolio-item .portfolio-wrap {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.portfolio-item .portfolio-wrap:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 30px rgba(18, 214, 64, 0.15);
}
```

- [ ] **Step 4: Adicionar scroll-to-top button em `style.css`**

```css
/*--------------------------------------------------------------
# Scroll-to-top Button
--------------------------------------------------------------*/
#scroll-top {
  position: fixed;
  bottom: 24px;
  right: 24px;
  z-index: 9999;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: #12d640;
  color: #010e1b;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  font-weight: 700;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s ease, visibility 0.3s ease, transform 0.2s ease;
  box-shadow: 0 4px 16px rgba(18, 214, 64, 0.35);
}

#scroll-top.visible {
  opacity: 1;
  visibility: visible;
}

#scroll-top:hover {
  transform: translateY(-3px);
  background: #1c7d32;
}
```

- [ ] **Step 5: Adicionar scroll-to-top HTML + JS em `index.html`**

Logo antes de `</body>` (após os scripts de vendor), adicionar o botão e o script:

```html
<!-- Scroll to Top -->
<button id="scroll-top" aria-label="Voltar ao topo">
  <i class="bx bx-up-arrow-alt"></i>
</button>

<script>
  (function() {
    var btn = document.getElementById('scroll-top');
    window.addEventListener('scroll', function() {
      if (window.scrollY > 300) {
        btn.classList.add('visible');
      } else {
        btn.classList.remove('visible');
      }
    });
    btn.addEventListener('click', function() {
      window.scrollTo({ top: 0, behavior: 'smooth' });
    });
  })();
</script>
```

- [ ] **Step 6: Verificar no browser**

- Scrollar para baixo 300px → botão verde aparece no canto inferior direito
- Clicar no botão → volta suavemente ao topo
- Verificar que os títulos de seção (Sobre, Formação, Experiência, etc.) têm a linha verde à esquerda
- Hover nos cards de projeto → card levanta 6px com sombra esverdeada
- Scrollar para fora do hero → conferir se a navbar muda de aparência (blur + levemente translúcida)

- [ ] **Step 7: Verificar a classe do header no `main.js`**

Abrir `assets/js/main.js` e procurar pela função que adiciona classe ao header ao scrollar. Se a classe for diferente de `.header-scrolled`, voltar ao Step 1 e corrigir o seletor no CSS.

- [ ] **Step 8: Commit**

```bash
git add index.html assets/css/style.css
git commit -m "feat: navbar blur, border-left nos títulos, hover nos cards e botão scroll-to-top"
```

---

## Task 6: Certificações — diferenciação visual por curso

**Files:**
- Modify: `index.html` (seção `#education` → Certificações Online)
- Modify: `assets/css/style.css`

**Contexto:** Os 3 cards de certificação têm logo DIO idêntica. O nome do curso existe no `<h4>` dentro de `.portfolio-info` mas só aparece no hover. Vamos torná-lo visível e adicionar uma cor identificadora.

- [ ] **Step 1: Adicionar CSS das certificações em `style.css`**

```css
/*--------------------------------------------------------------
# Certificações — diferenciação visual
--------------------------------------------------------------*/
.portfolio-item .cert-label {
  text-align: center;
  font-size: 0.8rem;
  font-weight: 600;
  color: #dee2e6;
  padding: 6px 8px 4px;
  background: rgba(9, 32, 58, 0.9);
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
}

.portfolio-item.cert-azure  .portfolio-wrap { border-top: 3px solid #0078d4; }
.portfolio-item.cert-db     .portfolio-wrap { border-top: 3px solid #e8672c; }
.portfolio-item.cert-ml     .portfolio-wrap { border-top: 3px solid #12d640; }
```

- [ ] **Step 2: Atualizar os 3 cards de certificação no `index.html`**

Localizar a seção "Certificações Online" e substituir os 3 cards:

```html
<!-- Azure AI -->
<div class="col-lg-4 col-md-6 portfolio-item filter-app cert-azure">
  <div class="portfolio-wrap">
    <img src="assets/img/certification/dio_logo.png" class="img-fluid" alt="DIO" loading="lazy">
    <div class="cert-label">Azure AI</div>
    <div class="portfolio-info">
      <h4>Azure AI</h4>
      <div class="portfolio-links">
        <a href="https://www.dio.me/certificate/0WKHA92I/share" target="_blank" title="Ver Certificado"><i class="bx bx-link"></i></a>
      </div>
    </div>
  </div>
</div>

<!-- Banco de Dados -->
<div class="col-lg-4 col-md-6 portfolio-item filter-app cert-db">
  <div class="portfolio-wrap">
    <img src="assets/img/certification/dio_logo.png" class="img-fluid" alt="DIO" loading="lazy">
    <div class="cert-label">Banco de Dados</div>
    <div class="portfolio-info">
      <h4>Banco de Dados</h4>
      <div class="portfolio-links">
        <a href="https://www.dio.me/certificate/WE7ZH8DS/share" target="_blank" title="Ver Certificado"><i class="bx bx-link"></i></a>
      </div>
    </div>
  </div>
</div>

<!-- Machine Learning com Python -->
<div class="col-lg-4 col-md-6 portfolio-item filter-project cert-ml">
  <div class="portfolio-wrap">
    <img src="assets/img/certification/dio_logo.png" class="img-fluid" alt="DIO" loading="lazy">
    <div class="cert-label">Machine Learning com Python</div>
    <div class="portfolio-info">
      <h4>Machine Learning com Python</h4>
      <div class="portfolio-links">
        <a href="https://www.dio.me/certificate/721AJ79L/share" target="_blank" title="Ver Certificado"><i class="bx bx-link"></i></a>
      </div>
    </div>
  </div>
</div>
```

**Atenção:** Remover o `style="position: absolute; left: ...; top: ...;"` que estava nos cards originais — era gerado pelo Isotope para o grid de certificações e pode causar problemas de layout. O Isotope recalcula as posições automaticamente via JS.

- [ ] **Step 3: Verificar no browser**

- Seção Certificações exibe os 3 cards com bordas superior coloridas distintas (azul / laranja / verde)
- Label do curso visível abaixo da logo DIO
- Filtros Todos/Aplicações/Scripts ainda funcionam

- [ ] **Step 4: Commit**

```bash
git add index.html assets/css/style.css
git commit -m "feat: certificações com identificação visual por curso (cor de borda + label visível)"
```

---

## Task 7: Deploy e verificação final

**Files:** nenhum

- [ ] **Step 1: Teste completo local**

Abrir `index.html` em um browser e percorrer todas as seções clicando em cada item do menu. Verificar checklist:

| Item | OK? |
|---|---|
| Hero 2-col visível no desktop |  |
| Bloco de código com cursor piscando |  |
| Botão "→ Ver Projetos" funciona |  |
| Animações AOS ao scrollar |  |
| Logo React/Next.js/TS/Tailwind nas habilidades |  |
| Logos Git/CSS/MongoDB carregam corretamente |  |
| Cards de projeto levantam no hover |  |
| Botão scroll-to-top aparece após 300px |  |
| Certificações com bordas coloridas e labels |  |
| Section titles com border-left verde |  |
| Mobile (<993px): código decorativo some |  |
| DevTools Application → Manifest: sem erros |  |

- [ ] **Step 2: Lighthouse audit (opcional mas recomendado)**

No Chrome DevTools → Lighthouse → rodar para Desktop e Mobile.
Focar nos scores de Performance e Accessibility. Anotar para referência.

- [ ] **Step 3: Deploy**

```bash
git push origin master
```

GitHub Pages publica automaticamente em ~1-2 minutos.

- [ ] **Step 4: Verificar em `me.gaqtech.dev`**

Abrir o domínio e repetir o checklist do Step 1 no ambiente de produção. Verificar que as CDNs externas (devicons, AOS, Google Fonts) carregam corretamente.

- [ ] **Step 5: Lembrete GA/GTM**

Após deploy, configurar as propriedades no Google Analytics e Google Tag Manager e substituir o placeholder comentado no `index.html` pelos snippets reais.

---

## Notas de implementação

**Ordem das tasks:** 1 → 2 → 3 → 4 → 5 → 6 → 7. Cada task pode ser commitada independentemente.

**Conflito CSS potencial:** A task 4 (hero) sobrescreve o `@media (max-width: 992px)` existente que adiciona `flex-direction: column` ao `#header .container`. O CSS novo adiciona uma contrapartida `@media (min-width: 993px)` — isso deve funcionar corretamente, mas testar em 992px exato para confirmar.

**Next step:** Após concluir este plano, o próximo projeto é criar o **projeto-demo** na Vercel com Next.js — spec separado.
