<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1.0"/>
<title>InvestRadar · AI Investment Intelligence</title>
<style>
/* ── NO EXTERNAL FONTS — all system monospace ── */
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}

html,body{
background:#020408 !important;
color:#e8f4ff;
font-family: ‘Courier New’, Courier, monospace;
min-height:100vh;
overflow-x:hidden;
}

/* force dark everywhere */
*{ color-scheme: dark; }

:root{
–bg:    #020408;
–bg2:   #040b12;
–bg3:   #060f18;
–bg4:   #091422;
–b0:    #0a2030;
–b1:    #0d2840;
–t1:    #e8f4ff;
–t2:    #7ab3cc;
–t3:    #3a6070;
–cy:    #00d4ff;
–cya:   rgba(0,212,255,0.18);
–cyb:   rgba(0,212,255,0.06);
–gn:    #00ff88;
–gna:   rgba(0,255,136,0.15);
–gnb:   rgba(0,255,136,0.05);
–am:    #ffaa00;
–rd:    #ff3a5c;
–pu:    #9b59ff;
}

/* ── BACKGROUND GRID ── */
body::before{
content:’’;
position:fixed;inset:0;
background-image:
linear-gradient(var(–b0) 1px, transparent 1px),
linear-gradient(90deg, var(–b0) 1px, transparent 1px);
background-size:40px 40px;
opacity:.4;
pointer-events:none;
z-index:0;
}

/* ── GLOW ORBS ── */
body::after{
content:’’;
position:fixed;inset:0;
background:
radial-gradient(ellipse 40% 30% at 10% 10%, rgba(0,212,255,0.06) 0%, transparent 60%),
radial-gradient(ellipse 40% 30% at 90% 90%, rgba(155,89,255,0.04) 0%, transparent 60%);
pointer-events:none;
z-index:0;
}

/* ── ANIMATIONS ── */
@keyframes PULSE {0%,100%{opacity:1}50%{opacity:.15}}
@keyframes SCROLL {from{transform:translateX(0)}to{transform:translateX(-50%)}}
@keyframes FADEIN {from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:translateY(0)}}

/* ══════════════════════════════════════
OVERLAY  —  display:flex by default
══════════════════════════════════════ */
#OV{
position:fixed;inset:0;z-index:9999;
background:rgba(2,4,8,0.96);
display:flex;
align-items:center;justify-content:center;
padding:20px;
}
#OV-BOX{
background:#060f18;
border:1px solid var(–cya);
border-radius:8px;
padding:32px 28px;
width:100%;max-width:420px;
position:relative;overflow:hidden;
}
#OV-BOX::before{
content:’’;
position:absolute;top:0;left:0;right:0;height:2px;
background:linear-gradient(90deg,transparent,var(–cy),transparent);
}
.ov-logo{
font-size:24px;font-weight:900;letter-spacing:4px;
color:var(–cy);margin-bottom:4px;
}
.ov-logo em{color:var(–t2);font-style:normal;font-weight:400}
.ov-sub{font-size:9px;letter-spacing:3px;color:var(–t3);margin-bottom:24px}
.ov-body{font-size:13px;color:var(–t2);line-height:1.8;margin-bottom:6px}
.ov-body a{color:var(–cy);text-decoration:none}
.ov-body a:hover{text-decoration:underline}
.ov-hint{font-size:9px;color:var(–t3);line-height:1.7;margin-bottom:16px}
.key-wrap{position:relative;margin-bottom:6px}
#KI{
width:100%;
background:#040b12;
border:1px solid var(–b1);
border-radius:4px;
color:var(–t1);
font-family:inherit;font-size:11px;
padding:11px 42px 11px 13px;
outline:none;
transition:border-color .2s;
letter-spacing:.5px;
}
#KI::placeholder{color:var(–t3)}
#KI:focus{border-color:var(–cya)}
#EYE{
position:absolute;right:10px;top:50%;transform:translateY(-50%);
background:none;border:none;color:var(–t3);
cursor:pointer;font-size:14px;padding:2px;
transition:color .15s;
}
#EYE:hover{color:var(–cy)}
#KERR{
font-size:9px;color:var(–rd);
min-height:16px;margin-bottom:10px;
letter-spacing:.5px;
}
#START{
width:100%;
background:var(–cyb);
border:1px solid var(–cy);
border-radius:4px;color:var(–cy);
font-family:inherit;font-size:11px;font-weight:700;
letter-spacing:4px;padding:13px;
cursor:pointer;
transition:background .2s,box-shadow .2s;
text-transform:uppercase;
}
#START:hover{background:rgba(0,212,255,0.14);box-shadow:0 0 24px rgba(0,212,255,0.12)}
.ov-note{font-size:8px;color:var(–t3);text-align:center;margin-top:14px;line-height:1.7}

