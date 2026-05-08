<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>InvestRadar · AI Investment Intelligence</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet" />
<style>
/* ═══════════════════════════════════════════
   RESET & TOKENS
═══════════════════════════════════════════ */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
–bg:       #06090f;
–bg2:      #0b1019;
–bg3:      #101722;
–bg4:      #141d2b;
–border:   #1c2a3a;
–border2:  #243344;

–cyan:     #00e5ff;
–cyan-dim: rgba(0,229,255,.15);
–cyan-glow:rgba(0,229,255,.08);
–green:    #00ff9d;
–green-dim:rgba(0,255,157,.12);
–amber:    #ffb300;
–red:      #ff4060;
–purple:   #a78bfa;

–text:     #ddeeff;
–text2:    #6a8faa;
–text3:    #2d4558;

–mono: ‘Space Mono’, monospace;
–sans: ‘Syne’, sans-serif;

–radius: 6px;
–max-w: 560px;
}

html, body {
background: var(–bg);
color: var(–text);
font-family: var(–sans);
min-height: 100vh;
overflow-x: hidden;
}

/* ═══════════════════════════════════════════
BACKGROUND GRID
═══════════════════════════════════════════ */
body::before {
content: ‘’;
position: fixed;
inset: 0;
background-image:
linear-gradient(var(–border) 1px, transparent 1px),
linear-gradient(90deg, var(–border) 1px, transparent 1px);
background-size: 44px 44px;
opacity: .35;
pointer-events: none;
z-index: 0;
}
body::after {
content: ‘’;
position: fixed;
inset: 0;
background:
radial-gradient(ellipse 60% 40% at 15% 10%, rgba(0,229,255,.05) 0%, transparent 60%),
radial-gradient(ellipse 50% 40% at 85% 85%, rgba(167,139,250,.04) 0%, transparent 60%);
pointer-events: none;
z-index: 0;
}

/* ═══════════════════════════════════════════
KEY OVERLAY  (shown first, always)
═══════════════════════════════════════════ */
#overlay {
position: fixed;
inset: 0;
background: rgba(6,9,15,.96);
backdrop-filter: blur(12px);
z-index: 9999;
display: flex;
align-items: center;
justify-content: center;
padding: 24px;
}

#overlay-box {
background: var(–bg2);
border: 1px solid var(–border2);
border-radius: 12px;
padding: 36px 32px;
width: 100%;
max-width: 420px;
position: relative;
overflow: hidden;
}
#overlay-box::before {
content: ‘’;
position: absolute;
top: 0; left: 0; right: 0;
height: 2px;
background: linear-gradient(90deg, transparent, var(–cyan), transparent);
}

.overlay-logo {
font-family: var(–sans);
font-size: 26px;
font-weight: 800;
letter-spacing: -0.5px;
color: var(–cyan);
margin-bottom: 4px;
}
.overlay-logo span { color: var(–text2); font-weight: 400; }

.overlay-sub {
font-family: var(–mono);
font-size: 9px;
letter-spacing: 3px;
color: var(–text3);
margin-bottom: 28px;
text-transform: uppercase;
}

.overlay-label {
font-family: var(–mono);
font-size: 10px;
letter-spacing: 1px;
color: var(–text2);
margin-bottom: 10px;
line-height: 1.6;
}
.overlay-label a {
color: var(–cyan);
text-decoration: none;
}
.overlay-label a:hover { text-decoration: underline; }

.key-wrap {
position: relative;
margin-bottom: 6px;
}
#key-input {
width: 100%;
background: var(–bg3);
border: 1px solid var(–border2);
border-radius: var(–radius);
color: var(–text);
font-family: var(–mono);
font-size: 11px;
padding: 12px 44px 12px 14px;
outline: none;
transition: border-color .2s;
letter-spacing: 0.5px;
}
#key-input::placeholder { color: var(–text3); }
#key-input:focus { border-color: var(–cyan); }

#eye-btn {
position: absolute;
right: 12px;
top: 50%;
transform: translateY(-50%);
background: none;
border: none;
color: var(–text3);
cursor: pointer;
font-size: 15px;
line-height: 1;
padding: 2px;
transition: color .2s;
}
#eye-btn:hover { color: var(–cyan); }

#key-error {
font-family: var(–mono);
font-size: 10px;
color: var(–red);
min-height: 18px;
margin-bottom: 14px;
}

#start-btn {
width: 100%;
background: var(–cyan-dim);
border: 1px solid var(–cyan);
border-radius: var(–radius);
color: var(–cyan);
font-family: var(–sans);
font-size: 13px;
font-weight: 700;
letter-spacing: 3px;
padding: 14px;
cursor: pointer;
text-transform: uppercase;
transition: background .2s, box-shadow .2s;
}
#start-btn:hover {
background: rgba(0,229,255,.22);
box-shadow: 0 0 24px rgba(0,229,255,.12);
}
#start-btn:active { transform: scale(.99); }

.overlay-note {
font-family: var(–mono);
font-size: 9px;
color: var(–text3);
margin-top: 14px;
text-align: center;
line-height: 1.7;
}

/* ═══════════════════════════════════════════
MAIN APP
═══════════════════════════════════════════ */
#app {
display: none;
max-width: var(–max-w);
margin: 0 auto;
padding: 0 0 100px;
position: relative;
z-index: 1;
}

/* ── HEADER ── */
#header {
padding: 22px 20px 16px;
border-bottom: 1px solid var(–border);
position: relative;
}
#header::after {
content: ‘’;
position: absolute;
bottom: 0; left: 0; right: 0;
height: 1px;
background: linear-gradient(90deg, transparent, var(–cyan), transparent);
}

.header-row {
display: flex;
align-items: center;
justify-content: space-between;
margin-bottom: 6px;
}

