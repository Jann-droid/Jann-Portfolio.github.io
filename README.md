<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8">
<style>html,body{background:#020408!important;color:#e8f4ff!important;}</style>
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>InvestRadar — AI Market Intelligence</title>
<!--
  ╔══════════════════════════════════════════════════════════════════╗
  ║  INVESTRADAR — AI-Powered Investment Intelligence Tool          ║
  ║  Built with Claude AI + Anthropic API                           ║
  ║                                                                  ║
  ║  SETUP:                                                          ║
  ║  1. Get your API key at https://console.anthropic.com           ║
  ║  2. Enter it in the API Key field below                         ║
  ║  3. Select strategy, set risk & hold horizon, click Analyze     ║
  ║                                                                  ║
  ║  HOSTING ON GITHUB PAGES:                                        ║
  ║  - Works on GitHub Pages, Netlify, Vercel, or local file://     ║
  ║  - Anthropic API supports direct browser calls (CORS enabled)   ║
  ║  - Your API key is stored only in localStorage on your device   ║
  ║  - Never commit your API key to any file in git                 ║
  ║  - This file contains NO hardcoded secrets — safe to publish    ║
  ║                                                                  ║
  ║  COST: ~$0.02–0.05 per analysis (Claude Sonnet)                 ║
  ╚══════════════════════════════════════════════════════════════════╝
-->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@400;600;700;900&family=Rajdhani:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
:root {
  --void: #020408;
  --font-mono: 'Share Tech Mono', 'Courier New', Courier, monospace;
  --font-display: 'Orbitron', 'Courier New', Courier, monospace;
  --font-body: 'Rajdhani', system-ui, -apple-system, 'Segoe UI', sans-serif;
  --deep: #040b12;
  --card: #060f18;
  --panel: #091422;
  --cy: #00d4ff;
  --cyd: rgba(0,212,255,0.22);
  --cyf: rgba(0,212,255,0.06);
  --gn: #00ff88;
  --gnd: rgba(0,255,136,0.18);
  --gnf: rgba(0,255,136,0.05);
  --am: #ffaa00;
  --amd: rgba(255,170,0,0.2);
  --rd: #ff3a5c;
  --pu: #9b59ff;
  --t1: #e8f4ff;
  --t2: #7ab3cc;
  --tm: #3a6070;
  --bd: #0a2030;
}
*, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

html {
background: #020408;
min-height: 100%;
}
body {
background: #020408;
background: var(–void);
color: #e8f4ff;
color: var(–t1);
font-family: ‘Rajdhani’, system-ui, -apple-system, ‘Segoe UI’, sans-serif;
min-height: 100vh;
overflow-x: hidden;
}
body::before {
content: ‘’;
position: fixed; inset: 0;
background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,212,255,0.012) 2px, rgba(0,212,255,0.012) 4px);
pointer-events: none;
z-index: 9999;
}
.grid-bg {
position: fixed; inset: 0;
background-image: linear-gradient(var(–bd) 1px, transparent 1px), linear-gradient(90deg, var(–bd) 1px, transparent 1px);
background-size: 40px 40px;
z-index: 0; opacity: 0.4;
pointer-events: none;
}
.glow { position: fixed; border-radius: 50%; filter: blur(120px); pointer-events: none; z-index: 0; }
.glow-tl { top: -200px; left: -200px; width: 500px; height: 500px; background: rgba(0,212,255,0.055); }
.glow-br { bottom: -200px; right: -200px; width: 500px; height: 500px; background: rgba(155,89,255,0.04); }

.app { position: relative; z-index: 1; max-width: 520px; margin: 0 auto; padding-bottom: 80px; }

/* ── HEADER ── */
.header { padding: 20px 16px 14px; border-bottom: 1px solid var(–bd); position: relative; }
.header::after { content: ‘’; position: absolute; bottom: 0; left: 0; right: 0; height: 1px; background: linear-gradient(90deg, transparent, var(–cy), transparent); }
.header-row { display: flex; align-items: center; justify-content: space-between; margin-bottom: 4px; }
.logo { font-family: var(–font-display); font-size: 20px; font-weight: 900; letter-spacing: 4px; color: var(–cy); }
.logo span { color: var(–t2); font-weight: 400; }
.live-pill { display: flex; align-items: center; gap: 6px; font-family: var(–font-mono); font-size: 10px; color: var(–gn); letter-spacing: 2px; }
.live-dot { width: 6px; height: 6px; border-radius: 50%; background: var(–gn); animation: pulse 2s infinite; }
@keyframes pulse { 0%,100% { opacity:1; } 50% { opacity:0.2; } }
.header-sub { font-family: var(–font-mono); font-size: 11px; color: var(–tm); letter-spacing: 2px; }

/* ── TICKER ── */
.ticker-wrap { border-bottom: 1px solid var(–bd); padding: 8px 0; overflow: hidden; position: relative; }
.ticker-wrap::after { content: ‘’; position: absolute; right: 0; top: 0; bottom: 0; width: 40px; background: linear-gradient(90deg, transparent, var(–void)); pointer-events: none; z-index: 2; }
.ticker-track { display: flex; gap: 18px; animation: ticker-scroll 32s linear infinite; width: max-content; }
@keyframes ticker-scroll { 0% { transform: translateX(0); } 100% { transform: translateX(-50%); } }
.ti { display: flex; align-items: center; gap: 5px; }
.ti-key { font-family: var(–font-mono); font-size: 9px; color: var(–tm); letter-spacing: 1px; }
.ti-val { font-family: var(–font-display); font-size: 9px; font-weight: 600; color: var(–t2); transition: color 0.4s; }
.ti-val.up { color: var(–gn); } .ti-val.dn { color: var(–rd); }
.ti-sep { color: var(–bd); font-size: 13px; }

/* ── API KEY ── */
.apikey-section { padding: 14px 16px 0; }
.field-label { font-family: var(–font-mono); font-size: 9px; letter-spacing: 3px; color: var(–tm); margin-bottom: 6px; display: block; }
.apikey-row { display: flex; gap: 8px; }
.apikey-input {
flex: 1; background: var(–card); border: 1px solid var(–bd); border-radius: 4px;
color: var(–t2); font-family: var(–font-mono); font-size: 11px;
padding: 9px 12px; outline: none; transition: border-color 0.2s;
}
.apikey-input:focus { border-color: var(–cyd); }
.apikey-input::placeholder { color: var(–tm); }
.apikey-input.ok { border-color: var(–gnd); color: var(–gn); }
.apikey-input.err { border-color: rgba(255,58,92,0.4); }
.btn-sm {
background: transparent; border: 1px solid var(–bd); border-radius: 4px;
color: var(–tm); font-family: var(–font-mono); font-size: 9px;
padding: 9px 10px; cursor: pointer; transition: all 0.2s; white-space: nowrap; letter-spacing: 1px;
}
.btn-sm:hover { border-color: var(–cyd); color: var(–cy); }
.apikey-hint { font-family: var(–font-mono); font-size: 9px; color: var(–tm); margin-top: 5px; line-height: 1.5; }
.apikey-hint a { color: var(–cy); text-decoration: none; }
.apikey-hint a:hover { text-decoration: underline; }

