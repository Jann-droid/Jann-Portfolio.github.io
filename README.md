<!DOCTYPE html>

<html lang="de">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>InvestRadar — AI Investment Intelligence</title>
<style>
html,body{background:#020408!important;margin:0;padding:0;font-family:system-ui,-apple-system,'Segoe UI',sans-serif;color:#e8f4ff;min-height:100vh}
*,*::before,*::after{box-sizing:border-box}
@import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@700;900&display=swap');
:root{
  --void:#020408;--deep:#040b12;--card:#060f18;--panel:#091422;
  --cy:#00d4ff;--cyd:rgba(0,212,255,.22);--cyf:rgba(0,212,255,.06);
  --gn:#00ff88;--gnd:rgba(0,255,136,.18);--gnf:rgba(0,255,136,.05);
  --am:#ffaa00;--rd:#ff3a5c;--pu:#9b59ff;
  --t1:#e8f4ff;--t2:#7ab3cc;--tm:#3a6070;--bd:#0a2030;
}
.grid{display:grid}
.bg-card{background:var(--card);border:1px solid var(--bd);border-radius:6px}
input[type=range]{-webkit-appearance:none;appearance:none;height:2px;border-radius:2px;outline:none;cursor:pointer;flex:1}
input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;width:15px;height:15px;border-radius:50%;background:#020408;border:2px solid var(--cy);cursor:pointer}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.2}}
@keyframes scroll{0%{transform:translateX(0)}100%{transform:translateX(-50%)}}
@keyframes fadein{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:translateY(0)}}
.fade{animation:fadein .35s ease forwards}
#app{position:relative;z-index:1;max-width:520px;margin:0 auto;padding-bottom:80px}
.bg-grid{position:fixed;inset:0;background-image:linear-gradient(var(--bd) 1px,transparent 1px),linear-gradient(90deg,var(--bd) 1px,transparent 1px);background-size:40px 40px;opacity:.4;pointer-events:none}
.glow1{position:fixed;top:-180px;left:-180px;width:450px;height:450px;border-radius:50%;background:rgba(0,212,255,.05);filter:blur(120px);pointer-events:none}
.glow2{position:fixed;bottom:-180px;right:-180px;width:450px;height:450px;border-radius:50%;background:rgba(155,89,255,.04);filter:blur(120px);pointer-events:none}
.mono{font-family:'Share Tech Mono',monospace}
.orb{font-family:'Orbitron',monospace}
/* Key setup overlay */
#keyOverlay{position:fixed;inset:0;background:rgba(2,4,8,.97);z-index:999;display:flex;align-items:center;justify-content:center;padding:20px}
#keyBox{background:var(--card);border:1px solid var(--cyd);border-radius:8px;padding:28px;max-width:440px;width:100%}
.key-input-wrap{position:relative;margin:14px 0}
#keyInput{width:100%;background:var(--deep);border:1px solid var(--bd);border-radius:4px;color:var(--t1);font-family:monospace;font-size:12px;padding:10px 40px 10px 12px;outline:none}
#keyInput:focus{border-color:var(--cyd)}
#keyToggle{position:absolute;right:10px;top:50%;transform:translateY(-50%);background:none;border:none;color:var(--tm);cursor:pointer;font-size:14px}
#keySubmit{width:100%;background:transparent;border:1px solid var(--cy);border-radius:4px;color:var(--cy);font-family:'Orbitron',monospace;font-size:11px;font-weight:700;letter-spacing:3px;padding:12px;cursor:pointer}
#keySubmit:hover{background:var(--cyf)}
/* Ticker */
#ticker{border-bottom:1px solid var(--bd);padding:8px 0;overflow:hidden;position:relative}
#tickerInner{display:flex;gap:16px;animation:scroll 28s linear infinite;width:max-content}
#tickerFade{position:absolute;right:0;top:0;bottom:0;width:40px;background:linear-gradient(90deg,transparent,var(--void));z-index:1;pointer-events:none}
/* Strategy buttons */
#stratGrid{display:grid;grid-template-columns:1fr 1fr;gap:8px;padding:0 16px}
.strat-btn{background:var(--card);border:1px solid var(--bd);border-radius:4px;padding:10px 12px;cursor:pointer;text-align:left;position:relative;overflow:hidden;transition:border-color .2s,background .2s}
.strat-btn.active{background:var(--panel);border-color:var(--cyd)}
.strat-btn .bar{position:absolute;top:0;left:0;width:3px;height:100%;background:var(--cy);display:none}
.strat-btn.active .bar{display:block}
/* Sliders */
#sliders{padding:12px 16px 0}
.slider-row{display:flex;align-items:center;gap:12px;margin-bottom:10px}
/* Cards */
.card{background:var(--card);border:1px solid var(--bd);border-radius:6px;margin-bottom:10px;overflow:hidden}
.card-top{padding:14px;display:grid;grid-template-columns:auto 1fr auto;gap:0 12px;align-items:start}
.card-metrics{display:grid;grid-template-columns:repeat(3,1fr);border-top:1px solid var(--bd)}
.metric{padding:7px 10px;border-right:1px solid var(--bd);text-align:center}
.fit-bar{border-top:1px solid var(--bd);padding:7px 14px;display:flex;align-items:center;gap:8px}
.expand-btn{width:100%;background:transparent;border:none;border-top:1px solid var(--bd);padding:8px 14px;display:flex;align-items:center;justify-content:space-between;cursor:pointer;color:var(--cy)}
.reasoning{border-top:1px solid var(--bd);background:var(--deep);padding:16px;display:none}
.reasoning.open{display:block}
.cat-tag{font-family:monospace;font-size:9px;padding:3px 8px;border-radius:2px;border:1px solid var(--gnd);background:var(--gnf);color:var(--gn)}
/* Loading */
#loading{padding:24px;text-align:center;font-family:monospace;font-size:11px;color:var(--tm);letter-spacing:2px;display:none}
#loading.show{display:block}
/* Key change button */
#changeKeyBtn{position:fixed;bottom:20px;right:20px;background:var(--panel);border:1px solid var(--bd);border-radius:4px;color:var(--tm);font-family:monospace;font-size:9px;letter-spacing:1px;padding:8px 12px;cursor:pointer;z-index:100}
#changeKeyBtn:hover{border-color:var(--cyd);color:var(--cy)}
</style>
</head>
<body>
<div class="bg-grid"></div>
<div class="glow1"></div>
<div class="glow2"></div>