.logo {
font-family: var(–sans);
font-size: 22px;
font-weight: 800;
letter-spacing: -0.5px;
color: var(–cyan);
}
.logo span { color: var(–text2); font-weight: 400; }

.live-badge {
display: flex;
align-items: center;
gap: 7px;
font-family: var(–mono);
font-size: 9px;
letter-spacing: 2px;
color: var(–green);
}
.live-dot {
width: 7px; height: 7px;
border-radius: 50%;
background: var(–green);
animation: pulse 2s ease-in-out infinite;
}
@keyframes pulse { 0%,100%{opacity:1;transform:scale(1)} 50%{opacity:.3;transform:scale(.8)} }

.header-sub {
font-family: var(–mono);
font-size: 9px;
letter-spacing: 2px;
color: var(–text3);
}

/* ── TICKER ── */
#ticker-wrap {
border-bottom: 1px solid var(–border);
padding: 9px 0;
overflow: hidden;
position: relative;
}
#ticker-wrap::after {
content: ‘’;
position: absolute;
right: 0; top: 0; bottom: 0;
width: 60px;
background: linear-gradient(90deg, transparent, var(–bg));
pointer-events: none;
z-index: 2;
}
#ticker-inner {
display: flex;
gap: 0;
width: max-content;
animation: ticker 35s linear infinite;
}
@keyframes ticker { from{transform:translateX(0)} to{transform:translateX(-50%)} }

.tick-item {
display: flex;
align-items: center;
gap: 6px;
padding: 0 18px;
border-right: 1px solid var(–border);
}
.tick-key {
font-family: var(–mono);
font-size: 8px;
letter-spacing: 1px;
color: var(–text3);
}
.tick-val {
font-family: var(–mono);
font-size: 9px;
font-weight: 700;
}

/* ── MARKET SNAPSHOT ── */
#market-panel {
margin: 16px 20px 0;
background: var(–bg2);
border: 1px solid var(–border);
border-radius: var(–radius);
overflow: hidden;
}
.panel-head {
display: flex;
align-items: center;
justify-content: space-between;
padding: 10px 14px;
border-bottom: 1px solid var(–border);
}
.panel-title {
font-family: var(–mono);
font-size: 8px;
letter-spacing: 3px;
color: var(–cyan);
}
.panel-badge {
font-family: var(–mono);
font-size: 8px;
letter-spacing: 1px;
color: var(–green);
background: var(–green-dim);
border: 1px solid rgba(0,255,157,.2);
border-radius: 3px;
padding: 2px 8px;
}
.market-grid {
display: grid;
grid-template-columns: repeat(2, 1fr);
gap: 1px;
background: var(–border);
}
.market-cell {
background: var(–bg3);
padding: 9px 12px;
}
.market-cell-key {
font-family: var(–mono);
font-size: 8px;
letter-spacing: 1px;
color: var(–text3);
margin-bottom: 3px;
}
.market-cell-val {
font-family: var(–mono);
font-size: 13px;
font-weight: 700;
}
.sentiment-row {
padding: 10px 14px;
border-top: 1px solid var(–border);
}
.sentiment-label {
font-family: var(–mono);
font-size: 8px;
letter-spacing: 2px;
color: var(–text3);
margin-bottom: 3px;
}
.sentiment-val {
font-family: var(–mono);
font-size: 11px;
font-weight: 700;
color: var(–amber);
}
.market-summary {
padding: 10px 14px 14px;
font-size: 12px;
color: var(–text2);
line-height: 1.75;
border-top: 1px solid var(–border);
}

/* ── CONTROLS ── */
.section-label {
font-family: var(–mono);
font-size: 8px;
letter-spacing: 3px;
color: var(–text3);
padding: 18px 20px 10px;
text-transform: uppercase;
}

#strat-grid {
display: grid;
grid-template-columns: 1fr 1fr;
gap: 8px;
padding: 0 20px;
}

.strat-btn {
background: var(–bg2);
border: 1px solid var(–border);
border-radius: var(–radius);
padding: 11px 13px;
cursor: pointer;
text-align: left;
position: relative;
overflow: hidden;
transition: border-color .18s, background .18s;
}
.strat-btn:hover {
border-color: var(–border2);
background: var(–bg3);
}
.strat-btn.active {
border-color: var(–cyan);
background: var(–cyan-glow);
}
.strat-accent {
position: absolute;
top: 0; left: 0;
width: 3px; height: 100%;
background: var(–cyan);
display: none;
}
.strat-btn.active .strat-accent { display: block; }
.strat-name {
font-family: var(–mono);
font-size: 10px;
font-weight: 700;
letter-spacing: 1px;
color: var(–text2);
margin-bottom: 4px;
transition: color .18s;
}
.strat-btn.active .strat-name { color: var(–cyan); }
.strat-desc { font-size: 11px; color: var(–text3); line-height: 1.4; }

.sliders { padding: 14px 20px 0; }
.slider-row {
display: flex;
align-items: center;
gap: 12px;
margin-bottom: 11px;
}
.slider-key {
font-family: var(–mono);
font-size: 8px;
letter-spacing: 2px;
color: var(–text3);
width: 40px;
flex-shrink: 0;
}
input[type=range] {
flex: 1;
-webkit-appearance: none;
appearance: none;
height: 2px;
border-radius: 2px;
outline: none;
cursor: pointer;
}
input[type=range]::-webkit-slider-thumb {
-webkit-appearance: none;
width: 14px; height: 14px;
border-radius: 50%;
background: var(–bg);
border: 2px solid var(–cyan);
cursor: pointer;
}
input[type=range]::-moz-range-thumb {
width: 14px; height: 14px;
border-radius: 50%;
background: var(–bg);
border: 2px solid var(–cyan);
cursor: pointer;
}
.slider-val {
font-family: var(–mono);
font-size: 9px;
color: var(–cyan);
width: 78px;
text-align: right;
flex-shrink: 0;
}