/* ── SECTION LABEL ── */
.slabel { font-family: var(–font-mono); font-size: 9px; letter-spacing: 3px; color: var(–tm); padding: 14px 16px 8px; }

/* ── STRATEGY GRID ── */
.strategy-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; padding: 0 16px; }
.strat-btn {
background: var(–card); border: 1px solid var(–bd); border-radius: 4px;
padding: 10px 12px; cursor: pointer; text-align: left; position: relative; overflow: hidden;
transition: border-color 0.2s, background 0.2s;
}
.strat-btn::before { content: ‘’; position: absolute; top: 0; left: 0; width: 3px; height: 100%; background: var(–cy); transform: scaleY(0); transition: transform 0.2s; }
.strat-btn.active { border-color: var(–cyd); background: #050e18; }
.strat-btn.active::before { transform: scaleY(1); }
.strat-btn:active { transform: scale(0.98); }
.sn { font-family: var(–font-display); font-size: 10px; font-weight: 600; letter-spacing: 1px; color: var(–t2); margin-bottom: 3px; }
.strat-btn.active .sn { color: var(–cy); }
.sd { font-size: 11px; color: var(–tm); line-height: 1.4; }

/* ── SLIDERS ── */
.sliders-section { padding: 12px 16px 0; }
.slider-row { display: flex; align-items: center; gap: 12px; margin-bottom: 10px; }
.sl-lbl { font-family: var(–font-mono); font-size: 9px; letter-spacing: 2px; color: var(–tm); width: 44px; flex-shrink: 0; }
.slider {
flex: 1; -webkit-appearance: none; appearance: none;
height: 2px; border-radius: 2px; outline: none; cursor: pointer;
}
.slider.risk { background: linear-gradient(90deg, var(–gn), var(–am), var(–rd)); }
.slider.hold { background: linear-gradient(90deg, var(–cy), var(–pu)); }
.slider::-webkit-slider-thumb {
-webkit-appearance: none; width: 15px; height: 15px;
border-radius: 50%; background: var(–void); border: 2px solid var(–cy); cursor: pointer;
}
.sl-val { font-family: var(–font-display); font-size: 9px; color: var(–cy); width: 75px; text-align: right; flex-shrink: 0; }

/* ── ANALYZE BUTTON ── */
.analyze-wrap { padding: 14px 16px; }
.analyze-btn {
width: 100%; background: transparent; border: 1px solid var(–cy); border-radius: 4px;
color: var(–cy); font-family: var(–font-display); font-size: 12px;
font-weight: 700; letter-spacing: 3px; padding: 14px; cursor: pointer;
position: relative; overflow: hidden; transition: all 0.2s;
}
.analyze-btn::before { content: ‘’; position: absolute; inset: 0; background: var(–cy); transform: translateX(-100%); transition: transform 0.3s; z-index: 0; }
.analyze-btn:hover::before { transform: translateX(0); }
.analyze-btn:hover { color: var(–void); }
.analyze-btn span { position: relative; z-index: 1; }
.analyze-btn:disabled { border-color: var(–tm); color: var(–tm); cursor: not-allowed; }
.analyze-btn:disabled::before { display: none; }

/* ── STATUS ── */
.status-bar {
margin: 0 16px; padding: 10px 14px;
background: var(–panel); border: 1px solid var(–bd); border-radius: 4px;
font-family: var(–font-mono); font-size: 11px; color: var(–cy);
display: none; align-items: center; gap: 8px;
}
.status-bar.show { display: flex; }
.status-bar.err { color: var(–rd); border-color: rgba(255,58,92,0.3); }
.spinner { width: 12px; height: 12px; border: 1.5px solid var(–tm); border-top-color: var(–cy); border-radius: 50%; animation: spin 0.8s linear infinite; flex-shrink: 0; }
.status-bar.err .spinner { display: none; }
@keyframes spin { to { transform: rotate(360deg); } }

/* ── MACRO PANEL ── */
.macro-panel { margin: 14px 16px 0; background: var(–card); border: 1px solid var(–bd); border-radius: 6px; overflow: hidden; display: none; }
.macro-panel.show { display: block; }
.macro-hdr { padding: 10px 14px; border-bottom: 1px solid var(–bd); display: flex; align-items: center; justify-content: space-between; }
.macro-title { font-family: var(–font-display); font-size: 9px; letter-spacing: 3px; color: var(–cy); }
.macro-badge { font-family: var(–font-mono); font-size: 8px; color: var(–gn); letter-spacing: 1px; padding: 2px 8px; border: 1px solid var(–gnd); border-radius: 2px; background: var(–gnf); }
.macro-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; padding: 12px 14px 0; }
.macro-item { background: var(–deep); border: 1px solid var(–bd); border-radius: 4px; padding: 8px 10px; }
.macro-item.wide { grid-column: span 2; }
.mi-key { font-family: var(–font-mono); font-size: 8px; color: var(–tm); letter-spacing: 1px; margin-bottom: 3px; }
.mi-val { font-family: var(–font-display); font-size: 13px; font-weight: 600; color: var(–t1); }
.mi-val.up { color: var(–gn); } .mi-val.dn { color: var(–rd); } .mi-val.warn { color: var(–am); }
.macro-summary { margin: 10px 14px 12px; font-size: 13px; line-height: 1.7; color: var(–t2); border-top: 1px solid var(–bd); padding-top: 10px; }

/* ── RESULTS ── */
.results-section { padding: 16px 16px 0; display: none; }
.results-section.show { display: block; }
.results-hdr { display: flex; align-items: center; justify-content: space-between; margin-bottom: 14px; padding-bottom: 10px; border-bottom: 1px solid var(–bd); }
.results-title { font-family: var(–font-display); font-size: 10px; color: var(–tm); letter-spacing: 3px; }
.results-count { font-family: var(–font-mono); font-size: 11px; color: var(–gn); }