/* ══════════════════════════════════════
APP  —  display:none by default
══════════════════════════════════════ */
#APP{
display:none;
position:relative;z-index:1;
max-width:540px;margin:0 auto;
padding-bottom:80px;
}

/* ── HEADER ── */
#HDR{
padding:20px 16px 14px;
border-bottom:1px solid var(–b0);
position:relative;
}
#HDR::after{
content:’’;
position:absolute;bottom:0;left:0;right:0;height:1px;
background:linear-gradient(90deg,transparent,var(–cy),transparent);
}
.hdr-row{display:flex;align-items:center;justify-content:space-between;margin-bottom:5px}
.logo{font-size:22px;font-weight:900;letter-spacing:4px;color:var(–cy)}
.logo em{color:var(–t2);font-style:normal;font-weight:400}
.live-pill{display:flex;align-items:center;gap:6px;font-size:9px;color:var(–gn);letter-spacing:2px}
.live-dot{width:7px;height:7px;border-radius:50%;background:var(–gn);animation:PULSE 2s ease-in-out infinite}
.hdr-sub{font-size:9px;color:var(–t3);letter-spacing:2px}

/* ── TICKER ── */
#TKR{border-bottom:1px solid var(–b0);padding:8px 0;overflow:hidden;position:relative}
#TKR::after{
content:’’;position:absolute;right:0;top:0;bottom:0;width:50px;
background:linear-gradient(90deg,transparent,var(–bg));
pointer-events:none;z-index:2;
}
#TKR-IN{display:flex;width:max-content;animation:SCROLL 32s linear infinite}
.ti{display:flex;align-items:center;gap:6px;padding:0 18px;border-right:1px solid var(–b0)}
.ti-k{font-size:8px;color:var(–t3);letter-spacing:1px}
.ti-v{font-size:9px;font-weight:700}

/* ── MARKET PANEL ── */
#MKT{margin:16px 16px 0;background:var(–bg3);border:1px solid var(–b0);border-radius:6px;overflow:hidden}
.mkt-head{display:flex;align-items:center;justify-content:space-between;padding:9px 14px;border-bottom:1px solid var(–b0)}
.mkt-title{font-size:8px;letter-spacing:3px;color:var(–cy)}
.mkt-badge{font-size:8px;letter-spacing:1px;color:var(–gn);background:var(–gnb);border:1px solid var(–gna);border-radius:3px;padding:2px 8px}
.mkt-grid{display:grid;grid-template-columns:1fr 1fr;gap:1px;background:var(–b0)}
.mkt-cell{background:var(–bg2);padding:8px 12px}
.mkt-ck{font-size:8px;color:var(–t3);letter-spacing:1px;margin-bottom:3px}
.mkt-cv{font-size:13px;font-weight:700}
.mkt-sent{padding:9px 14px;border-top:1px solid var(–b0)}
.mkt-sk{font-size:8px;color:var(–t3);letter-spacing:2px;margin-bottom:2px}
.mkt-sv{font-size:11px;font-weight:700;color:var(–am)}
.mkt-sum{padding:10px 14px 14px;font-size:12px;color:var(–t2);line-height:1.8;border-top:1px solid var(–b0)}

/* ── SECTION LABEL ── */
.sec{font-size:8px;letter-spacing:3px;color:var(–t3);padding:18px 16px 10px}

/* ── STRATEGY GRID ── */
#SG{display:grid;grid-template-columns:1fr 1fr;gap:8px;padding:0 16px}
.sb{
background:var(–bg3) !important;
border:1px solid var(–b0) !important;
border-radius:4px;padding:11px 13px;
cursor:pointer;text-align:left;
position:relative;overflow:hidden;
transition:border-color .18s,background .18s;
display:block;width:100%;
}
.sb:hover{background:var(–bg4) !important;border-color:var(–b1) !important}
.sb.on{background:var(–cyb) !important;border-color:var(–cy) !important}
.sb-bar{position:absolute;top:0;left:0;width:3px;height:100%;background:var(–cy);display:none}
.sb.on .sb-bar{display:block}
.sb-name{font-size:10px;font-weight:700;letter-spacing:1px;color:var(–t2);margin-bottom:4px;transition:color .18s}
.sb.on .sb-name{color:var(–cy)}
.sb-desc{font-size:11px;color:var(–t3);line-height:1.4}