/* ── ANALYZE BUTTON ── */
#analyze-wrap { padding: 14px 20px 0; }
#analyze-btn {
width: 100%;
background: transparent;
border: 1px solid var(–cyan);
border-radius: var(–radius);
color: var(–cyan);
font-family: var(–sans);
font-size: 12px;
font-weight: 700;
letter-spacing: 4px;
padding: 15px;
cursor: pointer;
text-transform: uppercase;
transition: background .2s, box-shadow .2s;
position: relative;
overflow: hidden;
}
#analyze-btn:hover {
background: var(–cyan-dim);
box-shadow: 0 0 28px rgba(0,229,255,.1);
}
#analyze-btn:disabled {
opacity: .5;
cursor: not-allowed;
}

/* ── LOADING ── */
#loading {
display: none;
padding: 32px 20px;
text-align: center;
}
#loading.show { display: block; }
.loading-icon {
font-family: var(–mono);
font-size: 10px;
letter-spacing: 3px;
color: var(–cyan);
margin-bottom: 10px;
}
.loading-txt {
font-family: var(–mono);
font-size: 9px;
letter-spacing: 2px;
color: var(–text3);
}
@keyframes blink { 0%,100%{opacity:1} 50%{opacity:.2} }

/* ── RESULTS ── */
#results { padding: 16px 20px 0; }

.results-bar {
display: flex;
align-items: center;
justify-content: space-between;
margin-bottom: 16px;
padding-bottom: 12px;
border-bottom: 1px solid var(–border);
}
.results-title {
font-family: var(–mono);
font-size: 8px;
letter-spacing: 3px;
color: var(–text3);
}
.pills { display: flex; gap: 5px; align-items: center; flex-wrap: wrap; }
.pill {
font-family: var(–mono);
font-size: 8px;
font-weight: 700;
letter-spacing: 1px;
padding: 3px 8px;
border-radius: 3px;
}
.sig-count {
font-family: var(–mono);
font-size: 10px;
color: var(–green);
margin-left: 4px;
}

/* ── GROUP HEADER ── */
.group-head {
display: flex;
align-items: center;
gap: 10px;
margin: 22px 0 10px;
}
.group-icon {
width: 30px; height: 30px;
border-radius: 5px;
display: flex;
align-items: center;
justify-content: center;
font-size: 14px;
flex-shrink: 0;
}
.group-name {
font-family: var(–mono);
font-size: 10px;
font-weight: 700;
letter-spacing: 3px;
}
.group-sub {
font-family: var(–mono);
font-size: 8px;
color: var(–text3);
letter-spacing: 1px;
margin-top: 2px;
}
.group-count {
margin-left: auto;
font-family: var(–mono);
font-size: 8px;
padding: 3px 8px;
border-radius: 3px;
}
.group-divider {
height: 1px;
margin-bottom: 10px;
}

/* ── INVESTMENT CARD ── */
.inv-card {
background: var(–bg2);
border: 1px solid var(–border);
border-radius: var(–radius);
margin-bottom: 10px;
overflow: hidden;
transition: border-color .2s;
}
.inv-card:hover { border-color: var(–border2); }

.inv-card-top {
padding: 14px;
display: grid;
grid-template-columns: auto 1fr auto;
gap: 0 12px;
align-items: start;
}

.type-badge {
font-family: var(–mono);
font-size: 7px;
letter-spacing: 2px;
padding: 3px 7px;
border-radius: 3px;
margin-top: 2px;
white-space: nowrap;
}
.ticker-name {
font-family: var(–sans);
font-size: 18px;
font-weight: 800;
color: var(–text);
letter-spacing: -0.5px;
line-height: 1;
margin-bottom: 3px;
}
.full-name {
font-size: 11px;
color: var(–text2);
white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis;
margin-bottom: 4px;
}
.strat-tag {
font-family: var(–mono);
font-size: 8px;
color: var(–text3);
letter-spacing: 1px;
}
.conv-badge {
font-family: var(–mono);
font-size: 10px;
font-weight: 700;
letter-spacing: 1px;
margin-bottom: 5px;
text-align: right;
}
.hold-tag {
font-family: var(–mono);
font-size: 8px;
color: var(–text3);
white-space: nowrap;
text-align: right;
}

.inv-metrics {
display: grid;
grid-template-columns: repeat(3,1fr);
border-top: 1px solid var(–border);
}
.inv-metric {
padding: 8px 10px;
text-align: center;
border-right: 1px solid var(–border);
}
.inv-metric:last-child { border-right: none; }
.metric-label {
font-family: var(–mono);
font-size: 7px;
letter-spacing: 1px;
color: var(–text3);
margin-bottom: 3px;
}
.metric-val {
font-family: var(–mono);
font-size: 12px;
font-weight: 700;
}

.fit-row {
display: flex;
align-items: center;
gap: 10px;
padding: 8px 14px;
border-top: 1px solid var(–border);
}
.fit-label {
font-family: var(–mono);
font-size: 7px;
letter-spacing: 1px;
color: var(–text3);
width: 56px;
flex-shrink: 0;
}
.fit-track {
flex: 1;
height: 3px;
background: var(–border);
border-radius: 2px;
overflow: hidden;
}
.fit-fill { height: 100%; border-radius: 2px; }
.fit-pct {
font-family: var(–mono);
font-size: 9px;
font-weight: 700;
width: 30px;
text-align: right;
flex-shrink: 0;
}