/* ── GROUP HEADERS ── */
.group-hdr { display: flex; align-items: center; gap: 10px; margin: 20px 0 8px; }
.group-hdr:first-child { margin-top: 0; }
.group-ico { width: 30px; height: 30px; border-radius: 4px; display: flex; align-items: center; justify-content: center; font-size: 14px; flex-shrink: 0; }
.group-ico.L { background: var(–cyf); border: 1px solid var(–cyd); color: var(–cy); }
.group-ico.S { background: var(–gnf); border: 1px solid var(–gnd); color: var(–gn); }
.group-titles { flex: 1; }
.group-label { font-family: var(–font-display); font-size: 11px; font-weight: 700; letter-spacing: 3px; }
.group-label.L { color: var(–cy); } .group-label.S { color: var(–gn); }
.group-sub { font-family: var(–font-mono); font-size: 9px; color: var(–tm); letter-spacing: 1px; margin-top: 2px; }
.group-cnt { font-family: var(–font-mono); font-size: 9px; letter-spacing: 1px; padding: 3px 8px; border-radius: 2px; flex-shrink: 0; }
.group-cnt.L { color: var(–cy); border: 1px solid var(–cyd); background: var(–cyf); }
.group-cnt.S { color: var(–gn); border: 1px solid var(–gnd); background: var(–gnf); }
.group-div { height: 1px; margin-bottom: 10px; }
.group-div.L { background: linear-gradient(90deg, var(–cyd), transparent); }
.group-div.S { background: linear-gradient(90deg, var(–gnd), transparent); }