/* ── SLIDERS ── */
#SL{padding:13px 16px 0}
.sl-row{display:flex;align-items:center;gap:12px;margin-bottom:11px}
.sl-k{font-size:8px;letter-spacing:2px;color:var(–t3);width:40px;flex-shrink:0}
input[type=range]{
flex:1;height:2px;border-radius:2px;
-webkit-appearance:none;appearance:none;
outline:none;cursor:pointer;border:none;
background:linear-gradient(90deg,var(–gn),var(–am),var(–rd));
}
input[type=range]#HSL{background:linear-gradient(90deg,var(–cy),var(–pu))}
input[type=range]::-webkit-slider-thumb{
-webkit-appearance:none;
width:14px;height:14px;border-radius:50%;
background:var(–bg);border:2px solid var(–cy);cursor:pointer;
}
input[type=range]::-moz-range-thumb{
width:14px;height:14px;border-radius:50%;
background:var(–bg);border:2px solid var(–cy);cursor:pointer;
}
.sl-v{font-size:9px;color:var(–cy);width:76px;text-align:right;flex-shrink:0}

/* ── ANALYZE BTN ── */
#AW{padding:14px 16px 0}
#ABT{
width:100%;
background:transparent;
border:1px solid var(–cy);
border-radius:4px;color:var(–cy);
font-family:inherit;font-size:11px;font-weight:700;
letter-spacing:4px;padding:14px;cursor:pointer;
transition:background .2s,box-shadow .2s;
text-transform:uppercase;
}
#ABT:hover{background:var(–cyb);box-shadow:0 0 28px rgba(0,212,255,0.1)}
#ABT:disabled{opacity:.4;cursor:not-allowed}

/* ── LOADING ── */
#LD{display:none;padding:30px 16px;text-align:center}
#LD.show{display:block}
.ld-icon{font-size:9px;letter-spacing:3px;color:var(–cy);margin-bottom:8px}
.ld-txt{font-size:9px;letter-spacing:2px;color:var(–t3)}

/* ── RESULTS ── */
#RS{padding:14px 16px 0}
.rs-bar{display:flex;align-items:center;justify-content:space-between;margin-bottom:14px;padding-bottom:10px;border-bottom:1px solid var(–b0)}
.rs-title{font-size:8px;letter-spacing:3px;color:var(–t3)}
.pills{display:flex;gap:5px;align-items:center;flex-wrap:wrap}
.pill{font-size:8px;font-weight:700;letter-spacing:1px;padding:2px 8px;border-radius:3px}
.sig{font-size:9px;color:var(–gn);margin-left:3px}

/* ── GROUP ── */
.gh{display:flex;align-items:center;gap:10px;margin:20px 0 8px}
.gi{width:30px;height:30px;border-radius:4px;display:flex;align-items:center;justify-content:center;font-size:14px;flex-shrink:0}
.gn-label{font-size:10px;font-weight:700;letter-spacing:3px}
.gs{font-size:8px;color:var(–t3);letter-spacing:1px;margin-top:2px}
.gc{margin-left:auto;flex-shrink:0;font-size:8px;padding:2px 8px;border-radius:3px}
.gl{height:1px;margin-bottom:10px}

/* ── CARD ── */
.card{background:var(–bg3);border:1px solid var(–b0);border-radius:6px;margin-bottom:10px;overflow:hidden}
.card:hover{border-color:var(–b1)}
.ct{padding:14px;display:grid;grid-template-columns:auto 1fr auto;gap:0 12px;align-items:start}
.tb{font-size:7px;letter-spacing:2px;padding:3px 7px;border-radius:3px;margin-top:2px;white-space:nowrap}
.tkr{font-size:18px;font-weight:900;color:var(–t1);letter-spacing:1px;line-height:1;margin-bottom:3px}
.fn{font-size:12px;color:var(–t2);white-space:nowrap;overflow:hidden;text-overflow:ellipsis;margin-bottom:4px}
.stg{font-size:8px;color:var(–t3);letter-spacing:1px}
.cv{font-size:10px;font-weight:700;letter-spacing:1px;text-align:right;margin-bottom:5px}
.ht{font-size:8px;color:var(–t3);white-space:nowrap;text-align:right}
.met{display:grid;grid-template-columns:repeat(3,1fr);border-top:1px solid var(–b0)}
.mc{padding:7px 10px;text-align:center;border-right:1px solid var(–b0)}
.mc:last-child{border-right:none}
.mk{font-size:7px;letter-spacing:1px;color:var(–t3);margin-bottom:2px}
.mv{font-size:12px;font-weight:700}
.fr{display:flex;align-items:center;gap:9px;padding:7px 14px;border-top:1px solid var(–b0)}
.fl{font-size:7px;letter-spacing:1px;color:var(–t3);width:56px;flex-shrink:0}
.ft{flex:1;height:3px;background:var(–b0);border-radius:2px;overflow:hidden}
.ff{height:100%;border-radius:2px}
.fp{font-size:9px;font-weight:700;width:30px;text-align:right;flex-shrink:0}
.xb{
width:100%;background:transparent;border:none;
border-top:1px solid var(–b0);
padding:8px 14px;
display:flex;align-items:center;justify-content:space-between;
cursor:pointer;color:var(–cy);
font-family:inherit;
transition:background .15s;
}
.xb:hover{background:var(–cyb)}
.xl{font-size:8px;letter-spacing:2px}
.xa{font-size:9px;transition:transform .2s;display:inline-block}
.xa.open{transform:rotate(180deg)}
.rp{display:none;border-top:1px solid var(–b0);background:var(–bg2);padding:16px 16px 18px}
.rp.open{display:block;animation:FADEIN .2s ease}
.wy{font-size:12px;color:var(–t2);line-height:1.8;margin-bottom:14px}
.cl{font-size:7px;letter-spacing:2px;color:var(–t3);margin-bottom:7px}
.cw{display:flex;flex-wrap:wrap;gap:5px;margin-bottom:12px}
.ct2{font-size:8px;padding:3px 9px;border-radius:3px;background:var(–gnb);border:1px solid var(–gna);color:var(–gn)}
.rr{display:flex;gap:8px;align-items:flex-start;padding-top:10px;border-top:1px solid var(–b0)}
.rl{font-size:7px;letter-spacing:2px;color:var(–t3);flex-shrink:0;padding-top:2px}
.rt{font-size:11px;color:#ff7a8a;line-height:1.6}
.hi{color:var(–cy);font-weight:700}

/* ── ERROR ── */
.eb{padding:14px 16px;background:rgba(255,58,92,0.06);border:1px solid rgba(255,58,92,0.3);border-radius:4px;font-size:9px;color:var(–rd);line-height:1.7}

/* ── DISCLAIMER ── */
.disc{margin:20px 16px 0;padding:10px 14px;border:1px solid var(–b0);border-radius:4px;font-size:8px;color:var(–t3);line-height:1.7}

/* ── CHANGE KEY ── */
#CKB{
position:fixed;bottom:16px;right:16px;
background:var(–bg4);border:1px solid var(–b0);
border-radius:4px;color:var(–t3);
font-family:inherit;font-size:8px;letter-spacing:1px;
padding:8px 12px;cursor:pointer;z-index:100;
transition:border-color .2s,color .2s;
}
#CKB:hover{border-color:var(–cya);color:var(–cy)}
</style>

