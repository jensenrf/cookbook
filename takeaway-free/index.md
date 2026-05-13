---
layout: default
title: Takeaway Free
permalink: /takeaway-free/
description: Clip any recipe from the web for free — no API key needed.
---

<style>
  .takeaway-wrap {
    max-width: 620px;
    margin: 0 auto;
    padding: 3.5rem 2rem 6rem;
  }

  .takeaway-header { margin-bottom: 3rem; }

  .takeaway-eyebrow {
    font-size: .75rem;
    letter-spacing: .14em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: .6rem;
  }

  .takeaway-title {
    font-family: 'Playfair Display', serif;
    font-size: clamp(1.8rem, 4vw, 2.6rem);
    font-weight: 600;
    letter-spacing: -.025em;
    line-height: 1.1;
    margin-bottom: .8rem;
  }

  .takeaway-subtitle {
    font-size: .95rem;
    color: var(--muted);
    font-style: italic;
    margin: 0;
    line-height: 1.6;
  }

  /* ── Free badge ── */
  .free-badge {
    display: inline-block;
    font-size: .68rem;
    letter-spacing: .1em;
    text-transform: uppercase;
    color: #4a8c5c;
    background: #edf7f1;
    border: 1px solid #b8dfc6;
    border-radius: 4px;
    padding: .25em .7em;
    margin-left: .6rem;
    vertical-align: middle;
    position: relative;
    top: -3px;
  }

  /* ── Clip form ── */
  .clip-form {
    display: flex;
    gap: .75rem;
    align-items: stretch;
    margin-bottom: 1.5rem;
  }

  @media (max-width: 480px) { .clip-form { flex-direction: column; } }

  .url-input {
    flex: 1;
    padding: .9rem 1.1rem;
    border: 1px solid var(--border);
    border-radius: 10px;
    background: var(--surface);
    font-family: 'Source Serif 4', serif;
    font-size: .92rem;
    color: var(--ink);
    outline: none;
    transition: border-color .15s, box-shadow .15s;
    min-width: 0;
  }

  .url-input:focus {
    border-color: var(--ink);
    box-shadow: 0 0 0 3px rgba(26,23,20,.06);
  }

  .url-input::placeholder { color: var(--border); }

  /* ── Takeaway container button ── */
  .btn-takeaway {
    flex-shrink: 0;
    width: 58px;
    height: 58px;
    border: none;
    background: var(--ink);
    border-radius: 10px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background .15s, transform .12s;
    position: relative;
  }

  .btn-takeaway:hover:not(:disabled) { background: #2e2a26; }
  .btn-takeaway:active:not(:disabled) { transform: scale(.94); }
  .btn-takeaway:disabled { background: var(--border); cursor: not-allowed; }

  .btn-takeaway svg {
    width: 28px;
    height: 28px;
    color: white;
    transition: opacity .15s;
  }

  .btn-takeaway.loading svg { opacity: 0; }

  .btn-takeaway .spinner {
    position: absolute;
    width: 20px; height: 20px;
    border: 2px solid rgba(255,255,255,.25);
    border-top-color: white;
    border-radius: 50%;
    animation: spin .65s linear infinite;
    display: none;
  }

  .btn-takeaway.loading .spinner { display: block; }

  @keyframes spin { to { transform: rotate(360deg); } }

  /* ── Status ── */
  .status-line {
    font-size: .85rem;
    color: var(--muted);
    font-style: italic;
    min-height: 1.4rem;
    margin-bottom: 1.5rem;
    display: flex;
    align-items: center;
    gap: .5rem;
  }

  .status-line.error { color: #b84040; font-style: normal; }
  .status-line.success { color: #4a8c5c; font-style: normal; }

  /* ── Preview card ── */
  .preview-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    overflow: hidden;
    display: none;
  }

  .preview-card.visible { display: block; }

  .preview-card-header {
    padding: 1rem 1.2rem;
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
  }

  .preview-filename {
    font-family: monospace;
    font-size: .8rem;
    color: var(--muted);
  }

  .btn-download {
    display: inline-flex;
    align-items: center;
    gap: .4rem;
    padding: .5rem 1rem;
    background: var(--ink);
    color: white;
    border: none;
    border-radius: 7px;
    font-family: 'Source Serif 4', serif;
    font-size: .82rem;
    cursor: pointer;
    white-space: nowrap;
    transition: background .15s;
  }

  .btn-download:hover { background: #2e2a26; }
  .btn-download.saved { background: #4a8c5c; }

  .preview-body {
    padding: 1.2rem;
    font-family: monospace;
    font-size: .78rem;
    line-height: 1.6;
    color: var(--ink);
    max-height: 340px;
    overflow-y: auto;
    white-space: pre-wrap;
    word-break: break-word;
    background: #fdfcfb;
  }

  /* ── Fallback CTA ── */
  .fallback-cta {
    display: none;
    margin-top: 2rem;
    padding: 1.4rem 1.6rem;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
  }

  .fallback-cta.visible { display: block; }

  .fallback-cta-eyebrow {
    font-size: .68rem;
    letter-spacing: .12em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: .5rem;
  }

  .fallback-cta h3 {
    font-family: 'Playfair Display', serif;
    font-size: 1.05rem;
    font-weight: 600;
    margin-bottom: .5rem;
    color: var(--ink);
  }

  .fallback-cta p {
    font-size: .88rem;
    color: var(--muted);
    margin-bottom: 1rem;
    line-height: 1.55;
  }

  .cta-link {
    display: inline-flex;
    align-items: center;
    gap: .4rem;
    padding: .6rem 1.1rem;
    background: var(--ink);
    color: white;
    border-radius: 7px;
    font-size: .85rem;
    text-decoration: none;
    transition: background .15s;
  }

  .cta-link:hover { background: #2e2a26; text-decoration: none; }

  /* ── Supported sites ── */
  .sites-note {
    margin-top: 2rem;
    padding: 1.2rem 1.4rem;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
  }

  .sites-note p {
    font-size: .82rem;
    color: var(--muted);
    margin: 0;
    line-height: 1.6;
  }

  .sites-note strong { color: var(--ink); font-weight: 400; }

  /* ── How it works ── */
  .how-it-works {
    margin-top: 3.5rem;
    padding-top: 2.5rem;
    border-top: 1px solid var(--border);
  }

  .how-it-works h2 {
    font-family: 'Playfair Display', serif;
    font-size: 1rem;
    font-weight: 600;
    margin-bottom: 1.2rem;
  }

  .steps {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1.5rem;
  }

  @media (max-width: 480px) { .steps { grid-template-columns: 1fr; } }

  .step {
    font-size: .85rem;
    color: var(--muted);
    line-height: 1.55;
  }

  .step-num {
    font-family: 'Playfair Display', serif;
    font-size: 1.3rem;
    color: var(--ink);
    display: block;
    margin-bottom: .3rem;
  }

  .inline-link {
    color: var(--ink);
    text-decoration: underline;
    text-decoration-color: var(--border);
  }

  .inline-link:hover { text-decoration-color: var(--ink); }
</style>

<div class="takeaway-wrap">

  <div class="takeaway-header">
    <p class="takeaway-eyebrow">Recipe Clipper</p>
    <h1 class="takeaway-title">Takeaway <span class="free-badge">Free</span></h1>
    <p class="takeaway-subtitle">Paste a recipe link. Get a markdown file instantly — no account, no API key.</p>
  </div>

  <!-- Clip form -->
  <div class="clip-form">
    <input
      type="url"
      class="url-input"
      id="urlInput"
      placeholder="https://www.seriouseats.com/your-recipe"
      autocomplete="off"
      spellcheck="false"
    >
    <button class="btn-takeaway" id="btnClip" title="Clip recipe">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
        <path d="M4 8h16l-1.5 10a2 2 0 0 1-2 1.8H7.5a2 2 0 0 1-2-1.8L4 8Z"/>
        <path d="M3 6a1 1 0 0 1 1-1h16a1 1 0 0 1 1 1v2H3V6Z"/>
        <path d="M9 5V4a1 1 0 0 1 1-1h4a1 1 0 0 1 1 1v1"/>
        <line x1="9" y1="12" x2="9" y2="16"/>
        <line x1="12" y1="11" x2="12" y2="16"/>
        <line x1="15" y1="12" x2="15" y2="16"/>
      </svg>
      <div class="spinner"></div>
    </button>
  </div>

  <div class="status-line" id="statusLine"></div>

  <!-- Result preview -->
  <div class="preview-card" id="previewCard">
    <div class="preview-card-header">
      <span class="preview-filename" id="previewFilename">recipe.md</span>
      <button class="btn-download" id="btnDownload">↓ Save to Downloads</button>
    </div>
    <div class="preview-body" id="previewBody"></div>
  </div>

  <!-- Fallback CTA — shown when parsing fails -->
  <div class="fallback-cta" id="fallbackCta">
    <p class="fallback-cta-eyebrow">Didn't work on this site?</p>
    <h3>Try Takeaway with AI</h3>
    <p>Large sites like Food Network, NYT Cooking, and Serious Eats block external requests — and some don't use standard recipe markup at all. The AI-powered version uses Claude to read any page directly, so it works everywhere.</p>
    <a class="cta-link" href="/cookbook/takeaway/">Try Takeaway → with your API key</a>
  </div>

  <!-- Supported sites note -->
  <div class="sites-note" id="sitesNote">
    <p><strong>Works best with:</strong> food blogs, WordPress recipe sites, AllRecipes, BBC Good Food, Tasty, Delish, and any site using standard recipe markup. <strong>Doesn't work with:</strong> Food Network, NYT Cooking, Serious Eats, Bon Appétit, or any site that blocks external requests or requires a login — use <a class="inline-link" href="/cookbook/takeaway/">Takeaway →</a> for those.</p>
  </div>

  <!-- How it works -->
  <div class="how-it-works">
    <h2>How it works</h2>
    <div class="steps">
      <div class="step">
        <span class="step-num">1.</span>
        Paste any recipe URL from a major site and click the container. No sign-in needed.
      </div>
      <div class="step">
        <span class="step-num">2.</span>
        The page is fetched and its embedded recipe data (JSON-LD schema) is extracted instantly.
      </div>
      <div class="step">
        <span class="step-num">3.</span>
        Download the <code>.md</code> file and drop it into <code>_recipes/</code> in your repo.
      </div>
    </div>
  </div>

</div>

<script>
(function () {
  'use strict';

  const urlInput      = document.getElementById('urlInput');
  const btnClip       = document.getElementById('btnClip');
  const statusLine    = document.getElementById('statusLine');
  const previewCard   = document.getElementById('previewCard');
  const previewBody   = document.getElementById('previewBody');
  const previewFilename = document.getElementById('previewFilename');
  const btnDownload   = document.getElementById('btnDownload');
  const fallbackCta   = document.getElementById('fallbackCta');
  const sitesNote     = document.getElementById('sitesNote');

  let currentMarkdown = '';
  let currentFilename = 'recipe.md';

  urlInput.addEventListener('keydown', e => { if (e.key === 'Enter') btnClip.click(); });

  btnClip.addEventListener('click', async () => {
    const url = urlInput.value.trim();
    if (!url || !url.startsWith('http')) {
      setStatus('error', 'Please enter a valid recipe URL.');
      return;
    }

    setLoading(true);
    previewCard.classList.remove('visible');
    fallbackCta.classList.remove('visible');
    setStatus('', 'Fetching recipe…');

    try {
      const html = await fetchHtml(url);
      const recipe = extractRecipeSchema(html, url);

      if (!recipe) {
        setStatus('error', 'No recipe data found — this site may not use standard recipe markup.');
        fallbackCta.classList.add('visible');
        return;
      }

      const markdown = buildMarkdown(recipe, url);
      currentMarkdown = markdown;
      currentFilename = toFilename(recipe.name || recipe.title || url);

      previewBody.textContent = markdown;
      previewFilename.textContent = currentFilename;
      btnDownload.textContent = '↓ Save to Downloads';
      btnDownload.classList.remove('saved');
      previewCard.classList.add('visible');
      setStatus('success', '✓ Recipe clipped — preview below.');

    } catch (err) {
      setStatus('error', err.message);
      fallbackCta.classList.add('visible');
    } finally {
      setLoading(false);
    }
  });

  btnDownload.addEventListener('click', () => {
    if (!currentMarkdown) return;
    const blob = new Blob([currentMarkdown], { type: 'text/markdown' });
    const a = document.createElement('a');
    a.href = URL.createObjectURL(blob);
    a.download = currentFilename;
    a.click();
    URL.revokeObjectURL(a.href);
    btnDownload.textContent = '✓ Saved!';
    btnDownload.classList.add('saved');
    setTimeout(() => {
      btnDownload.textContent = '↓ Save to Downloads';
      btnDownload.classList.remove('saved');
    }, 2500);
  });

  // ── Fetch HTML via CORS proxy ──────────────────────────────────
  // Known sites that block proxy fetches
  const BLOCKED_SITES = [
    'foodnetwork.com', 'nytimes.com', 'cooking.nytimes.com',
    'seriouseats.com', 'bonappetit.com', 'epicurious.com',
    'washingtonpost.com', 'wsj.com', 'saveur.com'
  ];

  function isBlockedSite(url) {
    try {
      const host = new URL(url).hostname.replace('www.', '');
      return BLOCKED_SITES.some(s => host.includes(s));
    } catch { return false; }
  }

  async function fetchHtml(url) {
    if (isBlockedSite(url)) {
      throw new Error('This site blocks external requests. Try Takeaway → with an API key instead — it can handle any site.');
    }
    const proxyUrl = `https://api.allorigins.win/get?url=${encodeURIComponent(url)}`;
    let res;
    try {
      res = await fetch(proxyUrl);
    } catch (e) {
      throw new Error('Network error — check your connection and try again.');
    }
    if (!res.ok) throw new Error('Could not reach the recipe page. The site may be blocking requests.');
    const data = await res.json();
    if (!data.contents || data.contents.length < 500) {
      throw new Error('The page returned no usable content — it may require a login or block external access.');
    }
    return data.contents;
  }

  // ── Extract JSON-LD Recipe schema ─────────────────────────────
  function extractRecipeSchema(html, sourceUrl) {
    // Parse the HTML string into a document
    const parser = new DOMParser();
    const doc = parser.parseFromString(html, 'text/html');

    // Find all JSON-LD script tags
    const scripts = doc.querySelectorAll('script[type="application/ld+json"]');
    let recipe = null;

    for (const script of scripts) {
      try {
        const raw = script.textContent.trim();
        if (!raw) continue;
        const json = JSON.parse(raw);

        // Handle single object or @graph array
        const candidates = json['@graph']
          ? json['@graph']
          : Array.isArray(json) ? json : [json];

        for (const item of candidates) {
          const type = item['@type'];
          const isRecipe = type === 'Recipe' ||
            (Array.isArray(type) && type.includes('Recipe'));
          if (isRecipe) { recipe = item; break; }
        }
        if (recipe) break;
      } catch (e) { /* malformed JSON, skip */ }
    }

    // Fallback: try meta tags for title/image if no schema but at least get something
    if (!recipe) return null;
    return recipe;
  }

  // ── Build Jekyll markdown from schema ────────────────────────
  function buildMarkdown(r, sourceUrl) {
    const title       = clean(r.name || r.headline || 'Untitled Recipe');
    const description = clean(r.description || '');
    const image       = extractImage(r);
    const prepTime    = parseDuration(r.prepTime);
    const cookTime    = parseDuration(r.cookTime);
    const totalTime   = parseDuration(r.totalTime);
    const servings    = extractServings(r);
    const category    = guessCategory(r);
    const tags        = extractTags(r);
    const difficulty  = guessDifficulty(r);
    const ingredients = extractIngredients(r);
    const instructions = extractInstructions(r);
    const imageSlug   = '/assets/images/' + toFilename(title).replace('.md', '.jpg');

    let fm = '---\n';
    fm += `layout: recipe\n`;
    fm += `title: "${title.replace(/"/g, "'")}"\n`;
    if (description) fm += `description: "${truncate(description, 120).replace(/"/g, "'")}"\n`;
    if (category)    fm += `category: ${category}\n`;
    if (tags.length) fm += `tags: [${tags.join(', ')}]\n`;
    if (prepTime)    fm += `prep_time: ${prepTime}\n`;
    if (cookTime || totalTime) fm += `cook_time: ${cookTime || totalTime}\n`;
    if (servings)    fm += `servings: ${servings}\n`;
    if (difficulty)  fm += `difficulty: ${difficulty}\n`;
    fm += `image: ${image || imageSlug}\n`;
    fm += '\ningredients:\n';
    for (const ing of ingredients) fm += `  - ${clean(ing)}\n`;
    fm += '---\n\n';

    for (const step of instructions) {
      fm += `1. ${clean(step)}\n\n`;
    }

    return fm.trimEnd() + '\n';
  }

  // ── Schema helpers ────────────────────────────────────────────
  function extractImage(r) {
    if (!r.image) return null;
    if (typeof r.image === 'string') return r.image;
    if (Array.isArray(r.image)) return r.image[0]?.url || r.image[0];
    if (typeof r.image === 'object') return r.image.url || null;
    return null;
  }

  function extractServings(r) {
    const s = r.recipeYield;
    if (!s) return '';
    if (typeof s === 'string') return s;
    if (Array.isArray(s)) return s[0];
    return String(s);
  }

  function extractIngredients(r) {
    const raw = r.recipeIngredient || r.ingredients || [];
    return Array.isArray(raw) ? raw.filter(Boolean) : [String(raw)];
  }

  function extractInstructions(r) {
    const raw = r.recipeInstructions || r.instructions || [];
    if (!raw) return [];

    // Can be string, array of strings, array of HowToStep objects, or array of HowToSection
    if (typeof raw === 'string') {
      return raw.split(/\n+/).map(s => s.trim()).filter(Boolean);
    }

    const steps = [];
    const items = Array.isArray(raw) ? raw : [raw];

    for (const item of items) {
      if (typeof item === 'string') {
        steps.push(item.trim());
      } else if (item['@type'] === 'HowToSection' && item.itemListElement) {
        // Sections contain nested steps
        for (const sub of item.itemListElement) {
          const text = sub.text || sub.name || '';
          if (text.trim()) steps.push(text.trim());
        }
      } else {
        const text = item.text || item.name || '';
        if (text.trim()) steps.push(text.trim());
      }
    }

    return steps.filter(Boolean);
  }

  function extractTags(r) {
    const raw = r.keywords || r.recipeCategory || [];
    let tags = [];
    if (typeof raw === 'string') {
      tags = raw.split(/[,;]/).map(t => t.trim()).filter(Boolean);
    } else if (Array.isArray(raw)) {
      tags = raw.flatMap(t => typeof t === 'string' ? t.split(/[,;]/) : []).map(t => t.trim()).filter(Boolean);
    }
    // Cap at 4 tags, title-case them
    return tags.slice(0, 4).map(t => t.charAt(0).toUpperCase() + t.slice(1).toLowerCase());
  }

  const CATEGORY_MAP = {
    pasta: 'Pasta', noodle: 'Pasta', spaghetti: 'Pasta', rigatoni: 'Pasta',
    bak: 'Baking', bread: 'Baking', cake: 'Baking', cookie: 'Baking', muffin: 'Baking', pastry: 'Baking', tart: 'Baking',
    chicken: 'Poultry', turkey: 'Poultry', duck: 'Poultry', poultry: 'Poultry',
    beef: 'Beef', steak: 'Beef', burger: 'Beef', lamb: 'Beef', pork: 'Beef',
    fish: 'Seafood', salmon: 'Seafood', shrimp: 'Seafood', seafood: 'Seafood', prawn: 'Seafood',
    salad: 'Salad',
    soup: 'Soup', stew: 'Soup', chili: 'Soup', broth: 'Soup',
    breakfast: 'Breakfast', egg: 'Breakfast', pancake: 'Breakfast', waffle: 'Breakfast', oat: 'Breakfast',
    dessert: 'Dessert', 'ice cream': 'Dessert', pudding: 'Dessert', brownie: 'Dessert',
    cocktail: 'Drinks', smoothie: 'Drinks', juice: 'Drinks', drink: 'Drinks',
    vegetarian: 'Vegetarian', vegan: 'Vegetarian', veggie: 'Vegetarian',
    side: 'Sides', rice: 'Sides', potato: 'Sides', roast: 'Sides'
  };

  function guessCategory(r) {
    const haystack = [
      r.name, r.description, r.recipeCategory,
      ...(Array.isArray(r.keywords) ? r.keywords : [r.keywords])
    ].filter(Boolean).join(' ').toLowerCase();

    for (const [kw, cat] of Object.entries(CATEGORY_MAP)) {
      if (haystack.includes(kw)) return cat;
    }
    return 'Sides';
  }

  function guessDifficulty(r) {
    // Some schemas include difficulty or aggregate ratings we can use
    if (r.difficulty) return r.difficulty;
    const steps = extractInstructions(r).length;
    const ings  = extractIngredients(r).length;
    const prep  = parseDurationMinutes(r.prepTime) || 0;
    const cook  = parseDurationMinutes(r.cookTime) || parseDurationMinutes(r.totalTime) || 0;
    const total = prep + cook;
    if (steps <= 5 && ings <= 8 && total <= 30) return 'Easy';
    if (steps >= 12 || ings >= 16 || total >= 120) return 'Hard';
    return 'Medium';
  }

  // ── Duration parsing ──────────────────────────────────────────
  // ISO 8601: PT1H30M → "1 hr 30 min"
  function parseDuration(iso) {
    if (!iso) return '';
    const h = iso.match(/(\d+)H/);
    const m = iso.match(/(\d+)M/);
    const hours = h ? parseInt(h[1]) : 0;
    const mins  = m ? parseInt(m[1]) : 0;
    if (!hours && !mins) return '';
    if (hours && mins) return `${hours} hr ${mins} min`;
    if (hours) return `${hours} hr`;
    return `${mins} min`;
  }

  function parseDurationMinutes(iso) {
    if (!iso) return 0;
    const h = iso.match(/(\d+)H/);
    const m = iso.match(/(\d+)M/);
    return (h ? parseInt(h[1]) * 60 : 0) + (m ? parseInt(m[1]) : 0);
  }

  // ── String helpers ────────────────────────────────────────────
  function clean(str) {
    if (typeof str !== 'string') str = String(str || '');
    return str.replace(/<[^>]+>/g, '').replace(/\s+/g, ' ').trim();
  }

  function truncate(str, n) {
    return str.length <= n ? str : str.slice(0, n).replace(/\s\S*$/, '') + '…';
  }

  function toFilename(str) {
    return String(str)
      .toLowerCase()
      .replace(/[^a-z0-9\s-]/g, '')
      .trim()
      .replace(/\s+/g, '-')
      + '.md';
  }

  // ── UI helpers ────────────────────────────────────────────────
  function setLoading(on) {
    btnClip.disabled = on;
    btnClip.classList.toggle('loading', on);
  }

  function setStatus(type, msg) {
    statusLine.className = 'status-line' + (type ? ' ' + type : '');
    statusLine.textContent = msg;
  }

})();
</script>