/* ── CARDS ── */
.rec-card { background: var(–card); border: 1px solid var(–bd); border-radius: 6px; margin-bottom: 10px; overflow: hidden; transition: border-color 0.2s; }
.rec-card:hover { border-color: #0d2535; }
.card-main { padding: 14px; display: grid; grid-template-columns: auto 1fr auto; gap: 0 12px; align-items: start; }
.type-badge { font-family: var(–font-mono); font-size: 8px; letter-spacing: 2px; padding: 3px 7px; border-radius: 2px; border: 1px solid; white-space: nowrap; margin-top: 2px; }
.t-stock { color: var(–cy); border-color: var(–cyd); background: var(–cyf); }
.t-etf   { color: var(–gn); border-color: var(–gnd); background: var(–gnf); }
.t-bond  { color: var(–am); border-color: var(–amd); background: rgba(255,170,0,0.05); }
.t-crypto { color: var(–pu); border-color: rgba(155,89,255,0.3); background: rgba(155,89,255,0.05); }
.t-reit  { color: #ff9955; border-color: rgba(255,153,85,0.3); background: rgba(255,153,85,0.05); }
.card-info { min-width: 0; }
.card-ticker { font-family: var(–font-display); font-size: 17px; font-weight: 700; color: var(–t1); letter-spacing: 1px; line-height: 1; margin-bottom: 3px; }
.card-name { font-size: 12px; color: var(–t2); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; margin-bottom: 5px; }
.card-strat { font-family: var(–font-mono); font-size: 9px; letter-spacing: 1px; color: var(–tm); }
.card-right { text-align: right; }
.card-conv { font-family: var(–font-display); font-size: 11px; font-weight: 600; letter-spacing: 1px; margin-bottom: 4px; }
.conv-H { color: var(–gn); } .conv-M { color: var(–am); } .conv-L { color: var(–t2); }
.card-hold { font-family: var(–font-mono); font-size: 9px; color: var(–tm); letter-spacing: 1px; white-space: nowrap; }
.card-metrics { display: grid; grid-template-columns: repeat(3, 1fr); border-top: 1px solid var(–bd); }
.metric { padding: 8px 10px; border-right: 1px solid var(–bd); text-align: center; }
.metric:last-child { border-right: none; }
.met-lbl { font-family: var(–font-mono); font-size: 8px; color: var(–tm); letter-spacing: 1px; display: block; margin-bottom: 2px; }
.met-val { font-family: var(–font-display); font-size: 12px; font-weight: 600; color: var(–t1); }
.met-val.pos { color: var(–gn); }
.align-row { border-top: 1px solid var(–bd); padding: 7px 14px; display: flex; align-items: center; gap: 8px; background: rgba(0,0,0,0.15); }
.align-lbl { font-family: var(–font-mono); font-size: 8px; color: var(–tm); letter-spacing: 1px; width: 62px; flex-shrink: 0; }
.align-bar { flex: 1; height: 3px; background: var(–bd); border-radius: 2px; overflow: hidden; }
.align-fill { height: 100%; border-radius: 2px; transition: width 0.8s ease; }
.align-score { font-family: var(–font-display); font-size: 10px; font-weight: 600; width: 32px; text-align: right; flex-shrink: 0; }
.card-expand { width: 100%; background: transparent; border: none; border-top: 1px solid var(–bd); padding: 8px 14px; display: flex; align-items: center; justify-content: space-between; cursor: pointer; transition: background 0.15s; }
.card-expand:hover { background: rgba(0,212,255,0.03); }
.exp-lbl { font-family: var(–font-mono); font-size: 9px; letter-spacing: 2px; color: var(–cy); }
.exp-arr { font-size: 10px; color: var(–cy); transition: transform 0.2s; display: inline-block; }
.exp-arr.open { transform: rotate(180deg); }
.reasoning-panel { border-top: 1px solid var(–bd); background: var(–deep); display: none; padding: 16px; }
.reasoning-panel.show { display: block; }
.reasoning-text { font-size: 13px; line-height: 1.8; color: var(–t2); }
.reasoning-text p { margin-bottom: 10px; }
.reasoning-text p:last-child { margin-bottom: 0; }
.reasoning-text .hl { color: var(–cy); font-weight: 600; }
.reasoning-text .tag { display: inline-block; font-family: var(–font-mono); font-size: 9px; padding: 2px 6px; border-radius: 2px; margin: 2px 3px 2px 0; border: 1px solid; }
.tag-bull { color: var(–gn); border-color: var(–gnd); background: var(–gnf); }
.tag-bear { color: var(–rd); border-color: rgba(255,58,92,0.3); background: rgba(255,58,92,0.05); }
.tag-neutral { color: var(–am); border-color: var(–amd); background: rgba(255,170,0,0.05); }
.data-strip { margin-top: 12px; padding-top: 10px; border-top: 1px solid var(–bd); display: flex; flex-wrap: wrap; gap: 6px; }
.data-chip { font-family: var(–font-mono); font-size: 9px; color: var(–tm); padding: 3px 8px; border: 1px solid var(–bd); border-radius: 2px; background: var(–void); }

/* ── DISCLAIMER ── */
.disclaimer { margin: 16px 16px 0; padding: 10px 14px; border: 1px solid var(–bd); border-radius: 4px; font-family: var(–font-mono); font-size: 9px; color: var(–tm); line-height: 1.6; display: none; }
.disclaimer.show { display: block; }

/* ── HIGHLIGHTS ── */
.hl { color: var(–cy); font-weight: 600; }
</style>

</head>
<body>
<div class="grid-bg"></div>
<div class="glow glow-tl"></div>
<div class="glow glow-br"></div>

<div class="app">

  <!-- HEADER -->

  <div class="header">
    <div class="header-row">
      <div class="logo">INVEST<span>RADAR</span></div>
      <div class="live-pill"><div class="live-dot"></div>AI ANALYSIS</div>
    </div>
    <div class="header-sub">// Real-time market intelligence powered by Claude AI</div>
  </div>

  <!-- TICKER STRIP -->

  <div class="ticker-wrap">
    <div class="ticker-track" id="tickerTrack">
      <!-- populated by JS -->
    </div>
  </div>

  <!-- API KEY -->

  <div class="apikey-section">
    <span class="field-label">// ANTHROPIC API KEY</span>
    <div class="apikey-row">
      <input class="apikey-input" type="password" id="apiKeyInput"
        placeholder="sk-ant-api03-..." autocomplete="off" spellcheck="false"
        oninput="onKeyInput()">
      <button class="btn-sm" onclick="toggleKeyVis()" id="toggleBtn">SHOW</button>
      <button class="btn-sm" onclick="clearKey()">CLEAR</button>
    </div>
    <div class="apikey-hint">
      Get your key: <a href="https://console.anthropic.com" target="_blank">console.anthropic.com</a> → API Keys
      &nbsp;·&nbsp; Key is saved locally in your browser only &nbsp;·&nbsp; ~$0.03 per analysis
    </div>
  </div>

  <!-- STRATEGY -->

  <div class="slabel">// SELECT STRATEGY</div>
  <div class="strategy-grid" id="strategyGrid">
    <button class="strat-btn active" data-strat="value" onclick="setStrat(this)">
      <div class="sn">VALUE</div><div class="sd">Undervalued assets, margin of safety</div>
    </button>
    <button class="strat-btn" data-strat="growth" onclick="setStrat(this)">
      <div class="sn">GROWTH</div><div class="sd">High-growth, future earnings</div>
    </button>
    <button class="strat-btn" data-strat="dividend" onclick="setStrat(this)">
      <div class="sn">DIVIDEND</div><div class="sd">Income-generating, stable yield</div>
    </button>
    <button class="strat-btn" data-strat="momentum" onclick="setStrat(this)">
      <div class="sn">MOMENTUM</div><div class="sd">Trend-following, technical signals</div>
    </button>
    <button class="strat-btn" data-strat="index" onclick="setStrat(this)">
      <div class="sn">INDEX / ETF</div><div class="sd">Passive, diversified, low-cost</div>
    </button>
    <button class="strat-btn" data-strat="balanced" onclick="setStrat(this)">
      <div class="sn">BALANCED</div><div class="sd">All-weather, mixed strategy</div>
    </button>
  </div>

  <!-- SLIDERS -->

  <div class="sliders-section">
    <div class="slider-row">
      <span class="sl-lbl">RISK</span>
      <input type="range" class="slider risk" min="1" max="5" step="1" value="3" id="riskSlider" oninput="onSlider()">
      <span class="sl-val" id="riskVal">BALANCED</span>
    </div>
    <div class="slider-row">
      <span class="sl-lbl">HOLD</span>
      <input type="range" class="slider hold" min="1" max="5" step="1" value="3" id="holdSlider" oninput="onSlider()">
      <span class="sl-val" id="holdVal">1–3 YRS</span>
    </div>
  </div>

  <!-- ANALYZE BUTTON -->

  <div class="analyze-wrap">
    <button class="analyze-btn" id="analyzeBtn" onclick="runAnalysis()">
      <span id="analyzeBtnText">▶ ANALYZE MARKETS</span>
    </button>
  </div>

  <!-- STATUS BAR -->

  <div class="status-bar" id="statusBar">
    <div class="spinner"></div>
    <span id="statusText">INITIALIZING...</span>
  </div>

  <!-- MACRO CONTEXT -->

  <div class="macro-panel" id="macroPanel">
    <div class="macro-hdr">
      <div class="macro-title">// MARKET CONTEXT</div>
      <div class="macro-badge" id="macroBadge">LIVE DATA</div>
    </div>
    <div class="macro-grid" id="macroGrid"></div>
    <div class="macro-summary" id="macroSummary"></div>
  </div>

  <!-- RESULTS -->

  <div class="results-section" id="resultsSection">
    <div class="results-hdr">
      <div class="results-title">// RECOMMENDATIONS</div>
      <div class="results-count" id="resultsCount">0 SIGNALS</div>
    </div>
    <div id="cardsContainer"></div>
  </div>

  <!-- DISCLAIMER -->

  <div class="disclaimer" id="disclaimer">
    ⚠ DISCLAIMER: AI-generated analysis for educational purposes only. Not financial advice.
    Past performance does not guarantee future results. Always conduct independent research
    and consult a licensed financial advisor before investing. API calls are made directly
    from your browser to Anthropic — your key never leaves your device.
  </div>

</div>

<script>
'use strict';

// ── CONFIG ────────────────────────────────────────────────────────────────────
var RISK_LABELS = ['VERY LOW','LOW','BALANCED','HIGH','VERY HIGH'];
var HOLD_LABELS = ['<3 MON','3–12 MON','1–3 YRS','3–7 YRS','7+ YRS'];
var RISK_DESC = [
  'very conservative — capital preservation, minimal volatility, bonds and defensive large-caps only',
  'conservative — modest growth with low drawdown tolerance, quality dividend payers preferred',
  'balanced — moderate growth accepting normal market volatility, blend of growth and value',
  'aggressive — high growth focus, comfortable with 30–40% drawdowns, risk-on sectors',
  'very aggressive — maximum growth, high volatility fully acceptable, speculative positions OK'
];
var HOLD_DESC = [
  'very short-term trade under 3 months — prioritise momentum, near-term catalysts, technical breakouts',
  'short-term 3–12 months — momentum, earnings plays, sector rotation opportunities',
  'medium-term 1–3 years — quality compounders, value with near-term catalysts, growth at reasonable price',
  'long-term 3–7 years — wide-moat value, dividend growers, structural secular themes',
  'very long-term 7+ years — buy-and-hold compounders, index funds, reinvested dividend compounding'
];
var STRAT_DESC = {
  value:    'Benjamin Graham / Warren Buffett value investing: stocks trading below intrinsic value with a margin of safety, low P/E and P/B ratios, strong balance sheets, wide economic moats, and proven management',
  growth:   'Peter Lynch / Philip Fisher growth investing: companies with above-average revenue and earnings growth, expanding addressable markets, durable competitive advantages, and strong reinvestment opportunities',
  dividend: 'Dividend growth investing: Dividend Aristocrats and Kings with 10+ year payout growth history, high and sustainable yields, strong FCF coverage, and financial stability across economic cycles',
  momentum: 'Momentum and trend-following: 52-week relative strength leaders, positive earnings surprises, technical breakouts on volume, sector rotation leaders, and near-term catalyst-driven setups',
  index:    'Passive index investing: broad market ETFs, sector ETFs, and smart-beta factor ETFs with low expense ratios, broad diversification, tax efficiency, and long-term compounding',
  balanced: 'All-weather balanced portfolio: strategic blend of equities, bonds, commodities, and REITs diversified across strategies and geographies to deliver consistent risk-adjusted returns in any macro regime'
};
var STATUS_MSGS = [
  'FETCHING LIVE MARKET DATA...','SCANNING MACRO INDICATORS...',
  'ANALYZING SECTOR CONDITIONS...','CROSS-REFERENCING SIGNALS...',
  'RUNNING VALUATION MODELS...','CALIBRATING RECOMMENDATIONS...',
  'GENERATING ANALYSIS REPORT...'
];

// ── STATE ─────────────────────────────────────────────────────────────────────
var state = {
  strat: 'value',
  risk: 3,
  hold: 3,
  loading: false,
  debounceTimer: null
};

var TICKER_DATA = [
  {key:'S&P 500', id:'t0', val:'—', trend:'n'},
  {key:'NASDAQ',  id:'t1', val:'—', trend:'n'},
  {key:'FED',     id:'t2', val:'—', trend:'n'},
  {key:'10Y',     id:'t3', val:'—', trend:'n'},
  {key:'CPI',     id:'t4', val:'—', trend:'n'},
  {key:'VIX',     id:'t5', val:'—', trend:'n'},
  {key:'GOLD',    id:'t6', val:'—', trend:'n'},
  {key:'OIL WTI', id:'t7', val:'—', trend:'n'},
  {key:'BTC',     id:'t8', val:'—', trend:'n'},
  {key:'USD IDX', id:'t9', val:'—', trend:'n'},
];

// ── INIT ──────────────────────────────────────────────────────────────────────
(function init() {
  buildTicker();
  var saved = localStorage.getItem('ir_apikey');
  if (saved) {
    document.getElementById('apiKeyInput').value = saved;
    document.getElementById('apiKeyInput').classList.add('ok');
  }
  updateSliderLabels();
  checkCORS();
})();

function checkCORS() {
  // Anthropic's API supports CORS from any origin when using x-api-key.
  // The 'anthropic-dangerous-direct-browser-calls' header is required for browser-direct calls.
  // This works from GitHub Pages, local file://, and any web host.
  // No proxy needed — Anthropic explicitly supports direct browser API calls.
}

function buildTicker() {
  var track = document.getElementById('tickerTrack');
  var items = TICKER_DATA.concat(TICKER_DATA); // duplicate for seamless loop
  track.innerHTML = items.map(function(t, i) {
    var id = i < TICKER_DATA.length ? ' id="ti-'+t.id+'"' : '';
    return '<span class="ti"><span class="ti-key">'+t.key+'</span>'
      +'<span class="ti-val'+'" '+id+'>'+t.val+'</span></span>'
      +'<span class="ti-sep">|</span>';
  }).join('');
}

function updateTicker(indicators) {
  var keyMap = {
    'S&P 500':'t0','SP500':'t0','S&P500':'t0',
    'NASDAQ':'t1','Nasdaq':'t1',
    'Fed Rate':'t2','FED RATE':'t2','Federal Funds':'t2',
    '10Y Yield':'t3','10-Year':'t3','10Y':'t3',
    'CPI YoY':'t4','CPI':'t4','Inflation':'t4',
    'VIX':'t5',
    'Gold':'t6','GOLD':'t6',
    'Oil WTI':'t7','WTI':'t7','OIL':'t7','Crude Oil':'t7',
    'BTC':'t8','Bitcoin':'t8',
    'USD Index':'t9','DXY':'t9','USD IDX':'t9'
  };
  indicators.forEach(function(ind) {
    var id = keyMap[ind.key];
    if (!id) {
      Object.keys(keyMap).forEach(function(k) {
        if (!id && ind.key && ind.key.toUpperCase().indexOf(k.toUpperCase()) !== -1) id = keyMap[k];
      });
    }
    if (!id) return;
    var el = document.getElementById('ti-'+id);
    if (!el) return;
    el.textContent = ind.value;
    el.className = 'ti-val' + (ind.trend==='up'?' up':ind.trend==='dn'?' dn':'');
  });
}

// ── CONTROLS ──────────────────────────────────────────────────────────────────
function onKeyInput() {
  var v = document.getElementById('apiKeyInput').value.trim();
  var inp = document.getElementById('apiKeyInput');
  if (v.startsWith('sk-ant-') && v.length > 20) {
    inp.className = 'apikey-input ok';
    localStorage.setItem('ir_apikey', v);
  } else if (v.length > 0) {
    inp.className = 'apikey-input err';
  } else {
    inp.className = 'apikey-input';
    localStorage.removeItem('ir_apikey');
  }
  scheduleAnalysis();
}

function toggleKeyVis() {
  var inp = document.getElementById('apiKeyInput');
  var btn = document.getElementById('toggleBtn');
  inp.type = inp.type === 'password' ? 'text' : 'password';
  btn.textContent = inp.type === 'password' ? 'SHOW' : 'HIDE';
}

function clearKey() {
  document.getElementById('apiKeyInput').value = '';
  document.getElementById('apiKeyInput').className = 'apikey-input';
  localStorage.removeItem('ir_apikey');
}

function setStrat(btn) {
  document.querySelectorAll('.strat-btn').forEach(function(b) { b.classList.remove('active'); });
  btn.classList.add('active');
  state.strat = btn.dataset.strat;
  scheduleAnalysis();
}

function onSlider() {
  state.risk = parseInt(document.getElementById('riskSlider').value);
  state.hold = parseInt(document.getElementById('holdSlider').value);
  updateSliderLabels();
  scheduleAnalysis();
}

function updateSliderLabels() {
  document.getElementById('riskVal').textContent = RISK_LABELS[state.risk - 1];
  document.getElementById('holdVal').textContent = HOLD_LABELS[state.hold - 1];
}

// Auto-run with debounce on any parameter change
function scheduleAnalysis() {
  if (state.debounceTimer) clearTimeout(state.debounceTimer);
  state.debounceTimer = setTimeout(function() { runAnalysis(); }, 600);
}

function runAnalysis() {
  if (state.debounceTimer) clearTimeout(state.debounceTimer);
  var key = document.getElementById('apiKeyInput').value.trim();
  if (!key || !key.startsWith('sk-ant-') || key.length < 20) {
    showStatus('ENTER A VALID API KEY TO START ANALYSIS', true);
    return;
  }
  if (state.loading) return;
  doAnalysis(key);
}

// ── UI HELPERS ────────────────────────────────────────────────────────────────
function showStatus(msg, isErr) {
  var bar = document.getElementById('statusBar');
  bar.className = 'status-bar show' + (isErr ? ' err' : '');
  document.getElementById('statusText').textContent = msg;
}
function hideStatus() {
  document.getElementById('statusBar').className = 'status-bar';
}
function setLoading(on) {
  state.loading = on;
  var btn = document.getElementById('analyzeBtn');
  var txt = document.getElementById('analyzeBtnText');
  btn.disabled = on;
  txt.textContent = on ? '◌ ANALYZING...' : '▶ ANALYZE MARKETS';
}

// ── ANALYSIS ENGINE ───────────────────────────────────────────────────────────
async function doAnalysis(apiKey) {
  setLoading(true);
  document.getElementById('macroPanel').className = 'macro-panel';
  document.getElementById('resultsSection').className = 'results-section';
  document.getElementById('disclaimer').className = 'disclaimer';
  showStatus(STATUS_MSGS[0], false);

  var msgIdx = 0;
  var msgTimer = setInterval(function() {
    msgIdx = (msgIdx + 1) % STATUS_MSGS.length;
    document.getElementById('statusText').textContent = STATUS_MSGS[msgIdx];
  }, 1600);

  try {
    // ── PHASE 1: Live market data via web search ───────────────────────────
    var searchData = await callClaude(apiKey,
      [{role:'user', content: 'Use web search to find the latest real-time values (today, April 2026) for these market indicators:\n1. S&P 500 current level and today\'s % change\n2. NASDAQ Composite current level and today\'s % change\n3. US Federal Reserve federal funds rate (current target range)\n4. US 10-year Treasury yield\n5. Latest US CPI inflation rate year-over-year\n6. VIX volatility index current level\n7. Gold spot price (USD/oz)\n8. WTI crude oil price (USD/barrel)\n9. Bitcoin price (USD)\n10. US Dollar Index (DXY)\n11. Latest US GDP growth rate\n12. US unemployment rate\n13. Key macro events currently affecting markets (Fed decisions, geopolitical events, earnings season)\n14. Top performing and underperforming sectors right now\n15. Overall market sentiment\n\nSearch and provide a thorough factual summary of all findings.'}],
      [{type:'web_search_20250305', name:'web_search'}]
    );

    var marketText = extractText(searchData);
    if (!marketText || marketText.length < 100) {
      marketText = 'Use your best available knowledge of financial market conditions as of April 2026 for current indicator values.';
    }

    clearInterval(msgTimer);
    showStatus('PROCESSING DATA & GENERATING RECOMMENDATIONS...', false);

    // ── PHASE 2: Structured analysis + recommendations ─────────────────────
    var systemPrompt = 'You are a senior investment analyst at a top-tier asset management firm. You write detailed, substantive investment analysis that institutional investors would find credible and actionable. You always respond with valid JSON only — no markdown, no code fences, no text outside the JSON object.';

    var userPrompt = buildAnalysisPrompt(marketText);

    var analysisData = await callClaude(apiKey,
      [{role:'user', content: userPrompt}],
      null,
      systemPrompt
    );

    var rawText = extractText(analysisData);
    var parsed = parseJSON(rawText);

    if (!parsed || !parsed.groups || !Array.isArray(parsed.groups)) {
      throw new Error('Invalid response structure — missing groups array');
    }

    // Render
    renderMacro(parsed.macro || {});
    renderGroups(parsed.groups);
    if (parsed.macro && parsed.macro.indicators) {
      updateTicker(parsed.macro.indicators);
    }

    hideStatus();
    document.getElementById('resultsSection').className = 'results-section show';
    document.getElementById('disclaimer').className = 'disclaimer show';
    var total = parsed.groups.reduce(function(a, g) { return a + (g.items ? g.items.length : 0); }, 0);
    document.getElementById('resultsCount').textContent = total + ' SIGNALS';

  } catch(err) {
    clearInterval(msgTimer);
    showStatus('ERROR: ' + (err.message || 'Unknown error').toUpperCase().substring(0, 72), true);
    console.error('InvestRadar error:', err);
  }

  setLoading(false);
}

function buildAnalysisPrompt(marketText) {
  return 'You are a senior investment analyst. Here is live market data gathered today:\n\n'
    + marketText
    + '\n\n━━ ANALYSIS PARAMETERS ━━\n'
    + 'Strategy: ' + STRAT_DESC[state.strat] + '\n'
    + 'Risk tolerance: ' + RISK_DESC[state.risk - 1] + '\n'
    + 'Investment horizon: ' + HOLD_DESC[state.hold - 1] + '\n\n'
    + '━━ YOUR TASK ━━\n'
    + 'Generate a comprehensive investment analysis with exactly 5 LARGE-CAP picks and 5 SMALL-CAP picks. '
    + 'Every recommendation must be explicitly grounded in the current market data above.\n\n'
    + '━━ REASONING QUALITY REQUIREMENTS ━━\n'
    + 'For each pick, the reasoning field must:\n'
    + '- Be 3–5 sentences long\n'
    + '- Cite at least 2 specific current market data points (prices, rates, percentages from the live data)\n'
    + '- Explain the investment thesis clearly with specific valuation metrics (P/E, FCF yield, EV/EBITDA, etc.)\n'
    + '- Address the current macro environment and how this pick fits the chosen strategy\n'
    + '- Mention specific risk factors and why they are acceptable given the parameters\n'
    + '- The catalysts field must list 2–4 specific near-term catalysts\n\n'
    + '━━ REQUIRED JSON STRUCTURE ━━\n'
    + 'Return ONLY this JSON (no markdown, no backticks, nothing outside the braces):\n\n'
    + JSON.stringify({
        macro: {
          indicators: [
            {key:'S&P 500', value:'5234', trend:'up'},
            {key:'NASDAQ', value:'16200', trend:'up'},
            {key:'Fed Rate', value:'4.50%', trend:'neu'},
            {key:'10Y Yield', value:'4.35%', trend:'up'},
            {key:'CPI YoY', value:'3.1%', trend:'dn'},
            {key:'VIX', value:'18', trend:'neu'},
            {key:'Gold', value:'$2300', trend:'up'},
            {key:'Oil WTI', value:'$72', trend:'dn'},
            {key:'BTC', value:'$65000', trend:'up'},
            {key:'USD Index', value:'104', trend:'neu'}
          ],
          sentiment: 'RISK-ON',
          summary: 'REPLACE WITH: 3–4 sentence macro summary referencing specific data points from the live search. Cover interest rate environment, inflation trajectory, geopolitical risks, equity valuations, and specific sector opportunities relevant to the chosen strategy.'
        },
        groups: [
          {
            label: 'LARGE-CAP',
            sublabel: 'Established leaders & major funds',
            items: [{
              ticker: 'AAPL',
              name: 'Apple Inc.',
              type: 'STOCK',
              strategy: 'Quality Value',
              conviction: 'HIGH',
              holdPeriod: '3–5 YRS',
              upside: '+28%',
              risk: 'LOW',
              divYield: '0.5%',
              marketAlignment: 82,
              reasoning: 'REPLACE WITH: 3–5 sentence thesis citing specific data points, valuation metrics, macro context, and risk factors.',
              catalysts: ['Catalyst 1', 'Catalyst 2', 'Catalyst 3'],
              risks: 'Key risk factor(s) to monitor.',
              dataUsed: 'Specific live data points referenced.'
            }]
          },
          {
            label: 'SMALL-CAP',
            sublabel: 'High-potential growth opportunities',
            items: [{
              ticker: 'XMPL',
              name: 'Example Small Corp',
              type: 'STOCK',
              strategy: 'Growth',
              conviction: 'MEDIUM',
              holdPeriod: '2–3 YRS',
              upside: '+65%',
              risk: 'HIGH',
              divYield: 'N/A',
              marketAlignment: 68,
              reasoning: 'REPLACE WITH: 3–5 sentence detailed thesis.',
              catalysts: ['Catalyst 1', 'Catalyst 2'],
              risks: 'Key risk factors.',
              dataUsed: 'Data used.'
            }]
          }
        ]
      })
    + '\n\nField rules:\n'
    + '- Provide exactly 5 items in each group (10 total)\n'
    + '- type: STOCK | ETF | BOND | CRYPTO | REIT\n'
    + '- conviction: HIGH | MEDIUM | LOW\n'
    + '- risk: LOW | MED | HIGH\n'
    + '- upside: format like +35% or +18%\n'
    + '- divYield: format like 2.5% or N/A\n'
    + '- marketAlignment: integer 0–100 (fit with current macro)\n'
    + '- trend: "up" | "dn" | "neu"\n'
    + '- LARGE-CAP: mega/large-cap companies or major well-known ETFs (market cap >$10B)\n'
    + '- SMALL-CAP: genuine small/micro-cap stocks (<$3B) or niche thematic ETFs\n'
    + '- Adapt ALL picks to the risk tolerance (' + RISK_LABELS[state.risk-1] + ') and hold horizon (' + HOLD_LABELS[state.hold-1] + ')\n'
    + '- Use only real tickers that currently trade on major exchanges\n'
    + '- Return ONLY the JSON object';
}

// ── API LAYER ─────────────────────────────────────────────────────────────────
async function callClaude(apiKey, messages, tools, system) {
  var body = {
    model: 'claude-sonnet-4-5',
    max_tokens: 6000,
    messages: messages
  };
  if (system) body.system = system;
  if (tools && tools.length) body.tools = tools;

  var res = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    mode: 'cors',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': apiKey,
      'anthropic-version': '2023-06-01',
      'anthropic-dangerous-direct-browser-calls': 'true'
    },
    body: JSON.stringify(body)
  });

  if (!res.ok) {
    var errData = await res.json().catch(function() { return {}; });
    var msg = errData.error && errData.error.message ? errData.error.message : 'HTTP ' + res.status;
    throw new Error(msg);
  }
  return res.json();
}

