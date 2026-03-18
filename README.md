[nifty_pretrade_analyzer (1).html](https://github.com/user-attachments/files/26090412/nifty_pretrade_analyzer.1.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NIFTY Options — Pre-Trade Analyzer</title>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500;600&family=IBM+Plex+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #0a0c0f;
  --surface: #111418;
  --surface2: #181c22;
  --border: #232830;
  --border2: #2e3540;
  --text: #e8eaf0;
  --muted: #5a6370;
  --muted2: #3d4550;
  --green: #00d68f;
  --green-bg: rgba(0,214,143,0.08);
  --green-border: rgba(0,214,143,0.2);
  --amber: #f5a623;
  --amber-bg: rgba(245,166,35,0.08);
  --amber-border: rgba(245,166,35,0.2);
  --red: #ff4d4d;
  --red-bg: rgba(255,77,77,0.08);
  --red-border: rgba(255,77,77,0.2);
  --blue: #4d9eff;
  --blue-bg: rgba(77,158,255,0.08);
  --mono: 'IBM Plex Mono', monospace;
  --sans: 'IBM Plex Sans', sans-serif;
}
* { box-sizing: border-box; margin: 0; padding: 0; }
body { background: var(--bg); color: var(--text); font-family: var(--sans); font-size: 14px; min-height: 100vh; }

/* Layout */
.shell { display: grid; grid-template-columns: 320px 1fr; min-height: 100vh; }
.panel-left { background: var(--surface); border-right: 1px solid var(--border); padding: 24px 20px; overflow-y: auto; }
.panel-right { padding: 24px 20px; overflow-y: auto; }

/* Header */
.app-header { margin-bottom: 24px; }
.app-title { font-family: var(--mono); font-size: 13px; font-weight: 600; color: var(--green); letter-spacing: .12em; text-transform: uppercase; }
.app-sub { font-size: 11px; color: var(--muted); margin-top: 3px; font-family: var(--mono); }

/* Sections */
.sec { margin-bottom: 24px; }
.sec-label { font-family: var(--mono); font-size: 10px; font-weight: 600; letter-spacing: .14em; color: var(--muted); text-transform: uppercase; margin-bottom: 12px; }

/* Fields */
.field { margin-bottom: 14px; }
.field-header { display: flex; justify-content: space-between; align-items: baseline; margin-bottom: 6px; }
.field-name { font-size: 12px; color: var(--muted); }
.field-val { font-family: var(--mono); font-size: 13px; font-weight: 500; color: var(--text); }
input[type=range] {
  -webkit-appearance: none; width: 100%; height: 3px;
  background: var(--border2); border-radius: 2px; outline: none; cursor: pointer;
}
input[type=range]::-webkit-slider-thumb {
  -webkit-appearance: none; width: 14px; height: 14px; border-radius: 50%;
  background: var(--green); border: 2px solid var(--bg); cursor: pointer;
  transition: transform .1s;
}
input[type=range]:hover::-webkit-slider-thumb { transform: scale(1.2); }

/* Select */
select {
  width: 100%; background: var(--surface2); border: 1px solid var(--border2);
  color: var(--text); font-family: var(--sans); font-size: 13px;
  padding: 8px 10px; border-radius: 4px; outline: none; cursor: pointer;
  appearance: none; -webkit-appearance: none;
}

/* Toggle row */
.toggle-row { display: flex; gap: 6px; margin-bottom: 14px; }
.tog { flex: 1; padding: 7px 4px; text-align: center; font-size: 11px; font-family: var(--mono);
  border: 1px solid var(--border2); border-radius: 4px; cursor: pointer; color: var(--muted);
  transition: all .15s; user-select: none; }
.tog.active { border-color: var(--green); color: var(--green); background: var(--green-bg); }
.tog.active.bear { border-color: var(--red); color: var(--red); background: var(--red-bg); }

/* Counters */
.counter-row { display: flex; gap: 8px; margin-bottom: 14px; }
.counter { flex: 1; display: flex; align-items: center; gap: 6px; }
.counter-btn { width: 26px; height: 26px; border: 1px solid var(--border2); border-radius: 4px;
  background: none; color: var(--text); font-size: 16px; cursor: pointer; display: flex;
  align-items: center; justify-content: center; transition: background .1s; }
.counter-btn:hover { background: var(--surface2); }
.counter-val { font-family: var(--mono); font-size: 13px; min-width: 20px; text-align: center; }
.counter-label { font-size: 11px; color: var(--muted); }

/* Right panel grid */
.results-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 16px; }
.verdict-pill { font-family: var(--mono); font-size: 11px; font-weight: 600; letter-spacing: .1em;
  padding: 5px 14px; border-radius: 20px; text-transform: uppercase; transition: all .2s; }