.expand-btn {
width: 100%;
background: transparent;
border: none;
border-top: 1px solid var(–border);
padding: 9px 14px;
display: flex;
align-items: center;
justify-content: space-between;
cursor: pointer;
color: var(–cyan);
transition: background .15s;
}
.expand-btn:hover { background: var(–cyan-glow); }
.expand-label {
font-family: var(–mono);
font-size: 8px;
letter-spacing: 2px;
}
.expand-arrow {
font-size: 9px;
transition: transform .2s;
}
.expand-arrow.open { transform: rotate(180deg); }

.reasoning-panel {
display: none;
border-top: 1px solid var(–border);
background: var(–bg3);
padding: 16px 16px 18px;
animation: fadeSlide .2s ease;
}
.reasoning-panel.open { display: block; }
@keyframes fadeSlide { from{opacity:0;transform:translateY(-4px)} to{opacity:1;transform:translateY(0)} }

.why-text {
font-size: 12px;
color: var(–text2);
line-height: 1.8;
margin-bottom: 14px;
}
.cats-label {
font-family: var(–mono);
font-size: 7px;
letter-spacing: 2px;
color: var(–text3);
margin-bottom: 8px;
}
.cats-wrap { display: flex; flex-wrap: wrap; gap: 5px; margin-bottom: 12px; }
.cat-pill {
font-family: var(–mono);
font-size: 8px;
padding: 3px 9px;
border-radius: 3px;
background: var(–green-dim);
border: 1px solid rgba(0,255,157,.15);
color: var(–green);
line-height: 1.5;
}
.risks-row {
padding-top: 10px;
border-top: 1px solid var(–border);
display: flex;
gap: 8px;
align-items: flex-start;
}
.risks-label {
font-family: var(–mono);
font-size: 7px;
letter-spacing: 2px;
color: var(–text3);
flex-shrink: 0;
padding-top: 2px;
}
.risks-text { font-size: 11px; color: #ff7a8a; line-height: 1.6; }

/* ── HIGHLIGHT ── */
.hi { color: var(–cyan); font-weight: 700; }

/* ── ERROR ── */
.error-box {
margin: 16px 20px;
padding: 16px;
background: rgba(255,64,96,.06);
border: 1px solid rgba(255,64,96,.3);
border-radius: var(–radius);
font-family: var(–mono);
font-size: 10px;
color: var(–red);
line-height: 1.7;
}

/* ── CHANGE KEY BUTTON ── */
#change-key-btn {
position: fixed;
bottom: 18px;
right: 18px;
background: var(–bg2);
border: 1px solid var(–border);
border-radius: var(–radius);
color: var(–text3);
font-family: var(–mono);
font-size: 8px;
letter-spacing: 1px;
padding: 8px 12px;
cursor: pointer;
z-index: 100;
transition: border-color .2s, color .2s;
}
#change-key-btn:hover { border-color: var(–cyan); color: var(–cyan); }

/* ── DISCLAIMER ── */
.disclaimer {
margin: 20px 20px 0;
padding: 10px 14px;
border: 1px solid var(–border);
border-radius: var(–radius);
font-family: var(–mono);
font-size: 8px;
color: var(–text3);
line-height: 1.7;
}
</style>

</head>
<body>

<!-- ═══════════════════════════════════════════
     KEY OVERLAY
═══════════════════════════════════════════ -->

<div id="overlay">
  <div id="overlay-box">
    <div class="overlay-logo">Invest<span>Radar</span></div>
    <div class="overlay-sub">// AI Investment Intelligence</div>

```
<div class="overlay-label">
  Enter your <strong style="color:var(--cyan)">Anthropic API Key</strong> to start.<br>
  Stored locally in your browser only — never transmitted elsewhere.<br><br>
  Get a key at <a href="https://console.anthropic.com" target="_blank" rel="noopener">console.anthropic.com</a> → API Keys → Create Key.<br>
  You'll need a small credit balance (€5 covers ~200+ analyses).
</div>

<div class="key-wrap">
  <input
    id="key-input"
    type="password"
    placeholder="sk-ant-api03-..."
    autocomplete="off"
    spellcheck="false"
  />
  <button id="eye-btn" type="button" onclick="toggleEye()" title="Show/hide key">◉</button>
</div>
<div id="key-error"></div>

<button id="start-btn" type="button" onclick="submitKey()">▶ START</button>

<div class="overlay-note">
  Your key is saved in localStorage so you don't have to re-enter it.<br>
  Use the ⚙ button to change it at any time.
</div>
```

  </div>
</div>

<!-- ═══════════════════════════════════════════
     MAIN APP
═══════════════════════════════════════════ -->