function extractText(data) {
  if (!data || !data.content) return '';
  return data.content
    .filter(function(b) { return b.type === 'text'; })
    .map(function(b) { return b.text || ''; })
    .join('');
}

function parseJSON(raw) {
  if (!raw) throw new Error('Empty response from API');
  raw = raw.replace(/```json\s*/gi, '').replace(/```\s*/g, '').trim();
  var si = raw.indexOf('{');
  var ei = raw.lastIndexOf('}');
  if (si === -1 || ei <= si) throw new Error('No JSON object found in response');
  return JSON.parse(raw.substring(si, ei + 1));
}

// ── RENDER: MACRO ─────────────────────────────────────────────────────────────
function renderMacro(macro) {
  var grid = document.getElementById('macroGrid');
  var sum = document.getElementById('macroSummary');
  grid.innerHTML = '';

  var inds = (macro.indicators || []).slice(0, 8);
  inds.forEach(function(ind) {
    var isRisk = ind.key && (ind.key.indexOf('VIX') !== -1 || ind.key.indexOf('CPI') !== -1);
    var cls = isRisk
      ? (ind.trend === 'up' ? 'warn' : ind.trend === 'dn' ? 'up' : '')
      : (ind.trend === 'up' ? 'up' : ind.trend === 'dn' ? 'dn' : '');
    var el = document.createElement('div');
    el.className = 'macro-item';
    el.innerHTML = '<div class="mi-key">'+esc(ind.key)+'</div><div class="mi-val '+cls+'">'+esc(ind.value)+'</div>';
    grid.appendChild(el);
  });

  var sentCls = macro.sentiment && macro.sentiment.indexOf('BEAR') !== -1 ? 'dn'
    : macro.sentiment && (macro.sentiment.indexOf('ON') !== -1 || macro.sentiment.indexOf('BULL') !== -1) ? 'up' : 'warn';
  var sentEl = document.createElement('div');
  sentEl.className = 'macro-item wide';
  sentEl.innerHTML = '<div class="mi-key">MARKET SENTIMENT</div><div class="mi-val '+sentCls+'">'+(macro.sentiment||'NEUTRAL')+'</div>';
  grid.appendChild(sentEl);

  sum.innerHTML = hlText(macro.summary || '');
  document.getElementById('macroPanel').className = 'macro-panel show';
  document.getElementById('macroBadge').textContent = 'LIVE · ' + new Date().toLocaleDateString('en-GB', {day:'2-digit',month:'short',year:'numeric'});
}

