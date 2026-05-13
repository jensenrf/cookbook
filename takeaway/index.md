---
layout: default
title: Takeaway
permalink: /takeaway/
description: Clip any recipe from the web and save it as a markdown file for your cookbook.
---

<style>
  /* ── Takeaway page styles ── */
  .takeaway-wrap {
    max-width: 620px;
    margin: 0 auto;
    padding: 3.5rem 2rem 6rem;
  }

  .takeaway-header {
    margin-bottom: 3rem;
  }

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

  /* ── API key row ── */
  .key-row {
    display: flex;
    gap: .5rem;
    align-items: center;
    margin-bottom: 2.5rem;
    padding: 1rem 1.2rem;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
  }

  .key-row label {
    font-size: .7rem;
    letter-spacing: .1em;
    text-transform: uppercase;
    color: var(--muted);
    white-space: nowrap;
    flex-shrink: 0;
  }

  .key-row input {
    flex: 1;
    border: none;
    outline: none;
    background: transparent;
    font-family: monospace;
    font-size: .82rem;
    color: var(--ink);
    min-width: 0;
  }

  .key-row input::placeholder { color: var(--border); }

  .btn-key-save {
    font-size: .7rem;
    letter-spacing: .07em;
    text-transform: uppercase;
    color: var(--muted);
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 5px;
    padding: .35em .8em;
    cursor: pointer;
    white-space: nowrap;
    font-family: inherit;
    transition: color .15s, border-color .15s;
    flex-shrink: 0;
  }

  .btn-key-save:hover { color: var(--ink); border-color: var(--ink); }

  .key-saved-badge {
    font-size: .7rem;
    color: #4a8c5c;
    white-space: nowrap;
    flex-shrink: 0;
    display: none;
  }

  /* ── URL input + button ── */
  .clip-form {
    display: flex;
    gap: .75rem;
    align-items: stretch;
    margin-bottom: 1.5rem;
  }

  @media (max-width: 480px) {
    .clip-form { flex-direction: column; }
  }

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

  /* ── The takeaway container button ── */
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

  /* SVG takeaway container icon */
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

  @media (max-width: 480px) {
    .steps { grid-template-columns: 1fr; }
  }

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

  .api-link {
    color: var(--ink);
    text-decoration: underline;
    text-decoration-color: var(--border);
  }
  .api-link:hover { text-decoration-color: var(--ink); }
</style>