</head>
<body>

<!-- ══════════════════════════ OVERLAY ══════════════════════════ -->

<div id="OV">
  <div id="OV-BOX">
    <div class="ov-logo">INVEST<em>RADAR</em></div>
    <div class="ov-sub">// AI INVESTMENT INTELLIGENCE · CLAUDE-POWERED</div>
    <div class="ov-body">
      Enter your <strong style="color:var(--cy)">Anthropic API Key</strong> to start.<br>
      Stored locally in your browser only — never sent anywhere else.
    </div>
    <div class="ov-hint">
      Get a key at <a href="https://console.anthropic.com" target="_blank" rel="noopener">console.anthropic.com</a> → API Keys → Create Key<br>
      A small credit balance is required (~€5 covers 100+ analyses)
    </div>
    <div class="key-wrap">
      <input id="KI" type="password" placeholder="sk-ant-api03-..." autocomplete="off" spellcheck="false"/>
      <button id="EYE" type="button" onclick="toggleEye()">◉</button>
    </div>
    <div id="KERR"></div>
    <button id="START" type="button" onclick="submitKey()">▶ START</button>
    <div class="ov-note">Key is saved so you won't need to re-enter it · Use ⚙ button to change</div>
  </div>
</div>

<!-- ══════════════════════════ APP ══════════════════════════ -->

<div id="APP">

  <div id="HDR">
    <div class="hdr-row">
      <div class="logo">INVEST<em>RADAR</em></div>
      <div class="live-pill"><div class="live-dot"></div>LIVE · MAY 2026</div>
    </div>
    <div class="hdr-sub">// Claude-powered · AI investment analysis · BYOK</div>
  </div>

  <div id="TKR"><div id="TKR-IN"></div></div>

  <div id="MKT">
    <div class="mkt-head">
      <div class="mkt-title">// MARKET SNAPSHOT · MAY 5 2026</div>
      <div class="mkt-badge">LIVE DATA</div>
    </div>
    <div class="mkt-grid" id="MGRID"></div>
    <div class="mkt-sent">
      <div class="mkt-sk">MARKET SENTIMENT</div>
      <div class="mkt-sv">RISK-OFF / TARIFF UNCERTAINTY</div>
    </div>
    <div class="mkt-sum">
      S&amp;P 500 down <span class="hi">~17%</span> from February peak on US tariff shock (<span class="hi">145%</span> on China, 90-day pause on others).
      Fed holds at <span class="hi">4.25–4.50%</span> — two cuts priced for late 2026. CPI cooled to <span class="hi">2.4%</span> YoY.
      Gold at <span class="hi">$3,271</span> near all-time high. WTI collapsed to <span class="hi">$56.42</span> on demand fears.
      BTC at <span class="hi">$94,200</span> outperforms as macro hedge. USD weakened to <span class="hi">99.4</span>.
    </div>
  </div>

  <div class="sec">// SELECT STRATEGY</div>
  <div id="SG"></div>

  <div id="SL">
    <div class="sl-row">
      <span class="sl-k">RISK</span>
      <input type="range" id="RSL" min="1" max="5" value="3"/>
      <span class="sl-v" id="RLB">BALANCED</span>
    </div>
    <div class="sl-row">
      <span class="sl-k">HOLD</span>
      <input type="range" id="HSL" min="1" max="5" value="3"/>
      <span class="sl-v" id="HLB">1–3 YRS</span>
    </div>
  </div>

  <div id="AW">
    <button id="ABT" type="button" onclick="runAnalysis()">▶ ANALYSE STARTEN</button>
  </div>

  <div id="LD">
    <div class="ld-icon">■ ANALYSING MARKETS</div>
    <div class="ld-txt" id="LTXT">Claude is thinking...</div>
  </div>

  <div id="RS"></div>

  <div class="disc">⚠ AI-generated analysis · Educational purposes only · Not financial advice · Consult a licensed advisor before investing</div>