<!-- API Key Overlay -->

<div id="keyOverlay">
  <div id="keyBox">
    <div class="orb" style="font-size:18px;font-weight:900;color:var(--cy);letter-spacing:4px;margin-bottom:6px">INVEST<span style="color:var(--t2);font-weight:400">RADAR</span></div>
    <div class="mono" style="font-size:10px;color:var(--tm);letter-spacing:2px;margin-bottom:18px">// AI INVESTMENT INTELLIGENCE</div>
    <div style="font-size:13px;color:var(--t2);line-height:1.7;margin-bottom:6px">
      Gib deinen <strong style="color:var(--cy)">Anthropic API Key</strong> ein.<br>
      Er wird nur in deinem Browser gespeichert und nicht übertragen.
    </div>
    <div style="font-size:11px;color:var(--tm);margin-bottom:4px">
      Key erhältlich unter <a href="https://console.anthropic.com" target="_blank" style="color:var(--cy)">console.anthropic.com</a> → API Keys
    </div>
    <div class="key-input-wrap">
      <input id="keyInput" type="password" placeholder="sk-ant-api03-..." autocomplete="off"/>
      <button id="keyToggle" onclick="toggleKeyVis()">👁</button>
    </div>
    <button id="keySubmit" onclick="saveKey()">▶ STARTEN</button>
    <div id="keyError" style="font-family:monospace;font-size:10px;color:var(--rd);margin-top:10px;display:none"></div>
  </div>
</div>

<!-- Main App -->

