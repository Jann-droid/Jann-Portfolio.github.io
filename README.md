<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1.0"/>
<title>InvestRadar · AI Investment Intelligence</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@400;600;700;900&family=Rajdhani:wght@300;400;500;600&display=swap');

/* ── RESET ── */
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html{background:#020408}
body{
background:#020408;
color:#e8f4ff;
font-family:‘Rajdhani’,system-ui,sans-serif;
min-height:100vh;
overflow-x:hidden;
}

/* ── TOKENS ── */
:root{
–void:#020408;
–deep:#040b12;
–card:#060f18;
–panel:#091422;
–bd:#0a2030;
–bd2:#0d2840;
–t1:#e8f4ff;
–t2:#7ab3cc;
–tm:#3a6070;
–cy:#00d4ff;
–cyd:rgba(0,212,255,.22);
–cyf:rgba(0,212,255,.06);
–gn:#00ff88;
–gnd:rgba(0,255,136,.18);
–gnf:rgba(0,255,136,.05);
–am:#ffaa00;
–amd:rgba(255,170,0,.2);
–rd:#ff3a5c;
–pu:#9b59ff;
–max:520px;
}

/* ── BACKGROUND ── */
.bg-grid{
position:fixed;inset:0;
background-image:
linear-gradient(var(–bd) 1px,transparent 1px),
linear-gradient(90deg,var(–bd) 1px,transparent 1px);
background-size:40px 40px;
opacity:.4;
pointer-events:none;
z-index:0;
}
.bg-glow1{
position:fixed;top:-200px;left:-200px;
width:500px;height:500px;border-radius:50%;
background:rgba(0,212,255,.05);
filter:blur(120px);pointer-events:none;z-index:0;
}
.bg-glow2{
position:fixed;bottom:-200px;right:-200px;
width:500px;height:500px;border-radius:50%;
background:rgba(155,89,255,.04);
filter:blur(120px);pointer-events:none;z-index:0;
}

/* ── KEYFRAMES ── */
@keyframes ir-pulse{0%,100%{opacity:1}50%{opacity:.2}}
@keyframes ir-scroll{0%{transform:translateX(0)}100%{transform:translateX(-50%)}}
@keyframes ir-fadein{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(0)}}

/* ════════════════════════════════════════
OVERLAY  (shown by default via CSS)
════════════════════════════════════════ */
#overlay{
position:fixed;inset:0;
background:rgba(2,4,8,.97);
backdrop-filter:blur(8px);
z-index:9999;
display:flex;
align-items:center;
justify-content:center;
padding:20px;
}
.ov-box{
background:var(–card);
border:1px solid var(–cyd);
border-radius:8px;
padding:32px 28px;
width:100%;max-width:420px;
position:relative;overflow:hidden;
}
.ov-box::before{
content:’’;
position:absolute;top:0;left:0;right:0;height:2px;
background:linear-gradient(90deg,transparent,var(–cy),transparent);
}
.ov-logo{
font-family:‘Orbitron’,monospace;
font-size:22px;font-weight:900;
color:var(–cy);letter-spacing:4px;
margin-bottom:4px;
}
.ov-logo span{color:var(–t2);font-weight:400}
.ov-sub{
font-family:‘Share Tech Mono’,monospace;
font-size:9px;letter-spacing:3px;
color:var(–tm);margin-bottom:22px;
}
.ov-desc{
font-size:13px;color:var(–t2);
line-height:1.75;margin-bottom:6px;
}
.ov-desc a{color:var(–cy);text-decoration:none}
.ov-desc a:hover{text-decoration:underline}
.ov-hint{
font-family:‘Share Tech Mono’,monospace;
font-size:9px;color:var(–tm);
margin-bottom:16px;line-height:1.6;
}
.key-wrap{position:relative;margin-bottom:6px}
#key-input{
width:100%;
background:var(–deep);
border:1px solid var(–bd2);
border-radius:4px;
color:var(–t1);
font-family:‘Share Tech Mono’,monospace;
font-size:11px;
padding:11px 42px 11px 13px;
outline:none;
transition:border-color .2s;
letter-spacing:.5px;
}
#key-input::placeholder{color:var(–tm)}
#key-input:focus{border-color:var(–cyd)}
#eye-btn{
position:absolute;right:11px;top:50%;transform:translateY(-50%);
background:none;border:none;
color:var(–tm);cursor:pointer;font-size:14px;
transition:color .2s;padding:2px;
}
#eye-btn:hover{color:var(–cy)}
#key-err{
font-family:‘Share Tech Mono’,monospace;
font-size:9px;color:var(–rd);
min-height:16px;margin-bottom:12px;
}
#start-btn{
width:100%;
background:var(–cyf);
border:1px solid var(–cy);
border-radius:4px;
color:var(–cy);
font-family:‘Orbitron’,monospace;
font-size:11px;font-weight:700;
letter-spacing:3px;padding:13px;
cursor:pointer;
transition:background .2s,box-shadow .2s;
}
#start-btn:hover{
background:rgba(0,212,255,.14);
box-shadow:0 0 24px rgba(0,212,255,.1);
}
.ov-note{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;color:var(–tm);
text-align:center;margin-top:14px;line-height:1.7;
}

