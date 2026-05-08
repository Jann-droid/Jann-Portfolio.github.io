<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>InvestRadar</title>
<style>
/* ─── EXACT colors from JSX: C.void=#020408 C.deep=#040b12 C.card=#060f18 C.panel=#091422
       C.bd=#0a2030 C.cy=#00d4ff C.gn=#00ff88 C.am=#ffaa00 C.rd=#ff3a5c C.pu=#9b59ff
       C.t1=#e8f4ff C.t2=#7ab3cc C.tm=#3a6070
       C.cyd=rgba(0,212,255,.22) C.cyf=rgba(0,212,255,.06)
       C.gnd=rgba(0,255,136,.18) C.gnf=rgba(0,255,136,.05)  ─── */

*,*::before,*::after { box-sizing:border-box; margin:0; padding:0; }

html { background:#020408; }

body {
background:#020408;
color:#e8f4ff;
font-family: ‘Courier New’, Courier, monospace;
font-size:14px;
min-height:100vh;
-webkit-font-smoothing:antialiased;
}

/* ── BG GRID (from JSX body bg) ── */
body::before {
content:’’;
position:fixed; inset:0; z-index:0; pointer-events:none;
background-image:
linear-gradient(#0a2030 1px, transparent 1px),
linear-gradient(90deg, #0a2030 1px, transparent 1px);
background-size:40px 40px;
opacity:.4;
}
/* ── GLOW ORBS ── */
body::after {
content:’’;
position:fixed; inset:0; z-index:0; pointer-events:none;
background:
radial-gradient(ellipse 45% 35% at 8% 8%, rgba(0,212,255,.06) 0%, transparent 60%),
radial-gradient(ellipse 40% 30% at 92% 92%, rgba(155,89,255,.04) 0%, transparent 60%);
}

/* ── KEYFRAMES ── */
@keyframes PULSE  { 0%,100%{opacity:1} 50%{opacity:.18} }
@keyframes SCROLL { from{transform:translateX(0)} to{transform:translateX(-50%)} }
@keyframes FADEIN { from{opacity:0;transform:translateY(6px)} to{opacity:1;transform:translateY(0)} }

/* ══════════════════════════════════════════════════════
OVERLAY  ——  CSS display:flex so it shows immediately
══════════════════════════════════════════════════════ */
#OV {
position:fixed; inset:0; z-index:9999;
background:rgba(2,4,8,.97);
display:flex;
align-items:center; justify-content:center;
padding:20px;
}
#OV-BOX {
background:#060f18;
border:1px solid rgba(0,212,255,.22);
border-radius:8px;
padding:32px 28px;
width:100%; max-width:420px;
position:relative; overflow:hidden;
}
#OV-BOX::before {
content:’’;
position:absolute; top:0; left:0; right:0; height:2px;
background:linear-gradient(90deg,transparent,#00d4ff,transparent);
}
.ov-logo {
font-size:22px; font-weight:900; letter-spacing:4px;
color:#00d4ff; margin-bottom:4px;
}
.ov-logo s { color:#7ab3cc; font-weight:400; text-decoration:none; }
.ov-sub  { font-size:9px; letter-spacing:3px; color:#3a6070; margin-bottom:22px; }
.ov-body { font-size:13px; color:#7ab3cc; line-height:1.8; margin-bottom:6px; }
.ov-body a { color:#00d4ff; text-decoration:none; }
.ov-body a:hover { text-decoration:underline; }
.ov-hint { font-size:9px; color:#3a6070; line-height:1.7; margin-bottom:16px; }
.key-row { position:relative; margin-bottom:6px; }
#KIN {
width:100%; background:#040b12;
border:1px solid #0d2840; border-radius:4px;
color:#e8f4ff; font-family:inherit; font-size:11px;
padding:11px 42px 11px 13px;
outline:none; transition:border-color .2s; letter-spacing:.5px;
}
#KIN::placeholder { color:#3a6070; }
#KIN:focus { border-color:rgba(0,212,255,.4); }
#EBTN {
position:absolute; right:10px; top:50%; transform:translateY(-50%);
background:none; border:none; color:#3a6070; cursor:pointer;
font-size:14px; padding:2px; transition:color .15s;
}
#EBTN:hover { color:#00d4ff; }
#KERR { font-size:9px; color:#ff3a5c; min-height:16px; margin-bottom:10px; letter-spacing:.5px; }
#SBTN {
width:100%; background:rgba(0,212,255,.06);
border:1px solid #00d4ff; border-radius:4px;
color:#00d4ff; font-family:inherit; font-size:11px; font-weight:700;
letter-spacing:4px; padding:13px; cursor:pointer; text-transform:uppercase;
transition:background .2s, box-shadow .2s;
}
#SBTN:hover { background:rgba(0,212,255,.14); box-shadow:0 0 24px rgba(0,212,255,.12); }
.ov-note { font-size:8px; color:#3a6070; text-align:center; margin-top:14px; line-height:1.7; }

/* ══════════════════════════════════════════════════════
APP  ——  hidden until key entered
══════════════════════════════════════════════════════ */
#APP {
display:none;
position:relative; z-index:1;
max-width:520px; margin:0 auto;
padding-bottom:80px;
}

/* ── HEADER — from JSX padding:“20px 16px 14px” ── */
.hdr {
padding:20px 16px 14px;
border-bottom:1px solid #0a2030;
position:relative;
}
.hdr::after {
content:’’; position:absolute; bottom:0; left:0; right:0; height:1px;
background:linear-gradient(90deg,transparent,#00d4ff,transparent);
}
.hdr-row { display:flex; align-items:center; justify-content:space-between; margin-bottom:4px; }
.logo { font-size:20px; font-weight:900; color:#00d4ff; letter-spacing:4px; }
.logo s { color:#7ab3cc; font-weight:400; text-decoration:none; }
.live { display:flex; align-items:center; gap:6px; font-size:9px; color:#00ff88; letter-spacing:2px; }
.ldot { width:7px; height:7px; border-radius:50%; background:#00ff88; animation:PULSE 2s ease-in-out infinite; }
.hdr-sub { font-size:9px; color:#3a6070; letter-spacing:2px; }

/* ── TICKER — from JSX animation:ir-scroll 28s ── */
.tkr { border-bottom:1px solid #0a2030; padding:8px 0; overflow:hidden; position:relative; }
.tkr::after {
content:’’; position:absolute; right:0; top:0; bottom:0; width:50px;
background:linear-gradient(90deg,transparent,#020408);
pointer-events:none; z-index:2;
}
.tkr-in { display:flex; width:max-content; animation:SCROLL 28s linear infinite; }
.ti { display:flex; align-items:center; gap:6px; padding:0 18px; border-right:1px solid #0a2030; }
.ti-k { font-size:8px; color:#3a6070; letter-spacing:1px; }
.ti-v { font-size:9px; font-weight:700; }

/* ── MARKET PANEL — from JSX: margin:“12px 16px 0” background:C.card border:C.bd ── */
.mkt {
margin:12px 16px 0;
background:#060f18;
border:1px solid #0a2030;
border-radius:6px; overflow:hidden;
}
.mkt-hd {
padding:9px 14px; border-bottom:1px solid #0a2030;
display:flex; align-items:center; justify-content:space-between;
}
.mkt-title { font-size:8px; letter-spacing:3px; color:#00d4ff; }
.mkt-badge {
font-size:8px; letter-spacing:1px; color:#00ff88;
padding:2px 7px; border:1px solid rgba(0,255,136,.18);
background:rgba(0,255,136,.05); border-radius:2px;
}
/* JSX: gap:7 padding:“10px 12px 0” grid-template-columns:“1fr 1fr” */
.mkt-grid {
display:grid; grid-template-columns:1fr 1fr;
gap:7px; padding:10px 12px 0;
}
/* JSX each cell: background:C.deep border:C.bd borderRadius:4 padding:“7px 10px” */
.mkt-cell {
background:#040b12; border:1px solid #0a2030;
border-radius:4px; padding:7px 10px;
}
.mkt-cell.full { grid-column:span 2; }
.mkt-ck { font-size:8px; color:#3a6070; letter-spacing:1px; margin-bottom:2px; }
.mkt-cv { font-size:13px; font-weight:700; }
/* JSX: margin:“10px 12px 12px” fontSize:12 lineHeight:1.7 color:C.t2 borderTop:C.bd paddingTop:10 */
.mkt-sum {
margin:10px 12px 12px;
font-size:12px; line-height:1.7; color:#7ab3cc;
border-top:1px solid #0a2030; padding-top:10px;
}

/* ── SECTION LABEL — JSX: fontFamily:monospace fontSize:9 letterSpacing:3 color:C.tm padding:“14px 16px 8px” ── */
.sec { font-size:9px; letter-spacing:3px; color:#3a6070; padding:14px 16px 8px; }

/* ── STRATEGY GRID — JSX: display:grid gridTemplateColumns:“1fr 1fr” gap:8 padding:“0 16px” ── */
.sg { display:grid; grid-template-columns:1fr 1fr; gap:8px; padding:0 16px; }
/* JSX strat-btn: background:C.card border:C.bd borderRadius:4 padding:“10px 12px” */
.sb {
background:#060f18 !important;
border:1px solid #0a2030 !important;
border-radius:4px; padding:10px 12px;
cursor:pointer; text-align:left;
position:relative; overflow:hidden;
transition:border-color .18s, background .18s;
display:block; width:100%;
/* CRITICAL: prevent any browser stylesheet from making these white */
color:#e8f4ff;
-webkit-appearance:none; appearance:none;
}
.sb:hover { background:#091422 !important; border-color:#0d2840 !important; }
.sb.on    { background:rgba(0,212,255,.06) !important; border-color:#00d4ff !important; }
/* JSX: left accent bar on active */
.sb-bar { position:absolute; top:0; left:0; width:3px; height:100%; background:#00d4ff; display:none; }
.sb.on .sb-bar { display:block; }
/* JSX strat-name: fontFamily:monospace fontSize:10 fontWeight:700 letterSpacing:1 color:C.t2  */
.sb-name { font-size:10px; font-weight:700; letter-spacing:1px; color:#7ab3cc; margin-bottom:3px; transition:color .18s; }
.sb.on .sb-name { color:#00d4ff; }
/* JSX strat-desc: fontSize:11 color:C.tm lineHeight:1.4 */
.sb-desc { font-size:11px; color:#3a6070; line-height:1.4; }

/* ── SLIDERS — JSX: padding:“12px 16px 0” ── */
.sls { padding:12px 16px 0; }
.sl-row { display:flex; align-items:center; gap:12px; margin-bottom:10px; }
.sl-k { font-size:8px; letter-spacing:2px; color:#3a6070; width:40px; flex-shrink:0; }
input[type=range] {
flex:1; height:2px; border-radius:2px;
-webkit-appearance:none; appearance:none;
outline:none; cursor:pointer; border:none;
}
input[type=range]#RSL { background:linear-gradient(90deg,#00ff88,#ffaa00,#ff3a5c); }
input[type=range]#HSL { background:linear-gradient(90deg,#00d4ff,#9b59ff); }
input[type=range]::-webkit-slider-thumb {
-webkit-appearance:none;
width:14px; height:14px; border-radius:50%;
background:#020408; border:2px solid #00d4ff; cursor:pointer;
}
input[type=range]::-moz-range-thumb {
width:14px; height:14px; border-radius:50%;
background:#020408; border:2px solid #00d4ff; cursor:pointer;
}
.sl-v { font-size:9px; color:#00d4ff; width:76px; text-align:right; flex-shrink:0; }

/* ── ANALYZE BTN — JSX: width:“100%” background:transparent border:C.cy borderRadius:4 color:C.cy fontFamily:monospace fontSize:12 fontWeight:700 letterSpacing:3 padding:14 ── */
.abw { padding:14px 16px 0; }
#ABTN {
width:100%; background:transparent;
border:1px solid #00d4ff; border-radius:4px;
color:#00d4ff; font-family:inherit;
font-size:12px; font-weight:700; letter-spacing:3px;
padding:14px; cursor:pointer; text-transform:uppercase;
transition:background .2s, box-shadow .2s;
}
#ABTN:hover { background:rgba(0,212,255,.06); box-shadow:0 0 28px rgba(0,212,255,.1); }
#ABTN:disabled { opacity:.4; cursor:not-allowed; }

/* ── LOADING ── */
#LD { display:none; padding:30px 16px; text-align:center; }
#LD.show { display:block; }
.ld-i { font-size:9px; letter-spacing:3px; color:#00d4ff; margin-bottom:8px; }
.ld-t { font-size:9px; letter-spacing:2px; color:#3a6070; }

/* ── RESULTS wrapper ── */
#RS { /* populated by JS */ }

/* ── RESULTS BAR ── */
.rs-bar {
display:flex; align-items:center; justify-content:space-between;
margin-bottom:12px; padding-bottom:10px;
border-bottom:1px solid #0a2030;
}
/* JSX: fontFamily:monospace fontSize:10 color:C.tm letterSpacing:3 */
.rs-title { font-size:10px; color:#3a6070; letter-spacing:3px; }
.pills { display:flex; gap:6px; align-items:center; flex-wrap:wrap; }
.pill {
font-size:9px; font-weight:700; letter-spacing:1px;
padding:2px 8px; border-radius:3px;
}
.sig { font-size:10px; color:#00ff88; margin-left:4px; }

/* ── GROUP HEADER — JSX margin:“20px 0 8px” ── */
.gh { display:flex; align-items:center; gap:10px; margin:20px 0 8px; }
/* JSX icon: width:30 height:30 borderRadius:4 fontSize:14 */
.gi { width:30px; height:30px; border-radius:4px; display:flex; align-items:center; justify-content:center; font-size:14px; flex-shrink:0; }
/* JSX group-name: fontFamily:monospace fontSize:11 fontWeight:700 letterSpacing:3 */
.gn { font-size:11px; font-weight:700; letter-spacing:3px; }
/* JSX group-sub: fontFamily:monospace fontSize:9 color:C.tm letterSpacing:1 marginTop:2 */
.gs { font-size:9px; color:#3a6070; letter-spacing:1px; margin-top:2px; }
.gc { font-size:9px; padding:3px 8px; border-radius:2px; margin-left:auto; flex-shrink:0; }
.gl { height:1px; margin-bottom:10px; }

/* ── INVESTMENT CARD — JSX background:C.card border:C.bd borderRadius:6 marginBottom:10 ── */
.card {
background:#060f18; border:1px solid #0a2030;
border-radius:6px; margin-bottom:10px; overflow:hidden;
}
/* JSX card-top: padding:14 grid grid-template-columns:“auto 1fr auto” gap:“0 12px” ── */
.ct { padding:14px; display:grid; grid-template-columns:auto 1fr auto; gap:0 12px; align-items:start; }

/* JSX type badge: fontFamily:monospace fontSize:8 letterSpacing:2 padding:“3px 7px” borderRadius:2 marginTop:2 */
.tb { font-size:8px; letter-spacing:2px; padding:3px 7px; border-radius:2px; margin-top:2px; white-space:nowrap; }

/* JSX ticker: fontFamily:monospace fontSize:17 fontWeight:700 color:C.t1 letterSpacing:1 lineHeight:1 marginBottom:3 */
.tkrtxt { font-size:17px; font-weight:700; color:#e8f4ff; letter-spacing:1px; line-height:1; margin-bottom:3px; }
/* JSX full-name: fontSize:12 color:C.t2 whitespace:nowrap overflow:hidden textOverflow:ellipsis marginBottom:4 */
.fn { font-size:12px; color:#7ab3cc; white-space:nowrap; overflow:hidden; text-overflow:ellipsis; margin-bottom:4px; }
/* JSX strat-tag: fontFamily:monospace fontSize:9 color:C.tm letterSpacing:1 */
.stg { font-size:9px; color:#3a6070; letter-spacing:1px; }
/* JSX conviction: fontFamily:monospace fontSize:11 fontWeight:700 letterSpacing:1 marginBottom:4 text-align:right */
.cv { font-size:11px; font-weight:700; letter-spacing:1px; margin-bottom:4px; text-align:right; }
/* JSX hold: fontFamily:monospace fontSize:9 color:C.tm letterSpacing:1 white-space:nowrap */
.ht { font-size:9px; color:#3a6070; letter-spacing:1px; white-space:nowrap; text-align:right; }

/* JSX metrics row: display:grid gridTemplateColumns:“repeat(3,1fr)” borderTop:C.bd */
.mrow { display:grid; grid-template-columns:repeat(3,1fr); border-top:1px solid #0a2030; }
/* JSX metric cell: padding:“7px 10px” borderRight:C.bd textAlign:center */
.mc { padding:7px 10px; border-right:1px solid #0a2030; text-align:center; }
.mc:last-child { border-right:none; }
/* JSX metric label: fontFamily:monospace fontSize:8 color:C.tm letterSpacing:1 display:block marginBottom:2 */
.mk { font-size:8px; color:#3a6070; letter-spacing:1px; display:block; margin-bottom:2px; }
/* JSX metric value: fontFamily:monospace fontSize:12 fontWeight:700 */
.mv { font-size:12px; font-weight:700; }

/* JSX fit row: borderTop:C.bd padding:“7px 14px” display:flex alignItems:center gap:8 background:rgba(0,0,0,.15) */
.frow {
border-top:1px solid #0a2030; padding:7px 14px;
display:flex; align-items:center; gap:8px;
background:rgba(0,0,0,.15);
}
/* JSX fit label: fontFamily:monospace fontSize:8 color:C.tm letterSpacing:1 width:60 flexShrink:0 */
.flbl { font-size:8px; color:#3a6070; letter-spacing:1px; width:60px; flex-shrink:0; }
.ftrk { flex:1; height:3px; background:#0a2030; border-radius:2px; overflow:hidden; }
.ffll { height:100%; border-radius:2px; }
/* JSX fit pct: fontFamily:monospace fontSize:10 fontWeight:600 width:32 textAlign:right */
.fpct { font-size:10px; font-weight:600; width:32px; text-align:right; flex-shrink:0; }

/* JSX expand btn: width:“100%” background:transparent border:none borderTop:C.bd padding:“8px 14px” display:flex alignItems:center justifyContent:space-between cursor:pointer color:C.cy */
.xbtn {
width:100%; background:transparent; border:none;
border-top:1px solid #0a2030; padding:8px 14px;
display:flex; align-items:center; justify-content:space-between;
cursor:pointer; color:#00d4ff; font-family:inherit;
transition:background .15s;
}
.xbtn:hover { background:rgba(0,212,255,.06); }
/* JSX expand label: fontFamily:monospace fontSize:9 letterSpacing:2 */
.xlbl { font-size:9px; letter-spacing:2px; }
.xarr { font-size:10px; transition:transform .2s; display:inline-block; }
.xarr.open { transform:rotate(180deg); }

/* JSX reasoning panel: borderTop:C.bd background:C.deep padding:16 display:none */
.rp { display:none; border-top:1px solid #0a2030; background:#040b12; padding:16px; }
.rp.open { display:block; animation:FADEIN .2s ease; }
/* JSX why text: fontSize:13 lineHeight:1.8 color:C.t2 marginBottom:12 */
.wy { font-size:13px; line-height:1.8; color:#7ab3cc; margin-bottom:12px; }
/* JSX cats label: fontFamily:monospace fontSize:8 color:C.tm letterSpacing:2 marginBottom:6 */
.cl { font-size:8px; color:#3a6070; letter-spacing:2px; margin-bottom:6px; }
.cw { display:flex; flex-wrap:wrap; gap:5px; margin-bottom:10px; }
/* JSX cat tag: fontFamily:monospace fontSize:9 padding:“3px 8px” borderRadius:2 border:C.gnd background:C.gnf color:C.gn */
.ctg { font-size:9px; padding:3px 8px; border-radius:2px; border:1px solid rgba(0,255,136,.18); background:rgba(0,255,136,.05); color:#00ff88; }
/* JSX risks: fontSize:12 color:#ff7a8a lineHeight:1.5 */
.rrow { border-top:1px solid #0a2030; padding-top:8px; margin-top:4px; }
.rlbl { font-size:8px; color:#3a6070; letter-spacing:2px; }
.rtxt { font-size:12px; color:#ff7a8a; line-height:1.5; }
.hi { color:#00d4ff; font-weight:700; }

/* ── ERROR ── */
.eb { padding:14px 16px; background:rgba(255,58,92,.06); border:1px solid rgba(255,58,92,.3); border-radius:4px; font-size:9px; color:#ff3a5c; line-height:1.7; }

/* ── DISCLAIMER — JSX: margin:“16px 16px 0” padding:“10px 14px” border:C.bd borderRadius:4 fontFamily:monospace fontSize:9 color:C.tm lineHeight:1.6 ── */
.disc { margin:16px 16px 0; padding:10px 14px; border:1px solid #0a2030; border-radius:4px; font-size:9px; color:#3a6070; line-height:1.6; }

/* ── CHANGE KEY ── */
#CKB {
position:fixed; bottom:16px; right:16px; z-index:100;
background:#091422; border:1px solid #0a2030; border-radius:4px;
color:#3a6070; font-family:inherit; font-size:8px; letter-spacing:1px;
padding:8px 12px; cursor:pointer; transition:border-color .2s, color .2s;
}
#CKB:hover { border-color:rgba(0,212,255,.22); color:#00d4ff; }

/* ── RESULTS WRAP padding from JSX ── */
.rs-wrap { padding:14px 16px 0; }
</style>

</head>
<body>

<!-- ══════════ OVERLAY ══════════ -->

<div id="OV">
  <div id="OV-BOX">
    <div class="ov-logo">INVEST<s>RADAR</s></div>
    <div class="ov-sub">// AI INVESTMENT INTELLIGENCE · CLAUDE-POWERED</div>
    <div class="ov-body">
      Enter your <strong style="color:#00d4ff">Anthropic API Key</strong> to start.<br>
      Stored locally in your browser only — never sent anywhere else.
    </div>
    <div class="ov-hint">
      Get a key at <a href="https://console.anthropic.com" target="_blank" rel="noopener">console.anthropic.com</a>
      → API Keys → Create Key<br>
      (~€5 credit covers 100+ analyses)
    </div>
    <div class="key-row">
      <input id="KIN" type="password" placeholder="sk-ant-api03-..." autocomplete="off" spellcheck="false"/>
      <button id="EBTN" type="button" onclick="toggleEye()">◉</button>
    </div>
    <div id="KERR"></div>
    <button id="SBTN" type="button" onclick="submitKey()">▶ START</button>
    <div class="ov-note">Key is saved so you won't need to re-enter it · Use ⚙ to change</div>
  </div>
</div>

<!-- ══════════ APP ══════════ -->

<div id="APP">

  <!-- Header -->

  <div class="hdr">
    <div class="hdr-row">
      <div class="logo">INVEST<s>RADAR</s></div>
      <div class="live"><div class="ldot"></div>LIVE · MAY 2026</div>
    </div>
    <div class="hdr-sub">// Claude-powered · AI investment analysis · BYOK</div>
  </div>

  <!-- Ticker -->

  <div class="tkr"><div class="tkr-in" id="TKRI"></div></div>

  <!-- Market panel -->

  <div class="mkt">
    <div class="mkt-hd">
      <div class="mkt-title">// MARKET CONTEXT</div>
      <div class="mkt-badge">LIVE · MAY 5 2026</div>
    </div>
    <div class="mkt-grid" id="MGRID"></div>
    <div class="mkt-sum" id="MSUM"></div>
  </div>

  <!-- Strategy -->

  <div class="sec">// SELECT STRATEGY</div>
  <div class="sg" id="SG"></div>

  <!-- Sliders -->

  <div class="sls">
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

  <!-- Analyze button -->

  <div class="abw">
    <button id="ABTN" type="button" onclick="runAnalysis()">▶ ANALYSE STARTEN</button>
  </div>

  <!-- Loading -->

  <div id="LD">
    <div class="ld-i">■ ANALYSING MARKETS</div>
    <div class="ld-t" id="LTXT">Claude is thinking...</div>
  </div>

  <!-- Results -->

  <div id="RS"></div>

  <!-- Disclaimer -->

  <div class="disc">⚠ AI-generated analysis · May 2026 · Educational only — not financial advice · Consult a licensed advisor before investing</div>
</div>

<button id="CKB" type="button" onclick="openOverlay()">⚙ API KEY</button>

<script>
(function(){
'use strict';

/* ═══ DATA ═════════════════════════════════════════════ */
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
  {k:'Fed Rate',  v:'4.25–4.50%', t:'neu'},
  {k:'10Y Yield', v:'4.37%',      t:'up'},
  {k:'CPI YoY',   v:'2.4%',       t:'dn'},
  {k:'VIX',       v:'22.3',       t:'up'},
  {k:'Gold',      v:'$3,271',     t:'up'},
  {k:'WTI Oil',   v:'$56.42',     t:'dn'},
  {k:'BTC',       v:'$94,200',    t:'up'},
  {k:'USD Index', v:'99.4',       t:'dn'},
];

var MARKET_SUMMARY = 'Markets in May 2026 are navigating significant macro turbulence. The S&P 500 has fallen ~<span class="hi">17%</span> from its February peak as US tariff policy (<span class="hi">90-day pause</span> on reciprocal tariffs, <span class="hi">145%</span> on China) creates uncertainty. The Fed holds at <span class="hi">4.25–4.50%</span> — markets price two cuts in 2026. CPI has cooled to <span class="hi">2.4%</span> YoY. Gold at <span class="hi">$3,271</span> reflects safe-haven demand; oil collapsed to <span class="hi">$56.42</span>. BTC at <span class="hi">$94,200</span> outperforms as a macro hedge.';

var MKT_CTX = [
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

/* ═══ STATE ════════════════════════════════════════════ */
var KEY  = '';
var STR  = 'value';
var TMRZ = null;

/* ═══ BOOT ═════════════════════════════════════════════ */
document.addEventListener('DOMContentLoaded', function(){
  var saved = '';
  try { saved = localStorage.getItem('ir_key') || ''; } catch(e){}
  if (saved) document.getElementById('KIN').value = saved;

  document.getElementById('KIN').addEventListener('keydown', function(e){
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

/* ═══ OVERLAY ══════════════════════════════════════════ */
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
  var i = document.getElementById('KIN');
  i.type = i.type === 'password' ? 'text' : 'password';
}
function submitKey(){
  var v = (document.getElementById('KIN').value || '').trim();
  var e = document.getElementById('KERR');
  if (!v)                         { e.textContent = '⚠ Please enter your API key.'; return; }
  if (v.indexOf('sk-ant-') !== 0) { e.textContent = '⚠ Invalid — must start with sk-ant-'; return; }
  e.textContent = ''; KEY = v;
  try { localStorage.setItem('ir_key', v); } catch(x){}
  closeOverlay();
}

/* ═══ BUILDERS ═════════════════════════════════════════ */
function tc(t){ return t==='up'?'#00ff88':t==='dn'?'#ff3a5c':'#7ab3cc'; }

function buildTicker(){
  var d = TICKS.concat(TICKS);
  document.getElementById('TKRI').innerHTML = d.map(function(t){
    return '<div class="ti"><span class="ti-k">'+t.k+'</span>'
          +'<span class="ti-v" style="color:'+tc(t.t)+'">'+t.v+'</span></div>';
  }).join('');
}

function buildMktGrid(){
  // 10 cells + 1 full-width sentiment cell
  var cells = TICKS.map(function(t){
    var isR = t.k==='VIX'||t.k==='CPI YoY';
    var c = isR ? (t.t==='up'?'#ffaa00':t.t==='dn'?'#00ff88':'#e8f4ff') : tc(t.t);
    return '<div class="mkt-cell"><div class="mkt-ck">'+t.k+'</div>'
          +'<div class="mkt-cv" style="color:'+c+'">'+t.v+'</div></div>';
  }).join('');
  // Sentiment cell spans 2 cols
  cells += '<div class="mkt-cell full"><div class="mkt-ck" style="letter-spacing:1px">MARKET SENTIMENT</div>'
          +'<div class="mkt-cv" style="color:#ffaa00;font-size:11px">RISK-OFF / TARIFF UNCERTAINTY</div></div>';
  document.getElementById('MGRID').innerHTML = cells;
  document.getElementById('MSUM').innerHTML  = MARKET_SUMMARY;
}

function buildStrats(){
  document.getElementById('SG').innerHTML = STRATS.map(function(s){
    var on = s.k === STR;
    return '<button class="sb'+(on?' on':'')+'" type="button" onclick="selStr(\''+s.k+'\')">'
          +'<div class="sb-bar"></div>'
          +'<div class="sb-name">'+s.l+'</div>'
          +'<div class="sb-desc">'+s.d+'</div>'
          +'</button>';
  }).join('');
}

function selStr(k){ STR = k; buildStrats(); }

/* ═══ ANALYSIS ═════════════════════════════════════════ */
function runAnalysis(){
  if (!KEY || KEY.indexOf('sk-ant-') !== 0){ openOverlay(); return; }

  var r = parseInt(document.getElementById('RSL').value, 10);
  var h = parseInt(document.getElementById('HSL').value, 10);
  var rl = RL[r-1], hl = HL[h-1];

  document.getElementById('RS').innerHTML = '';
  document.getElementById('LD').classList.add('show');
  document.getElementById('ABTN').disabled = true;

  var dots = 0;
  if (TMRZ) clearInterval(TMRZ);
  TMRZ = setInterval(function(){
    dots = (dots+1)%4;
    document.getElementById('LTXT').textContent = 'Claude is analysing'+'.'.repeat(dots+1);
  }, 450);

  function done(){
    clearInterval(TMRZ); TMRZ = null;
    document.getElementById('LD').classList.remove('show');
    document.getElementById('ABTN').disabled = false;
  }

  var prompt =
    'You are a professional investment analyst.\n\n'
    + MKT_CTX + '\n\n'
    + 'USER PARAMETERS:\n'
    + '- Strategy: ' + STR.toUpperCase() + '\n'
    + '- Risk Tolerance: ' + rl + ' (' + r + '/5)\n'
    + '- Hold Period: ' + hl + '\n\n'
    + 'Generate exactly 10 picks: 5 LARGE-CAP and 5 SMALL-CAP.\n'
    + 'All picks must fit ' + STR.toUpperCase() + ' strategy with ' + rl + ' risk and ' + hl + ' hold.\n'
    + 'Reference specific market data numbers in each why field.\n\n'
    + 'CRITICAL: Output ONLY raw JSON — no markdown, no code fences, no text before/after.\n\n'
    + 'Exact schema:\n'
    + '{"groups":['
    + '{"label":"LARGE-CAP","sublabel":"Established leaders & major funds","items":['
    + '{"ticker":"X","name":"Full Name","type":"STOCK|ETF|BOND|REIT",'
    + '"strategy":"short label max 5 words","conviction":"HIGH|MEDIUM|LOW",'
    + '"hold":"e.g. 2–4 YRS","upside":"+XX%","risk":"LOW|MED|HIGH",'
    + '"div":"X.X%|N/A","fit":80,'
    + '"why":"3-4 sentences with specific numbers from the market data.",'
    + '"cats":["Catalyst 1","Catalyst 2","Catalyst 3"],'
    + '"risks":"One sentence on key risks."}'
    + ']},'
    + '{"label":"SMALL-CAP","sublabel":"High-potential opportunities","items":[...5 items same schema...]}'
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
        throw new Error((b&&b.error&&b.error.message?b.error.message:'Error')+' ('+res.status+')');
      }).catch(function(e2){
        if (e2.message) throw e2;
        throw new Error('HTTP '+res.status);
      });
    }
    return res.json();
  })
  .then(function(data){
    var txt = '';
    (data.content||[]).forEach(function(b){ if(b.type==='text') txt += b.text; });
    txt = txt.trim().replace(/^```[\w]*\s*/,'').replace(/\s*```$/,'').trim();
    var s = txt.indexOf('{'), e = txt.lastIndexOf('}');
    if (s===-1||e===-1) throw new Error('No JSON in response. Try again.');
    var parsed = JSON.parse(txt.slice(s,e+1));
    if (!parsed.groups||!Array.isArray(parsed.groups)) throw new Error('Unexpected JSON structure.');
    done();
    render(parsed.groups, rl, hl);
  })
  .catch(function(e){
    done();
    var m = e&&e.message ? e.message : String(e);
    var x = '';
    if (m.indexOf('401')!==-1) x = ' — Invalid API key, please re-enter.';
    else if (m.indexOf('429')!==-1) x = ' — Rate limit, wait a moment.';
    else if (/failed to fetch|load failed|networkerror/i.test(m))
      x = ' — Network error. Access via HTTPS (GitHub Pages), not file://.';
    document.getElementById('RS').innerHTML = '<div class="rs-wrap"><div class="eb">⚠ '+m+x+'</div></div>';
  });
}

/* ═══ RENDER ═══════════════════════════════════════════ */
function hl2(t){
  if (!t) return '';
  // escape HTML first
  t = t.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
  // highlight numbers
  t = t.replace(/(\$[\d,]+(?:\.\d+)?[BMK]?|\d+(?:\.\d+)?x|\d+(?:\.\d+)?%)/g,'<span class="hi">$1</span>');
  // highlight abbreviations
  t = t.replace(/\b(P\/E|FCF|CAGR|EBITDA|YoY|ATH|VIX|CPI|NIM|PEG|ROE|AUM|WTI|LNG|ETF|REIT|GAAP|AI|EV)\b/g,'<span class="hi">$1</span>');
  return t;
}

function mkPill(txt, col){
  return '<span class="pill" style="background:'+col+'18;border:1px solid '+col+'33;color:'+col+'">'+txt+'</span>';
}

function render(groups, rl, hl){
  var total = 0;
  (groups||[]).forEach(function(g){ total += (g.items||[]).length; });

  var H = '<div class="rs-wrap">';

  // results bar
  H += '<div class="rs-bar">'
     +'<div class="rs-title">// RECOMMENDATIONS</div>'
     +'<div class="pills">'
     + mkPill(STR.toUpperCase(),'#00d4ff')
     + mkPill(rl, '#ffaa00')
     + mkPill(hl, '#9b59ff')
     +'<span class="sig">'+total+' SIG</span>'
     +'</div></div>';

  (groups||[]).forEach(function(g){
    if (!g.items||!g.items.length) return;
    var sm  = g.label.indexOf('SMALL')!==-1;
    var col = sm?'#00ff88':'#00d4ff';
    var dim = sm?'rgba(0,255,136,.18)':'rgba(0,212,255,.22)';
    var fnt = sm?'rgba(0,255,136,.05)':'rgba(0,212,255,.06)';
    var ico = sm?'◇':'◈';

    // group header — from JSX Group component
    H += '<div class="gh">'
       +'<div class="gi" style="background:'+fnt+';border:1px solid '+dim+';color:'+col+'">'+ico+'</div>'
       +'<div><div class="gn" style="color:'+col+'">'+(g.label||'')+'</div>'
       +'<div class="gs">'+(g.sublabel||'')+'</div></div>'
       +'<div class="gc" style="background:'+fnt+';border:1px solid '+dim+';color:'+col+'">'+g.items.length+' PICKS</div>'
       +'</div>'
       +'<div class="gl" style="background:linear-gradient(90deg,'+dim+',transparent)"></div>';

    g.items.forEach(function(r, idx){
      var uid = 'c'+Date.now()+'-'+idx;
      var tk  = (r.type||'STOCK').toLowerCase();
      var TS  = {
        stock:{c:'#00d4ff',b:'rgba(0,212,255,.22)',bg:'rgba(0,212,255,.06)'},
        etf:  {c:'#00ff88',b:'rgba(0,255,136,.18)',bg:'rgba(0,255,136,.05)'},
        bond: {c:'#ffaa00',b:'rgba(255,170,0,.2)', bg:'rgba(255,170,0,.05)'},
        reit: {c:'#fb923c',b:'rgba(251,146,60,.2)',bg:'rgba(251,146,60,.05)'},
      };
      var ts  = TS[tk]||TS.stock;
      var fit = Math.max(0,Math.min(100,parseInt(r.fit,10)||50));
      // from JSX: fit>=70 green, fit>=40 amber, else red
      var fc  = fit>=70?'#00ff88':fit>=40?'#ffaa00':'#ff3a5c';
      var cc  = r.conviction==='HIGH'?'#00ff88':r.conviction==='MEDIUM'?'#ffaa00':'#7ab3cc';

      var cats='';
      if (r.cats&&r.cats.length){
        cats='<div class="cl">▶ KEY CATALYSTS</div><div class="cw">'
            +r.cats.map(function(c){return '<span class="ctg">'+c+'</span>';}).join('')
            +'</div>';
      }
      var risks = r.risks
        ?'<div class="rrow"><span class="rlbl">■ KEY RISKS &nbsp;</span><span class="rtxt">'+r.risks+'</span></div>'
        :'';

      H += '<div class="card">'
          // top grid: type badge | ticker info | conviction
          +'<div class="ct">'
            +'<span class="tb" style="border:1px solid '+ts.b+';background:'+ts.bg+';color:'+ts.c+'">'+(r.type||'STOCK')+'</span>'
            +'<div style="min-width:0">'
              +'<div class="tkrtxt">'+(r.ticker||'')+'</div>'
              +'<div class="fn">'+(r.name||'')+'</div>'
              +'<div class="stg">'+(r.strategy||'')+'</div>'
            +'</div>'
            +'<div>'
              +'<div class="cv" style="color:'+cc+'">'+(r.conviction||'')+'</div>'
              +'<div class="ht">HOLD: '+(r.hold||'')+'</div>'
            +'</div>'
          +'</div>'
          // metrics
          +'<div class="mrow">'
            +'<div class="mc"><span class="mk">UPSIDE</span><span class="mv" style="color:#00ff88">'+(r.upside||'—')+'</span></div>'
            +'<div class="mc"><span class="mk">RISK</span><span class="mv" style="color:#e8f4ff">'+(r.risk||'—')+'</span></div>'
            +'<div class="mc"><span class="mk">DIV YIELD</span><span class="mv" style="color:#e8f4ff">'+(r.div||'—')+'</span></div>'
          +'</div>'
          // fit bar
          +'<div class="frow">'
            +'<div class="flbl">MARKET FIT</div>'
            +'<div class="ftrk"><div class="ffll" style="width:'+fit+'%;background:'+fc+'"></div></div>'
            +'<div class="fpct" style="color:'+fc+'">'+fit+'%</div>'
          +'</div>'
          // expand button
          +'<button class="xbtn" type="button" onclick="tog(\''+uid+'\')">'
            +'<span class="xlbl">▶ FULL ANALYSIS &amp; REASONING</span>'
            +'<span class="xarr" id="xa-'+uid+'">▼</span>'
          +'</button>'
          // reasoning panel
          +'<div class="rp" id="'+uid+'">'
            +'<div class="wy">'+hl2(r.why||'')+'</div>'
            +cats+risks
          +'</div>'
          +'</div>';
    });
  });

  H += '</div>';
  document.getElementById('RS').innerHTML = H;
  try { document.getElementById('RS').scrollIntoView({behavior:'smooth',block:'start'}); } catch(x){}
}

function tog(uid){
  var p = document.getElementById(uid);
  var a = document.getElementById('xa-'+uid);
  if (!p||!a) return;
  p.classList.toggle('open');
  a.classList.toggle('open', p.classList.contains('open'));
}

/* expose to global scope for onclick handlers */
window.openOverlay = openOverlay;
window.toggleEye   = toggleEye;
window.submitKey   = submitKey;
window.runAnalysis = runAnalysis;
window.selStr      = selStr;
window.tog         = tog;

})();
</script>

</body>
</html>