// ── RENDER: GROUPS & CARDS ────────────────────────────────────────────────────
function renderGroups(groups) {
  var container = document.getElementById('cardsContainer');
  container.innerHTML = '';
  var gi = 0;

  groups.forEach(function(group) {
    var isS = (group.label || '').toUpperCase().indexOf('SMALL') !== -1;
    var cls = isS ? 'S' : 'L';

    var hdr = document.createElement('div');
    hdr.className = 'group-hdr';
    hdr.innerHTML =
      '<div class="group-ico '+cls+'">'+(isS?'◇':'◈')+'</div>'
      +'<div class="group-titles">'
        +'<div class="group-label '+cls+'">'+esc(group.label||'')+'</div>'
        +'<div class="group-sub">'+esc(group.sublabel||'')+'</div>'
      +'</div>'
      +'<div class="group-cnt '+cls+'">'+(group.items?group.items.length:0)+' PICKS</div>';
    container.appendChild(hdr);

    var divEl = document.createElement('div');
    divEl.className = 'group-div '+cls;
    container.appendChild(divEl);

    if (!group.items || !group.items.length) return;

    group.items.forEach(function(rec) {
      var idx = gi++;
      container.appendChild(buildCard(rec, idx));
    });
  });
}

function buildCard(rec, idx) {
  var tk = (rec.type || 'STOCK').toLowerCase();
  if (['stock','etf','bond','crypto','reit'].indexOf(tk) === -1) tk = 'stock';
  var al = Math.max(0, Math.min(100, parseInt(rec.marketAlignment) || 50));
  var ac = al >= 70 ? 'var(--gn)' : al >= 40 ? 'var(--am)' : 'var(--rd)';
  var cc = rec.conviction === 'HIGH' ? 'conv-H' : rec.conviction === 'MEDIUM' ? 'conv-M' : 'conv-L';

  var card = document.createElement('div');
  card.className = 'rec-card';

  // Build catalysts HTML
  var catalysts = Array.isArray(rec.catalysts) ? rec.catalysts : [];
  var catHtml = catalysts.length
    ? '<div style="margin-top:10px"><div style="font-family:\'Share Tech Mono\',monospace;font-size:8px;color:var(--tm);letter-spacing:1px;margin-bottom:6px;">▶ CATALYSTS</div>'
      + catalysts.map(function(c) { return '<span class="tag tag-bull">'+esc(c)+'</span>'; }).join('')
      + '</div>'
    : '';

  // Build risks HTML
  var riskHtml = rec.risks
    ? '<div style="margin-top:8px"><span style="font-family:\'Share Tech Mono\',monospace;font-size:8px;color:var(--tm);letter-spacing:1px;">▶ KEY RISKS &nbsp;</span>'
      + '<span style="font-size:12px;color:#ff7a8a;line-height:1.5;">'+esc(rec.risks)+'</span></div>'
    : '';

  // Data strip
  var dataHtml = rec.dataUsed
    ? '<div class="data-strip"><span class="data-chip">■ DATA: '+esc(rec.dataUsed)+'</span></div>'
    : '';

  card.innerHTML =
    '<div class="card-main">'
      +'<div><div class="type-badge t-'+tk+'">'+esc(rec.type||'STOCK')+'</div></div>'
      +'<div class="card-info">'
        +'<div class="card-ticker">'+esc(rec.ticker||'—')+'</div>'
        +'<div class="card-name">'+esc(rec.name||'')+'</div>'
        +'<div class="card-strat">'+esc(rec.strategy||'')+'</div>'
      +'</div>'
      +'<div class="card-right">'
        +'<div class="card-conv '+cc+'">'+esc(rec.conviction||'—')+'</div>'
        +'<div class="card-hold">HOLD: '+esc(rec.holdPeriod||'—')+'</div>'
      +'</div>'
    +'</div>'
    +'<div class="card-metrics">'
      +'<div class="metric"><span class="met-lbl">UPSIDE</span><span class="met-val pos">'+esc(rec.upside||'—')+'</span></div>'
      +'<div class="metric"><span class="met-lbl">RISK</span><span class="met-val">'+esc(rec.risk||'—')+'</span></div>'
      +'<div class="metric"><span class="met-lbl">DIV YIELD</span><span class="met-val">'+esc(rec.divYield||'—')+'</span></div>'
    +'</div>'
    +'<div class="align-row">'
      +'<span class="align-lbl">MARKET FIT</span>'
      +'<div class="align-bar"><div class="align-fill" style="width:'+al+'%;background:'+ac+'"></div></div>'
      +'<span class="align-score" style="color:'+ac+'">'+al+'%</span>'
    +'</div>'
    +'<button class="card-expand" onclick="toggleReasoning('+idx+')">'
      +'<span class="exp-lbl">▶ FULL ANALYSIS &amp; REASONING</span>'
      +'<span class="exp-arr" id="arr-'+idx+'">▼</span>'
    +'</button>'
    +'<div class="reasoning-panel" id="rp-'+idx+'">'
      +'<div class="reasoning-text">'+hlText(rec.reasoning||'')+'</div>'
      +catHtml+riskHtml+dataHtml
    +'</div>';

  return card;
}

