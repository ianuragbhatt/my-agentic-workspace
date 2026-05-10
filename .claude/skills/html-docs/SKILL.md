---
name: html-docs
description: Create all documentation, explanations, ADRs, implementation plans, incident reports, and guides as styled HTML files using the Anthropic editorial design system. Trigger whenever the user asks for: documentation, an ADR (Architecture Decision Record), an explanation of code or a concept, an implementation plan, a post-mortem or incident report, a guide, a reference doc, or any written artifact meant to be read rather than executed. Never output these as Markdown.
---

This skill governs how to produce readable, well-structured HTML documentation using an editorial design system. Every output must be a `.html` file — never Markdown.

## When to Use

Trigger on any request that produces a document a human will read:
- "document X", "write docs for X", "create documentation"
- "explain X", "help me understand X", "walk me through X"
- "create an ADR", "architecture decision record"
- "implementation plan", "plan for X"
- "incident report", "post-mortem", "RCA"
- "guide", "reference", "runbook", "overview"

## Output Rules

1. Always write to a `.html` file at a sensible path (e.g. `docs/adr-001-auth.html`, `docs/plan-comment-threads.html`).
2. Never return the document as inline Markdown or a code block in chat. Write the file, then tell the user the path.
3. The file must be self-contained — all CSS inline in `<style>`, no external dependencies.
4. Always include a fixed sidebar TOC for documents with 3+ sections (visible on screens ≥1100px).
5. Use semantic HTML: `<header>`, `<section>`, `<nav>`, `<footer>`, `<table>`, `<code>`.

---

## Base HTML Shell

Every document starts from this shell. Copy it exactly — do not alter the CSS variables.

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title><!-- Document title --></title>
  <style>
    /* ── Design tokens ── */
    :root {
      --ivory:    #FAF9F5;
      --slate:    #141413;
      --clay:     #D97757;
      --oat:      #E3DACC;
      --olive:    #788C5D;
      --rust:     #B04A3F;
      --gray-150: #F0EEE6;
      --gray-300: #D1CFC5;
      --gray-500: #87867F;
      --gray-700: #3D3D3A;
      --white:    #FFFFFF;

      --serif: ui-serif, Georgia, 'Times New Roman', serif;
      --sans:  system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
      --mono:  ui-monospace, 'SF Mono', Menlo, Monaco, monospace;

      --radius-panel: 12px;
      --radius-card:  10px;
      --radius-tag:   6px;
      --border: 1.5px solid var(--gray-300);
    }

    /* ── Reset ── */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    html { scroll-behavior: smooth; }

    /* ── Base ── */
    body {
      font-family: var(--sans);
      background: var(--ivory);
      color: var(--gray-700);
      font-size: 15px;
      line-height: 1.6;
      padding: 56px 24px 120px;
      -webkit-font-smoothing: antialiased;
    }
    .page { max-width: 820px; margin: 0 auto; }

    /* ── Typography ── */
    h1 {
      font-family: var(--serif);
      font-weight: 500;
      font-size: 36px;
      letter-spacing: -0.01em;
      line-height: 1.2;
      color: var(--slate);
      margin-bottom: 18px;
    }
    h2 {
      font-family: var(--serif);
      font-weight: 500;
      font-size: 24px;
      letter-spacing: -0.01em;
      color: var(--slate);
      margin-bottom: 6px;
    }
    h3 {
      font-family: var(--serif);
      font-weight: 500;
      font-size: 19px;
      color: var(--slate);
      margin-bottom: 4px;
    }
    p { margin-bottom: 14px; }
    p:last-child { margin-bottom: 0; }
    code {
      font-family: var(--mono);
      font-size: 13px;
      background: var(--gray-150);
      padding: 1.5px 5px;
      border-radius: 4px;
    }
    a { color: var(--clay); text-decoration: none; }
    a:hover { text-decoration: underline; }

    /* ── Section chrome ── */
    section { margin-bottom: 52px; scroll-margin-top: 24px; }
    .sec-head {
      display: flex;
      align-items: baseline;
      gap: 14px;
      margin-bottom: 8px;
    }
    .sec-num {
      font-family: var(--mono);
      font-size: 12px;
      background: var(--oat);
      color: var(--slate);
      padding: 3px 9px;
      border-radius: 8px;
      flex-shrink: 0;
    }
    hr.rule {
      border: none;
      border-top: 1px solid var(--gray-300);
      margin-bottom: 22px;
    }

    /* ── Fixed TOC (≥1100px) ── */
    .toc { display: none; }
    @media (min-width: 1100px) {
      .toc {
        display: block;
        position: fixed;
        top: 72px;
        right: max(24px, calc(50vw - 410px - 210px));
        width: 180px;
        font-size: 13px;
      }
      .toc-title {
        font-family: var(--mono);
        font-size: 10px;
        text-transform: uppercase;
        letter-spacing: 0.1em;
        color: var(--gray-500);
        margin-bottom: 12px;
      }
      .toc a {
        display: block;
        color: var(--gray-500);
        text-decoration: none;
        padding: 5px 0 5px 12px;
        border-left: 1.5px solid var(--gray-300);
        line-height: 1.3;
        margin-bottom: 2px;
      }
      .toc a:hover { color: var(--slate); border-left-color: var(--clay); }
    }

    /* ── Footer ── */
    footer {
      margin-top: 64px;
      padding-top: 20px;
      border-top: 1px solid var(--gray-300);
      font-family: var(--mono);
      font-size: 12px;
      color: var(--gray-500);
    }
  </style>