/* ════════════════════════════════════════
APP  (hidden by default via CSS)
════════════════════════════════════════ */
#app{
display:none;
position:relative;z-index:1;
max-width:var(–max);
margin:0 auto;
padding-bottom:80px;
}

/* ── HEADER ── */
.hdr{
padding:20px 16px 14px;
border-bottom:1px solid var(–bd);
position:relative;
}
.hdr::after{
content:’’;
position:absolute;bottom:0;left:0;right:0;height:1px;
background:linear-gradient(90deg,transparent,var(–cy),transparent);
}
.hdr-row{
display:flex;align-items:center;
justify-content:space-between;
margin-bottom:4px;
}
.logo{
font-family:‘Orbitron’,monospace;
font-size:20px;font-weight:900;
color:var(–cy);letter-spacing:4px;
}
.logo span{color:var(–t2);font-weight:400}
.live{
display:flex;align-items:center;gap:6px;
font-family:‘Share Tech Mono’,monospace;
font-size:9px;color:var(–gn);letter-spacing:2px;
}
.live-dot{
width:7px;height:7px;border-radius:50%;
background:var(–gn);
animation:ir-pulse 2s ease-in-out infinite;
}
.hdr-sub{
font-family:‘Share Tech Mono’,monospace;
font-size:9px;color:var(–tm);letter-spacing:2px;
}

/* ── TICKER ── */
.ticker-wrap{
border-bottom:1px solid var(–bd);
padding:8px 0;overflow:hidden;position:relative;
}
.ticker-fade{
position:absolute;right:0;top:0;bottom:0;width:50px;
background:linear-gradient(90deg,transparent,var(–void));
pointer-events:none;z-index:2;
}
.ticker-inner{
display:flex;gap:0;
animation:ir-scroll 32s linear infinite;
width:max-content;
}
.tick-item{
display:flex;align-items:center;gap:7px;
padding:0 18px;
border-right:1px solid var(–bd);
}
.tick-k{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;color:var(–tm);letter-spacing:1px;
}
.tick-v{
font-family:‘Share Tech Mono’,monospace;
font-size:9px;font-weight:700;
}

/* ── MARKET PANEL ── */
.mkt-panel{
margin:16px 16px 0;
background:var(–card);
border:1px solid var(–bd);
border-radius:6px;overflow:hidden;
}
.mkt-head{
display:flex;align-items:center;justify-content:space-between;
padding:9px 14px;border-bottom:1px solid var(–bd);
}
.mkt-title{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;letter-spacing:3px;color:var(–cy);
}
.mkt-badge{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;letter-spacing:1px;color:var(–gn);
background:var(–gnf);border:1px solid var(–gnd);
border-radius:3px;padding:2px 8px;
}
.mkt-grid{
display:grid;grid-template-columns:1fr 1fr;
gap:1px;background:var(–bd);
}
.mkt-cell{
background:var(–deep);padding:8px 12px;
}
.mkt-cell-k{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;color:var(–tm);letter-spacing:1px;margin-bottom:3px;
}
.mkt-cell-v{
font-family:‘Share Tech Mono’,monospace;
font-size:13px;font-weight:700;
}
.mkt-sentiment{
padding:9px 14px;border-top:1px solid var(–bd);
}
.mkt-sent-k{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;color:var(–tm);letter-spacing:2px;margin-bottom:2px;
}
.mkt-sent-v{
font-family:‘Share Tech Mono’,monospace;
font-size:11px;font-weight:700;color:var(–am);
}
.mkt-summary{
padding:10px 14px 14px;
font-size:12px;color:var(–t2);
line-height:1.75;border-top:1px solid var(–bd);
}

/* ── SECTION LABEL ── */
.sec-label{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;letter-spacing:3px;color:var(–tm);
padding:18px 16px 10px;
}

/* ── STRATEGY GRID ── */
.strat-grid{
display:grid;grid-template-columns:1fr 1fr;
gap:8px;padding:0 16px;
}
.strat-btn{
background:var(–card);
border:1px solid var(–bd);
border-radius:4px;padding:10px 12px;
cursor:pointer;text-align:left;
position:relative;overflow:hidden;
transition:border-color .18s,background .18s;
}
.strat-btn:hover{border-color:var(–bd2);background:var(–panel)}
.strat-btn.on{border-color:var(–cy);background:var(–cyf)}
.strat-bar{
position:absolute;top:0;left:0;
width:3px;height:100%;
background:var(–cy);display:none;
}
.strat-btn.on .strat-bar{display:block}
.strat-name{
font-family:‘Share Tech Mono’,monospace;
font-size:10px;font-weight:700;letter-spacing:1px;
color:var(–t2);margin-bottom:3px;
transition:color .18s;
}
.strat-btn.on .strat-name{color:var(–cy)}
.strat-desc{font-size:11px;color:var(–tm);line-height:1.4}