<div id="app">

  <!-- Header -->

  <div id="header">
    <div class="header-row">
      <div class="logo">Invest<span>Radar</span></div>
      <div class="live-badge">
        <div class="live-dot"></div>
        LIVE · MAY 2026
      </div>
    </div>
    <div class="header-sub">// Claude-powered · AI investment analysis · BYOK</div>
  </div>

  <!-- Ticker -->

  <div id="ticker-wrap">
    <div id="ticker-inner"></div>
  </div>

  <!-- Market Snapshot -->

  <div id="market-panel">
    <div class="panel-head">
      <div class="panel-title">// MARKET SNAPSHOT · MAY 5 2026</div>
      <div class="panel-badge">LIVE DATA</div>
    </div>
    <div class="market-grid" id="market-grid"></div>
    <div class="sentiment-row">
      <div class="sentiment-label">MARKET SENTIMENT</div>
      <div class="sentiment-val">RISK-OFF / TARIFF UNCERTAINTY</div>
    </div>
    <div class="market-summary">
      S&P 500 down <span class="hi">~17%</span> from February peak on US tariff shock (<span class="hi">145%</span> on China, 90-day pause on others). Fed holds at <span class="hi">4.25–4.50%</span> — two cuts priced for late 2026. CPI cooled to <span class="hi">2.4%</span> YoY. Gold at <span class="hi">$3,271</span> (near all-time high) on safe-haven demand. WTI oil collapsed to <span class="hi">$56.42</span> on demand fears. BTC at <span class="hi">$94,200</span> outperforms as macro hedge. USD weakened to <span class="hi">99.4</span>.
    </div>
  </div>

  <!-- Strategy -->

  <div class="section-label">// SELECT STRATEGY</div>
  <div id="strat-grid"></div>

  <!-- Sliders -->

  <div class="sliders">
    <div class="slider-row">
      <span class="slider-key">RISK</span>
      <input type="range" id="risk-slider" min="1" max="5" value="3"
        style="background:linear-gradient(90deg,var(--green),var(--amber),var(--red))" />
      <span class="slider-val" id="risk-label">BALANCED</span>
    </div>
    <div class="slider-row">
      <span class="slider-key">HOLD</span>
      <input type="range" id="hold-slider" min="1" max="5" value="3"
        style="background:linear-gradient(90deg,var(--cyan),var(--purple))" />
      <span class="slider-val" id="hold-label">1–3 YRS</span>
    </div>
  </div>

  <!-- Analyze button -->

  <div id="analyze-wrap">
    <button id="analyze-btn" type="button" onclick="runAnalysis()">▶ Analyse starten</button>
  </div>

  <!-- Loading -->

  <div id="loading">
    <div class="loading-icon" id="loading-icon">■ ANALYSING MARKETS</div>
    <div class="loading-txt" id="loading-txt">Claude is thinking...</div>
  </div>

  <!-- Results -->

  <div id="results"></div>

  <!-- Disclaimer -->

  <div class="disclaimer">
    ⚠ AI-generated analysis for educational purposes only — not financial advice.
    Always consult a licensed financial advisor before investing. Past performance ≠ future results.
  </div>

</div>

<!-- Change key button (visible once app is open) -->

<button id="change-key-btn" type="button" onclick="openOverlay()">⚙ API KEY</button>

<!-- ═══════════════════════════════════════════
     JAVASCRIPT
═══════════════════════════════════════════ -->

<script>
'use strict';

/* ─── CONSTANTS ─────────────────────────────── */
var RISK_LABELS = ['VERY LOW', 'LOW', 'BALANCED', 'HIGH', 'VERY HIGH'];
var HOLD_LABELS = ['< 3 MON', '3–12 MON', '1–3 YRS', '3–7 YRS', '7+ YRS'];

var STRATEGIES = [
  { k: 'value',    l: 'VALUE',     d: 'Undervalued, margin of safety' },
  { k: 'growth',   l: 'GROWTH',   d: 'High-growth, future earnings'  },
  { k: 'dividend', l: 'DIVIDEND', d: 'Income-generating, stable yield'},
  { k: 'momentum', l: 'MOMENTUM', d: 'Trend-following, breakouts'    },
  { k: 'index',    l: 'INDEX/ETF',d: 'Passive, diversified, low-cost'},
  { k: 'balanced', l: 'BALANCED', d: 'All-weather, mixed strategy'   },
];

var TICKERS = [
  { k: 'S&P 500',   v: '5,631',      t: 'dn'  },
  { k: 'NASDAQ',    v: '17,876',     t: 'dn'  },
  { k: 'Fed Rate',  v: '4.25–4.50%', t: 'neu' },
  { k: '10Y Yield', v: '4.37%',      t: 'up'  },
  { k: 'CPI YoY',   v: '2.4%',       t: 'dn'  },
  { k: 'VIX',       v: '22.3',       t: 'up'  },
  { k: 'Gold',      v: '$3,271',     t: 'up'  },
  { k: 'WTI Oil',   v: '$56.42',     t: 'dn'  },
  { k: 'BTC',       v: '$94,200',    t: 'up'  },
  { k: 'USD Index', v: '99.4',       t: 'dn'  },
];

var MARKET_CONTEXT = [
  'CURRENT MARKET DATA (May 5, 2026):',
  '- S&P 500: 5,631 (down ~17% from February all-time high)',
  '- NASDAQ: 17,876 (down ~18% from peak)',
  '- Fed Funds Rate: 4.25-4.50% (on hold, two cuts priced for late 2026)',
  '- 10Y Treasury Yield: 4.37%',
  '- CPI YoY: 2.4% (cooling toward 2% target)',
  '- VIX: 22.3 (elevated fear, risk-off environment)',
  '- Gold: $3,271/oz (near all-time high, safe-haven demand)',
  '- WTI Crude Oil: $56.42/bbl (collapsed on tariff demand fears + OPEC+ supply)',
  '- Bitcoin: $94,200 (outperforming, emerging as macro hedge)',
  '- USD Index (DXY): 99.4 (weakening on tariff retaliation concerns)',
  '',
  'MACRO CONTEXT:',
  'US tariff shock drove S&P 500 down 17% from February highs.',
  'Tariff structure: 145% on China, 90-day pause on most other countries.',
  'Fed is on hold - rates at 4.25-4.50%, markets expect first cut in late 2026.',
  'CPI cooling to 2.4% but tariff cost-push inflation risk remains.',
  'Gold at near-ATH, BTC resilient, oil collapsed, USD weakening.',
  'Earnings season ongoing: financials beat, consumer discretionary misses.',
  'Sentiment: RISK-OFF. Investors rotating to defensives, gold, bonds.',
].join('\n');

/* ─── STATE ─────────────────────────────────── */
var apiKey = '';
var activeStrat = 'value';