</head>
<body>
  <nav class="toc">
    <div class="toc-title">On this page</div>
    <!-- <a href="#section-id">Section name</a> -->
  </nav>

  <div class="page">
    <header>
      <div class="eyebrow"><!-- Category · Context --></div>
      <h1><!-- Title --></h1>
      <!-- meta-row pills here -->
    </header>

    <!-- sections -->

    <footer><!-- Author · Date --></footer>
  </div>
</body>
</html>
```

---

## Component Library

Add only the components the document needs. Each is self-contained CSS + HTML.

### Eyebrow / Category Label
```css
.eyebrow {
  font-family: var(--mono);
  font-size: 12px;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--gray-500);
  margin-bottom: 10px;
}
```
```html
<div class="eyebrow">ADR · Authentication</div>
```

### Pills (status badges)
```css
.meta-row { display: flex; flex-wrap: wrap; gap: 10px 12px; margin-top: 14px; }
.pill {
  display: inline-flex; align-items: baseline; gap: 6px;
  font-family: var(--sans); font-size: 12px; font-weight: 600;
  border-radius: 999px; padding: 5px 12px; line-height: 1;
}
.pill .k { font-weight: 400; opacity: 0.75; }
.pill .v { font-family: var(--mono); }
.pill.accent   { background: var(--clay);  color: var(--white); }
.pill.success  { background: var(--olive); color: var(--white); }
.pill.neutral  { background: var(--gray-150); color: var(--gray-700); border: var(--border); }
.pill.danger   { background: var(--rust);  color: var(--white); }
```
```html
<div class="meta-row">
  <span class="pill accent">Accepted</span>
  <span class="pill neutral"><span class="k">Date</span><span class="v">2025-05-10</span></span>
  <span class="pill neutral"><span class="k">Owner</span><span class="v">Anurag</span></span>
</div>
```

### TL;DR Panel (dark callout)
```css
.tldr {
  background: var(--slate);
  color: var(--ivory);
  border-radius: var(--radius-panel);
  padding: 22px 26px;
  margin-bottom: 48px;
}
.tldr-label {
  font-family: var(--mono);
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--oat);
  margin-bottom: 10px;
}
.tldr p { font-size: 15.5px; line-height: 1.65; }
.tldr code {
  font-family: var(--mono);
  font-size: 13.5px;
  background: rgba(250,249,245,0.12);
  padding: 1px 5px;
  border-radius: 4px;
}
```
```html
<div class="tldr">
  <div class="tldr-label">TL;DR</div>
  <p>One-paragraph summary of the most important thing to understand.</p>