/* ── SLIDERS ── */
.sliders{padding:13px 16px 0}
.slider-row{
display:flex;align-items:center;gap:12px;margin-bottom:11px;
}
.slider-k{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;letter-spacing:2px;color:var(–tm);
width:40px;flex-shrink:0;
}
input[type=range]{
-webkit-appearance:none;appearance:none;
flex:1;height:2px;border-radius:2px;outline:none;cursor:pointer;
}
input[type=range]::-webkit-slider-thumb{
-webkit-appearance:none;
width:14px;height:14px;border-radius:50%;
background:var(–void);border:2px solid var(–cy);cursor:pointer;
}
input[type=range]::-moz-range-thumb{
width:14px;height:14px;border-radius:50%;
background:var(–void);border:2px solid var(–cy);cursor:pointer;
border-style:solid;
}
.slider-v{
font-family:‘Share Tech Mono’,monospace;
font-size:9px;color:var(–cy);
width:78px;text-align:right;flex-shrink:0;
}

/* ── ANALYZE BUTTON ── */
.analyze-wrap{padding:14px 16px 0}
#analyze-btn{
width:100%;
background:transparent;
border:1px solid var(–cy);
border-radius:4px;color:var(–cy);
font-family:‘Orbitron’,monospace;
font-size:11px;font-weight:700;letter-spacing:3px;
padding:14px;cursor:pointer;
transition:background .2s,box-shadow .2s;
}
#analyze-btn:hover{
background:var(–cyf);
box-shadow:0 0 28px rgba(0,212,255,.1);
}
#analyze-btn:disabled{opacity:.45;cursor:not-allowed}

/* ── LOADING ── */
#loading{
display:none;
padding:32px 16px;
text-align:center;
}
#loading.show{display:block}
.load-icon{
font-family:‘Share Tech Mono’,monospace;
font-size:9px;letter-spacing:3px;color:var(–cy);
margin-bottom:8px;
}
.load-txt{
font-family:‘Share Tech Mono’,monospace;
font-size:9px;letter-spacing:2px;color:var(–tm);
}

/* ── RESULTS ── */
#results{padding:14px 16px 0}
.results-bar{
display:flex;align-items:center;justify-content:space-between;
margin-bottom:14px;padding-bottom:10px;
border-bottom:1px solid var(–bd);
}
.results-title{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;letter-spacing:3px;color:var(–tm);
}
.pills{display:flex;gap:5px;align-items:center;flex-wrap:wrap}
.pill{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;font-weight:700;letter-spacing:1px;
padding:2px 8px;border-radius:3px;
}
.sig-count{
font-family:‘Share Tech Mono’,monospace;
font-size:9px;color:var(–gn);margin-left:3px;
}

/* ── GROUP ── */
.group-head{
display:flex;align-items:center;gap:10px;
margin:20px 0 8px;
}
.group-icon{
width:30px;height:30px;border-radius:4px;
display:flex;align-items:center;justify-content:center;
font-size:14px;flex-shrink:0;
}
.group-name{
font-family:‘Orbitron’,monospace;
font-size:10px;font-weight:700;letter-spacing:3px;
}
.group-sub{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;color:var(–tm);letter-spacing:1px;margin-top:2px;
}
.group-count{
margin-left:auto;flex-shrink:0;
font-family:‘Share Tech Mono’,monospace;
font-size:8px;padding:2px 8px;border-radius:3px;
}
.group-line{height:1px;margin-bottom:10px}