.verdict-pill.go { background: var(--green-bg); color: var(--green); border: 1px solid var(--green-border); }
.verdict-pill.warn { background: var(--amber-bg); color: var(--amber); border: 1px solid var(--amber-border); }
.verdict-pill.no { background: var(--red-bg); color: var(--red); border: 1px solid var(--red-border); }
.verdict-msg { font-size: 13px; color: var(--muted); margin-left: 4px; }

/* Stat grid */
.stat-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px; margin-bottom: 16px; }
.stat { background: var(--surface); border: 1px solid var(--border); border-radius: 6px; padding: 12px 14px; }
.stat-label { font-size: 10px; font-family: var(--mono); color: var(--muted); letter-spacing: .08em; text-transform: uppercase; margin-bottom: 6px; }
.stat-value { font-family: var(--mono); font-size: 20px; font-weight: 500; }
.stat-hint { font-size: 10px; color: var(--muted); margin-top: 3px; }
.stat.ok .stat-value { color: var(--green); }
.stat.warn .stat-value { color: var(--amber); }
.stat.bad .stat-value { color: var(--red); }
.stat.neutral .stat-value { color: var(--text); }

/* Analysis blocks */
.analysis-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 16px; }
.block { background: var(--surface); border: 1px solid var(--border); border-radius: 6px; padding: 14px 16px; }
.block-title { font-family: var(--mono); font-size: 10px; font-weight: 600; letter-spacing: .1em; text-transform: uppercase; color: var(--muted); margin-bottom: 10px; }
.row-item { display: flex; justify-content: space-between; align-items: center; padding: 5px 0; border-bottom: 1px solid var(--border); }
.row-item:last-child { border-bottom: none; }
.row-key { font-size: 12px; color: var(--muted); }
.row-v { font-family: var(--mono); font-size: 12px; font-weight: 500; }
.row-v.ok { color: var(--green); }
.row-v.warn { color: var(--amber); }
.row-v.bad { color: var(--red); }
.row-v.neu { color: var(--text); }

/* Checks list */
.checklist { display: flex; flex-direction: column; gap: 6px; margin-bottom: 16px; }
.check-item { display: flex; align-items: flex-start; gap: 10px; padding: 10px 14px;
  border-radius: 6px; border: 1px solid; font-size: 13px; transition: all .2s; }
.check-item.pass { background: var(--green-bg); border-color: var(--green-border); }
.check-item.fail { background: var(--red-bg); border-color: var(--red-border); }
.check-item.caution { background: var(--amber-bg); border-color: var(--amber-border); }
.check-dot { width: 7px; height: 7px; border-radius: 50%; margin-top: 4px; flex-shrink: 0; }
.pass .check-dot { background: var(--green); }
.fail .check-dot { background: var(--red); }
.caution .check-dot { background: var(--amber); }
.check-content { flex: 1; }
.check-title { font-weight: 500; margin-bottom: 2px; }
.pass .check-title { color: var(--green); }
.fail .check-title { color: var(--red); }
.caution .check-title { color: var(--amber); }
.check-body { font-size: 11px; color: var(--muted); line-height: 1.5; }

/* Expiry table */
.expiry-table { width: 100%; border-collapse: collapse; }
.expiry-table th { font-family: var(--mono); font-size: 10px; letter-spacing: .08em; color: var(--muted); text-transform: uppercase; text-align: left; padding: 6px 10px; border-bottom: 1px solid var(--border); }
.expiry-table td { font-size: 12px; padding: 7px 10px; border-bottom: 1px solid var(--border); }
.expiry-table tr:last-child td { border-bottom: none; }
.expiry-table tr.highlight td { background: var(--surface2); }

/* Trade scenario */
.scenario { background: var(--surface); border: 1px solid var(--border); border-radius: 6px; padding: 16px; margin-bottom: 10px; }
.scenario-header { font-family: var(--mono); font-size: 10px; font-weight: 600; letter-spacing: .12em; text-transform: uppercase; color: var(--muted); margin-bottom: 12px; }
.scenario-grid { display: grid; grid-template-columns: repeat(3,1fr); gap: 8px; }
.scenario-item { text-align: center; }
.scenario-item .s-val { font-family: var(--mono); font-size: 16px; font-weight: 500; }
.scenario-item .s-lbl { font-size: 10px; color: var(--muted); margin-top: 3px; }