<div id="app" style="display:none">

  <!-- Header -->

  <div style="padding:20px 16px 14px;border-bottom:1px solid var(--bd);position:relative">
    <div style="position:absolute;bottom:0;left:0;right:0;height:1px;background:linear-gradient(90deg,transparent,var(--cy),transparent)"></div>
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:4px">
      <div class="orb" style="font-size:20px;font-weight:900;letter-spacing:4px;color:var(--cy)">INVEST<span style="color:var(--t2);font-weight:400">RADAR</span></div>
      <div class="mono" style="display:flex;align-items:center;gap:6px;font-size:10px;color:var(--gn);letter-spacing:2px">
        <div style="width:6px;height:6px;border-radius:50%;background:var(--gn);animation:pulse 2s infinite"></div>
        May 5, 2026
      </div>
    </div>
    <div class="mono" style="font-size:11px;color:var(--tm);letter-spacing:2px">// AI investment intelligence · live analysis · Claude-powered</div>
  </div>

  <!-- Ticker -->

  <div id="ticker">
    <div id="tickerFade"></div>
    <div id="tickerInner"></div>
  </div>

  <!-- Strategy -->

  <div class="mono" style="font-size:9px;letter-spacing:3px;color:var(--tm);padding:14px 16px 8px">// SELECT STRATEGY</div>
  <div id="stratGrid"></div>

  <!-- Sliders -->

  <div id="sliders">
    <div class="slider-row">
      <span class="mono" style="font-size:9px;letter-spacing:2px;color:var(--tm);width:44px;flex-shrink:0">RISK</span>
      <input type="range" id="riskSlider" min="1" max="5" step="1" value="3"
        style="background:linear-gradient(90deg,var(--gn),var(--am),var(--rd))"/>
      <span class="mono" id="riskLabel" style="font-size:9px;color:var(--cy);width:80px;text-align:right;flex-shrink:0">BALANCED</span>
    </div>
    <div class="slider-row">
      <span class="mono" style="font-size:9px;letter-spacing:2px;color:var(--tm);width:44px;flex-shrink:0">HOLD</span>
      <input type="range" id="holdSlider" min="1" max="5" step="1" value="3"
        style="background:linear-gradient(90deg,var(--cy),var(--pu))"/>
      <span class="mono" id="holdLabel" style="font-size:9px;color:var(--cy);width:80px;text-align:right;flex-shrink:0">1–3 YRS</span>
    </div>
  </div>

  <!-- Analyze Button -->

  <div style="padding:14px 16px">
    <button onclick="analyze()" style="width:100%;background:transparent;border:1px solid var(--cy);border-radius:4px;color:var(--cy);font-family:'Orbitron',monospace;font-size:12px;font-weight:700;letter-spacing:3px;padding:14px;cursor:pointer" onmouseover="this.style.background='var(--cyf)'" onmouseout="this.style.background='transparent'">
      ▶ MÄRKTE ANALYSIEREN
    </button>
  </div>

  <!-- Loading -->

  <div id="loading" class="mono">
    <div style="margin-bottom:8px;color:var(--cy)">■ ANALYSIERE MÄRKTE...</div>
    <div id="loadingDots" style="color:var(--tm)">Claude denkt nach</div>
  </div>

  <!-- Results -->

  <div id="results"></div>

  <!-- Disclaimer -->

  <div class="mono" style="margin:16px 16px 0;padding:10px 14px;border:1px solid var(--bd);border-radius:4px;font-size:9px;color:var(--tm);line-height:1.6">
    ⚠ KI-generierte Analyse · May 5, 2026 · Nur zu Bildungszwecken — keine Finanzberatung · Immer einen zugelassenen Berater konsultieren
  </div>
</div>

<button id="changeKeyBtn" onclick="changeKey()">⚙ API KEY ÄNDERN</button>

<script>
// ── MARKET DATA ────────────────────────────────────────────────────────────────
const MARKET = {
  asOf: "May 5, 2026",
  indicators: [
    {key:"S&P 500",   value:"5,631",      trend:"dn"},
    {key:"NASDAQ",    value:"17,876",     trend:"dn"},
    {key:"Fed Rate",  value:"4.25–4.50%", trend:"neu"},
    {key:"10Y Yield", value:"4.37%",      trend:"up"},
    {key:"CPI YoY",   value:"2.4%",       trend:"dn"},
    {key:"VIX",       value:"22.3",       trend:"up"},
    {key:"Gold",      value:"$3,271",     trend:"up"},
    {key:"WTI Oil",   value:"$56.42",     trend:"dn"},
    {key:"BTC",       value:"$94,200",    trend:"up"},
    {key:"USD Index", value:"99.4",       trend:"dn"},
  ],
  sentiment: "RISK-OFF / TARIFF UNCERTAINTY",
  summary: "Markets in May 2026 face significant turbulence. S&P 500 down ~17% from February peak on tariff uncertainty (90-day pause, 145% on China). Fed holds 4.25–4.50%; two cuts priced for 2026. CPI cooled to 2.4%. Gold at $3,271 all-time high. WTI collapsed to $56.42 on demand fears. BTC $94,200 outperforms as macro hedge. USD weakened to 99.4 on reserve diversification concerns.",
};