/* ── INVESTMENT CARD ── */
.inv-card{
background:var(–card);
border:1px solid var(–bd);
border-radius:6px;
margin-bottom:10px;overflow:hidden;
transition:border-color .2s;
}
.inv-card:hover{border-color:var(–bd2)}
.card-top{
padding:14px;
display:grid;
grid-template-columns:auto 1fr auto;
gap:0 12px;align-items:start;
}
.type-badge{
font-family:‘Share Tech Mono’,monospace;
font-size:7px;letter-spacing:2px;
padding:3px 7px;border-radius:3px;margin-top:2px;white-space:nowrap;
}
.ticker{
font-family:‘Orbitron’,monospace;
font-size:17px;font-weight:700;
color:var(–t1);letter-spacing:1px;
line-height:1;margin-bottom:3px;
}
.full-name{
font-size:12px;color:var(–t2);
white-space:nowrap;overflow:hidden;text-overflow:ellipsis;
margin-bottom:4px;
}
.strat-tag{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;color:var(–tm);letter-spacing:1px;
}
.conv{
font-family:‘Share Tech Mono’,monospace;
font-size:10px;font-weight:700;letter-spacing:1px;
text-align:right;margin-bottom:5px;
}
.hold-tag{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;color:var(–tm);white-space:nowrap;text-align:right;
}
.metrics{
display:grid;grid-template-columns:repeat(3,1fr);
border-top:1px solid var(–bd);
}
.metric{
padding:7px 10px;text-align:center;
border-right:1px solid var(–bd);
}
.metric:last-child{border-right:none}
.metric-k{
font-family:‘Share Tech Mono’,monospace;
font-size:7px;letter-spacing:1px;color:var(–tm);
margin-bottom:2px;
}
.metric-v{
font-family:‘Share Tech Mono’,monospace;
font-size:12px;font-weight:700;
}
.fit-row{
display:flex;align-items:center;gap:9px;
padding:7px 14px;border-top:1px solid var(–bd);
}
.fit-lbl{
font-family:‘Share Tech Mono’,monospace;
font-size:7px;letter-spacing:1px;color:var(–tm);
width:56px;flex-shrink:0;
}
.fit-track{
flex:1;height:3px;background:var(–bd);
border-radius:2px;overflow:hidden;
}
.fit-fill{height:100%;border-radius:2px}
.fit-pct{
font-family:‘Share Tech Mono’,monospace;
font-size:9px;font-weight:700;
width:30px;text-align:right;flex-shrink:0;
}
.expand-btn{
width:100%;background:transparent;border:none;
border-top:1px solid var(–bd);
padding:8px 14px;
display:flex;align-items:center;justify-content:space-between;
cursor:pointer;color:var(–cy);
transition:background .15s;
}
.expand-btn:hover{background:var(–cyf)}
.expand-lbl{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;letter-spacing:2px;
}
.expand-arr{font-size:9px;transition:transform .2s}
.expand-arr.open{transform:rotate(180deg)}
.reason-panel{
display:none;
border-top:1px solid var(–bd);
background:var(–deep);
padding:16px 16px 18px;
}
.reason-panel.open{
display:block;
animation:ir-fadein .2s ease;
}
.why-txt{font-size:12px;color:var(–t2);line-height:1.8;margin-bottom:14px}
.cats-lbl{
font-family:‘Share Tech Mono’,monospace;
font-size:7px;letter-spacing:2px;color:var(–tm);margin-bottom:7px;
}
.cats-wrap{display:flex;flex-wrap:wrap;gap:5px;margin-bottom:12px}
.cat-tag{
font-family:‘Share Tech Mono’,monospace;
font-size:8px;padding:3px 9px;border-radius:3px;
background:var(–gnf);border:1px solid var(–gnd);color:var(–gn);
}
.risks-row{
display:flex;gap:8px;align-items:flex-start;
padding-top:10px;border-top:1px solid var(–bd);
}
.risks-lbl{
font-family:‘Share Tech Mono’,monospace;
font-size:7px;letter-spacing:2px;color:var(–tm);
flex-shrink:0;padding-top:2px;
}
.risks-txt{font-size:11px;color:#ff7a8a;line-height:1.6}
.hi{color:var(–cy);font-weight:700}

/* ── ERROR ── */
.err-box{
margin:0 0 16px;padding:14px 16px;
background:rgba(255,58,92,.06);
border:1px solid rgba(255,58,92,.3);
border-radius:4px;
font-family:‘Share Tech Mono’,monospace;
font-size:9px;color:var(–rd);line-height:1.7;
}

/* ── DISCLAIMER ── */
.disclaimer{
margin:20px 16px 0;padding:10px 14px;
border:1px solid var(–bd);border-radius:4px;
font-family:‘Share Tech Mono’,monospace;
font-size:8px;color:var(–tm);line-height:1.7;
}

/* ── CHANGE KEY BTN ── */
#change-btn{
position:fixed;bottom:16px;right:16px;
background:var(–panel);border:1px solid var(–bd);
border-radius:4px;color:var(–tm);
font-family:‘Share Tech Mono’,monospace;
font-size:8px;letter-spacing:1px;
padding:8px 12px;cursor:pointer;z-index:100;
transition:border-color .2s,color .2s;
}
#change-btn:hover{border-color:var(–cyd);color:var(–cy)}
</style>

</head>
<body>

<div class="bg-grid"></div>
<div class="bg-glow1"></div>
<div class="bg-glow2"></div>

<!-- ══════════════════════════════════════
     OVERLAY  — always visible on load
══════════════════════════════════════ -->

<div id="overlay">
  <div class="ov-box">
    <div class="ov-logo">INVEST<span>RADAR</span></div>
    <div class="ov-sub">// AI INVESTMENT INTELLIGENCE</div>

```
<div class="ov-desc">
  Enter your <strong style="color:var(--cy)">Anthropic API Key</strong> to start.<br>
  Stored only in your browser — never sent anywhere else.
</div>
<div class="ov-hint">
  Get a key: <a href="https://console.anthropic.com" target="_blank" rel="noopener">console.anthropic.com</a>
  → API Keys → Create Key<br>
  (A small credit balance is needed; ~€5 covers 100+ analyses)
</div>

<div class="key-wrap">
  <input id="key-input" type="password" placeholder="sk-ant-api03-..." autocomplete="off" spellcheck="false"/>
  <button id="eye-btn" type="button" onclick="toggleEye()">◉</button>
</div>
<div id="key-err"></div>

<button id="start-btn" type="button" onclick="submitKey()">▶ START</button>

<div class="ov-note">
  Your key is saved so you won't need to re-enter it.<br>
  Use the ⚙ button (bottom-right) to change it at any time.
</div>
```

  </div>
</div>

<!-- ══════════════════════════════════════
     MAIN APP  — hidden until key entered
══════════════════════════════════════ -->