/* Day indicator */
.day-row { display: flex; gap: 6px; margin-top: 8px; }
.day-pill { flex: 1; text-align: center; font-family: var(--mono); font-size: 10px; padding: 5px 2px; border-radius: 3px; border: 1px solid var(--border); color: var(--muted); }
.day-pill.good { border-color: var(--green-border); color: var(--green); background: var(--green-bg); }
.day-pill.mixed { border-color: var(--amber-border); color: var(--amber); background: var(--amber-bg); }
.day-pill.fast { border-color: var(--blue); color: var(--blue); background: var(--blue-bg); }

/* Scrollbar */
::-webkit-scrollbar { width: 4px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 2px; }

@media (max-width: 900px) {
  .shell { grid-template-columns: 1fr; }
  .panel-left { border-right: none; border-bottom: 1px solid var(--border); }
  .stat-grid { grid-template-columns: repeat(2,1fr); }
  .analysis-grid { grid-template-columns: 1fr; }
}
</style>
</head>
<body>
<div class="shell">

<!-- LEFT: INPUTS -->
<div class="panel-left">
  <div class="app-header">
    <div class="app-title">Pre-Trade Analyzer</div>
    <div class="app-sub">NIFTY Options · ₹15K Capital Framework</div>
  </div>

  <!-- Capital -->
  <div class="sec">
    <div class="sec-label">Capital</div>
    <div class="field">
      <div class="field-header"><span class="field-name">Trading capital</span><span class="field-val" id="lbl-capital">₹15,000</span></div>
      <input type="range" id="inp-capital" min="5000" max="100000" step="1000" value="15000" oninput="update()">
    </div>
    <div class="field">
      <div class="field-header"><span class="field-name">Trades today</span><span class="field-val" id="lbl-trades">0</span></div>
      <div class="counter-row">
        <div class="counter">
          <button class="counter-btn" onclick="adj('trades',-1)">−</button>
          <span class="counter-val" id="cnt-trades">0</span>
          <button class="counter-btn" onclick="adj('trades',1)">+</button>
          <span class="counter-label">/ 3 max</span>
        </div>
        <div class="counter">
          <button class="counter-btn" onclick="adj('losses',-1)">−</button>
          <span class="counter-val" id="cnt-losses">0</span>
          <button class="counter-btn" onclick="adj('losses',1)">+</button>
          <span class="counter-label">losses / 2</span>
        </div>
      </div>
    </div>
  </div>

  <!-- Market -->
  <div class="sec">
    <div class="sec-label">Market context</div>
    <div class="field">
      <div class="field-header"><span class="field-name">NIFTY spot</span><span class="field-val" id="lbl-spot">23,600</span></div>
      <input type="range" id="inp-spot" min="21000" max="27000" step="50" value="23600" oninput="update()">
    </div>
    <div class="field">
      <div class="field-header"><span class="field-name">India VIX</span><span class="field-val" id="lbl-vix">14.5</span></div>
      <input type="range" id="inp-vix" min="8" max="35" step="0.5" value="14.5" oninput="update()">
    </div>
    <div class="field">
      <div class="field-header"><span class="field-name">Day of week</span></div>
      <select id="inp-day" onchange="update()">
        <option value="0">Monday</option>
        <option value="1">Tuesday</option>
        <option value="2">Wednesday</option>
        <option value="3">Thursday (expiry)</option>
      </select>
    </div>
    <div class="field">
      <div class="field-header"><span class="field-name">Market phase</span></div>
      <select id="inp-phase" onchange="update()">
        <option value="orb">Opening range breakout</option>
        <option value="vwap">VWAP reclaim / rejection</option>
        <option value="liq">Liquidity sweep + reversal</option>
        <option value="chop">Mid-range / choppy</option>
        <option value="fomo">First 5 min (FOMO zone)</option>
      </select>
    </div>
    <div class="field">
      <div class="field-header"><span class="field-name">Bias</span></div>
      <div class="toggle-row">
        <div class="tog active" id="tog-bull" onclick="setBias('bull')">CE / Bull</div>
        <div class="tog bear" id="tog-bear" onclick="setBias('bear')">PE / Bear</div>
      </div>
    </div>
  </div>

  <!-- Strike -->
  <div class="sec">
    <div class="sec-label">Strike & premium</div>
    <div class="field">
      <div class="field-header"><span class="field-name">Premium paid (₹)</span><span class="field-val" id="lbl-prem">₹120</span></div>
      <input type="range" id="inp-prem" min="15" max="400" step="5" value="120" oninput="update()">
    </div>
    <div class="field">
      <div class="field-header"><span class="field-name">Structure SL level</span><span class="field-val" id="lbl-sl">23,540</span></div>
      <input type="range" id="inp-sl" min="21000" max="27000" step="10" value="23540" oninput="update()">
    </div>
    <div class="field">
      <div class="field-header"><span class="field-name">Target level</span><span class="field-val" id="lbl-tgt">23,720</span></div>
      <input type="range" id="inp-tgt" min="21000" max="27000" step="10" value="23720" oninput="update()">
    </div>
  </div>