/* ─── INIT ──────────────────────────────────── */
document.addEventListener('DOMContentLoaded', function () {
  buildTicker();
  buildMarketGrid();
  buildStratGrid();

  // Slider listeners
  document.getElementById('risk-slider').addEventListener('input', function () {
    document.getElementById('risk-label').textContent = RISK_LABELS[this.value - 1];
  });
  document.getElementById('hold-slider').addEventListener('input', function () {
    document.getElementById('hold-label').textContent = HOLD_LABELS[this.value - 1];
  });

  // Enter key in input
  document.getElementById('key-input').addEventListener('keydown', function (e) {
    if (e.key === 'Enter') submitKey();
  });

  // Load saved key and pre-fill
  var saved = '';
  try { saved = localStorage.getItem('ir_api_key') || ''; } catch (e) {}
  if (saved) {
    document.getElementById('key-input').value = saved;
  }
});

/* ─── OVERLAY ───────────────────────────────── */
function openOverlay () {
  document.getElementById('overlay').style.display = 'flex';
  document.getElementById('app').style.display = 'none';
  document.getElementById('key-error').textContent = '';
}

function closeOverlay () {
  document.getElementById('overlay').style.display = 'none';
  document.getElementById('app').style.display = 'block';
}

function toggleEye () {
  var inp = document.getElementById('key-input');
  inp.type = inp.type === 'password' ? 'text' : 'password';
}

function submitKey () {
  var val = (document.getElementById('key-input').value || '').trim();
  var errEl = document.getElementById('key-error');

  if (!val) {
    errEl.textContent = '⚠ Please enter your API key.';
    return;
  }
  if (val.indexOf('sk-ant-') !== 0) {
    errEl.textContent = '⚠ Invalid key — must start with sk-ant-';
    return;
  }

  errEl.textContent = '';
  apiKey = val;
  try { localStorage.setItem('ir_api_key', val); } catch (e) {}

  closeOverlay();
}

/* ─── BUILDERS ──────────────────────────────── */
function buildTicker () {
  var doubled = TICKERS.concat(TICKERS);
  var html = doubled.map(function (t) {
    var col = t.t === 'up' ? '#00ff9d' : t.t === 'dn' ? '#ff4060' : '#6a8faa';
    return '<div class="tick-item">'
      + '<span class="tick-key">' + t.k + '</span>'
      + '<span class="tick-val" style="color:' + col + '">' + t.v + '</span>'
      + '</div>';
  }).join('');
  document.getElementById('ticker-inner').innerHTML = html;
}

function buildMarketGrid () {
  var html = TICKERS.map(function (t) {
    var isRisk = t.k === 'VIX' || t.k === 'CPI YoY';
    var col;
    if (isRisk) {
      col = t.t === 'up' ? '#ffb300' : t.t === 'dn' ? '#00ff9d' : '#ddeeff';
    } else {
      col = t.t === 'up' ? '#00ff9d' : t.t === 'dn' ? '#ff4060' : '#ddeeff';
    }
    return '<div class="market-cell">'
      + '<div class="market-cell-key">' + t.k + '</div>'
      + '<div class="market-cell-val" style="color:' + col + '">' + t.v + '</div>'
      + '</div>';
  }).join('');
  document.getElementById('market-grid').innerHTML = html;
}

function buildStratGrid () {
  var html = STRATEGIES.map(function (s) {
    var on = s.k === activeStrat;
    return '<button class="strat-btn' + (on ? ' active' : '') + '" type="button" onclick="selectStrat(\'' + s.k + '\')">'
      + '<div class="strat-accent"></div>'
      + '<div class="strat-name">' + s.l + '</div>'
      + '<div class="strat-desc">' + s.d + '</div>'
      + '</button>';
  }).join('');
  document.getElementById('strat-grid').innerHTML = html;
}

function selectStrat (k) {
  activeStrat = k;
  buildStratGrid();
}