<div id="app">

  <!-- Header -->

  <div class="hdr">
    <div class="hdr-row">
      <div class="logo">INVEST<span>RADAR</span></div>
      <div class="live">
        <div class="live-dot"></div>
        LIVE · MAY 2026
      </div>
    </div>
    <div class="hdr-sub">// Claude-powered · AI investment analysis · BYOK</div>
  </div>

  <!-- Ticker -->

  <div class="ticker-wrap">
    <div class="ticker-fade"></div>
    <div class="ticker-inner" id="ticker-inner"></div>
  </div>

  <!-- Market panel -->

  <div class="mkt-panel">
    <div class="mkt-head">
      <div class="mkt-title">// MARKET SNAPSHOT · MAY 5 2026</div>
      <div class="mkt-badge">LIVE DATA</div>
    </div>
    <div class="mkt-grid" id="mkt-grid"></div>
    <div class="mkt-sentiment">
      <div class="mkt-sent-k">MARKET SENTIMENT</div>
      <div class="mkt-sent-v">RISK-OFF / TARIFF UNCERTAINTY</div>
    </div>
    <div class="mkt-summary">
      S&amp;P 500 down <span class="hi">~17%</span> from February peak on US tariff shock
      (<span class="hi">145%</span> on China, 90-day pause on others).
      Fed holds at <span class="hi">4.25–4.50%</span> — two cuts priced for late 2026.
      CPI cooled to <span class="hi">2.4%</span> YoY.
      Gold at <span class="hi">$3,271</span> near all-time high on safe-haven demand.
      WTI collapsed to <span class="hi">$56.42</span> on demand fears.
      BTC at <span class="hi">$94,200</span> outperforms as macro hedge. USD weakened to <span class="hi">99.4</span>.
    </div>
  </div>

  <!-- Strategy -->

  <div class="sec-label">// SELECT STRATEGY</div>
  <div class="strat-grid" id="strat-grid"></div>

  <!-- Sliders -->

  <div class="sliders">
    <div class="slider-row">
      <span class="slider-k">RISK</span>
      <input type="range" id="risk-sl" min="1" max="5" value="3"
        style="background:linear-gradient(90deg,#00ff88,#ffaa00,#ff3a5c)"/>
      <span class="slider-v" id="risk-lbl">BALANCED</span>
    </div>
    <div class="slider-row">
      <span class="slider-k">HOLD</span>
      <input type="range" id="hold-sl" min="1" max="5" value="3"
        style="background:linear-gradient(90deg,#00d4ff,#9b59ff)"/>
      <span class="slider-v" id="hold-lbl">1–3 YRS</span>
    </div>
  </div>

  <!-- Analyze button -->

  <div class="analyze-wrap">
    <button id="analyze-btn" type="button" onclick="runAnalysis()">▶ ANALYSE STARTEN</button>
  </div>

  <!-- Loading -->

  <div id="loading">
    <div class="load-icon">■ ANALYSING MARKETS</div>
    <div class="load-txt" id="load-txt">Claude is thinking...</div>
  </div>

  <!-- Results -->

  <div id="results"></div>

  <div class="disclaimer">
    ⚠ AI-generated analysis · Educational purposes only · Not financial advice ·
    Always consult a licensed financial advisor before investing
  </div>
</div>

<button id="change-btn" type="button" onclick="openOverlay()">⚙ API KEY</button>

<script>
'use strict';

/* ─── DATA ───────────────────────────────── */
var RISK_LABELS = ['VERY LOW','LOW','BALANCED','HIGH','VERY HIGH'];
var HOLD_LABELS = ['< 3 MON','3–12 MON','1–3 YRS','3–7 YRS','7+ YRS'];

var STRATEGIES = [
  {k:'value',    l:'VALUE',    d:'Undervalued, margin of safety'},
  {k:'growth',   l:'GROWTH',  d:'High-growth, future earnings'},
  {k:'dividend', l:'DIVIDEND',d:'Income-generating, stable yield'},
  {k:'momentum', l:'MOMENTUM',d:'Trend-following, breakouts'},
  {k:'index',    l:'INDEX/ETF',d:'Passive, diversified, low-cost'},
  {k:'balanced', l:'BALANCED',d:'All-weather, mixed strategy'},
];

var TICKERS = [
  {k:'S&P 500',   v:'5,631',      t:'dn'},
  {k:'NASDAQ',    v:'17,876',     t:'dn'},
  {k:'Fed Rate',  v:'4.25–4.50%', t:'neu'},
  {k:'10Y Yield', v:'4.37%',      t:'up'},
  {k:'CPI YoY',   v:'2.4%',       t:'dn'},
  {k:'VIX',       v:'22.3',       t:'up'},
  {k:'Gold',      v:'$3,271',     t:'up'},
  {k:'WTI Oil',   v:'$56.42',     t:'dn'},
  {k:'BTC',       v:'$94,200',    t:'up'},
  {k:'USD Index', v:'99.4',       t:'dn'},
];

var MKT_CTX = [
  'CURRENT MARKET DATA (May 5, 2026):',
  '- S&P 500: 5,631 (down ~17% from February all-time high)',
  '- NASDAQ: 17,876 (down ~18% from peak)',
  '- Fed Funds Rate: 4.25-4.50% (on hold; two cuts priced for late 2026)',
  '- 10Y Treasury Yield: 4.37%',
  '- CPI YoY: 2.4% (cooling toward 2% target)',
  '- VIX: 22.3 (elevated; risk-off environment)',
  '- Gold: $3,271/oz (near all-time high; strong safe-haven demand)',
  '- WTI Crude Oil: $56.42/bbl (collapsed on tariff demand fears + OPEC+ supply increase)',
  '- Bitcoin: $94,200 (outperforming; emerging as macro hedge)',
  '- USD Index (DXY): 99.4 (weakening on tariff retaliation concerns)',
  '',
  'MACRO CONTEXT:',
  'US tariff shock: 145% on China, 90-day pause on most other countries.',
  'S&P 500 fell 17% from its February 2026 all-time high.',
  'Fed on hold — rates at 4.25-4.50%; first cut expected late 2026.',
  'CPI cooling but tariff cost-push inflation remains a risk.',
  'Gold near ATH, BTC resilient, oil collapsed, USD weakening.',
  'Earnings season: financials beating, consumer discretionary missing.',
  'Investors rotating to defensives, gold, and bonds. Sentiment: RISK-OFF.',
].join('\n');

