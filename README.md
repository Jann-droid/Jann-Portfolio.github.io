<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>INVESTMENT RADAR</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@400;600;700;900&family=Rajdhani:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --bg-void: #020408;
    --bg-deep: #040b12;
    --bg-card: #060f18;
    --bg-panel: #091422;
    --accent-cyan: #00d4ff;
    --accent-cyan-dim: #00d4ff40;
    --accent-green: #00ff88;
    --accent-green-dim: #00ff8830;
    --accent-amber: #ffaa00;
    --accent-amber-dim: #ffaa0030;
    --accent-red: #ff3a5c;
    --accent-red-dim: #ff3a5c25;
    --accent-purple: #9b59ff;
    --text-primary: #e8f4ff;
    --text-secondary: #7ab3cc;
    --text-muted: #3a6070;
    --border-glow: #00d4ff25;
    --border-dim: #0a2030;
    --scanline: rgba(0,212,255,0.02);
  }
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body {
    background: var(--bg-void);
    color: var(--text-primary);
    font-family: 'Rajdhani', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
    position: relative;
  }
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(0deg, transparent, transparent 2px, var(--scanline) 2px, var(--scanline) 4px);
    pointer-events: none;
    z-index: 1000;
  }
  .grid-bg {
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(var(--border-dim) 1px, transparent 1px),
      linear-gradient(90deg, var(--border-dim) 1px, transparent 1px);
    background-size: 40px 40px;
    z-index: 0;
    opacity: 0.6;
  }
  .corner-glow {
    position: fixed;
    width: 400px; height: 400px;
    border-radius: 50%;
    filter: blur(120px);
    pointer-events: none;
    z-index: 0;
  }
  .corner-glow.tl { top: -150px; left: -150px; background: rgba(0,212,255,0.07); }
  .corner-glow.br { bottom: -150px; right: -150px; background: rgba(155,89,255,0.05); }
  .app {
    position: relative;
    z-index: 1;
    max-width: 480px;
    margin: 0 auto;
    padding: 0 0 100px;
  }
  .header {
    padding: 20px 16px 12px;
    border-bottom: 1px solid var(--border-dim);
    position: relative;
    overflow: hidden;
  }
  .header::after {
    content: '';
    position: absolute;
    bottom: 0; left: 0; right: 0; height: 1px;
    background: linear-gradient(90deg, transparent, var(--accent-cyan), transparent);
  }
  .header-top {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 4px;
  }
  .logo {
    font-family: 'Orbitron', monospace;
    font-size: 18px;
    font-weight: 900;
    letter-spacing: 4px;
    color: var(--accent-cyan);
    text-shadow: 0 0 20px var(--accent-cyan-dim);
  }
  .logo span { color: var(--text-secondary); font-weight: 400; }
  .live-badge {
    display: flex;
    align-items: center;
    gap: 6px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    color: var(--accent-green);
    letter-spacing: 2px;
  }
  .live-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--accent-green);
    animation: pulse 2s infinite;
  }
  @keyframes pulse {
    0%, 100% { opacity: 1; box-shadow: 0 0 6px var(--accent-green); }
    50% { opacity: 0.4; box-shadow: none; }
  }
  .header-sub {
    font-size: 11px;
    color: var(--text-muted);
    letter-spacing: 2px;
    text-transform: uppercase;
    font-family: 'Share Tech Mono', monospace;
  }
  .section-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    letter-spacing: 3px;
    color: var(--text-muted);
    text-transform: uppercase;
    padding: 14px 16px 8px;
  }
  .strategy-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px;
    padding: 0 16px;
  }
  .strat-btn {
    background: var(--bg-card);
    border: 1px solid var(--border-dim);
    border-radius: 4px;
    padding: 10px 12px;
    cursor: pointer;
    transition: all 0.2s;
    text-align: left;
    position: relative;
    overflow: hidden;
  }
  .strat-btn::before {
    content: '';
    position: absolute;
    top: 0; left: 0; width: 3px; height: 100%;
    background: var(--accent-cyan);
    transform: scaleY(0);
    transition: transform 0.2s;
  }
  .strat-btn.active { border-color: var(--accent-cyan-dim); background: #050e18; }
  .strat-btn.active::before { transform: scaleY(1); }
  .strat-btn:active { transform: scale(0.98); }
  .strat-name {
    font-family: 'Orbitron', monospace;
    font-size: 10px;
    font-weight: 600;
    letter-spacing: 1px;
    color: var(--text-secondary);
    margin-bottom: 3px;
  }
  .strat-btn.active .strat-name { color: var(--accent-cyan); }
  .strat-desc { font-size: 11px; color: var(--text-muted); line-height: 1.4; }
  .risk-row {
    padding: 14px 16px 0;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .risk-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    letter-spacing: 2px;
    color: var(--text-muted);
    min-width: 40px;
  }
  input[type=range] {
    flex: 1;
    -webkit-appearance: none;
    height: 2px;
    background: linear-gradient(90deg, var(--accent-green), var(--accent-amber), var(--accent-red));
    border-radius: 2px;
    outline: none;
  }
  input[type=range]::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 16px; height: 16px;
    border-radius: 50%;
    background: var(--bg-void);
    border: 2px solid var(--accent-cyan);
    cursor: pointer;
    box-shadow: 0 0 10px var(--accent-cyan-dim);
  }
  .risk-val {
    font-family: 'Orbitron', monospace;
    font-size: 11px;
    color: var(--accent-cyan);
    min-width: 30px;
    text-align: right;
  }
  .generate-wrap { padding: 14px 16px; }
  .generate-btn {
    width: 100%;
    background: transparent;
    border: 1px solid var(--accent-cyan);
    border-radius: 4px;
    color: var(--accent-cyan);
    font-family: 'Orbitron', monospace;
    font-size: 12px;
    font-weight: 700;
    letter-spacing: 3px;
    padding: 14px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
    transition: all 0.2s;
    text-transform: uppercase;
  }
  .generate-btn::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--accent-cyan);
    transform: translateX(-100%);
    transition: transform 0.3s;
    z-index: 0;
  }
  .generate-btn:hover::before { transform: translateX(0); }
  .generate-btn:hover { color: var(--bg-void); }
  .generate-btn:active { transform: scale(0.99); }
  .generate-btn span { position: relative; z-index: 1; }
  .generate-btn:disabled {
    border-color: var(--text-muted);
    color: var(--text-muted);
    cursor: not-allowed;
  }
  .generate-btn:disabled::before { display: none; }
  .status-bar {
    margin: 0 16px;
    padding: 10px 14px;
    background: var(--bg-panel);
    border: 1px solid var(--border-dim);
    border-radius: 4px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--accent-cyan);
    display: none;
    align-items: center;
    gap: 8px;
  }
  .status-bar.visible { display: flex; }
  .status-spinner {
    width: 12px; height: 12px;
    border: 1.5px solid var(--text-muted);
    border-top-color: var(--accent-cyan);
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
    flex-shrink: 0;
  }
  @keyframes spin { to { transform: rotate(360deg); } }
  .results { padding: 16px 16px 0; display: none; }
  .results.visible { display: block; }
  .results-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 14px;
    padding-bottom: 10px;
    border-bottom: 1px solid var(--border-dim);
  }
  .results-title {
    font-family: 'Orbitron', monospace;
    font-size: 10px;
    color: var(--text-muted);
    letter-spacing: 3px;
  }
  .results-count {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--accent-green);
  }
  /* GROUP HEADERS */
  .group-header {
    display: flex;
    align-items: center;
    gap: 10px;
    margin: 20px 0 8px;
  }
  .group-header:first-child { margin-top: 0; }
  .group-icon {
    width: 30px; height: 30px;
    border-radius: 4px;
    display: flex; align-items: center; justify-content: center;
    font-size: 14px;
    flex-shrink: 0;
  }
  .group-icon.large { background: rgba(0,212,255,0.1); border: 1px solid var(--accent-cyan-dim); }
  .group-icon.small { background: rgba(0,255,136,0.1); border: 1px solid var(--accent-green-dim); }
  .group-titles { flex: 1; min-width: 0; }
  .group-label {
    font-family: 'Orbitron', monospace;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 3px;
  }
  .group-label.large { color: var(--accent-cyan); }
  .group-label.small { color: var(--accent-green); }
  .group-sublabel {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    color: var(--text-muted);
    letter-spacing: 1px;
    margin-top: 2px;
  }
  .group-count {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    letter-spacing: 1px;
    padding: 3px 8px;
    border-radius: 2px;
    flex-shrink: 0;
  }
  .group-count.large { color: var(--accent-cyan); border: 1px solid var(--accent-cyan-dim); background: rgba(0,212,255,0.05); }
  .group-count.small { color: var(--accent-green); border: 1px solid var(--accent-green-dim); background: rgba(0,255,136,0.05); }
  .group-divider {
    height: 1px;
    margin-bottom: 10px;
  }
  .group-divider.large { background: linear-gradient(90deg, var(--accent-cyan-dim), transparent); }
  .group-divider.small { background: linear-gradient(90deg, var(--accent-green-dim), transparent); }
  /* CARDS */
  .rec-card {
    background: var(--bg-card);
    border: 1px solid var(--border-dim);
    border-radius: 6px;
    margin-bottom: 10px;
    overflow: hidden;
    transition: border-color 0.2s;
  }
  .rec-card:hover { border-color: #0a2535; }
  .card-main {
    padding: 14px;
    display: grid;
    grid-template-columns: auto 1fr auto;
    gap: 0 12px;
    align-items: start;
  }
  .type-badge {
    font-family: 'Share Tech Mono', monospace;
    font-size: 8px;
    letter-spacing: 2px;
    padding: 3px 7px;
    border-radius: 2px;
    border: 1px solid;
    white-space: nowrap;
    margin-top: 2px;
  }
  .type-stock { color: var(--accent-cyan); border-color: var(--accent-cyan-dim); background: rgba(0,212,255,0.05); }
  .type-etf { color: var(--accent-green); border-color: var(--accent-green-dim); background: rgba(0,255,136,0.05); }
  .type-bond { color: var(--accent-amber); border-color: var(--accent-amber-dim); background: rgba(255,170,0,0.05); }
  .type-crypto { color: var(--accent-purple); border-color: rgba(155,89,255,0.3); background: rgba(155,89,255,0.05); }
  .type-reit { color: #ff9955; border-color: rgba(255,153,85,0.3); background: rgba(255,153,85,0.05); }
  .card-info { min-width: 0; }
  .card-ticker {
    font-family: 'Orbitron', monospace;
    font-size: 16px;
    font-weight: 700;
    color: var(--text-primary);
    letter-spacing: 1px;
    line-height: 1;
    margin-bottom: 3px;
  }
  .card-name {
    font-size: 12px;
    color: var(--text-secondary);
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    margin-bottom: 5px;
  }
  .card-strategy {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    letter-spacing: 1px;
    color: var(--text-muted);
  }
  .card-right { text-align: right; }
  .conviction {
    font-family: 'Orbitron', monospace;
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 1px;
    margin-bottom: 4px;
  }
  .conv-high { color: var(--accent-green); }
  .conv-med { color: var(--accent-amber); }
  .conv-low { color: var(--text-secondary); }
  .hold-period {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    color: var(--text-muted);
    letter-spacing: 1px;
    white-space: nowrap;
  }
  .card-metrics {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    border-top: 1px solid var(--border-dim);
  }
  .metric {
    padding: 8px 10px;
    border-right: 1px solid var(--border-dim);
    text-align: center;
  }
  .metric:last-child { border-right: none; }
  .metric-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 8px;
    color: var(--text-muted);
    letter-spacing: 1px;
    display: block;
    margin-bottom: 2px;
  }
  .metric-val {
    font-family: 'Orbitron', monospace;
    font-size: 12px;
    font-weight: 600;
    color: var(--text-primary);
  }
  .metric-val.pos { color: var(--accent-green); }
  .metric-val.neg { color: var(--accent-red); }
  .card-expand {
    width: 100%;
    background: transparent;
    border: none;
    border-top: 1px solid var(--border-dim);
    padding: 8px 14px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    cursor: pointer;
    transition: background 0.15s;
    color: var(--text-muted);
  }
  .card-expand:hover { background: rgba(0,212,255,0.03); }
  .expand-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    letter-spacing: 2px;
    color: var(--accent-cyan);
  }
  .expand-arrow {
    font-size: 12px;
    color: var(--accent-cyan);
    transition: transform 0.2s;
  }
  .expand-arrow.open { transform: rotate(180deg); }
  .reasoning-panel {
    border-top: 1px solid var(--border-dim);
    background: var(--bg-deep);
    display: none;
    padding: 14px;
  }
  .reasoning-panel.visible { display: block; }
  .reasoning-text {
    font-size: 13px;
    line-height: 1.7;
    color: var(--text-secondary);
  }
  .reasoning-text .highlight { color: var(--accent-cyan); font-weight: 600; }
  .disclaimer {
    margin: 16px 16px 0;
    padding: 10px 14px;
    border: 1px solid var(--border-dim);
    border-radius: 4px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    color: var(--text-muted);
    line-height: 1.6;
    letter-spacing: 0.5px;
    display: none;
  }
  .disclaimer.visible { display: block; }
  .risk-1 .card-ticker { color: #00ff88; }
  .risk-2 .card-ticker { color: #80ff44; }
  .risk-3 .card-ticker { color: var(--text-primary); }
  .risk-4 .card-ticker { color: #ffcc44; }
  .risk-5 .card-ticker { color: var(--accent-amber); }
</style>
</head>
<body>
<div class="grid-bg"></div>
<div class="corner-glow tl"></div>
<div class="corner-glow br"></div>

<div class="app">
  <div class="header">
    <div class="header-top">
      <div class="logo">INVEST<span>RADAR</span></div>
      <div class="live-badge"><div class="live-dot"></div>AI ANALYSIS</div>
    </div>
    <div class="header-sub">// AI-powered portfolio intelligence system</div>
  </div>

  <div class="section-label">// SELECT STRATEGY</div>
  <div class="strategy-grid">
    <button class="strat-btn active" data-strat="value" onclick="selectStrat(this)">
      <div class="strat-name">VALUE</div>
      <div class="strat-desc">Undervalued assets, margin of safety</div>
    </button>
    <button class="strat-btn" data-strat="growth" onclick="selectStrat(this)">
      <div class="strat-name">GROWTH</div>
      <div class="strat-desc">High-growth potential, future earnings</div>
    </button>
    <button class="strat-btn" data-strat="dividend" onclick="selectStrat(this)">
      <div class="strat-name">DIVIDEND</div>
      <div class="strat-desc">Income-generating, stable yield</div>
    </button>
    <button class="strat-btn" data-strat="momentum" onclick="selectStrat(this)">
      <div class="strat-name">MOMENTUM</div>
      <div class="strat-desc">Trend-following, technical signals</div>
    </button>
    <button class="strat-btn" data-strat="index" onclick="selectStrat(this)">
      <div class="strat-name">INDEX/ETF</div>
      <div class="strat-desc">Passive, diversified, low-cost</div>
    </button>
    <button class="strat-btn" data-strat="balanced" onclick="selectStrat(this)">
      <div class="strat-name">BALANCED</div>
      <div class="strat-desc">Mixed strategy, all-weather</div>
    </button>
  </div>

  <div class="risk-row">
    <div class="risk-label">RISK</div>
    <input type="range" min="1" max="5" step="1" value="3" id="riskSlider" oninput="updateRisk(this.value)">
    <div class="risk-val" id="riskVal">3/5</div>
  </div>

  <div class="generate-wrap">
    <button class="generate-btn" id="genBtn" onclick="generateRecommendations()">
      <span>&#9654; ANALYZE MARKETS</span>
    </button>
  </div>

  <div class="status-bar" id="statusBar">
    <div class="status-spinner"></div>
    <span id="statusText">INITIALIZING ANALYSIS MATRIX...</span>
  </div>

  <div class="results" id="results">
    <div class="results-header">
      <div class="results-title">// RECOMMENDATIONS</div>
      <div class="results-count" id="resultsCount">0 SIGNALS</div>
    </div>
    <div id="cardContainer"></div>
  </div>

  <div class="disclaimer" id="disclaimer">
    WARNING: AI-generated analysis for educational purposes only. Not financial advice. Past performance does not indicate future results. Always conduct your own research and consult a licensed financial advisor before investing.
  </div>
</div>

<script>
let selectedStrat = 'value';
let riskLevel = 3;

function selectStrat(btn) {
  document.querySelectorAll('.strat-btn').forEach(function(b) { b.classList.remove('active'); });
  btn.classList.add('active');
  selectedStrat = btn.dataset.strat;
}

function updateRisk(val) {
  riskLevel = parseInt(val);
  document.getElementById('riskVal').textContent = val + '/5';
}

var statusMessages = [
  'SCANNING MARKET DATA...',
  'APPLYING STRATEGY FILTERS...',
  'CALCULATING RISK PARAMETERS...',
  'RUNNING VALUATION MODELS...',
  'CROSS-REFERENCING SIGNALS...',
  'GENERATING INTELLIGENCE REPORT...'
];

async function generateRecommendations() {
  var btn = document.getElementById('genBtn');
  var statusBar = document.getElementById('statusBar');
  var statusText = document.getElementById('statusText');
  var results = document.getElementById('results');
  var disclaimer = document.getElementById('disclaimer');

  btn.disabled = true;
  results.classList.remove('visible');
  disclaimer.classList.remove('visible');
  statusBar.classList.add('visible');
  statusText.textContent = statusMessages[0];

  var msgIdx = 0;
  var msgInterval = setInterval(function() {
    msgIdx = (msgIdx + 1) % statusMessages.length;
    statusText.textContent = statusMessages[msgIdx];
  }, 1400);

  var stratDescriptions = {
    value: 'Benjamin Graham / Warren Buffett value investing: stocks trading below intrinsic value, strong fundamentals, low P/E, wide economic moat',
    growth: 'Peter Lynch / Philip Fisher growth investing: companies with above-average revenue growth, expanding TAM, strong competitive advantage',
    dividend: 'Dividend growth investing: dividend aristocrats, high yield, consistent payout history, financial stability',
    momentum: 'Momentum / trend following: relative strength, 52-week highs, positive earnings surprises, technical breakouts',
    index: 'Passive index investing: broad market ETFs, sector ETFs, factor ETFs, low expense ratios, long-term compounding',
    balanced: 'All-weather / balanced portfolio: mix of stocks, bonds, ETFs, REITs across multiple strategies for stability'
  };

  var stratDesc = stratDescriptions[selectedStrat];
  var risk = riskLevel;

  var promptText = 'You are a professional investment analyst. Generate investment recommendations for the "' + selectedStrat + '" strategy (' + stratDesc + ') with a risk tolerance of ' + risk + '/5 (1=very conservative, 5=very aggressive).\n\nProvide TWO groups:\n1. LARGE-CAP: 5 picks from well-established large/mega-cap companies or major broadly-known ETFs/index funds\n2. SMALL-CAP: 5 picks from smaller companies (small/micro-cap) or niche/specialized ETFs with higher growth potential\n\nRespond with ONLY valid compact JSON. No markdown, no backticks, no explanation outside the JSON. Keep reasoning fields to max 2 sentences.\n\nExact format:\n{"groups":[{"label":"LARGE-CAP","sublabel":"Established leaders & major funds","items":[{"ticker":"","name":"","type":"STOCK","strategy":"","conviction":"HIGH","holdPeriod":"","upside":"","risk":"LOW","divYield":"","reasoning":""}]},{"label":"SMALL-CAP","sublabel":"High-potential growth opportunities","items":[{"ticker":"","name":"","type":"STOCK","strategy":"","conviction":"HIGH","holdPeriod":"","upside":"","risk":"MED","divYield":"","reasoning":""}]}]}\n\nRules: type must be one of STOCK/ETF/BOND/CRYPTO/REIT. conviction: HIGH/MEDIUM/LOW. risk: LOW/MED/HIGH. upside format: +XX%. Risk level ' + risk + ': ' + (risk <= 2 ? 'choose defensive, stable, lower-volatility picks' : risk === 3 ? 'balanced mix of stability and growth' : 'aggressive growth, higher volatility acceptable') + '. Use real tickers only.';

  try {
    var response = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'anthropic-dangerous-direct-browser-calls': 'true'
      },
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 3000,
        messages: [{ role: 'user', content: promptText }]
      })
    });

    var data = await response.json();
    clearInterval(msgInterval);

    var rawText = data.content.map(function(i) { return i.text || ''; }).join('');
    var clean = rawText.replace(/```json/g, '').replace(/```/g, '').trim();
    var parsed = JSON.parse(clean);

    renderGroups(parsed.groups);
    statusBar.classList.remove('visible');
    results.classList.add('visible');
    disclaimer.classList.add('visible');

    var total = parsed.groups.reduce(function(acc, g) { return acc + g.items.length; }, 0);
    document.getElementById('resultsCount').textContent = total + ' SIGNALS';
  } catch (err) {
    clearInterval(msgInterval);
    var msg = err.message || 'UNKNOWN ERROR';
    statusText.textContent = 'ERROR: ' + msg.substring(0, 55).toUpperCase();
    setTimeout(function() { statusBar.classList.remove('visible'); }, 4000);
    console.error('InvestRadar error:', err);
  }

  btn.disabled = false;
}