/* ─── ANALYSIS ──────────────────────────────── */
function runAnalysis () {
  // Validate key in memory
  if (!apiKey || apiKey.indexOf('sk-ant-') !== 0) {
    openOverlay();
    return;
  }

  var risk = parseInt(document.getElementById('risk-slider').value, 10);
  var hold = parseInt(document.getElementById('hold-slider').value, 10);
  var riskLabel = RISK_LABELS[risk - 1];
  var holdLabel = HOLD_LABELS[hold - 1];

  // UI: loading state
  document.getElementById('results').innerHTML = '';
  document.getElementById('loading').classList.add('show');
  document.getElementById('analyze-btn').disabled = true;

  var dots = 0;
  var dotTimer = setInterval(function () {
    dots = (dots + 1) % 4;
    document.getElementById('loading-txt').textContent
      = 'Claude is analysing' + '.'.repeat(dots + 1);
  }, 450);

  var prompt = 'You are a professional investment analyst. Generate 10 investment recommendations based on the market data below.\n\n'
    + MARKET_CONTEXT + '\n\n'
    + 'USER PARAMETERS:\n'
    + '- Investment Strategy: ' + activeStrat.toUpperCase() + '\n'
    + '- Risk Tolerance: ' + riskLabel + ' (' + risk + ' out of 5)\n'
    + '- Investment Hold Period: ' + holdLabel + '\n\n'
    + 'TASK: Generate exactly 10 picks — 5 LARGE-CAP and 5 SMALL-CAP — that best fit a '
    + activeStrat.toUpperCase() + ' strategy with ' + riskLabel + ' risk and ' + holdLabel + ' hold period.\n'
    + 'Reference specific numbers from the market data in your analysis.\n\n'
    + 'IMPORTANT: Respond with ONLY a single valid JSON object. No markdown. No code fences. No text before or after. Just the raw JSON.\n\n'
    + 'JSON SCHEMA:\n'
    + '{\n'
    + '  "groups": [\n'
    + '    {\n'
    + '      "label": "LARGE-CAP",\n'
    + '      "sublabel": "Established leaders & major funds",\n'
    + '      "items": [\n'
    + '        {\n'
    + '          "ticker": "string",\n'
    + '          "name": "string",\n'
    + '          "type": "STOCK | ETF | BOND | REIT",\n'
    + '          "strategy": "short label max 6 words",\n'
    + '          "conviction": "HIGH | MEDIUM | LOW",\n'
    + '          "hold": "e.g. 2-4 YRS",\n'
    + '          "upside": "+XX%",\n'
    + '          "risk": "LOW | MED | HIGH",\n'
    + '          "div": "X.X% | N/A",\n'
    + '          "fit": 85,\n'
    + '          "why": "3-4 sentence analysis citing specific numbers from the market data. Explain why this fits the chosen strategy and parameters.",\n'
    + '          "cats": ["Catalyst 1", "Catalyst 2", "Catalyst 3"],\n'
    + '          "risks": "Key risks in one clear sentence."\n'
    + '        }\n'
    + '      ]\n'
    + '    },\n'
    + '    {\n'
    + '      "label": "SMALL-CAP",\n'
    + '      "sublabel": "High-potential opportunities",\n'
    + '      "items": [ ...same structure, 5 items... ]\n'
    + '    }\n'
    + '  ]\n'
    + '}';

  fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': apiKey,
      'anthropic-version': '2023-06-01',
      'anthropic-dangerous-direct-browser-access': 'true'
    },
    body: JSON.stringify({
      model: 'claude-sonnet-4-20250514',
      max_tokens: 4096,
      messages: [{ role: 'user', content: prompt }]
    })
  })
  .then(function (res) {
    if (!res.ok) {
      return res.json().then(function (errBody) {
        var msg = (errBody && errBody.error && errBody.error.message)
          ? errBody.error.message
          : 'HTTP ' + res.status;
        throw new Error(msg + ' (status ' + res.status + ')');
      });
    }
    return res.json();
  })
  .then(function (data) {
    // Extract text from response
    var text = '';
    if (data && data.content && Array.isArray(data.content)) {
      data.content.forEach(function (block) {
        if (block.type === 'text') text += block.text;
      });
    }

    // Strip markdown fences if Claude wrapped it
    text = text.trim();
    if (text.indexOf('```') !== -1) {
      text = text.replace(/```[\w]*\n?/g, '').replace(/```/g, '').trim();
    }

    // Parse JSON
    var parsed = JSON.parse(text);
    if (!parsed || !Array.isArray(parsed.groups)) {
      throw new Error('Unexpected response format from Claude.');
    }

    renderResults(parsed.groups, riskLabel, holdLabel);
  })
  .catch(function (err) {
    var msg = err && err.message ? err.message : String(err);
    // Check for auth errors
    if (msg.indexOf('401') !== -1 || msg.toLowerCase().indexOf('authentication') !== -1) {
      showError('Invalid API key (401). Please check your key and try again.',
        true /* showChangeKey */);
    } else if (msg.indexOf('403') !== -1) {
      showError('Access denied (403). Your key may not have the required permissions.');
    } else if (msg.indexOf('429') !== -1) {
      showError('Rate limit reached (429). Please wait a moment and try again.');
    } else if (msg.indexOf('529') !== -1 || msg.toLowerCase().indexOf('overloaded') !== -1) {
      showError('Anthropic API is temporarily overloaded. Please try again in a few seconds.');
    } else if (msg.toLowerCase().indexOf('failed to fetch') !== -1
            || msg.toLowerCase().indexOf('networkerror') !== -1
            || msg.toLowerCase().indexOf('load failed') !== -1) {
      showError('Network error: Could not reach the Anthropic API. This is usually a CORS issue — make sure you are accessing this page via HTTPS (e.g. GitHub Pages), not a local file:// URL.');
    } else {
      showError('Error: ' + msg);
    }
  })
  .finally(function () {
    clearInterval(dotTimer);
    document.getElementById('loading').classList.remove('show');
    document.getElementById('analyze-btn').disabled = false;
  });
}

/* ─── RENDER ────────────────────────────────── */
function highlight (text) {
  if (!text) return '';
  // Numbers with $ or % or x
  text = text.replace(/(\$[\d,]+(?:\.\d+)?[BMK]?|\d+(?:\.\d+)?x|\d+(?:\.\d+)?%)/g,
    '<span class="hi">$1</span>');
  // Key abbreviations
  text = text.replace(/\b(P\/E|FCF|CAGR|EBITDA|YoY|QoQ|ATH|VIX|CPI|NIM|PEG|ROE|AUM|WTI|LNG|ETF|REIT|GAAP|AI|NTM|TTM|EV|NAV)\b/g,
    '<span class="hi">$1</span>');
  return text;
}

function mkPill (text, color) {
  return '<span class="pill" style="background:' + color + '18;border:1px solid ' + color + '33;color:' + color + '">' + text + '</span>';
}

function showError (msg, showChangeBtn) {
  var extra = showChangeBtn
    ? ' <button onclick="openOverlay()" style="font-family:var(--mono);font-size:9px;color:var(--cyan);background:none;border:none;cursor:pointer;text-decoration:underline;padding:0;margin-left:4px">Change API Key</button>'
    : '';
  document.getElementById('results').innerHTML =
    '<div class="error-box">⚠ ' + msg + extra + '</div>';
}