/* ─── STATE ──────────────────────────────── */
var apiKey    = '';
var activeStr = 'value';

/* ─── BOOT ───────────────────────────────── */
document.addEventListener('DOMContentLoaded', function() {
  // Pre-fill saved key (but overlay stays visible)
  var saved = '';
  try { saved = localStorage.getItem('ir_api_key') || ''; } catch(e){}
  if (saved) document.getElementById('key-input').value = saved;

  // Enter key submits
  document.getElementById('key-input').addEventListener('keydown', function(e){
    if (e.key === 'Enter') submitKey();
  });

  // Slider labels
  document.getElementById('risk-sl').addEventListener('input', function(){
    document.getElementById('risk-lbl').textContent = RISK_LABELS[this.value - 1];
  });
  document.getElementById('hold-sl').addEventListener('input', function(){
    document.getElementById('hold-lbl').textContent = HOLD_LABELS[this.value - 1];
  });

  buildTicker();
  buildMktGrid();
  buildStratGrid();
});

/* ─── OVERLAY ────────────────────────────── */
function openOverlay() {
  document.getElementById('overlay').style.display = 'flex';
  document.getElementById('app').style.display     = 'none';
  document.getElementById('key-err').textContent   = '';
}

function closeOverlay() {
  document.getElementById('overlay').style.display = 'none';
  document.getElementById('app').style.display     = 'block';
}

function toggleEye() {
  var inp = document.getElementById('key-input');
  inp.type = inp.type === 'password' ? 'text' : 'password';
}

function submitKey() {
  var val = (document.getElementById('key-input').value || '').trim();
  var err = document.getElementById('key-err');
  if (!val) { err.textContent = '⚠ Please enter your API key.'; return; }
  if (val.indexOf('sk-ant-') !== 0) { err.textContent = '⚠ Invalid key — must start with sk-ant-'; return; }
  err.textContent = '';
  apiKey = val;
  try { localStorage.setItem('ir_api_key', val); } catch(e){}
  closeOverlay();
}

/* ─── UI BUILDERS ────────────────────────── */
function tickColor(t) {
  return t === 'up' ? '#00ff88' : t === 'dn' ? '#ff3a5c' : '#7ab3cc';
}

function buildTicker() {
  var doubled = TICKERS.concat(TICKERS);
  var html = doubled.map(function(t) {
    return '<div class="tick-item">'
      + '<span class="tick-k">' + t.k + '</span>'
      + '<span class="tick-v" style="color:' + tickColor(t.t) + '">' + t.v + '</span>'
      + '</div>';
  }).join('');
  document.getElementById('ticker-inner').innerHTML = html;
}

function buildMktGrid() {
  var html = TICKERS.map(function(t) {
    var isRisk = t.k === 'VIX' || t.k === 'CPI YoY';
    var col;
    if (isRisk) col = t.t === 'up' ? '#ffaa00' : t.t === 'dn' ? '#00ff88' : '#e8f4ff';
    else        col = tickColor(t.t);
    return '<div class="mkt-cell">'
      + '<div class="mkt-cell-k">' + t.k + '</div>'
      + '<div class="mkt-cell-v" style="color:' + col + '">' + t.v + '</div>'
      + '</div>';
  }).join('');
  document.getElementById('mkt-grid').innerHTML = html;
}

function buildStratGrid() {
  var html = STRATEGIES.map(function(s) {
    var on = s.k === activeStr;
    return '<button class="strat-btn' + (on ? ' on' : '') + '" type="button" onclick="selectStrat(\'' + s.k + '\')">'
      + '<div class="strat-bar"></div>'
      + '<div class="strat-name">' + s.l + '</div>'
      + '<div class="strat-desc">' + s.d + '</div>'
      + '</button>';
  }).join('');
  document.getElementById('strat-grid').innerHTML = html;
}

function selectStrat(k) {
  activeStr = k;
  buildStratGrid();
}