</div>

<button id="CKB" type="button" onclick="openOverlay()">⚙ API KEY</button>

<script>
(function(){
'use strict';

/* ═══ CONSTANTS ═══════════════════════════════ */
var RL = ['VERY LOW','LOW','BALANCED','HIGH','VERY HIGH'];
var HL = ['< 3 MON','3–12 MON','1–3 YRS','3–7 YRS','7+ YRS'];

var STRATS = [
  {k:'value',    l:'VALUE',    d:'Undervalued, margin of safety'},
  {k:'growth',   l:'GROWTH',  d:'High-growth, future earnings'},
  {k:'dividend', l:'DIVIDEND',d:'Income-generating, stable yield'},
  {k:'momentum', l:'MOMENTUM',d:'Trend-following, breakouts'},
  {k:'index',    l:'INDEX/ETF',d:'Passive, diversified, low-cost'},
  {k:'balanced', l:'BALANCED',d:'All-weather, mixed strategy'},
];

var TICKS = [
  {k:'S&P 500',   v:'5,631',      t:'dn'},
  {k:'NASDAQ',    v:'17,876',     t:'dn'},
  {k:'Fed Rate',  v:'4.25-4.50%', t:'neu'},
  {k:'10Y Yield', v:'4.37%',      t:'up'},
  {k:'CPI YoY',   v:'2.4%',       t:'dn'},
  {k:'VIX',       v:'22.3',       t:'up'},
  {k:'Gold',      v:'$3,271',     t:'up'},
  {k:'WTI Oil',   v:'$56.42',     t:'dn'},
  {k:'BTC',       v:'$94,200',    t:'up'},
  {k:'USD Index', v:'99.4',       t:'dn'},
];

var MKT = [
  'CURRENT MARKET DATA (May 5, 2026):',
  '- S&P 500: 5,631 (down ~17% from February all-time high)',
  '- NASDAQ: 17,876 (down ~18% from peak)',
  '- Fed Funds Rate: 4.25-4.50% (on hold; two cuts priced for late 2026)',
  '- 10Y Treasury Yield: 4.37%',
  '- CPI YoY: 2.4% (cooling toward 2% target)',
  '- VIX: 22.3 (elevated; risk-off environment)',
  '- Gold: $3,271/oz (near all-time high; strong safe-haven demand)',
  '- WTI Crude Oil: $56.42/bbl (collapsed on tariff demand fears)',
  '- Bitcoin: $94,200 (outperforming as macro hedge)',
  '- USD Index: 99.4 (weakening on tariff retaliation concerns)',
  '',
  'MACRO: US tariff shock (145% on China, 90-day pause on others) drove S&P -17%.',
  'Fed on hold at 4.25-4.50%. First cut expected late 2026.',
  'Investors rotating to defensives, gold, bonds. Sentiment: RISK-OFF.',
].join('\n');

/* ═══ STATE ═══════════════════════════════════ */
var KEY  = '';
var STR  = 'value';
var TMRZ = null;

/* ═══ BOOT ════════════════════════════════════ */
document.addEventListener('DOMContentLoaded', function(){
  var saved = '';
  try { saved = localStorage.getItem('ir_key') || ''; } catch(e){}
  if (saved) document.getElementById('KI').value = saved;

  document.getElementById('KI').addEventListener('keydown', function(e){
    if (e.key === 'Enter') submitKey();
  });
  document.getElementById('RSL').addEventListener('input', function(){
    document.getElementById('RLB').textContent = RL[this.value - 1];
  });
  document.getElementById('HSL').addEventListener('input', function(){
    document.getElementById('HLB').textContent = HL[this.value - 1];
  });

  buildTicker();
  buildMktGrid();
  buildStrats();
});

/* ═══ OVERLAY ═════════════════════════════════ */
function openOverlay(){
  document.getElementById('OV').style.display  = 'flex';
  document.getElementById('APP').style.display = 'none';
  document.getElementById('KERR').textContent  = '';
}
function closeOverlay(){
  document.getElementById('OV').style.display  = 'none';
  document.getElementById('APP').style.display = 'block';
}
function toggleEye(){
  var i = document.getElementById('KI');
  i.type = i.type === 'password' ? 'text' : 'password';
}
function submitKey(){
  var v = (document.getElementById('KI').value || '').trim();
  var e = document.getElementById('KERR');
  if (!v)                         { e.textContent = '⚠ Please enter your API key.'; return; }
  if (v.indexOf('sk-ant-') !== 0) { e.textContent = '⚠ Invalid key — must start with sk-ant-'; return; }
  e.textContent = '';
  KEY = v;
  try { localStorage.setItem('ir_key', v); } catch(e2){}
  closeOverlay();
}

/* ═══ BUILDERS ════════════════════════════════ */
function tcol(t){ return t==='up'?'#00ff88':t==='dn'?'#ff3a5c':'#7ab3cc'; }

function buildTicker(){
  var d = TICKS.concat(TICKS);
  var h = d.map(function(t){
    return '<div class="ti"><span class="ti-k">'+t.k+'</span>'
          +'<span class="ti-v" style="color:'+tcol(t.t)+'">'+t.v+'</span></div>';
  }).join('');
  document.getElementById('TKR-IN').innerHTML = h;
}

function buildMktGrid(){
  var h = TICKS.map(function(t){
    var isR = t.k==='VIX'||t.k==='CPI YoY';
    var c = isR ? (t.t==='up'?'#ffaa00':t.t==='dn'?'#00ff88':'#e8f4ff') : tcol(t.t);
    return '<div class="mkt-cell"><div class="mkt-ck">'+t.k+'</div>'
          +'<div class="mkt-cv" style="color:'+c+'">'+t.v+'</div></div>';
  }).join('');
  document.getElementById('MGRID').innerHTML = h;
}

function buildStrats(){
  var h = STRATS.map(function(s){
    var on = s.k === STR;
    return '<button class="sb'+(on?' on':'')+'" type="button" onclick="selStr(\''+s.k+'\')">'
          +'<div class="sb-bar"></div>'
          +'<div class="sb-name">'+s.l+'</div>'
          +'<div class="sb-desc">'+s.d+'</div>'
          +'</button>';
  }).join('');
  document.getElementById('SG').innerHTML = h;
}

function selStr(k){ STR = k; buildStrats(); }

/* ═══ ANALYSIS ════════════════════════════════ */
function runAnalysis(){
  if (!KEY || KEY.indexOf('sk-ant-') !== 0){ openOverlay(); return; }

  var r    = parseInt(document.getElementById('RSL').value, 10);
  var h    = parseInt(document.getElementById('HSL').value, 10);
  var rlbl = RL[r-1];
  var hlbl = HL[h-1];

  document.getElementById('RS').innerHTML = '';
  document.getElementById('LD').classList.add('show');
  document.getElementById('ABT').disabled = true;

  var dots = 0;
  if (TMRZ) clearInterval(TMRZ);
  TMRZ = setInterval(function(){
    dots = (dots+1)%4;
    document.getElementById('LTXT').textContent = 'Claude is analysing'+'.'.repeat(dots+1);
  }, 450);

  function done(){
    clearInterval(TMRZ);
    document.getElementById('LD').classList.remove('show');
    document.getElementById('ABT').disabled = false;
  }

  var prompt =
    'You are a professional investment analyst.\n\n'
    + MKT + '\n\n'
    + 'USER PARAMETERS:\n'
    + '- Strategy: ' + STR.toUpperCase() + '\n'
    + '- Risk Tolerance: ' + rlbl + ' (' + r + '/5)\n'
    + '- Hold Period: ' + hlbl + '\n\n'
    + 'Generate exactly 10 picks: 5 LARGE-CAP and 5 SMALL-CAP.\n'
    + 'Picks must fit a ' + STR.toUpperCase() + ' strategy with ' + rlbl + ' risk and ' + hlbl + ' hold.\n'
    + 'Reference specific numbers from the market data in each analysis.\n\n'
    + 'CRITICAL: Return ONLY raw JSON. No markdown. No code fences. No text before or after. Just JSON.\n\n'
    + 'Schema:\n'
    + '{"groups":['
    +   '{"label":"LARGE-CAP","sublabel":"Established leaders & major funds","items":['
    +     '{"ticker":"X","name":"Full Name","type":"STOCK|ETF|BOND|REIT",'
    +      '"strategy":"short label","conviction":"HIGH|MEDIUM|LOW",'
    +      '"hold":"e.g. 2-4 YRS","upside":"+XX%","risk":"LOW|MED|HIGH",'
    +      '"div":"X.X%|N/A","fit":80,'
    +      '"why":"3-4 sentences citing specific market data numbers.",'
    +      '"cats":["Catalyst 1","Catalyst 2","Catalyst 3"],'
    +      '"risks":"One sentence on key risks."}'
    +   ']},'  
    +   '{"label":"SMALL-CAP","sublabel":"High-potential opportunities","items":[/* 5 items same schema */]}'
    + ']}';

  fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': KEY,
      'anthropic-version': '2023-06-01',
      'anthropic-dangerous-direct-browser-access': 'true'
    },
    body: JSON.stringify({
      model: 'claude-haiku-4-5',
      max_tokens: 4096,
      messages: [{ role: 'user', content: prompt }]
    })
  })
  .then(function(res){
    if (!res.ok){
      return res.json().then(function(b){
        var m = (b && b.error && b.error.message) ? b.error.message : 'HTTP '+res.status;
        throw new Error(m+' ('+res.status+')');
      }).catch(function(e2){
        // if json parse failed on error body
        if (e2 && e2.message && e2.message.indexOf('HTTP') !== -1) throw e2;
        throw new Error('HTTP '+res.status);
      });
    }
    return res.json();
  })
  .then(function(data){
    var text = '';
    (data.content || []).forEach(function(b){ if(b.type==='text') text += b.text; });
    text = text.trim();
    // Strip any accidental markdown fences
    text = text.replace(/^```[\w]*\s*/,'').replace(/\s*```$/,'').trim();
    // Find JSON object
    var start = text.indexOf('{');
    var end   = text.lastIndexOf('}');
    if (start === -1 || end === -1) throw new Error('No JSON found in response.');
    text = text.slice(start, end+1);
    var parsed = JSON.parse(text);
    if (!parsed.groups || !Array.isArray(parsed.groups)) throw new Error('Unexpected JSON structure.');
    done();
    renderResults(parsed.groups, rlbl, hlbl);
  })
  .catch(function(e){
    done();
    var m = (e && e.message) ? e.message : String(e);
    var extra = '';
    if (m.indexOf('401') !== -1)                               extra = ' — Check your API key.';
    else if (m.indexOf('429') !== -1)                          extra = ' — Rate limit hit, wait a moment.';
    else if (m.toLowerCase().indexOf('failed to fetch') !== -1
          || m.toLowerCase().indexOf('load failed') !== -1
          || m.toLowerCase().indexOf('networkerror') !== -1)   extra = ' — Network error. Make sure you access this page via HTTPS (GitHub Pages), not file://.';
    showErr('⚠ '+m+extra);
  });
}