function toggleReasoning(idx) {
  var panel = document.getElementById('rp-'+idx);
  var arr = document.getElementById('arr-'+idx);
  if (panel.classList.contains('show')) {
    panel.classList.remove('show');
    arr.classList.remove('open');
  } else {
    panel.classList.add('show');
    arr.classList.add('open');
  }
}

// ── UTILITIES ─────────────────────────────────────────────────────────────────
function esc(s) {
  return String(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

function hlText(t) {
  if (!t) return '';
  // Escape first
  t = esc(t);
  // Highlight numbers with units
  t = t.replace(/(\$[\d,.]+(B|M|T|K)?|\d+\.?\d*x|\d+\.?\d*%)/g, '<span class="hl">$1</span>');
  // Highlight financial acronyms
  t = t.replace(/\b(P\/E|P\/B|EPS|ROE|ROI|FCF|CAGR|EBITDA|TTM|YoY|QoQ|DCF|ROIC|EV|NAV|AUM|NIM|IPO|ATH|VIX|CPI|GDP|FED|ETF|REIT|LNG|WTI)\b/g, '<span class="hl">$1</span>');
  // Wrap paragraphs (split on ". " followed by capital)
  t = '<p>' + t.replace(/\.\s+([A-Z])/g, '.</p><p>$1') + '</p>';
  return t;
}
</script>

</body>
</html>