/* ─── ANALYSIS ───────────────────────────── */
function runAnalysis() {
  if (!apiKey || apiKey.indexOf('sk-ant-') !== 0) { openOverlay(); return; }

  var risk     = parseInt(document.getElementById('risk-sl').value, 10);
  var hold     = parseInt(document.getElementById('hold-sl').value, 10);
  var riskLbl  = RISK_LABELS[risk - 1];
  var holdLbl  = HOLD_LABELS[hold - 1];

  // UI state
  document.getElementById('results').innerHTML = '';
  document.getElementById('loading').classList.add('show');
  document.getElementById('analyze-btn').disabled = true;

  var dots = 0;
  var timer = setInterval(function() {
    dots = (dots + 1) % 4;
    document.getElementById('load-txt').textContent = 'Claude is analysing' + '.'.repeat(dots + 1);
  }, 450);

  function done() {
    clearInterval(timer);
    document.getElementById('loading').classList.remove('show');
    document.getElementById('analyze-btn').disabled = false;
  }

  var prompt = 'You are a professional investment analyst.\n\n'
    + MKT_CTX + '\n\n'
    + 'USER PARAMETERS:\n'
    + '- Strategy: ' + activeStr.toUpperCase() + '\n'
    + '- Risk Tolerance: ' + riskLbl + ' (' + risk + '/5)\n'
    + '- Hold Period: ' + holdLbl + '\n\n'
    + 'Generate exactly 10 investment picks — 5 LARGE-CAP and 5 SMALL-CAP — '
    + 'suited to a ' + activeStr.toUpperCase() + ' strategy with '
    + riskLbl + ' risk and ' + holdLbl + ' hold period. '
    + 'Reference specific numbers from the market data in your analysis.\n\n'
    + 'CRITICAL: Respond with ONLY a raw JSON object. '
    + 'No markdown. No code fences. No explanation. Just the JSON.\n\n'
    + 'JSON structure:\n'
    + '{"groups":['
    +   '{"label":"LARGE-CAP","sublabel":"Established leaders & major funds","items":['
    +     '{"ticker":"string","name":"string","type":"STOCK|ETF|BOND|REIT",'
    +      '"strategy":"max 6 words","conviction":"HIGH|MEDIUM|LOW",'
    +      '"hold":"e.g. 2–4 YRS","upside":"+XX%","risk":"LOW|MED|HIGH",'
    +      '"div":"X.X%|N/A","fit":85,'
    +      '"why":"3-4 sentence analysis with specific data from market context.",'
    +      '"cats":["Catalyst 1","Catalyst 2","Catalyst 3"],'
    +      '"risks":"Key risks in one sentence."}'
    +   ']},'
    +   '{"label":"SMALL-CAP","sublabel":"High-potential opportunities","items":[...same 5 items...]}'
    + ']}';

  fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': apiKey,
      'anthropic-version': '2023-06-01',
      'anthropic-dangerous-direct-browser-access': 'true'
    },
    body: JSON.stringify({
      model: 'claude-haiku-4-5',
      max_tokens: 4096,
      messages: [{ role: 'user', content: prompt }]
    })
  })
  .then(function(res) {
    if (!res.ok) {
      return res.json().then(function(body) {
        var msg = body && body.error ? body.error.message : 'HTTP ' + res.status;
        throw new Error(msg + ' (status ' + res.status + ')');
      });
    }
    return res.json();
  })
  .then(function(data) {
    var text = '';
    (data.content || []).forEach(function(b) { if (b.type === 'text') text += b.text; });
    text = text.trim().replace(/^```[\w]*\n?/,'').replace(/\n?```$/,'').trim();
    var parsed = JSON.parse(text);
    if (!parsed || !Array.isArray(parsed.groups)) throw new Error('Unexpected response structure.');
    done();
    renderResults(parsed.groups, riskLbl, holdLbl);
  })
  .catch(function(e) {
    done();
    var msg = e && e.message ? e.message : String(e);
    var extra = '';
    if (msg.indexOf('401') !== -1) {
      extra = ' Please check your API key.';
    } else if (msg.indexOf('429') !== -1) {
      extra = ' Rate limit hit — please wait a moment.';
    } else if (msg.toLowerCase().indexOf('failed to fetch') !== -1
            || msg.toLowerCase().indexOf('load failed') !== -1
            || msg.toLowerCase().indexOf('networkerror') !== -1) {
      extra = ' Make sure you are accessing this page via HTTPS (GitHub Pages), not a local file:// URL.';
    }
    showErr('⚠ ' + msg + extra);
  });
}

/* ─── RENDER ─────────────────────────────── */
function hl(t) {
  if (!t) return '';
  return t
    .replace(/(\$[\d,]+(?:\.\d+)?[BMK]?|\d+(?:\.\d+)?x|\d+(?:\.\d+)?%)/g,
      '<span class="hi">$1</span>')
    .replace(/\b(P\/E|FCF|CAGR|EBITDA|YoY|ATH|VIX|CPI|NIM|PEG|ROE|AUM|WTI|LNG|ETF|REIT|GAAP|AI)\b/g,
      '<span class="hi">$1</span>');
}

function mkPill(txt, col) {
  return '<span class="pill" style="background:' + col + '18;border:1px solid ' + col + '33;color:' + col + '">' + txt + '</span>';
}

function showErr(msg) {
  document.getElementById('results').innerHTML = '<div class="err-box">' + msg + '</div>';
}