/* ═══ RENDER ══════════════════════════════════ */
function hl(t){
  if (!t) return '';
  t = t.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
  t = t.replace(/(\$[\d,]+(?:\.\d+)?[BMK]?|\d+(?:\.\d+)?x|\d+(?:\.\d+)?%)/g,'<span class="hi">$1</span>');
  t = t.replace(/\b(P\/E|FCF|CAGR|EBITDA|YoY|ATH|VIX|CPI|NIM|PEG|ROE|AUM|WTI|LNG|ETF|REIT|GAAP|AI)\b/g,'<span class="hi">$1</span>');
  return t;
}
function pill(txt,col){
  return '<span class="pill" style="background:'+col+'18;border:1px solid '+col+'33;color:'+col+'">'+txt+'</span>';
}
function showErr(m){
  document.getElementById('RS').innerHTML = '<div class="eb">'+m+'</div>';
}
function escQ(s){ return (s||'').replace(/'/g,"\\'"); }

function renderResults(groups, rlbl, hlbl){
  var total = 0;
  (groups||[]).forEach(function(g){ total += (g.items||[]).length; });

  var h = '<div class="rs-bar">'
    +'<div class="rs-title">// RECOMMENDATIONS</div>'
    +'<div class="pills">'
    +pill(STR.toUpperCase(),'#00d4ff')
    +pill(rlbl,'#ffaa00')
    +pill(hlbl,'#9b59ff')
    +'<span class="sig">'+total+' PICKS</span>'
    +'</div></div>';

  (groups||[]).forEach(function(g){
    if (!g.items||!g.items.length) return;
    var sm  = (g.label||'').indexOf('SMALL')!==-1;
    var col = sm?'#00ff88':'#00d4ff';
    var dim = sm?'rgba(0,255,136,.18)':'rgba(0,212,255,.22)';
    var fnt = sm?'rgba(0,255,136,.05)':'rgba(0,212,255,.06)';
    var ico = sm?'◇':'◈';

    h += '<div class="gh">'
      +'<div class="gi" style="background:'+fnt+';border:1px solid '+dim+';color:'+col+'">'+ico+'</div>'
      +'<div><div class="gn-label" style="color:'+col+'">'+(g.label||'')+'</div>'
      +'<div class="gs">'+(g.sublabel||'')+'</div></div>'
      +'<div class="gc" style="background:'+fnt+';border:1px solid '+dim+';color:'+col+'">'+(g.items.length)+' picks</div>'
      +'</div>'
      +'<div class="gl" style="background:linear-gradient(90deg,'+dim+',transparent)"></div>';

    g.items.forEach(function(r,idx){
      var uid = 'c'+Date.now()+idx;
      var tk  = (r.type||'STOCK').toLowerCase();
      var TS  = {
        stock:{c:'#00d4ff',b:'rgba(0,212,255,.22)',bg:'rgba(0,212,255,.06)'},
        etf:  {c:'#00ff88',b:'rgba(0,255,136,.18)',bg:'rgba(0,255,136,.05)'},
        bond: {c:'#ffaa00',b:'rgba(255,170,0,.2)', bg:'rgba(255,170,0,.05)'},
        reit: {c:'#fb923c',b:'rgba(251,146,60,.2)',bg:'rgba(251,146,60,.05)'},
      };
      var ts  = TS[tk]||TS.stock;
      var fit = Math.max(0,Math.min(100,parseInt(r.fit,10)||50));
      var fc  = fit>=75?'#00ff88':fit>=50?'#ffaa00':'#ff3a5c';
      var cc  = r.conviction==='HIGH'?'#00ff88':r.conviction==='MEDIUM'?'#ffaa00':'#7ab3cc';

      var cats='';
      if(r.cats&&r.cats.length){
        cats='<div class="cl">▶ KEY CATALYSTS</div><div class="cw">'
          +r.cats.map(function(c){return '<span class="ct2">'+c+'</span>';}).join('')
          +'</div>';
      }
      var risks = r.risks
        ? '<div class="rr"><span class="rl">▶ RISKS</span><span class="rt">'+r.risks+'</span></div>'
        : '';

      h += '<div class="card">'
        +'<div class="ct">'
          +'<span class="tb" style="border:1px solid '+ts.b+';background:'+ts.bg+';color:'+ts.c+'">'+(r.type||'STOCK')+'</span>'
          +'<div style="min-width:0">'
            +'<div class="tkr">'+(r.ticker||'')+'</div>'
            +'<div class="fn">'+(r.name||'')+'</div>'
            +'<div class="stg">'+(r.strategy||'')+'</div>'
          +'</div>'
          +'<div>'
            +'<div class="cv" style="color:'+cc+'">'+(r.conviction||'')+'</div>'
            +'<div class="ht">HOLD: '+(r.hold||'')+'</div>'
          +'</div>'
        +'</div>'
        +'<div class="met">'
          +'<div class="mc"><div class="mk">UPSIDE</div><div class="mv" style="color:#00ff88">'+(r.upside||'—')+'</div></div>'
          +'<div class="mc"><div class="mk">RISK</div><div class="mv" style="color:#e8f4ff">'+(r.risk||'—')+'</div></div>'
          +'<div class="mc"><div class="mk">YIELD</div><div class="mv" style="color:#e8f4ff">'+(r.div||'—')+'</div></div>'
        +'</div>'
        +'<div class="fr">'
          +'<div class="fl">MARKET FIT</div>'
          +'<div class="ft"><div class="ff" style="width:'+fit+'%;background:'+fc+'"></div></div>'
          +'<div class="fp" style="color:'+fc+'">'+fit+'%</div>'
        +'</div>'
        +'<button class="xb" type="button" onclick="toggleCard(\''+uid+'\')">'
          +'<span class="xl">▶ FULL ANALYSIS &amp; REASONING</span>'
          +'<span class="xa" id="xa'+uid+'">▼</span>'
        +'</button>'
        +'<div class="rp" id="'+uid+'">'
          +'<div class="wy">'+hl(r.why||'')+'</div>'
          +cats+risks
        +'</div>'
        +'</div>';
    });
  });

  document.getElementById('RS').innerHTML = h;
  try { document.getElementById('RS').scrollIntoView({behavior:'smooth',block:'start'}); } catch(e){}
}

function toggleCard(uid){
  var p = document.getElementById(uid);
  var a = document.getElementById('xa'+uid);
  if(!p||!a) return;
  var open = p.classList.toggle('open');
  a.classList.toggle('open', open);
}

/* expose globals for onclick */
window.openOverlay  = openOverlay;
window.toggleEye    = toggleEye;
window.submitKey    = submitKey;
window.runAnalysis  = runAnalysis;
window.selStr       = selStr;
window.toggleCard   = toggleCard;

})();
</script>

</body>
</html>