const STRATEGIES = [
  {key:"value",    label:"VALUE",    desc:"Undervalued, margin of safety"},
  {key:"growth",   label:"GROWTH",  desc:"High-growth, future earnings"},
  {key:"dividend", label:"DIVIDEND",desc:"Income-generating, stable yield"},
  {key:"momentum", label:"MOMENTUM",desc:"Trend-following, breakouts"},
  {key:"index",    label:"INDEX/ETF",desc:"Passive, diversified, low-cost"},
  {key:"balanced", label:"BALANCED",desc:"All-weather, mixed strategy"},
];

const RISK_LABELS = ["VERY LOW","LOW","BALANCED","HIGH","VERY HIGH"];
const HOLD_LABELS = ["<3 MON","3–12 MON","1–3 YRS","3–7 YRS","7+ YRS"];

// ── STATE ──────────────────────────────────────────────────────────────────────
let currentStrat = "value";
let apiKey = "";

// ── KEY MANAGEMENT ─────────────────────────────────────────────────────────────
function toggleKeyVis() {
  const inp = document.getElementById("keyInput");
  inp.type = inp.type === "password" ? "text" : "password";
}

function saveKey() {
  const val = document.getElementById("keyInput").value.trim();
  if (!val.startsWith("sk-ant-")) {
    document.getElementById("keyError").textContent = "Ungültiger Key — muss mit sk-ant- beginnen";
    document.getElementById("keyError").style.display = "block";
    return;
  }
  localStorage.setItem("ir_api_key", val);
  apiKey = val;
  document.getElementById("keyOverlay").style.display = "none";
  document.getElementById("app").style.display = "block";
  document.getElementById("keyError").style.display = "none";
  initApp();
}

function changeKey() {
  document.getElementById("keyInput").value = "";
  document.getElementById("keyOverlay").style.display = "flex";
  document.getElementById("app").style.display = "none";
}

// ── INIT ───────────────────────────────────────────────────────────────────────
window.onload = function() {
  const saved = localStorage.getItem("ir_api_key");
  if (saved && saved.startsWith("sk-ant-")) {
    apiKey = saved;
    document.getElementById("keyInput").value = saved;
    document.getElementById("keyOverlay").style.display = "none";
    document.getElementById("app").style.display = "block";
    initApp();
  }
};

function initApp() {
  buildTicker();
  buildStratButtons();
  buildSliders();
}

function buildTicker() {
  const inner = document.getElementById("tickerInner");
  const items = [...MARKET.indicators, ...MARKET.indicators];
  inner.innerHTML = items.map(ind => {
    const col = ind.trend==="up"?"#00ff88":ind.trend==="dn"?"#ff3a5c":"#7ab3cc";
    return `<span style="display:flex;align-items:center;gap:14px">
      <span style="display:flex;align-items:center;gap:5px">
        <span style="font-family:monospace;font-size:9px;color:#3a6070;letter-spacing:1px">${ind.key}</span>
        <span style="font-family:monospace;font-size:9px;font-weight:700;color:${col}">${ind.value}</span>
      </span>
      <span style="color:#0a2030">|</span>
    </span>`;
  }).join("");
}

function buildStratButtons() {
  const grid = document.getElementById("stratGrid");
  grid.innerHTML = STRATEGIES.map(s => `
    <button class="strat-btn${s.key===currentStrat?" active":""}" onclick="selectStrat('${s.key}')">
      <div class="bar"></div>
      <div class="mono" style="font-size:10px;font-weight:700;letter-spacing:1px;color:${s.key===currentStrat?"var(--cy)":"var(--t2)"};margin-bottom:3px">${s.label}</div>
      <div style="font-size:11px;color:var(--tm);line-height:1.4">${s.desc}</div>
    </button>
  `).join("");
}