</div>

<!-- RIGHT: ANALYSIS -->
<div class="panel-right">
  <div class="results-header">
    <div style="display:flex;align-items:center;gap:10px">
      <div class="verdict-pill go" id="verdict-pill">CHECKING...</div>
      <span class="verdict-msg" id="verdict-msg"></span>
    </div>
    <span style="font-family:var(--mono);font-size:11px;color:var(--muted)" id="live-clock"></span>
  </div>

  <!-- 4 stat cards -->
  <div class="stat-grid">
    <div class="stat neutral" id="sc-risk">
      <div class="stat-label">Max risk (3%)</div>
      <div class="stat-value" id="sv-risk">₹450</div>
      <div class="stat-hint" id="sh-risk">of ₹15,000</div>
    </div>
    <div class="stat neutral" id="sc-sl-pct">
      <div class="stat-label">SL % of premium</div>
      <div class="stat-value" id="sv-sl-pct">25%</div>
      <div class="stat-hint">Target ≤ 25%</div>
    </div>
    <div class="stat neutral" id="sc-rr">
      <div class="stat-label">Risk : Reward</div>
      <div class="stat-value" id="sv-rr">1 : 2.4</div>
      <div class="stat-hint">Target ≥ 1:2</div>
    </div>
    <div class="stat neutral" id="sc-lots">
      <div class="stat-label">Safe lots</div>
      <div class="stat-value" id="sv-lots">3</div>
      <div class="stat-hint" id="sh-lots">within risk cap</div>
    </div>
  </div>

  <!-- Analysis blocks row 1 -->
  <div class="analysis-grid">
    <div class="block">
      <div class="block-title">Capital math</div>
      <div class="row-item"><span class="row-key">Capital</span><span class="row-v neu" id="b-capital">₹15,000</span></div>
      <div class="row-item"><span class="row-key">Max risk / trade (3%)</span><span class="row-v" id="b-maxrisk">₹450</span></div>
      <div class="row-item"><span class="row-key">Daily loss cap</span><span class="row-v" id="b-dayloss">₹1,000</span></div>
      <div class="row-item"><span class="row-key">Remaining daily budget</span><span class="row-v" id="b-remaining">₹1,000</span></div>
      <div class="row-item"><span class="row-key">Trades used</span><span class="row-v" id="b-trades">0 / 3</span></div>
    </div>
    <div class="block">
      <div class="block-title">Strike analysis</div>
      <div class="row-item"><span class="row-key">Premium</span><span class="row-v" id="b-prem">₹120</span></div>
      <div class="row-item"><span class="row-key">Premium range check</span><span class="row-v" id="b-premcheck">₹80–180 ✓</span></div>
      <div class="row-item"><span class="row-key">Lot cost (50 qty)</span><span class="row-v neu" id="b-lotcost">₹6,000</span></div>
      <div class="row-item"><span class="row-key">Affordable lots</span><span class="row-v neu" id="b-afflots">2</span></div>
      <div class="row-item"><span class="row-key">Safe lots (risk-cap)</span><span class="row-v" id="b-safelots">1</span></div>
    </div>
    <div class="block">
      <div class="block-title">SL structure</div>
      <div class="row-item"><span class="row-key">Spot level</span><span class="row-v neu" id="b-spot">23,600</span></div>
      <div class="row-item"><span class="row-key">Structure SL</span><span class="row-v neu" id="b-slvl">23,540</span></div>
      <div class="row-item"><span class="row-key">Gap (pts)</span><span class="row-v" id="b-gap">60 pts</span></div>
      <div class="row-item"><span class="row-key">Est. premium at SL</span><span class="row-v" id="b-premsl">~₹90</span></div>
      <div class="row-item"><span class="row-key">₹ loss at SL (1 lot)</span><span class="row-v" id="b-lossrs">₹1,500</span></div>
    </div>
    <div class="block">
      <div class="block-title">Trade scenario</div>
      <div class="row-item"><span class="row-key">Target level</span><span class="row-v neu" id="b-tgt">23,720</span></div>
      <div class="row-item"><span class="row-key">Point move to target</span><span class="row-v" id="b-ptmove">120 pts</span></div>
      <div class="row-item"><span class="row-key">Est. premium at target</span><span class="row-v" id="b-premtgt">~₹180</span></div>
      <div class="row-item"><span class="row-key">+20% partial exit at</span><span class="row-v ok" id="b-partial">₹144</span></div>
      <div class="row-item"><span class="row-key">Cost-to-zero (trail)</span><span class="row-v ok" id="b-trail">₹120</span></div>
    </div>
  </div>

  <!-- VIX + Timing + Expiry block -->
  <div class="analysis-grid">
    <div class="block">
      <div class="block-title">Market context</div>
      <div class="row-item"><span class="row-key">India VIX</span><span class="row-v" id="b-vix">14.5</span></div>
      <div class="row-item"><span class="row-key">VIX status</span><span class="row-v" id="b-vixstatus">Neutral</span></div>
      <div class="row-item"><span class="row-key">Entry trigger</span><span class="row-v" id="b-phase">ORB</span></div>
      <div class="row-item"><span class="row-key">Trigger quality</span><span class="row-v" id="b-phasescore">Strong</span></div>
      <div class="row-item"><span class="row-key">Bias</span><span class="row-v" id="b-bias">CE / Bull</span></div>
    </div>
    <div class="block">
      <div class="block-title">Expiry day guide</div>
      <div id="expiry-guide"></div>
      <div class="day-row" id="day-row"></div>
    </div>
  </div>

  <!-- Checklist -->
  <div style="font-family:var(--mono);font-size:10px;font-weight:600;letter-spacing:.14em;color:var(--muted);text-transform:uppercase;margin-bottom:10px">Trade clearance checklist</div>
  <div class="checklist" id="checklist"></div>
