---
layout: default
title: Takeaway Free
permalink: /takeaway-free/
description: Paste any recipe text and get a markdown file for your cookbook instantly.
---

<style>
  .takeaway-wrap {
    max-width: 640px;
    margin: 0 auto;
    padding: 3.5rem 2rem 6rem;
  }

  .takeaway-header { margin-bottom: 2.5rem; }

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
    font-size: .65rem;
    letter-spacing: .1em;
    text-transform: uppercase;
    color: #4a8c5c;
    background: #edf7f1;
    border: 1px solid #b8dfc6;
    border-radius: 4px;
    padding: .22em .65em;
    margin-left: .5rem;
    vertical-align: middle;
    position: relative;
    top: -4px;
    font-family: 'Source Serif 4', serif;
    font-style: normal;
  }

  /* ── Step labels ── */
  .step-label {
    font-size: .7rem;
    letter-spacing: .12em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: .6rem;
    display: flex;
    align-items: center;
    gap: .5rem;
  }

  .step-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--border);
  }

  /* ── URL input row (optional, decorative) ── */
  .url-row {
    display: flex;
    gap: .5rem;
    margin-bottom: 1rem;
  }

  .url-input {
    flex: 1;
    padding: .75rem 1rem;
    border: 1px solid var(--border);
    border-radius: 8px;
    background: var(--surface);
    font-family: 'Source Serif 4', serif;
    font-size: .88rem;
    color: var(--muted);
    outline: none;
    transition: border-color .15s;
    min-width: 0;
  }

  .url-input:focus { border-color: var(--ink); color: var(--ink); }
  .url-input::placeholder { color: var(--border); }

  /* ── Paste textarea ── */
  .paste-area {
    width: 100%;
    min-height: 220px;
    padding: 1rem 1.1rem;
    border: 1px solid var(--border);
    border-radius: 10px;
    background: var(--surface);
    font-family: 'Source Serif 4', serif;
    font-size: .88rem;
    color: var(--ink);
    line-height: 1.65;
    resize: vertical;
    outline: none;
    transition: border-color .15s, box-shadow .15s;
    margin-bottom: .75rem;
  }

  .paste-area:focus {
    border-color: var(--ink);
    box-shadow: 0 0 0 3px rgba(26,23,20,.05);
  }

  .paste-area::placeholder { color: var(--border); }

  /* ── Action row ── */
  .action-row {
    display: flex;
    align-items: center;
    gap: .75rem;
    margin-bottom: 1.5rem;
  }

  .paste-hint {
    font-size: .8rem;
    color: var(--muted);
    font-style: italic;
    flex: 1;
  }

  /* ── Recipe card button ── */
  .btn-clip {
    flex-shrink: 0;
    width: 56px;
    height: 56px;
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

  .btn-clip:hover:not(:disabled) { background: #2e2a26; }
  .btn-clip:active:not(:disabled) { transform: scale(.94); }
  .btn-clip:disabled { background: var(--border); cursor: not-allowed; }

  .btn-clip svg {
    width: 26px;
    height: 26px;
    color: white;
    transition: opacity .15s;
  }

  .btn-clip.loading svg { opacity: 0; }

  .btn-clip .spinner {
    position: absolute;
    width: 18px; height: 18px;
    border: 2px solid rgba(255,255,255,.25);
    border-top-color: white;
    border-radius: 50%;
    animation: spin .65s linear infinite;
    display: none;
  }

  .btn-clip.loading .spinner { display: block; }

  @keyframes spin { to { transform: rotate(360deg); } }

  /* ── Status ── */
  .status-line {
    font-size: .85rem;
    color: var(--muted);
    font-style: italic;
    min-height: 1.3rem;
    margin-bottom: 1.2rem;
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
    padding: .9rem 1.2rem;
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
  }

  .preview-filename {
    font-family: monospace;
    font-size: .78rem;
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
    padding: 1.1rem 1.2rem;
    font-family: monospace;
    font-size: .76rem;
    line-height: 1.6;
    color: var(--ink);
    max-height: 320px;
    overflow-y: auto;
    white-space: pre-wrap;
    word-break: break-word;
    background: #fdfcfb;
  }

  /* ── Paid CTA ── */
  .paid-cta {
    margin-top: 2rem;
    padding: 1.2rem 1.4rem;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
  }

  .paid-cta-label {
    font-size: .68rem;
    letter-spacing: .1em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: .3rem;
  }

  .paid-cta p {
    font-size: .88rem;
    color: var(--muted);
    margin-bottom: .8rem;
    line-height: 1.55;
  }

  .cta-link {
    display: inline-flex;
    align-items: center;
    gap: .4rem;
    padding: .55rem 1rem;
    background: var(--ink);
    color: white;
    border-radius: 7px;
    font-size: .82rem;
    text-decoration: none;
    transition: background .15s;
  }

  .cta-link:hover { background: #2e2a26; text-decoration: none; }

  /* ── How it works ── */
  .how-it-works {
    margin-top: 3rem;
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

  @media (max-width: 500px) { .steps { grid-template-columns: 1fr; } }

  .step {
    font-size: .84rem;
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
    <p class="takeaway-subtitle">Copy any recipe text from any website — paste it here, get a markdown file. No account, no API key, no fetch required.</p>
  </div>

  <!-- Step 1: Optional URL for reference -->
  <p class="step-label">Step 1 — Paste the recipe text</p>

  <textarea
    class="paste-area"
    id="pasteArea"
    placeholder="Copy everything from the recipe page — title, ingredients, instructions, cook time — and paste it here. More text is better; the parser will extract what it needs."
    spellcheck="false"
  ></textarea>

  <div class="action-row">
    <p class="paste-hint">Paste from any recipe site — works even behind logins.</p>
    <button class="btn-clip" id="btnClip" title="Parse recipe">
      <!-- Recipe card icon (recreated from Noun Project #291038 by Marianna Nardella) -->
      <svg viewBox="0 0 100 100" fill="none" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
        <!-- Card body -->
        <rect x="15" y="8" width="70" height="84" rx="6" ry="6" fill="white" stroke="white" stroke-width="2"/>
        <!-- Fork on the left -->
        <line x1="28" y1="22" x2="28" y2="38" stroke="currentColor" stroke-width="5" stroke-linecap="round"/>
        <line x1="23" y1="22" x2="23" y2="30" stroke="currentColor" stroke-width="4" stroke-linecap="round"/>
        <line x1="33" y1="22" x2="33" y2="30" stroke="currentColor" stroke-width="4" stroke-linecap="round"/>
        <line x1="28" y1="38" x2="28" y2="52" stroke="currentColor" stroke-width="5" stroke-linecap="round"/>
        <!-- Text lines on the right -->
        <line x1="44" y1="26" x2="76" y2="26" stroke="currentColor" stroke-width="5" stroke-linecap="round"/>
        <line x1="44" y1="36" x2="76" y2="36" stroke="currentColor" stroke-width="5" stroke-linecap="round"/>
        <line x1="44" y1="46" x2="68" y2="46" stroke="currentColor" stroke-width="5" stroke-linecap="round"/>
        <!-- Divider -->
        <line x1="20" y1="62" x2="80" y2="62" stroke="currentColor" stroke-width="3" stroke-linecap="round" opacity=".4"/>
        <!-- Bottom text lines -->
        <line x1="20" y1="72" x2="80" y2="72" stroke="currentColor" stroke-width="4" stroke-linecap="round"/>
        <line x1="20" y1="81" x2="80" y2="81" stroke="currentColor" stroke-width="4" stroke-linecap="round"/>
        <line x1="20" y1="90" x2="60" y2="90" stroke="currentColor" stroke-width="4" stroke-linecap="round"/>
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

  <!-- Paid CTA -->
  <div class="paid-cta">
    <p class="paid-cta-label">Want to skip the copy-paste?</p>
    <p>Try <a class="inline-link" href="/cookbook/takeaway/">Takeaway →</a> — paste a URL and Claude fetches and formats the recipe automatically. Requires an Anthropic API key.</p>
  </div>

  <!-- How it works -->
  <div class="how-it-works">
    <h2>How it works</h2>
    <div class="steps">
      <div class="step">
        <span class="step-num">1.</span>
        Open any recipe page. Select all the text — title, ingredients, steps — and copy it.
      </div>
      <div class="step">
        <span class="step-num">2.</span>
        Paste it into the box and click the recipe card button. Formatting happens locally in your browser.
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

  const pasteArea    = document.getElementById('pasteArea');
  const btnClip      = document.getElementById('btnClip');
  const statusLine   = document.getElementById('statusLine');
  const previewCard  = document.getElementById('previewCard');
  const previewBody  = document.getElementById('previewBody');
  const previewFilename = document.getElementById('previewFilename');
  const btnDownload  = document.getElementById('btnDownload');

  let currentMarkdown = '';
  let currentFilename = 'recipe.md';

  // Parse on Cmd/Ctrl+Enter in textarea
  pasteArea.addEventListener('keydown', e => {
    if (e.key === 'Enter' && (e.metaKey || e.ctrlKey)) btnClip.click();
  });

  btnClip.addEventListener('click', () => {
    const text = pasteArea.value.trim();
    if (text.length < 80) {
      setStatus('error', 'Paste more text — a full recipe is needed to parse correctly.');
      return;
    }

    setLoading(true);
    previewCard.classList.remove('visible');
    setStatus('', 'Parsing…');

    // Run synchronously after a tick so the UI updates first
    setTimeout(() => {
      try {
        const recipe = parseRecipeText(text);
        currentMarkdown = buildMarkdown(recipe);
        currentFilename = toFilename(recipe.title);

        previewBody.textContent = currentMarkdown;
        previewFilename.textContent = currentFilename;
        btnDownload.textContent = '↓ Save to Downloads';
        btnDownload.classList.remove('saved');
        previewCard.classList.add('visible');
        setStatus('success', '✓ Recipe parsed — check the preview and save.');
      } catch (e) {
        setStatus('error', e.message);
      } finally {
        setLoading(false);
      }
    }, 30);
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

  // ─────────────────────────────────────────────────────────────
  //  PARSER — regex-based, works on plain pasted text
  // ─────────────────────────────────────────────────────────────

  function parseRecipeText(raw) {
    // Normalise line endings, collapse 3+ blank lines
    const text = raw.replace(/\r\n/g, '\n').replace(/\n{4,}/g, '\n\n\n').trim();
    const lines = text.split('\n').map(l => l.trim());

    return {
      title:        extractTitle(lines, text),
      description:  extractDescription(lines, text),
      prepTime:     extractTime(text, /prep\s*time[:\s]+([^\n,\.]+)/i),
      cookTime:     extractTime(text, /(?:cook|bake|total)\s*time[:\s]+([^\n,\.]+)/i),
      servings:     extractServings(text),
      difficulty:   extractDifficulty(text),
      category:     extractCategory(text),
      tags:         extractTags(text),
      ingredients:  extractIngredients(lines, text),
      instructions: extractInstructions(lines, text),
    };
  }

  function extractTitle(lines, text) {
    // First non-empty line that isn't a nav/meta artifact is usually the title
    for (const line of lines) {
      if (line.length > 4 && line.length < 100 &&
          !/^(jump to|print|pin|save|share|home|recipe|course|serves|prep|cook|total|rating|by |author)/i.test(line) &&
          !/^https?:/i.test(line)) {
        return line.replace(/^#+\s*/, '').trim();
      }
    }
    return 'Untitled Recipe';
  }

  function extractDescription(lines, text) {
    // Look for a sentence-length line after the title that reads like a description
    let foundTitle = false;
    for (const line of lines) {
      if (!foundTitle) { foundTitle = true; continue; }
      if (line.length > 30 && line.length < 280 &&
          /[a-z]/.test(line) &&
          !/^(ingredient|instruction|direction|step|method|note|prep|cook|total|serves|yield|jump|print|pin)/i.test(line) &&
          !line.match(/^\d/) &&
          !/^https?:/i.test(line)) {
        return line.replace(/[*_]/g, '').trim();
      }
    }
    return '';
  }

  function extractTime(text, pattern) {
    const m = text.match(pattern);
    if (!m) return '';
    // Clean up the captured group
    return m[1].trim()
      .replace(/\s+/g, ' ')
      .replace(/minutes?/gi, 'min')
      .replace(/hours?/gi, 'hr')
      .slice(0, 30);
  }

  function extractServings(text) {
    const m = text.match(/(?:serves?|yield|servings?|makes?)[:\s]+([^\n,\.]{1,20})/i);
    return m ? m[1].trim().slice(0, 20) : '';
  }

  function extractDifficulty(text) {
    const m = text.match(/\b(easy|medium|hard|simple|difficult|intermediate|beginner|advanced)\b/i);
    if (!m) {
      // Guess from step count
      return '';
    }
    const w = m[1].toLowerCase();
    if (['easy', 'simple', 'beginner'].includes(w)) return 'Easy';
    if (['hard', 'difficult', 'advanced'].includes(w)) return 'Hard';
    return 'Medium';
  }

  const CATEGORY_WORDS = [
    ['pasta', 'spaghetti', 'penne', 'rigatoni', 'fettuccine', 'linguine', 'noodle'],
    ['chicken', 'poultry', 'turkey', 'duck'],
    ['beef', 'steak', 'burger', 'lamb', 'pork', 'meatball'],
    ['fish', 'salmon', 'tuna', 'shrimp', 'prawn', 'seafood', 'cod', 'halibut'],
    ['salad', 'slaw'],
    ['soup', 'stew', 'chili', 'chowder', 'broth', 'bisque'],
    ['breakfast', 'pancake', 'waffle', 'omelette', 'egg', 'frittata', 'oatmeal'],
    ['cake', 'cookie', 'bread', 'muffin', 'brownie', 'tart', 'pastry', 'biscuit', 'scone', 'loaf'],
    ['dessert', 'pudding', 'ice cream', 'gelato', 'tiramisu', 'cheesecake'],
    ['cocktail', 'smoothie', 'juice', 'lemonade', 'drink', 'mocktail'],
    ['vegetarian', 'vegan', 'tofu', 'tempeh', 'lentil'],
  ];

  const CATEGORY_NAMES = ['Pasta','Poultry','Beef','Seafood','Salad','Soup','Breakfast','Baking','Dessert','Drinks','Vegetarian'];

  function extractCategory(text) {
    const lower = text.toLowerCase();
    for (let i = 0; i < CATEGORY_WORDS.length; i++) {
      if (CATEGORY_WORDS[i].some(w => lower.includes(w))) return CATEGORY_NAMES[i];
    }
    return 'Sides';
  }

  function extractTags(text) {
    const lower = text.toLowerCase();
    const candidates = [
      'quick','easy','one-pot','gluten-free','dairy-free','vegetarian','vegan',
      'mexican','italian','asian','greek','mediterranean','american',
      'grilled','baked','fried','roasted','slow cooker','instant pot',
      'weeknight','make-ahead','freezer-friendly','summer','winter',
    ];
    return candidates.filter(t => lower.includes(t.replace(/-/g, ' '))).slice(0, 4)
      .map(t => t.split(' ').map(w => w.charAt(0).toUpperCase() + w.slice(1)).join(' '));
  }

  function extractIngredients(lines, text) {
    // Find the ingredients section
    const ingStart = findSectionStart(lines, /^ingred/i);
    const ingEnd   = findSectionEnd(lines, ingStart, /^(instruct|direct|method|steps?|how to|preparat)/i);

    if (ingStart === -1) return extractFallbackIngredients(lines);

    const slice = lines.slice(ingStart + 1, ingEnd === -1 ? ingStart + 60 : ingEnd);
    const ings = [];

    for (const line of slice) {
      if (!line || line.length < 3) continue;
      if (/^(instruct|direct|method|step|note|tip|for the|how to)/i.test(line)) break;
      // Skip obvious non-ingredient lines
      if (line.length > 120) continue;
      if (/^#+/.test(line) && line.length > 40) continue;
      // Clean up bullets, dashes, numbers
      const cleaned = line.replace(/^[-•*·◦▪▸→]\s*/, '').replace(/^\d+\.\s*/, '').trim();
      if (cleaned.length > 2) ings.push(cleaned);
    }

    return ings.length > 0 ? ings : extractFallbackIngredients(lines);
  }

  function extractFallbackIngredients(lines) {
    // Heuristic: lines that look like measurements
    return lines.filter(l =>
      /^[\d¼½¾⅓⅔⅛⅜⅝⅞]/.test(l) &&
      l.length > 3 && l.length < 100
    ).slice(0, 20);
  }

  function extractInstructions(lines, text) {
    const instStart = findSectionStart(lines, /^(instruct|direct|method|steps?|how to|preparat)/i);
    if (instStart === -1) return extractFallbackInstructions(lines);

    const slice = lines.slice(instStart + 1);
    const steps = [];
    let current = '';

    for (const line of slice) {
      if (!line) {
        if (current.trim().length > 10) { steps.push(current.trim()); current = ''; }
        continue;
      }
      // New numbered step
      if (/^\d+[\.\)]\s+/.test(line)) {
        if (current.trim().length > 10) steps.push(current.trim());
        current = line.replace(/^\d+[\.\)]\s+/, '').trim();
      } else if (/^[-•*]\s+/.test(line) && line.length > 15) {
        if (current.trim().length > 10) steps.push(current.trim());
        current = line.replace(/^[-•*]\s+/, '').trim();
      } else if (line.length > 15 && !/^(note|tip|storage|nutrition|serving suggestion)/i.test(line)) {
        current += (current ? ' ' : '') + line;
      } else if (/^(note|tip|storage|nutrition)/i.test(line)) {
        break;
      }
    }
    if (current.trim().length > 10) steps.push(current.trim());

    return steps.length > 0 ? steps : extractFallbackInstructions(lines);
  }

  function extractFallbackInstructions(lines) {
    return lines.filter(l =>
      /^\d+[\.\)]/.test(l) && l.length > 20
    ).map(l => l.replace(/^\d+[\.\)]\s*/, '').trim());
  }

  function findSectionStart(lines, pattern) {
    return lines.findIndex(l => pattern.test(l.replace(/[:#*]/g, '').trim()));
  }

  function findSectionEnd(lines, start, pattern) {
    if (start === -1) return -1;
    const idx = lines.findIndex((l, i) => i > start && pattern.test(l.replace(/[:#*]/g, '').trim()));
    return idx === -1 ? -1 : idx;
  }

  // ─────────────────────────────────────────────────────────────
  //  MARKDOWN BUILDER
  // ─────────────────────────────────────────────────────────────

  function buildMarkdown(r) {
    const imageSlug = '/assets/images/' + toFilename(r.title).replace('.md', '.jpg');
    let md = '---\n';
    md += `layout: recipe\n`;
    md += `title: "${esc(r.title)}"\n`;
    if (r.description) md += `description: "${esc(truncate(r.description, 130))}"\n`;
    if (r.category)    md += `category: ${r.category}\n`;
    if (r.tags.length) md += `tags: [${r.tags.join(', ')}]\n`;
    if (r.prepTime)    md += `prep_time: ${r.prepTime}\n`;
    if (r.cookTime)    md += `cook_time: ${r.cookTime}\n`;
    if (r.servings)    md += `servings: ${r.servings}\n`;
    if (r.difficulty)  md += `difficulty: ${r.difficulty}\n`;
    md += `image: ${imageSlug}\n`;
    md += '\ningredients:\n';
    for (const i of r.ingredients) md += `  - ${esc(i)}\n`;
    md += '---\n\n';
    for (const s of r.instructions) md += `1. ${esc(s)}\n\n`;
    return md.trimEnd() + '\n';
  }

  // ─────────────────────────────────────────────────────────────
  //  UTILS
  // ─────────────────────────────────────────────────────────────

  function esc(s) { return String(s || '').replace(/"/g, "'"); }

  function truncate(s, n) {
    return s.length <= n ? s : s.slice(0, n).replace(/\s\S*$/, '') + '…';
  }

  function toFilename(title) {
    return String(title)
      .toLowerCase()
      .replace(/[^a-z0-9\s-]/g, '')
      .trim()
      .replace(/\s+/g, '-') + '.md';
  }

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