function selectStrat(key) {
  currentStrat = key;
  buildStratButtons();
}

function buildSliders() {
  document.getElementById("riskSlider").addEventListener("input", e => {
    document.getElementById("riskLabel").textContent = RISK_LABELS[e.target.value-1];
  });
  document.getElementById("holdSlider").addEventListener("input", e => {
    document.getElementById("holdLabel").textContent = HOLD_LABELS[e.target.value-1];
  });
}

// ── CLAUDE API CALL ────────────────────────────────────────────────────────────
async function analyze() {
  const risk = document.getElementById("riskSlider").value;
  const hold = document.getElementById("holdSlider").value;
  const riskLabel = RISK_LABELS[risk-1];
  const holdLabel = HOLD_LABELS[hold-1];

  document.getElementById("loading").classList.add("show");
  document.getElementById("results").innerHTML = "";

  // Animated loading dots
  let dots = 0;
  const dotInterval = setInterval(() => {
    dots = (dots + 1) % 4;
    document.getElementById("loadingDots").textContent = "Claude analysiert Märkte" + ".".repeat(dots);
  }, 400);

  const prompt = `You are an expert investment analyst. Based on the current market conditions on ${MARKET.asOf}, generate investment recommendations.

CURRENT MARKET DATA:
${MARKET.indicators.map(i => `- ${i.key}: ${i.value} (${i.trend})`).join('\n')}
Market Sentiment: ${MARKET.sentiment}
Context: ${MARKET.summary}

USER PARAMETERS:
- Strategy: ${currentStrat.toUpperCase()}
- Risk Tolerance: ${riskLabel} (${risk}/5)
- Hold Period: ${holdLabel}

Generate EXACTLY 10 investment recommendations (5 LARGE-CAP, 5 SMALL-CAP) for a ${currentStrat.toUpperCase()} strategy with ${riskLabel} risk and ${holdLabel} hold period.

Respond ONLY with valid JSON, no markdown, no preamble:
{
  "groups": [
    {
      "label": "LARGE-CAP",
      "sublabel": "Established leaders & major funds",
      "items": [
        {
          "ticker": "TICKER",
          "name": "Full Company Name",
          "type": "STOCK|ETF|BOND|REIT",
          "strategy": "Strategy description (max 6 words)",
          "conviction": "HIGH|MEDIUM|LOW",
          "hold": "Hold period string",
          "upside": "+XX%",
          "risk": "LOW|MED|HIGH",
          "div": "X.X%|N/A",
          "fit": 85,
          "why": "3-4 sentence analysis citing specific data points from the market context provided. Include specific numbers and current events.",
          "cats": ["Catalyst 1", "Catalyst 2", "Catalyst 3", "Catalyst 4"],
          "risks": "Key risks in one sentence."
        }
      ]
    },
    {
      "label": "SMALL-CAP",
      "sublabel": "High-potential growth opportunities",
      "items": [...]
    }
  ]
}`;

  try {
    const response = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "x-api-key": apiKey,
        "anthropic-version": "2023-06-01",
        "anthropic-dangerous-direct-browser-calls": "true",
      },
      body: JSON.stringify({
        model: "claude-sonnet-4-20250514",
        max_tokens: 4000,
        messages: [{ role: "user", content: prompt }]
      })
    });

    clearInterval(dotInterval);
    document.getElementById("loading").classList.remove("show");

    if (!response.ok) {
      const err = await response.json();
      if (response.status === 401) {
        showError("Ungültiger API Key. Bitte neu eingeben.");
        setTimeout(changeKey, 2000);
      } else {
        showError(`API Fehler: ${err.error?.message || response.status}`);
      }
      return;
    }

    const data = await response.json();
    const text = data.content?.map(b => b.text||"").join("") || "";
    const clean = text.replace(/```json|```/g,"").trim();
    const parsed = JSON.parse(clean);
    renderResults(parsed.groups, riskLabel, holdLabel);

  } catch(e) {
    clearInterval(dotInterval);
    document.getElementById("loading").classList.remove("show");
    showError("Fehler: " + e.message);
  }
}

function showError(msg) {
  document.getElementById("results").innerHTML = `
    <div style="margin:0 16px;padding:16px;border:1px solid var(--rd);border-radius:4px;font-family:monospace;font-size:11px;color:var(--rd)">
      ⚠ ${msg}
    </div>`;
}