</div>
</div>

<script>
let state = { trades: 0, losses: 0, bias: 'bull' };

function adj(key, d) {
  if (key === 'trades') state.trades = Math.max(0, Math.min(3, state.trades + d));
  if (key === 'losses') state.losses = Math.max(0, Math.min(2, state.losses + d));
  document.getElementById('cnt-trades').textContent = state.trades;
  document.getElementById('cnt-losses').textContent = state.losses;
  update();
}

function setBias(b) {
  state.bias = b;
  document.getElementById('tog-bull').classList.toggle('active', b === 'bull');
  document.getElementById('tog-bear').classList.toggle('active', b === 'bear');
  document.getElementById('tog-bear').classList.toggle('bear', b === 'bear');
  update();
}

function fmt(n) { return n.toLocaleString('en-IN'); }
function setClass(el, cls) {
  const e = document.getElementById(el);
  e.className = 'row-v ' + cls;
}
function setStat(id, statusId, cls) {
  document.getElementById(id).className = 'stat ' + cls;
}

function update() {
  const capital = +document.getElementById('inp-capital').value;
  const spot    = +document.getElementById('inp-spot').value;
  const vix     = +document.getElementById('inp-vix').value;
  const day     = +document.getElementById('inp-day').value;
  const phase   = document.getElementById('inp-phase').value;
  const prem    = +document.getElementById('inp-prem').value;
  const slLvl   = +document.getElementById('inp-sl').value;
  const tgtLvl  = +document.getElementById('inp-tgt').value;

  // Labels
  document.getElementById('lbl-capital').textContent = '₹' + fmt(capital);
  document.getElementById('lbl-spot').textContent = fmt(spot);
  document.getElementById('lbl-vix').textContent = vix.toFixed(1);
  document.getElementById('lbl-prem').textContent = '₹' + prem;
  document.getElementById('lbl-sl').textContent = fmt(slLvl);
  document.getElementById('lbl-tgt').textContent = fmt(tgtLvl);
  document.getElementById('lbl-trades').textContent = state.trades;

  // --- CALCULATIONS ---
  const LOT = 50;
  const maxRisk = Math.round(capital * 0.03);
  const dayCap = Math.round(capital * 0.067);
  const dailyUsed = state.losses * maxRisk;
  const remaining = Math.max(0, dayCap - dailyUsed);

  const lotCost = prem * LOT;
  const affordable = Math.floor(capital / lotCost);

  // Structure gap & SL%
  const gap = Math.abs(spot - slLvl);
  const delta = Math.min(0.55, Math.max(0.15, 0.5 - gap / 500));
  const premAtSL = Math.max(5, Math.round(prem - delta * gap));
  const slPct = Math.round(((prem - premAtSL) / prem) * 100);
  const lossPerLot = Math.round((prem - premAtSL) * LOT);
  const safeLots = lossPerLot > 0 ? Math.max(0, Math.floor(maxRisk / lossPerLot)) : affordable;

  // Target
  const tgtPts = Math.abs(tgtLvl - spot);
  const premAtTgt = Math.round(prem + delta * tgtPts * 0.9);
  const partial20 = Math.round(prem * 1.2);
  const rr = lossPerLot > 0 ? (((premAtTgt - prem) * LOT) / lossPerLot).toFixed(1) : '–';

  // VIX
  let vixStatus, vixClass;
  if (vix >= 18) { vixStatus = 'Rising — expansion'; vixClass = 'ok'; }
  else if (vix <= 11) { vixStatus = 'Falling — crush danger'; vixClass = 'bad'; }
  else { vixStatus = 'Neutral'; vixClass = 'warn'; }

  // Phase quality
  const phaseMap = {
    orb:  { label: 'Opening range breakout', score: 'Strong', cls: 'ok' },
    vwap: { label: 'VWAP reclaim / rejection', score: 'Strong', cls: 'ok' },
    liq:  { label: 'Liquidity sweep + reversal', score: 'Strong', cls: 'ok' },
    chop: { label: 'Mid-range / choppy', score: 'Avoid', cls: 'bad' },
    fomo: { label: 'First 5 min FOMO', score: 'Avoid', cls: 'bad' },
  };
  const ph = phaseMap[phase];

  // Premium check
  const premOk = prem >= 80 && prem <= 180;
  const premBad = prem < 50 || prem > 250;
  let premLabel, premClass;
  if (prem < 50) { premLabel = '< ₹50 lottery zone'; premClass = 'bad'; }
  else if (prem >= 50 && prem < 80) { premLabel = '₹50–80 breakout only'; premClass = 'warn'; }
  else if (prem >= 80 && prem <= 180) { premLabel = '₹80–180 ✓ sweet spot'; premClass = 'ok'; }
  else if (prem > 180 && prem <= 250) { premLabel = '₹180–250 marginal'; premClass = 'warn'; }
  else { premLabel = '> ₹250 capital drain'; premClass = 'bad'; }

  // --- UPDATE UI ---
  document.getElementById('b-capital').textContent = '₹' + fmt(capital);
  document.getElementById('b-maxrisk').textContent = '₹' + fmt(maxRisk);
  document.getElementById('b-maxrisk').className = 'row-v ok';
  document.getElementById('b-dayloss').textContent = '₹' + fmt(dayCap);
  document.getElementById('b-remaining').textContent = '₹' + fmt(remaining);
  document.getElementById('b-remaining').className = 'row-v ' + (remaining > 0 ? 'ok' : 'bad');
  document.getElementById('b-trades').textContent = state.trades + ' / 3';
  document.getElementById('b-trades').className = 'row-v ' + (state.trades < 3 ? 'ok' : 'bad');

  document.getElementById('b-prem').textContent = '₹' + prem;
  document.getElementById('b-prem').className = 'row-v ' + premClass;
  document.getElementById('b-premcheck').textContent = premLabel;
  document.getElementById('b-premcheck').className = 'row-v ' + premClass;
  document.getElementById('b-lotcost').textContent = '₹' + fmt(lotCost);
  document.getElementById('b-afflots').textContent = affordable;
  document.getElementById('b-safelots').textContent = safeLots;
  document.getElementById('b-safelots').className = 'row-v ' + (safeLots > 0 ? 'ok' : 'bad');

  document.getElementById('b-spot').textContent = fmt(spot);
  document.getElementById('b-slvl').textContent = fmt(slLvl);
  document.getElementById('b-gap').textContent = gap + ' pts';
  document.getElementById('b-gap').className = 'row-v ' + (gap >= 30 && gap <= 150 ? 'ok' : gap < 30 ? 'bad' : 'warn');
  document.getElementById('b-premsl').textContent = '~₹' + premAtSL;
  document.getElementById('b-lossrs').textContent = '₹' + fmt(lossPerLot);
  document.getElementById('b-lossrs').className = 'row-v ' + (lossPerLot <= maxRisk ? 'ok' : 'bad');

  document.getElementById('b-tgt').textContent = fmt(tgtLvl);
  document.getElementById('b-ptmove').textContent = tgtPts + ' pts';
  document.getElementById('b-premtgt').textContent = '~₹' + premAtTgt;
  document.getElementById('b-partial').textContent = '₹' + partial20;
  document.getElementById('b-trail').textContent = '₹' + prem;

  document.getElementById('b-vix').textContent = vix.toFixed(1);
  document.getElementById('b-vix').className = 'row-v ' + vixClass;
  document.getElementById('b-vixstatus').textContent = vixStatus;
  document.getElementById('b-vixstatus').className = 'row-v ' + vixClass;
  document.getElementById('b-phase').textContent = ph.label;
  document.getElementById('b-phasescore').textContent = ph.score;
  document.getElementById('b-phasescore').className = 'row-v ' + ph.cls;
  document.getElementById('b-bias').textContent = state.bias === 'bull' ? 'CE / Bull' : 'PE / Bear';

  // Stat cards
  document.getElementById('sv-risk').textContent = '₹' + fmt(maxRisk);
  document.getElementById('sh-risk').textContent = 'of ₹' + fmt(capital);
  document.getElementById('sc-risk').className = 'stat ok';

  document.getElementById('sv-sl-pct').textContent = slPct + '%';
  document.getElementById('sc-sl-pct').className = 'stat ' + (slPct <= 25 ? 'ok' : slPct <= 35 ? 'warn' : 'bad');

  document.getElementById('sv-rr').textContent = '1 : ' + rr;
  document.getElementById('sc-rr').className = 'stat ' + (parseFloat(rr) >= 2 ? 'ok' : parseFloat(rr) >= 1.5 ? 'warn' : 'bad');

  document.getElementById('sv-lots').textContent = safeLots;
  document.getElementById('sh-lots').textContent = 'within ₹' + fmt(maxRisk) + ' cap';
  document.getElementById('sc-lots').className = 'stat ' + (safeLots > 0 ? 'ok' : 'bad');

  // Expiry guide
  const dayLabels = ['Mon','Tue','Wed','Thu'];
  const dayGuides = [
    { text: 'Directional trades work well. Full checklist applies.', cls: 'ok' },
    { text: 'Directional trades work well. Full checklist applies.', cls: 'ok' },
    { text: 'Mixed / trap heavy. Higher false-breakout risk. Be cautious.', cls: 'warn' },
    { text: 'Expiry day. ATM only. Quick scalps — no holding. Fast premium moves.', cls: 'bad' },
  ];
  const dg = dayGuides[day];
  document.getElementById('expiry-guide').innerHTML =
    `<div class="row-item" style="border:none;padding-bottom:8px"><span class="row-key">Today</span><span class="row-v ${dg.cls}">${['Monday','Tuesday','Wednesday','Thursday'][day]}</span></div>` +
    `<div style="font-size:11px;color:var(--muted);line-height:1.5">${dg.text}</div>`;
  document.getElementById('day-row').innerHTML = dayLabels.map((d,i) =>
    `<div class="day-pill ${i===0||i===1?'good':i===2?'mixed':'fast'} ${i===day?'':''}${i===day?'':' '}">${d}</div>`).join('');

  // --- CHECKLIST ---
  const checks = [];

  if (state.losses >= 2) checks.push({ cls: 'fail', title: 'Daily loss cap hit', body: '2 losses today. Stop trading — no exceptions. Capital protection is non-negotiable.' });
  else if (state.losses === 1) checks.push({ cls: 'caution', title: '1 loss taken today', body: 'One more loss and trading stops for the day. Trade only A+ setups.' });
  else checks.push({ cls: 'pass', title: 'Loss count clear', body: '0 losses today. Daily loss budget: ₹' + fmt(dayCap) + ' fully intact.' });

  if (state.trades >= 3) checks.push({ cls: 'fail', title: 'Max trades reached', body: '3 trades done. Stop for the day regardless of how good the next setup looks.' });
  else checks.push({ cls: 'pass', title: 'Trade count OK', body: state.trades + ' of 3 trades used today.' });

  if (premOk) checks.push({ cls: 'pass', title: 'Premium in sweet spot', body: '₹' + prem + ' is in the ₹80–₹180 range. Good delta exposure without capital drain.' });
  else if (prem < 50) checks.push({ cls: 'fail', title: 'Premium too cheap — lottery', body: '₹' + prem + ' is below ₹50. Deep OTM: theta kills you before the move materialises. Avoid.' });
  else if (prem > 250) checks.push({ cls: 'fail', title: 'Premium too high', body: '₹' + prem + ' above ₹250 drains capital fast. Move 1–2 strikes further OTM.' });
  else checks.push({ cls: 'caution', title: 'Premium marginal', body: '₹' + prem + ' is outside the optimal ₹80–₹180 range but usable with discipline.' });

  if (slPct <= 25) checks.push({ cls: 'pass', title: 'SL % within limit', body: slPct + '% of premium at structure SL. Within the 25% ceiling — trade has healthy room.' });
  else if (slPct <= 35) checks.push({ cls: 'caution', title: 'SL % elevated', body: slPct + '% at SL. Consider moving to ATM (higher delta) to reduce this below 25%.' });
  else checks.push({ cls: 'fail', title: 'SL % too wide', body: slPct + '% at SL means you need a big move just to break even. Adjust strike — not the SL.' });

  if (lossPerLot <= maxRisk) checks.push({ cls: 'pass', title: 'Risk per lot fits capital cap', body: '₹' + fmt(lossPerLot) + ' loss/lot ≤ ₹' + fmt(maxRisk) + ' max risk. You can safely trade ' + safeLots + ' lot(s).' });
  else checks.push({ cls: 'fail', title: 'Loss exceeds risk cap', body: 'At ₹' + fmt(lossPerLot) + '/lot, 1 lot already exceeds your ₹' + fmt(maxRisk) + ' limit. Fix: tighten SL, or move closer to ATM.' });

  if (gap >= 30 && gap <= 150) checks.push({ cls: 'pass', title: 'Structure gap healthy', body: gap + ' pts between spot and SL. Clear of noise, placed at a real invalidation level.' });
  else if (gap < 30) checks.push({ cls: 'fail', title: 'SL inside noise zone', body: 'Only ' + gap + ' pts to SL. Random tick movement will hit this before the trade plays out. Use a real structure level.' });
  else checks.push({ cls: 'caution', title: 'SL gap wide', body: gap + ' pts is a wide structure gap. Double-check this is a real key level and not just a random distance.' });

  const rrNum = parseFloat(rr);
  if (rrNum >= 2) checks.push({ cls: 'pass', title: 'RR is ≥ 1:2', body: '1:' + rr + ' — asymmetric payout confirmed. Trade makes mathematical sense.' });
  else if (rrNum >= 1.5) checks.push({ cls: 'caution', title: 'RR below 1:2', body: '1:' + rr + ' is marginal. Move target or tighten SL. A 1:2 minimum is the threshold.' });
  else checks.push({ cls: 'fail', title: 'RR too low', body: '1:' + rr + ' does not justify the trade. You will not survive on this RR long-term with ₹' + fmt(capital) + '.' });

  if (ph.cls === 'ok') checks.push({ cls: 'pass', title: 'Entry trigger is valid', body: ph.label + ' — you are entering at a decision point, not in the middle of noise.' });
  else checks.push({ cls: 'fail', title: 'No valid entry trigger', body: ph.label + ' — small capital dies in sideways/choppy markets. Wait for a clear trigger.' });

  if (vixClass === 'ok') checks.push({ cls: 'pass', title: 'VIX supports premium buying', body: 'VIX ' + vix.toFixed(1) + ' — rising, premium expansion expected. Directional moves pay well.' });
  else if (vixClass === 'bad') checks.push({ cls: 'fail', title: 'VIX crush danger', body: 'VIX ' + vix.toFixed(1) + ' falling. Even a correct direction may not pay — premium gets crushed on the way. Avoid buying.' });
  else checks.push({ cls: 'caution', title: 'VIX neutral — be alert', body: 'VIX ' + vix.toFixed(1) + '. Monitor for direction change. Not a red flag, but not an explicit tailwind either.' });

  if (day === 2) checks.push({ cls: 'caution', title: 'Wednesday — trap day', body: 'Historically mixed with false breakouts. Reduce size, raise setup quality bar.' });
  if (day === 3) checks.push({ cls: 'caution', title: 'Thursday expiry — ATM scalps only', body: 'Expiry day dynamics. Fast moves and fast decay. Trade ATM only. No holding through moves.' });

  document.getElementById('checklist').innerHTML = checks.map(c =>
    `<div class="check-item ${c.cls}"><div class="check-dot"></div><div class="check-content"><div class="check-title">${c.title}</div><div class="check-body">${c.body}</div></div></div>`
  ).join('');

  // Overall verdict
  const fails = checks.filter(c => c.cls === 'fail').length;
  const cauts = checks.filter(c => c.cls === 'caution').length;
  const pill = document.getElementById('verdict-pill');
  const msg  = document.getElementById('verdict-msg');
  if (fails === 0 && cauts === 0) {
    pill.className = 'verdict-pill go'; pill.textContent = 'GO';
    msg.textContent = 'All checks pass. Execute with discipline.';
  } else if (fails === 0) {
    pill.className = 'verdict-pill warn'; pill.textContent = 'CAUTION';
    msg.textContent = cauts + ' marginal condition' + (cauts>1?'s':'') + '. Review before entry.';
  } else {
    pill.className = 'verdict-pill no'; pill.textContent = 'NO TRADE';
    msg.textContent = fails + ' critical failure' + (fails>1?'s':'') + '. Fix before entering.';
  }
}

// Clock
function tick() {
  const now = new Date();
  const t = now.toLocaleTimeString('en-IN', { hour12: false });
  const el = document.getElementById('live-clock');
  if (el) el.textContent = t;
}
tick();
setInterval(tick, 1000);

update();
</script>
</body>
</html>