function renderGroups(groups) {
  var container = document.getElementById('cardContainer');
  container.innerHTML = '';

  var globalIdx = 0;
  var icons = { 'LARGE-CAP': '◈', 'SMALL-CAP': '◇' };
  var sizeClass = { 'LARGE-CAP': 'large', 'SMALL-CAP': 'small' };

  groups.forEach(function(group) {
    var cls = sizeClass[group.label] || 'large';
    var icon = icons[group.label] || '◈';

    var headerEl = document.createElement('div');
    headerEl.className = 'group-header';
    headerEl.innerHTML =
      '<div class="group-icon ' + cls + '" style="color:' + (cls === 'large' ? 'var(--accent-cyan)' : 'var(--accent-green)') + '">' + icon + '</div>' +
      '<div class="group-titles">' +
        '<div class="group-label ' + cls + '">' + group.label + '</div>' +
        '<div class="group-sublabel">' + group.sublabel + '</div>' +
      '</div>' +
      '<div class="group-count ' + cls + '">' + group.items.length + ' PICKS</div>';
    container.appendChild(headerEl);

    var divider = document.createElement('div');
    divider.className = 'group-divider ' + cls;
    container.appendChild(divider);

    group.items.forEach(function(rec) {
      var idx = globalIdx++;
      var conv = rec.conviction === 'HIGH' ? 'conv-high' : rec.conviction === 'MEDIUM' ? 'conv-med' : 'conv-low';
      var typeRaw = (rec.type || 'STOCK').toLowerCase();
      var typeClass = 'type-' + typeRaw;
      var riskClass = 'risk-' + riskLevel;

      var card = document.createElement('div');
      card.className = 'rec-card ' + riskClass;
      card.innerHTML =
        '<div class="card-main">' +
          '<div><div class="type-badge ' + typeClass + '">' + rec.type + '</div></div>' +
          '<div class="card-info">' +
            '<div class="card-ticker">' + rec.ticker + '</div>' +
            '<div class="card-name">' + rec.name + '</div>' +
            '<div class="card-strategy">' + rec.strategy + '</div>' +
          '</div>' +
          '<div class="card-right">' +
            '<div class="conviction ' + conv + '">' + rec.conviction + '</div>' +
            '<div class="hold-period">HOLD: ' + rec.holdPeriod + '</div>' +
          '</div>' +
        '</div>' +
        '<div class="card-metrics">' +
          '<div class="metric"><span class="metric-label">UPSIDE</span><span class="metric-val pos">' + rec.upside + '</span></div>' +
          '<div class="metric"><span class="metric-label">RISK</span><span class="metric-val">' + rec.risk + '</span></div>' +
          '<div class="metric"><span class="metric-label">DIV YIELD</span><span class="metric-val">' + rec.divYield + '</span></div>' +
        '</div>' +
        '<button class="card-expand" onclick="toggleReasoning(' + idx + ')">' +
          '<span class="expand-label">&#9654; REASONING &amp; ANALYSIS</span>' +
          '<span class="expand-arrow" id="arrow-' + idx + '">&#9660;</span>' +
        '</button>' +
        '<div class="reasoning-panel" id="reasoning-' + idx + '">' +
          '<div class="reasoning-text">' + formatReasoning(rec.reasoning) + '</div>' +
        '</div>';
      container.appendChild(card);
    });
  });
}

function formatReasoning(text) {
  if (!text) return '';
  return text
    .replace(/(\d+\.?\d*%)/g, '<span class="highlight">$1</span>')
    .replace(/\b(P\/E|P\/B|EPS|ROE|FCF|CAGR|EBITDA|TTM|YoY|ETF|ATH|IPO|DCF|ROIC|EV)\b/g, '<span class="highlight">$1</span>');
}

function toggleReasoning(idx) {
  var panel = document.getElementById('reasoning-' + idx);
  var arrow = document.getElementById('arrow-' + idx);
  var isOpen = panel.classList.contains('visible');
  if (isOpen) {
    panel.classList.remove('visible');
    arrow.classList.remove('open');
  } else {
    panel.classList.add('visible');
    arrow.classList.add('open');
  }
}
</script>

</body>
</html>