function renderResults (groups, riskLabel, holdLabel) {
  var total = 0;
  groups.forEach(function (g) { total += (g.items || []).length; });

  var html = '<div class="results-bar">'
    + '<div class="results-title">// RECOMMENDATIONS</div>'
    + '<div class="pills">'
    + mkPill(activeStrat.toUpperCase(), '#00e5ff')
    + mkPill(riskLabel, '#ffb300')
    + mkPill(holdLabel, '#a78bfa')
    + '<span class="sig-count">' + total + ' PICKS</span>'
    + '</div>'
    + '</div>';

  groups.forEach(function (g) {
    if (!g.items || !g.items.length) return;

    var isSmall = (g.label || '').indexOf('SMALL') !== -1;
    var col    = isSmall ? '#00ff9d' : '#00e5ff';
    var dimBg  = isSmall ? 'rgba(0,255,157,.08)' : 'rgba(0,229,255,.06)';
    var dimBdr = isSmall ? 'rgba(0,255,157,.18)' : 'rgba(0,229,255,.18)';
    var icon   = isSmall ? '◇' : '◈';

    html += '<div class="group-head">'
      + '<div class="group-icon" style="background:' + dimBg + ';border:1px solid ' + dimBdr + ';color:' + col + '">' + icon + '</div>'
      + '<div>'
      + '<div class="group-name" style="color:' + col + '">' + (g.label || '') + '</div>'
      + '<div class="group-sub">' + (g.sublabel || '') + '</div>'
      + '</div>'
      + '<div class="group-count" style="background:' + dimBg + ';border:1px solid ' + dimBdr + ';color:' + col + '">'
      + (g.items.length) + ' picks'
      + '</div>'
      + '</div>'
      + '<div class="group-divider" style="background:linear-gradient(90deg,' + dimBdr + ',transparent)"></div>';

    g.items.forEach(function (r, idx) {
      var uid = 'card-' + Date.now() + '-' + idx;
      var tk  = (r.type || 'STOCK').toLowerCase();

      var typeMap = {
        stock: { c: '#00e5ff', b: 'rgba(0,229,255,.15)', bg: 'rgba(0,229,255,.05)' },
        etf:   { c: '#00ff9d', b: 'rgba(0,255,157,.15)', bg: 'rgba(0,255,157,.05)' },
        bond:  { c: '#ffb300', b: 'rgba(255,179,0,.15)', bg: 'rgba(255,179,0,.05)' },
        reit:  { c: '#fb923c', b: 'rgba(251,146,60,.15)', bg: 'rgba(251,146,60,.05)' },
      };
      var ts = typeMap[tk] || typeMap.stock;

      var fit    = Math.max(0, Math.min(100, parseInt(r.fit, 10) || 50));
      var fitCol = fit >= 75 ? '#00ff9d' : fit >= 50 ? '#ffb300' : '#ff4060';
      var convCol = r.conviction === 'HIGH' ? '#00ff9d' : r.conviction === 'MEDIUM' ? '#ffb300' : '#6a8faa';

      // catalysts html
      var catsHtml = '';
      if (r.cats && r.cats.length) {
        catsHtml = '<div class="cats-label">▶ KEY CATALYSTS</div><div class="cats-wrap">'
          + r.cats.map(function (c) { return '<span class="cat-pill">' + c + '</span>'; }).join('')
          + '</div>';
      }

      // risks html
      var risksHtml = '';
      if (r.risks) {
        risksHtml = '<div class="risks-row">'
          + '<span class="risks-label">▶ RISKS</span>'
          + '<span class="risks-text">' + r.risks + '</span>'
          + '</div>';
      }

      html += '<div class="inv-card">'

        // top section
        + '<div class="inv-card-top">'
        + '<span class="type-badge" style="border:1px solid ' + ts.b + ';background:' + ts.bg + ';color:' + ts.c + '">' + (r.type || 'STOCK') + '</span>'
        + '<div style="min-width:0">'
        + '<div class="ticker-name">' + (r.ticker || '') + '</div>'
        + '<div class="full-name">' + (r.name || '') + '</div>'
        + '<div class="strat-tag">' + (r.strategy || '') + '</div>'
        + '</div>'
        + '<div>'
        + '<div class="conv-badge" style="color:' + convCol + '">' + (r.conviction || '') + '</div>'
        + '<div class="hold-tag">HOLD: ' + (r.hold || '') + '</div>'
        + '</div>'
        + '</div>'

        // metrics
        + '<div class="inv-metrics">'
        + '<div class="inv-metric"><div class="metric-label">UPSIDE</div><div class="metric-val" style="color:#00ff9d">' + (r.upside || '—') + '</div></div>'
        + '<div class="inv-metric"><div class="metric-label">RISK</div><div class="metric-val" style="color:var(--text)">' + (r.risk || '—') + '</div></div>'
        + '<div class="inv-metric"><div class="metric-label">YIELD</div><div class="metric-val" style="color:var(--text)">' + (r.div || '—') + '</div></div>'
        + '</div>'

        // fit bar
        + '<div class="fit-row">'
        + '<div class="fit-label">MARKET FIT</div>'
        + '<div class="fit-track"><div class="fit-fill" style="width:' + fit + '%;background:' + fitCol + '"></div></div>'
        + '<div class="fit-pct" style="color:' + fitCol + '">' + fit + '%</div>'
        + '</div>'

        // expand button
        + '<button class="expand-btn" type="button" onclick="toggleCard(\'' + uid + '\')">'
        + '<span class="expand-label">▶ FULL ANALYSIS &amp; REASONING</span>'
        + '<span class="expand-arrow" id="arr-' + uid + '">▼</span>'
        + '</button>'

        // reasoning panel
        + '<div class="reasoning-panel" id="' + uid + '">'
        + '<div class="why-text">' + highlight(r.why || '') + '</div>'
        + catsHtml
        + risksHtml
        + '</div>'

        + '</div>'; // .inv-card
    });
  });

  document.getElementById('results').innerHTML = html;
}

function toggleCard (uid) {
  var panel = document.getElementById(uid);
  var arrow = document.getElementById('arr-' + uid);
  if (!panel || !arrow) return;
  var isOpen = panel.classList.toggle('open');
  arrow.classList.toggle('open', isOpen);
}
</script>

</body>
</html>