// ── RENDER ─────────────────────────────────────────────────────────────────────
function hl(t="") {
  return t
    .replace(/(\$[\d,]+\.?\d*[BMK]?|\d+\.?\d*x|\d+\.?\d*%)/g,
      '<span style="color:#00d4ff;font-weight:700">$1</span>')
    .replace(/\b(P\/E|FCF|CAGR|EBITDA|YoY|ATH|VIX|CPI|NIM|PEG|ROE|WTI|LNG|ETF|REIT|GAAP|AI)\b/g,
      '<span style="color:#00d4ff;font-weight:700">$1</span>');
}

function renderResults(groups, riskLabel, holdLabel) {
  const total = groups.reduce((a,g)=>a+(g.items?.length||0),0);

  let html = `<div class="fade" style="padding:14px 16px 0">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:12px;padding-bottom:10px;border-bottom:1px solid var(--bd)">
      <div class="mono" style="font-size:10px;color:var(--tm);letter-spacing:3px">// RECOMMENDATIONS</div>
      <div style="display:flex;gap:6px;align-items:center">
        ${pill(currentStrat.toUpperCase(),"#00d4ff")}
        ${pill(riskLabel,"#ffaa00")}
        ${pill(holdLabel,"#9b59ff")}
        <span class="mono" style="font-size:10px;color:var(--gn);margin-left:4px">${total} SIG</span>
      </div>
    </div>`;

  groups.forEach(g => {
    if (!g.items?.length) return;
    const isS = (g.label||"").includes("SMALL");
    const col = isS?"#00ff88":"#00d4ff";
    const dim = isS?"rgba(0,255,136,.18)":"rgba(0,212,255,.22)";
    const faint = isS?"rgba(0,255,136,.05)":"rgba(0,212,255,.06)";

    html += `<div style="margin:20px 0 8px;display:flex;align-items:center;gap:10px">
      <div style="width:30px;height:30px;border-radius:4px;display:flex;align-items:center;justify-content:center;font-size:14px;flex-shrink:0;background:${faint};border:1px solid ${dim};color:${col}">${isS?"◇":"◈"}</div>
      <div style="flex:1">
        <div class="mono" style="font-size:11px;font-weight:700;letter-spacing:3px;color:${col}">${g.label}</div>
        <div class="mono" style="font-size:9px;color:var(--tm);letter-spacing:1px;margin-top:2px">${g.sublabel||""}</div>
      </div>
      <div class="mono" style="font-size:9px;padding:3px 8px;border-radius:2px;color:${col};border:1px solid ${dim};background:${faint};flex-shrink:0">${g.items.length} PICKS</div>
    </div>
    <div style="height:1px;background:linear-gradient(90deg,${dim},transparent);margin-bottom:10px"></div>`;

    g.items.forEach((r,idx) => {
      const id = `c${Date.now()}${idx}`;
      const tk = (r.type||"STOCK").toLowerCase();
      const ts = ({
        stock:{c:"#00d4ff",b:"rgba(0,212,255,.22)",bg:"rgba(0,212,255,.06)"},
        etf:  {c:"#00ff88",b:"rgba(0,255,136,.18)",bg:"rgba(0,255,136,.05)"},
        bond: {c:"#ffaa00",b:"rgba(255,170,0,.2)",bg:"rgba(255,170,0,.05)"},
        reit: {c:"#ff9955",b:"rgba(255,153,85,.3)",bg:"rgba(255,153,85,.05)"},
      })[tk]||{c:"#00d4ff",b:"rgba(0,212,255,.22)",bg:"rgba(0,212,255,.06)"};
      const al = Math.max(0,Math.min(100,parseInt(r.fit)||50));
      const ac = al>=75?"#00ff88":al>=50?"#ffaa00":"#ff3a5c";
      const cc = r.conviction==="HIGH"?"#00ff88":r.conviction==="MEDIUM"?"#ffaa00":"#7ab3cc";

      html += `<div class="card">
        <div class="card-top">
          <span class="mono" style="font-size:8px;letter-spacing:2px;padding:3px 7px;border-radius:2px;border:1px solid ${ts.b};background:${ts.bg};color:${ts.c};white-space:nowrap;margin-top:2px">${r.type||"STOCK"}</span>
          <div style="min-width:0">
            <div class="mono" style="font-size:17px;font-weight:700;color:var(--t1);letter-spacing:1px;line-height:1;margin-bottom:3px">${r.ticker}</div>
            <div style="font-size:12px;color:var(--t2);overflow:hidden;text-overflow:ellipsis;white-space:nowrap;margin-bottom:4px">${r.name}</div>
            <div class="mono" style="font-size:9px;color:var(--tm);letter-spacing:1px">${r.strategy||""}</div>
          </div>
          <div style="text-align:right">
            <div class="mono" style="font-size:11px;font-weight:700;color:${cc};letter-spacing:1px;margin-bottom:4px">${r.conviction}</div>
            <div class="mono" style="font-size:9px;color:var(--tm);letter-spacing:1px;white-space:nowrap">HOLD: ${r.hold}</div>
          </div>
        </div>
        <div class="card-metrics">
          <div class="metric"><span class="mono" style="font-size:8px;color:var(--tm);letter-spacing:1px;display:block;margin-bottom:2px">UPSIDE</span><span class="mono" style="font-size:12px;font-weight:700;color:#00ff88">${r.upside||"—"}</span></div>
          <div class="metric"><span class="mono" style="font-size:8px;color:var(--tm);letter-spacing:1px;display:block;margin-bottom:2px">RISK</span><span class="mono" style="font-size:12px;font-weight:700;color:var(--t1)">${r.risk||"—"}</span></div>
          <div class="metric"><span class="mono" style="font-size:8px;color:var(--tm);letter-spacing:1px;display:block;margin-bottom:2px">YIELD</span><span class="mono" style="font-size:12px;font-weight:700;color:var(--t1)">${r.div||"—"}</span></div>
        </div>
        <div class="fit-bar">
          <span class="mono" style="font-size:8px;color:var(--tm);letter-spacing:1px;width:58px;flex-shrink:0">MARKET FIT</span>
          <div style="flex:1;height:3px;background:var(--bd);border-radius:2px;overflow:hidden"><div style="height:100%;width:${al}%;background:${ac};border-radius:2px"></div></div>
          <span class="mono" style="font-size:10px;font-weight:700;color:${ac};width:32px;text-align:right">${al}%</span>
        </div>
        <button class="expand-btn" onclick="toggleCard('${id}')">
          <span class="mono" style="font-size:9px;letter-spacing:2px">▶ FULL ANALYSIS &amp; REASONING</span>
          <span id="arr${id}" style="font-size:10px;transition:transform .2s">▼</span>
        </button>
        <div id="${id}" class="reasoning">
          <div style="font-size:13px;line-height:1.85;color:var(--t2);margin-bottom:14px">${hl(r.why||"")}</div>
          ${r.cats?.length?`<div style="margin-bottom:12px">
            <div class="mono" style="font-size:8px;color:var(--tm);letter-spacing:2px;margin-bottom:7px">▶ KEY CATALYSTS</div>
            <div style="display:flex;flex-wrap:wrap;gap:5px">${r.cats.map(c=>`<span class="cat-tag">${c}</span>`).join("")}</div>
          </div>`:""}
          ${r.risks?`<div style="border-top:1px solid var(--bd);padding-top:10px">
            <span class="mono" style="font-size:8px;color:var(--tm);letter-spacing:2px">▶ KEY RISKS &nbsp;</span>
            <span style="font-size:12px;color:#ff7a8a;line-height:1.6">${r.risks}</span>
          </div>`:""}
        </div>
      </div>`;
    });
  });

  html += `</div>`;
  document.getElementById("results").innerHTML = html;
}

function pill(label, color) {
  return `<span class="mono" style="font-size:9px;font-weight:700;letter-spacing:1px;padding:2px 8px;border-radius:2px;border:1px solid ${color}33;background:${color}10;color:${color}">${label}</span>`;
}

function toggleCard(id) {
  const el = document.getElementById(id);
  const arr = document.getElementById("arr"+id);
  const open = el.classList.toggle("open");
  arr.style.transform = open ? "rotate(180deg)" : "none";
}
</script>

</body>
</html>