<div class="takeaway-wrap">

  <div class="takeaway-header">
    <p class="takeaway-eyebrow">Recipe Clipper</p>
    <h1 class="takeaway-title">Takeaway</h1>
    <p class="takeaway-subtitle">Paste a recipe link. Get a markdown file, ready for your cookbook.</p>
  </div>

  <!-- API Key -->
  <div class="key-row" id="keyRow">
    <label for="apiKeyInput">API Key</label>
    <input type="password" id="apiKeyInput" placeholder="sk-ant-..." autocomplete="off" spellcheck="false">
    <span class="key-saved-badge" id="keySavedBadge">✓ saved</span>
    <button class="btn-key-save" id="btnKeySave">Save</button>
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
      <!-- Takeaway food container SVG -->
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
        <!-- container body -->
        <path d="M4 8h16l-1.5 10a2 2 0 0 1-2 1.8H7.5a2 2 0 0 1-2-1.8L4 8Z"/>
        <!-- lid -->
        <path d="M3 6a1 1 0 0 1 1-1h16a1 1 0 0 1 1 1v2H3V6Z"/>
        <!-- handle tabs on lid -->
        <path d="M9 5V4a1 1 0 0 1 1-1h4a1 1 0 0 1 1 1v1"/>
        <!-- fork lines inside container (food detail) -->
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

  <!-- How it works -->
  <div class="how-it-works">
    <h2>How it works</h2>
    <div class="steps">
      <div class="step">
        <span class="step-num">1.</span>
        Add your <a class="api-link" href="https://console.anthropic.com" target="_blank" rel="noopener">Anthropic API key</a> once — it's saved in your browser only.
      </div>
      <div class="step">
        <span class="step-num">2.</span>
        Paste any recipe URL and click the container. Claude reads the page and formats it.
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

  // ── Elements ────────────────────────────────────────────────────
  const apiKeyInput   = document.getElementById('apiKeyInput');
  const btnKeySave    = document.getElementById('btnKeySave');
  const keySavedBadge = document.getElementById('keySavedBadge');
  const urlInput      = document.getElementById('urlInput');
  const btnClip       = document.getElementById('btnClip');
  const statusLine    = document.getElementById('statusLine');
  const previewCard   = document.getElementById('previewCard');
  const previewBody   = document.getElementById('previewBody');
  const previewFilename = document.getElementById('previewFilename');
  const btnDownload   = document.getElementById('btnDownload');

  const LS_KEY = 'takeaway_anthropic_key';
  let currentMarkdown = '';
  let currentFilename = 'recipe.md';

  // ── Load saved key ───────────────────────────────────────────────
  const savedKey = localStorage.getItem(LS_KEY);
  if (savedKey) {
    apiKeyInput.value = savedKey;
    showKeyBadge();
  }

  // ── Save key ─────────────────────────────────────────────────────
  btnKeySave.addEventListener('click', () => {
    const k = apiKeyInput.value.trim();
    if (!k) return;
    if (!k.startsWith('sk-ant-')) {
      setStatus('error', 'Key should start with sk-ant-');
      return;
    }
    localStorage.setItem(LS_KEY, k);
    showKeyBadge();
    setStatus('', '');
  });

  // Also save on Enter in key field
  apiKeyInput.addEventListener('keydown', e => {
    if (e.key === 'Enter') btnKeySave.click();
  });

  function showKeyBadge() {
    keySavedBadge.style.display = 'inline';
    btnKeySave.textContent = 'Update';
  }

  // ── Clip on Enter in URL field ───────────────────────────────────
  urlInput.addEventListener('keydown', e => {
    if (e.key === 'Enter') btnClip.click();
  });

  // ── Main clip action ─────────────────────────────────────────────
  btnClip.addEventListener('click', async () => {
    const apiKey = localStorage.getItem(LS_KEY) || apiKeyInput.value.trim();
    const url    = urlInput.value.trim();

    if (!apiKey || !apiKey.startsWith('sk-ant-')) {
      setStatus('error', 'Please save your Anthropic API key first.');
      return;
    }

    if (!url || !url.startsWith('http')) {
      setStatus('error', 'Please enter a valid recipe URL.');
      return;
    }

    setLoading(true);
    previewCard.classList.remove('visible');
    setStatus('', 'Fetching recipe page…');

    try {
      // Step 1: fetch the recipe page via a CORS proxy
      const pageText = await fetchPageText(url);

      setStatus('', 'Parsing with Claude…');

      // Step 2: send to Anthropic API
      const markdown = await parseWithClaude(pageText, url, apiKey);

      currentMarkdown = markdown;
      currentFilename = filenameFromMarkdown(markdown, url);

      previewBody.textContent = markdown;
      previewFilename.textContent = currentFilename;
      btnDownload.textContent = '↓ Save to Downloads';
      btnDownload.classList.remove('saved');
      previewCard.classList.add('visible');

      setStatus('success', '✓ Recipe clipped — preview below.');
    } catch (err) {
      setStatus('error', err.message);
    } finally {
      setLoading(false);
    }
  });

  // ── Download ─────────────────────────────────────────────────────
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

  // ── Fetch page text via CORS proxy ────────────────────────────────
  async function fetchPageText(url) {
    // allorigins.win is a reliable open CORS proxy — returns plain text
    const proxyUrl = `https://api.allorigins.win/get?url=${encodeURIComponent(url)}`;
    const res = await fetch(proxyUrl);
    if (!res.ok) throw new Error('Could not fetch the recipe page. Check the URL and try again.');
    const data = await res.json();
    if (!data.contents) throw new Error('No content returned from the recipe page.');

    // Strip HTML tags, collapse whitespace, truncate
    const stripped = data.contents
      .replace(/<script[\s\S]*?<\/script>/gi, '')
      .replace(/<style[\s\S]*?<\/style>/gi, '')
      .replace(/<[^>]+>/g, ' ')
      .replace(/\s{2,}/g, '\n')
      .trim()
      .slice(0, 14000);

    if (stripped.length < 100) throw new Error('Page content too short — it may require a login or be behind a paywall.');
    return stripped;
  }

  // ── Anthropic API call ────────────────────────────────────────────
  async function parseWithClaude(pageText, sourceUrl, apiKey) {
    const system = `You are a recipe parser. You extract recipes from raw webpage text and format them as Jekyll markdown files for a personal cookbook.

Output ONLY valid Jekyll markdown — no preamble, no explanation, no code fences. Start with --- and end with the last instruction step.

Use EXACTLY this format:

---
layout: recipe
title: "Recipe Title Here"
description: "One sentence description of the dish."
category: Category
tags: [Tag1, Tag2, Tag3]
prep_time: X min
cook_time: X min
servings: X
difficulty: Easy
image: /assets/images/recipe-slug.jpg

ingredients:
  - ingredient one with amount
  - ingredient two with amount
---

1. First instruction step, written clearly.

1. Second step. (All steps use "1." — Jekyll auto-numbers.)

1. Third step.

Rules:
- category: one of Pasta, Baking, Poultry, Beef, Seafood, Vegetarian, Salad, Soup, Breakfast, Dessert, Drinks, Sides
- difficulty: Easy, Medium, or Hard
- tags: 2–4 short tags
- image filename: title lowercased, spaces to hyphens, .jpg extension
- ingredients use 2-space indent YAML list format
- Write instructions as clear numbered prose, not bullets
- Omit any front matter field you cannot determine`;

    const res = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': apiKey,
        'anthropic-version': '2023-06-01',
        'anthropic-dangerous-direct-browser-access': 'true'
      },
      body: JSON.stringify({
        model: 'claude-opus-4-5',
        max_tokens: 2000,
        system,
        messages: [{
          role: 'user',
          content: `Extract the recipe from this page and format it.\n\nSource URL: ${sourceUrl}\n\nPage text:\n${pageText}`
        }]
      })
    });

    if (!res.ok) {
      const err = await res.json().catch(() => ({}));
      const msg = err.error?.message || '';
      if (res.status === 401) throw new Error('Invalid API key — check it and try again.');
      if (res.status === 429) throw new Error('API rate limit hit — wait a moment and try again.');
      throw new Error('API error: ' + (msg || res.status));
    }

    const data = await res.json();
    const text = data.content?.[0]?.text?.trim();
    if (!text) throw new Error('Claude returned an empty response.');
    return text;
  }

  // ── Helpers ───────────────────────────────────────────────────────
  function setLoading(on) {
    btnClip.disabled = on;
    btnClip.classList.toggle('loading', on);
  }

  function setStatus(type, msg) {
    statusLine.className = 'status-line' + (type ? ' ' + type : '');
    statusLine.textContent = msg;
  }

  function filenameFromMarkdown(md, fallbackUrl) {
    const m = md.match(/^title:\s*["']?(.+?)["']?\s*$/m);
    if (m) {
      return m[1].toLowerCase()
        .replace(/[^a-z0-9\s-]/g, '')
        .trim()
        .replace(/\s+/g, '-') + '.md';
    }
    try {
      const slug = new URL(fallbackUrl).pathname.split('/').filter(Boolean).pop() || 'recipe';
      return slug.replace(/\.[^.]+$/, '') + '.md';
    } catch { return 'recipe.md'; }
  }

})();
</script>