</div>
```

### Stat Cards (summary grid)
```css
.stat-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  gap: 16px;
  margin-bottom: 52px;
}
.stat-card {
  background: var(--white);
  border: var(--border);
  border-radius: var(--radius-panel);
  padding: 18px 20px;
}
.stat-card .k {
  font-family: var(--mono);
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  color: var(--gray-500);
  margin-bottom: 6px;
}
.stat-card .v { font-size: 17px; color: var(--slate); font-weight: 600; }
.stat-card .v.accent { color: var(--clay); }
```
```html
<div class="stat-grid">
  <div class="stat-card"><div class="k">Status</div><div class="v accent">Accepted</div></div>
  <div class="stat-card"><div class="k">Effort</div><div class="v">~2 weeks</div></div>
  <div class="stat-card"><div class="k">Risk</div><div class="v">Medium</div></div>
</div>
```

### Timeline
```css
.timeline { position: relative; padding: 4px 0 4px 16px; }
.timeline::before {
  content: ""; position: absolute;
  left: 16px; top: 8px; bottom: 8px;
  width: 2px; background: var(--gray-300);
}
.tl-entry { position: relative; padding: 0 0 22px 28px; }
.tl-entry:last-child { padding-bottom: 0; }
.tl-dot {
  position: absolute; left: -5px; top: 6px;
  width: 12px; height: 12px; border-radius: 50%;
  background: var(--gray-500);
  border: 2px solid var(--ivory);
  box-sizing: content-box;
}
.tl-dot.accent    { background: var(--clay); }
.tl-dot.success   { background: var(--olive); }
.tl-dot.pending   { background: var(--white); border-color: var(--clay); }
.tl-time {
  display: inline-block;
  font-family: var(--mono); font-size: 12px;
  color: var(--gray-700);
  background: var(--gray-150);
  border: 1px solid var(--gray-300);
  border-radius: 6px; padding: 2px 8px; margin-bottom: 6px;
}
.tl-body { font-size: 14px; color: var(--gray-700); }
.tl-body strong { color: var(--slate); font-weight: 600; }
```
```html
<div class="timeline">
  <div class="tl-entry">
    <span class="tl-dot"></span>
    <span class="tl-time">2025-04-10</span>
    <div class="tl-body">Problem identified — <strong>session tokens stored insecurely</strong>.</div>
  </div>
  <div class="tl-entry">
    <span class="tl-dot success"></span>
    <span class="tl-time">2025-04-12</span>
    <div class="tl-body"><strong>Decision made.</strong> Switch to JWT with short-lived access tokens.</div>
  </div>