function renderResults(groups, riskLbl, holdLbl) {
  var total = 0;
  (groups || []).forEach(function(g){ total += (g.items || []).length; });

  var html = '<div class="results-bar">'
    + '<div class="results-title">// RECOMMENDATIONS</div>'
    + '<div class="pills">'
    + mkPill(activeStr.toUpperCase(), '#00d4ff')
    + mkPill(riskLbl, '#ffaa00')
    + mkPill(holdLbl, '#9b59ff')
    + '<span class="sig-count">' + total + ' PICKS</span>'
    + '</div></div>';

  (groups || []).forEach(function(g) {
    if (!g.items || !g.items.length) return;
    var sm  = (g.label || '').indexOf('SMALL') !== -1;
    var col = sm ? '#00ff88' : '#00d4ff';
    var dim = sm ? 'rgba(0,255,136,.18)' : 'rgba(0,212,255,.22)';
    var fnt = sm ? 'rgba(0,255,136,.05)' : 'rgba(0,212,255,.06)';
    var ico = sm ? '◇' : '◈';

    html += '<div class="group-head">'
      + '<div class="group-icon" style="background:' + fnt + ';border:1px solid ' + dim + ';color:' + col + '">' + ico + '</div>'
      + '<div><div class="group-name" style="color:' + col + '">' + (g.label||'') + '</div>'
      + '<div class="group-sub">' + (g.sublabel||'') + '</div></div>'
      + '<div class="group-count" style="background:' + fnt + ';border:1px solid ' + dim + ';color:' + col + '">'
      + (g.items.length) + ' picks</div>'
      + '</div>'
      + '<div class="group-line" style="background:linear-gradient(90deg,' + dim + ',transparent)"></div>';

    g.items.forEach(function(r, idx) {
      var uid = 'r' + Date.now() + idx;
      var tk  = (r.type || 'STOCK').toLowerCase();
      var TS  = {
        stock: {c:'#00d4ff',b:'rgba(0,212,255,.22)',bg:'rgba(0,212,255,.06)'},
        etf:   {c:'#00ff88',b:'rgba(0,255,136,.18)',bg:'rgba(0,255,136,.05)'},
        bond:  {c:'#ffaa00',b:'rgba(255,170,0,.2)', bg:'rgba(255,170,0,.05)'},
        reit:  {c:'#fb923c',b:'rgba(251,146,60,.2)',bg:'rgba(251,146,60,.05)'},
      };
      var ts  = TS[tk] || TS.stock;
      var fit = Math.max(0, Math.min(100, parseInt(r.fit,10) || 50));
      var fc  = fit >= 75 ? '#00ff88' : fit >= 50 ? '#ffaa00' : '#ff3a5c';
      var cc  = r.conviction === 'HIGH' ? '#00ff88' : r.conviction === 'MEDIUM' ? '#ffaa00' : '#7ab3cc';

      var cats = '';
      if (r.cats && r.cats.length) {
        cats = '<div class="cats-lbl">▶ KEY CATALYSTS</div><div class="cats-wrap">'
          + r.cats.map(function(c){ return '<span class="cat-tag">'+c+'</span>'; }).join('')
          + '</div>';
      }
      var risks = r.risks
        ? '<div class="risks-row"><span class="risks-lbl">▶ RISKS</span><span class="risks-txt">'+r.risks+'</span></div>'
        : '';

      html +=
        '<div class="inv-card">'
        + '<div class="card-top">'
        +   '<span class="type-badge" style="border:1px solid '+ts.b+';background:'+ts.bg+';color:'+ts.c+'">'+(r.type||'STOCK')+'</span>'
        +   '<div style="min-width:0">'
        +     '<div class="ticker">'+(r.ticker||'')+'</div>'
        +     '<div class="full-name">'+(r.name||'')+'</div>'
        +     '<div class="strat-tag">'+(r.strategy||'')+'</div>'
        +   '</div>'
        +   '<div>'
        +     '<div class="conv" style="color:'+cc+'">'+(r.conviction||'')+'</div>'
        +     '<div class="hold-tag">HOLD: '+(r.hold||'')+'</div>'
        +   '</div>'
        + '</div>'
        + '<div class="metrics">'
        +   '<div class="metric"><div class="metric-k">UPSIDE</div><div class="metric-v" style="color:#00ff88">'+(r.upside||'—')+'</div></div>'
        +   '<div class="metric"><div class="metric-k">RISK</div><div class="metric-v" style="color:var(--t1)">'+(r.risk||'—')+'</div></div>'
        +   '<div class="metric"><div class="metric-k">YIELD</div><div class="metric-v" style="color:var(--t1)">'+(r.div||'—')+'</div></div>'
        + '</div>'
        + '<div class="fit-row">'
        +   '<div class="fit-lbl">MARKET FIT</div>'
        +   '<div class="fit-track"><div class="fit-fill" style="width:'+fit+'%;background:'+fc+'"></div></div>'
        +   '<div class="fit-pct" style="color:'+fc+'">'+fit+'%</div>'
        + '</div>'
        + '<button class="expand-btn" type="button" onclick="toggleCard(\''+uid+'\')">'
        +   '<span class="expand-lbl">▶ FULL ANALYSIS &amp; REASONING</span>'
        +   '<span class="expand-arr" id="arr-'+uid+'">▼</span>'
        + '</button>'
        + '<div class="reason-panel" id="'+uid+'">'
        +   '<div class="why-txt">'+hl(r.why||'')+'</div>'
        +   cats + risks
        + '</div>'
        + '</div>';
    });
  });

  document.getElementById('results').innerHTML = html;
  // smooth scroll to results
  document.getElementById('results').scrollIntoView({behavior:'smooth', block:'start'});
}

function toggleCard(uid) {
  var panel = document.getElementById(uid);
  var arrow = document.getElementById('arr-' + uid);
  if (!panel || !arrow) return;
  var open = panel.classList.toggle('open');
  arrow.classList.toggle('open', open);
}
</script>

</body>
</html>