</div>
```

### Code Panel (dark, syntax-aware)
```css
.code-panel {
  background: var(--slate);
  color: #E8E6DE;
  border-radius: var(--radius-panel);
  padding: 18px 20px;
  font-family: var(--mono);
  font-size: 13px;
  line-height: 1.7;
  overflow-x: auto;
  margin: 8px 0 16px;
}
.code-panel .path { color: var(--gray-500); font-size: 12px; display: block; margin-bottom: 10px; }
.code-panel pre { white-space: pre; }
/* Syntax colours */
.kw  { color: var(--clay); }
.str { color: var(--olive); }
.cm  { color: var(--gray-500); }
.fn  { color: #C9B98A; }
/* Diff */
.diff-line     { white-space: pre; }
.diff-line.ctx { color: #D1CFC5; }
.diff-line.del { color: #E0897A; }
.diff-line.add { color: #A3B88A; }
```
```html
<div class="code-panel">
  <span class="path">src/auth/tokens.ts</span>
  <pre><span class="kw">export function</span> <span class="fn">createToken</span>(userId: string) {
  <span class="kw">return</span> jwt.<span class="fn">sign</span>({ sub: userId }, SECRET, { expiresIn: <span class="str">'15m'</span> });
}</pre>
</div>
```

### Risk / Decision Table
```css
.risk-table { border: var(--border); border-radius: var(--radius-panel); overflow: hidden; background: var(--white); }
.risk-table .row { display: grid; grid-template-columns: 2fr 80px 2fr; }
.risk-table .row + .row { border-top: var(--border); }
.risk-table .cell { padding: 14px 18px; font-size: 13.5px; }
.risk-table .cell + .cell { border-left: var(--border); }
.risk-table .head { background: var(--gray-150); font-size: 12px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.04em; color: var(--slate); }
.sev { display: inline-block; font-family: var(--mono); font-size: 11px; padding: 2px 8px; border-radius: 6px; font-weight: 600; }
.sev.high { background: #F3D9CC; color: #8A3B1E; }
.sev.med  { background: var(--oat); color: var(--slate); }
.sev.low  { background: #E4E9DC; color: #4B5C39; }
```
```html
<div class="risk-table">
  <div class="row">
    <div class="cell head">Risk</div>
    <div class="cell head">Level</div>
    <div class="cell head">Mitigation</div>
  </div>
  <div class="row">
    <div class="cell">Token leakage via XSS</div>
    <div class="cell"><span class="sev high">HIGH</span></div>
    <div class="cell">Store access token in memory, refresh token in httpOnly cookie.</div>
  </div>
</div>
```

### Open Questions Panel
```css
.open-questions { display: flex; flex-direction: column; gap: 14px; }
.question {
  background: var(--white);
  border: var(--border);
  border-left: 4px solid var(--clay);
  border-radius: var(--radius-card);
  padding: 16px 20px;
}
.question .qt { font-weight: 600; font-size: 15px; color: var(--slate); margin-bottom: 4px; }
.question .qd { font-size: 13.5px; color: var(--gray-500); }
.question .owner { font-family: var(--mono); font-size: 11.5px; color: var(--gray-500); margin-top: 8px; }
```
```html
<div class="open-questions">
  <div class="question">
    <div class="qt">Do we rotate refresh tokens on every use?</div>
    <div class="qd">Single-use rotation is more secure but adds complexity. Leaning yes for v1.</div>
    <div class="owner">Decide with · security team · before implementation</div>
  </div>
</div>
```

### Action Items (checklist)
```css
.actions { background: var(--white); border: var(--border); border-radius: var(--radius-panel); overflow: hidden; }
.ai-row {
  display: grid;
  grid-template-columns: 28px 36px 1fr 90px;
  align-items: center;
  gap: 14px;
  padding: 14px 18px;
  border-bottom: 1px solid var(--gray-150);
}
.ai-row:last-child { border-bottom: none; }
.ai-check { width: 18px; height: 18px; border: 1.5px solid var(--gray-300); border-radius: 5px; background: var(--white); position: relative; flex-shrink: 0; }
.ai-row.done .ai-check { background: var(--olive); border-color: var(--olive); }
.ai-row.done .ai-check::after {
  content: ""; position: absolute; left: 4px; top: 1px;
  width: 5px; height: 9px;
  border: solid var(--white); border-width: 0 2px 2px 0;
  transform: rotate(40deg);
}
.ai-row.done .ai-desc { color: var(--gray-500); text-decoration: line-through; text-decoration-color: var(--gray-300); }
.ai-avatar {
  width: 30px; height: 30px; border-radius: 50%;
  background: var(--oat); color: var(--gray-700);
  font-size: 11px; font-weight: 600;
  display: flex; align-items: center; justify-content: center;
}
.ai-desc { font-size: 14px; color: var(--slate); }
.ai-due { font-family: var(--mono); font-size: 12px; color: var(--gray-500); text-align: right; }
```
```html
<div class="actions">
  <div class="ai-row done">
    <span class="ai-check"></span>
    <span class="ai-avatar">AB</span>
    <span class="ai-desc">Define token shape and expiry policy</span>
    <span class="ai-due">May 08</span>
  </div>
  <div class="ai-row">
    <span class="ai-check"></span>
    <span class="ai-avatar">AB</span>
    <span class="ai-desc">Implement refresh endpoint with rotation</span>
    <span class="ai-due">May 15</span>
  </div>
</div>
```

### Consequence / Context Block (for ADRs)
```css
.context-block {
  background: var(--white);
  border: var(--border);
  border-radius: var(--radius-panel);
  padding: 20px 24px;
  margin-bottom: 16px;
}
.context-block .cb-label {
  font-family: var(--mono);
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--gray-500);
  margin-bottom: 8px;
}
```
```html
<div class="context-block">
  <div class="cb-label">Context</div>
  <p>Session-based auth requires sticky sessions, which complicates horizontal scaling...</p>
</div>
<div class="context-block">
  <div class="cb-label">Decision</div>
  <p>Use JWT access tokens (15-minute TTL) + httpOnly refresh tokens (7-day TTL).</p>
</div>
<div class="context-block">
  <div class="cb-label">Consequences</div>
  <p>Stateless auth enables easy horizontal scaling. Requires client-side token refresh logic.</p>
</div>
```

---

## Document Templates

Use the matching template as the structural skeleton. Populate with real content.

### ADR (Architecture Decision Record)

```
header
  eyebrow: "ADR-### · [Domain]"
  h1: [Decision title]
  meta-row pills: status (Proposed/Accepted/Deprecated/Superseded), date, owner

stat-grid: Status, Date, Deciders, Supersedes (if any)

tldr: one-paragraph decision summary

section#context   → Context block: problem and forces at play
section#decision  → Decision block: what was chosen and why
section#options   → Risk table of alternatives considered (option | trade-offs)
section#consequences → Consequence block: positive + negative outcomes
section#actions   → Action items (follow-up work)

footer: "ADR-### · Created [date] · [author]"
```

### Implementation Plan

```
header
  eyebrow: "Implementation plan · [Project/Feature]"
  h1: [Feature name]
  prompt-box: (optional) the original request or context

stat-grid: Effort, Surfaces/packages, New tables, Feature flag

section#milestones  → Timeline component with milestone entries
section#data-flow   → Code panel or diagram
section#key-code    → Code panels (2-column grid for complex ones)
section#risks       → Risk table
section#open-questions → Open questions panels

footer: "Plan · [date] · [author]"
```

### Incident / Post-mortem Report

```
header
  eyebrow: "INC-[ID]"
  h1: [Short description of the incident]
  meta-row pills: severity (SEV-1/2/3), status (Resolved/Ongoing), duration, detected, owner

tldr: what happened, root cause, resolution, data loss

section#timeline   → Timeline component
section#root-cause → Code panel (diff) + explanation
section#impact     → Stat cards or impact table
section#actions    → Action items checklist
section#lessons    → Open questions style panels but titled "Lessons Learned"

footer: "Authored from [source] · reviewed by [names]"
```

### Code / Concept Explanation

```
header
  eyebrow: "[Domain] · Explanation"
  h1: [What is being explained]

tldr: the core idea in one paragraph

section#overview    → Plain text with inline code
section#how-it-works → Timeline (step by step) or code panels
section#key-code    → Annotated code panel(s)
section#gotchas     → Open questions panels (relabeled "Watch out for")
section#further     → Links or references

footer: "Explanation · [date]"
```

### General Documentation / Guide

```
header
  eyebrow: "[Category]"
  h1: [Title]
  meta-row pills: version, last updated, owner

tldr: (optional) one-paragraph quick answer

section#overview
section#prerequisites  → Stat cards or bullet list
section#steps          → Numbered timeline
section#reference      → Code panels or tables
section#troubleshooting → Open questions panels (relabeled "Common issues")

footer: "[doc type] · [date] · [author]"
```

---

## Design Rules

1. **Hierarchy**: Eyebrow → H1 → section H2 (with `.sec-num` badge) → H3. Never skip levels.
2. **Color usage**: Clay (`#D97757`) for the ONE primary accent per doc (CTAs, active states, key highlights). Olive for success/resolved. Rust for errors. Use gray-500 for secondary text and labels.
3. **Dark panels**: Use the TL;DR dark panel for the single most important summary. Don't make multiple dark panels.
4. **Code**: Always use `.code-panel` for code — dark background, monospace, path label. Inline code uses `<code>` with light gray background.
5. **Whitespace**: Sections need `margin-bottom: 52px`. Don't compress — breathing room is readability.
6. **Muted vs. prominent**: Important values (`stat-card .v`) use `color: var(--slate)`. Labels and metadata use `color: var(--gray-500)`. Body text is `var(--gray-700)`.
7. **TOC**: Include for any doc with 3+ sections. Anchor every `<section>` with `id="slug"`.
8. **No external resources**: No Google Fonts, no CDN links. The file must render offline.
