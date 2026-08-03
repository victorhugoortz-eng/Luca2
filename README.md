<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Luca — SDR &amp; BDR Analytics</title>
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#f1f5f9;
  --surface:#ffffff;
  --surface2:#f8fafc;
  --border:#e2e8f0;
  --border2:#cbd5e1;
  --text:#0f172a;
  --muted:#64748b;
  --light:#94a3b8;
  --blue:#2563eb;
  --blue-soft:#eff6ff;
  --blue-mid:#dbeafe;
  --green:#059669;
  --green-soft:#ecfdf5;
  --amber:#d97706;
  --amber-soft:#fffbeb;
  --red:#dc2626;
  --red-soft:#fef2f2;
  --indigo:#4f46e5;
  --radius:10px;
}
html{font-size:15px;-webkit-text-size-adjust:100%}
body{background:var(--bg);color:var(--text);
  font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',system-ui,sans-serif;
  line-height:1.6;min-height:100vh;padding-bottom:48px}

/* HEADER */
.hdr{background:var(--surface);border-bottom:1px solid var(--border);
  padding:18px 24px;display:flex;align-items:center;
  justify-content:space-between;flex-wrap:wrap;gap:12px;
  position:sticky;top:0;z-index:10;}
.hdr-left{display:flex;align-items:center;gap:12px}
.logo{width:40px;height:40px;border-radius:9px;flex-shrink:0;
  background:linear-gradient(135deg,#2563eb,#4f46e5);
  display:flex;align-items:center;justify-content:center;
  font-weight:900;font-size:18px;color:#fff;letter-spacing:-1px;}
.hdr-title{font-size:1.15rem;font-weight:700;color:var(--text)}
.hdr-sub{font-size:.74rem;color:var(--muted);margin-top:1px}
.hdr-meta{font-size:.7rem;color:var(--muted);text-align:right;line-height:1.8}
.hdr-meta b{color:var(--text);font-weight:600}

/* LAYOUT */
.wrap{max-width:1260px;margin:0 auto;padding:0 16px}
section{margin-top:32px}
.sec-title{font-size:.68rem;font-weight:700;letter-spacing:.1em;text-transform:uppercase;
  color:var(--muted);padding-bottom:9px;border-bottom:2px solid var(--border);
  margin-bottom:16px;display:flex;align-items:center;gap:8px;}
.sec-dot{width:7px;height:7px;border-radius:50%;background:var(--blue);flex-shrink:0}

/* KPI GRID */
.kpi-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(130px,1fr));gap:10px}
.kpi{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);
  padding:14px 12px 11px;position:relative;overflow:hidden;}
.kpi::before{content:'';position:absolute;top:0;left:0;right:0;height:3px;
  border-radius:var(--radius) var(--radius) 0 0;background:var(--c,var(--blue));}
.kpi-lbl{font-size:.62rem;font-weight:700;letter-spacing:.07em;
  text-transform:uppercase;color:var(--muted);margin-bottom:7px}
.kpi-val{font-size:1.8rem;font-weight:800;line-height:1;letter-spacing:-1px;color:var(--text)}
.kpi-note{font-size:.65rem;color:var(--light);margin-top:4px}

/* INSIGHTS */
.insights-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(250px,1fr));gap:11px}
.insight-card{background:var(--surface);border:1px solid var(--border);
  border-radius:var(--radius);padding:14px 16px;border-left:4px solid var(--c,var(--blue));}
.insight-card.good{--c:var(--green)}.insight-card.warn{--c:var(--amber)}
.insight-card.danger{--c:var(--red)}.insight-card.info{--c:var(--blue)}
.insight-title{font-size:.8rem;font-weight:700;color:var(--text);margin-bottom:4px}
.insight-body{font-size:.74rem;color:var(--muted);line-height:1.5}
.insight-body b{color:var(--text)}

/* TWO COL */
.two-col{display:grid;grid-template-columns:1fr 1fr;gap:12px}
.panel{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:18px}
.panel-lbl{font-size:.68rem;font-weight:700;letter-spacing:.06em;
  text-transform:uppercase;color:var(--muted);margin-bottom:14px}

/* FUNNEL BARS */
.f-row{display:flex;align-items:center;gap:10px;margin-bottom:9px}
.f-lbl{width:170px;font-size:.77rem;color:var(--text);flex-shrink:0;white-space:nowrap}
.f-track{flex:1;height:18px;background:var(--bg);border-radius:4px;overflow:hidden;border:1px solid var(--border)}
.f-bar{height:100%;border-radius:3px}
.f-cnt{width:46px;text-align:right;font-size:.77rem;font-weight:600;color:var(--text);flex-shrink:0}
.f-note{font-size:.7rem;color:var(--muted);margin-top:10px;padding-top:9px;border-top:1px solid var(--border)}

/* CHART */
.chart-box{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:20px}
.chart-lbl{font-size:.68rem;font-weight:700;letter-spacing:.06em;text-transform:uppercase;
  color:var(--muted);margin-bottom:14px}

/* TABLES */
.t-wrap{overflow-x:auto;border-radius:var(--radius);border:1px solid var(--border);-webkit-overflow-scrolling:touch}
table{border-collapse:collapse;width:100%;font-size:.79rem;min-width:480px}
thead th{background:var(--surface2);color:var(--muted);
  font-size:.63rem;font-weight:700;letter-spacing:.08em;text-transform:uppercase;
  padding:9px 11px;border-bottom:1px solid var(--border2);
  white-space:nowrap;text-align:right;cursor:pointer;user-select:none;}
thead th:hover{background:var(--blue-mid);color:var(--blue)}
thead th.sorted{background:var(--blue-mid);color:var(--blue)}
thead th:first-child{text-align:left}
.sort-icon{margin-left:3px;opacity:.35;font-size:.75em}
thead th.sorted .sort-icon,thead th:hover .sort-icon{opacity:1}
tbody tr{border-bottom:1px solid var(--border)}
tbody tr:last-child{border-bottom:none}
tbody tr:hover{background:var(--blue-soft)}
.tr-alt{background:var(--surface2)}
td{padding:8px 11px;vertical-align:middle}
.td-name{color:var(--text);font-weight:500;white-space:nowrap}
.td-num{text-align:right;font-variant-numeric:tabular-nums;color:var(--muted)}
.cell-green{color:var(--green)!important;font-weight:600}
.cell-amber{color:var(--amber)!important;font-weight:600}
.cell-red{color:var(--red)!important;font-weight:600}
.rank-1{color:#b45309;font-weight:700}.rank-2{color:var(--muted);font-weight:600}.rank-3{color:#92400e;font-weight:600}
.badge{display:inline-block;padding:1px 6px;border-radius:4px;font-size:.62rem;font-weight:700;margin-right:3px}
.badge-ok{background:var(--green-soft);color:var(--green)}
.badge-warn{background:var(--amber-soft);color:var(--amber)}
.badge-danger{background:var(--red-soft);color:var(--red)}

/* HEATMAP */
.hm-legend{display:flex;align-items:center;gap:8px;font-size:.7rem;color:var(--muted);margin-bottom:14px;flex-wrap:wrap}
.hm-swatch{display:inline-block;width:28px;height:16px;border-radius:3px;border:1px solid var(--border);vertical-align:middle}
.th-hm{text-align:center!important;font-size:.62rem!important;padding:8px 5px!important;min-width:46px;white-space:nowrap}
.th-total{background:var(--surface2)!important;border-left:2px solid var(--border2)!important;color:var(--text)!important}
.td-hm{text-align:center;padding:6px 5px!important;font-size:.8rem;vertical-align:middle;font-variant-numeric:tabular-nums}
.td-total-val{font-size:.8rem;font-weight:600;border-left:2px solid var(--border2);color:var(--text)!important;background:var(--surface2)}
.td-ganadas-val{font-size:.8rem;font-weight:600;color:var(--green)!important}
.wr-pct{font-size:.65rem;color:var(--muted);font-weight:400}
.totals-row{border-top:2px solid var(--border2)!important}
.totals-row td{background:var(--surface2)!important;font-weight:700}
.hm-0{background:#fff;color:var(--light)}
.hm-1{background:#dbeafe;color:#1d4ed8}
.hm-2{background:#93c5fd;color:#1e3a8a}
.hm-3{background:#2563eb;color:#fff}

/* HINT */
.scroll-hint{display:none;font-size:.68rem;color:var(--light);text-align:right;margin-bottom:5px}

/* FOOTER */
footer{margin-top:48px;border-top:1px solid var(--border);background:var(--surface);
  padding:14px 24px;display:flex;justify-content:space-between;
  font-size:.68rem;color:var(--muted);flex-wrap:wrap;gap:6px;}

@media(max-width:720px){
  .hdr{padding:14px 16px}.hdr-meta{display:none}
  .wrap{padding:0 12px}
  .two-col{grid-template-columns:1fr}
  .kpi-grid{grid-template-columns:repeat(2,1fr)}
  .kpi-val{font-size:1.5rem}
  .insights-grid{grid-template-columns:1fr}
  .scroll-hint{display:block}
  .f-lbl{width:120px;font-size:.72rem}
  section{margin-top:24px}
}
@media(max-width:380px){.kpi-val{font-size:1.3rem}}
</style>
</head>
<body>

<header class="hdr">
  <div class="hdr-left">
    <div class="logo">L</div>
    <div>
      <div class="hdr-title">Luca Analytics</div>
      <div class="hdr-sub">Dashboard SDR &amp; BDR — Performance Comercial</div>
    </div>
  </div>
  <div class="hdr-meta">
    Generado: <b>03 Ago 2026</b><br>
    Org: <b>lucaedu.my.salesforce.com</b><br>
    <b>24</b> SDRs · <b>31</b> BDRs
  </div>
</header>

<div class="wrap">

<!-- KPIs -->
<section>
  <div class="sec-title"><span class="sec-dot"></span>KPIs Globales</div>
  <div class="kpi-grid">
    <div class="kpi" style="--c:#2563eb"><div class="kpi-lbl">Citas Generadas</div><div class="kpi-val">868</div><div class="kpi-note">con fecha confirmada</div></div>
    <div class="kpi" style="--c:#0891b2"><div class="kpi-lbl">Videollamadas</div><div class="kpi-val">316</div><div class="kpi-note">36% de citas</div></div>
    <div class="kpi" style="--c:#7c3aed"><div class="kpi-lbl">Presenciales</div><div class="kpi-val">332</div><div class="kpi-note">38% de citas</div></div>
    <div class="kpi" style="--c:#059669"><div class="kpi-lbl">Es Luca</div><div class="kpi-val">141</div><div class="kpi-note">16.2% conv. SDR</div></div>
    <div class="kpi" style="--c:#4f46e5"><div class="kpi-lbl">Demos BDR</div><div class="kpi-val">249</div><div class="kpi-note">en cita</div></div>
    <div class="kpi" style="--c:#d97706"><div class="kpi-lbl">En Negociación</div><div class="kpi-val">709</div><div class="kpi-note">propuestas activas</div></div>
    <div class="kpi" style="--c:#059669"><div class="kpi-lbl">Cerrada Ganada</div><div class="kpi-val">105</div><div class="kpi-note">win rate 35.2%</div></div>
    <div class="kpi" style="--c:#dc2626"><div class="kpi-lbl">Cerrada Perdida</div><div class="kpi-val">193</div><div class="kpi-note">64.8% de cerradas</div></div>
  </div>
</section>

<!-- INSIGHTS -->
<section>
  <div class="sec-title"><span class="sec-dot" style="background:var(--amber)"></span>Hallazgos Clave</div>
  <div class="insights-grid">
    <div class="insight-card good"><div class="insight-title">🏆 Top BDR: Luis Dominguez — 62.5% win rate</div><div class="insight-body">Con <b>20 cierres de 32 opps</b>, es el BDR más efectivo. Perfil presencial fuerte (28 visitas vs. 70 video).</div></div>
    <div class="insight-card good"><div class="insight-title">⭐ SDR destaque: Georgina Ruvalcaba — 26.3% conv.</div><div class="insight-body"><b>38 citas, 10 clientes</b>. Mejor conversión entre SDRs con volumen real. Candidata a mentora.</div></div>
    <div class="insight-card warn"><div class="insight-title">⚠️ Milton figura como SDR y BDR — revisar datos</div><div class="insight-body"><b>344 citas como SDR</b> (40% del equipo) + <b>179 demos como BDR con 0 cierres</b>. Posible rol duplicado en SF.</div></div>
    <div class="insight-card warn"><div class="insight-title">📊 709 opps en negociación sin resolución</div><div class="insight-body">Pipeline grande sin limpiar. Muchas pueden ser stale — el win rate real podría ser diferente al reportado.</div></div>
    <div class="insight-card danger"><div class="insight-title">🔴 Jorge Fuentes: 54 opps — 7.4% win rate</div><div class="insight-body"><b>50 en negociación, solo 4 ganadas</b>. Mayor volumen con menor efectividad. Requiere coaching urgente.</div></div>
    <div class="insight-card danger"><div class="insight-title">🔴 Alfonso Reyes: 88 opps — 6.1% win rate</div><div class="insight-body"><b>55 negociación, 31 perdidas, 2 ganadas</b>. Junto a Jorge, mayor brecha volumen/efectividad del equipo.</div></div>
    <div class="insight-card info"><div class="insight-title">💡 Luis Dominguez genera sus propias opps (63.6%)</div><div class="insight-body">Aparece también como SDR Soporte con la conversión más alta. Maneja ciclo completo o cuentas directas.</div></div>
    <div class="insight-card info"><div class="insight-title">📋 332 presenciales a cruzar con gastos</div><div class="insight-body">Cada visita presencial debe tener respaldo. Auditar gastos vs. registro en Salesforce mensualmente.</div></div>
  </div>
</section>

<!-- FUNNELS -->
<section>
  <div class="sec-title"><span class="sec-dot" style="background:var(--indigo)"></span>Funnels de Conversión</div>
  <div class="two-col">
    <div class="panel">
      <div class="panel-lbl">🎯 Funnel SDR — Cuentas por Etapa</div>
      <div class="f-row"><div class="f-lbl">Entrada</div><div class="f-track"><div class="f-bar" style="width:85.7%;background:#2563eb"></div></div><div class="f-cnt">2,622</div></div>
      <div class="f-row"><div class="f-lbl">En seguimiento</div><div class="f-track"><div class="f-bar" style="width:57.2%;background:#4f46e5"></div></div><div class="f-cnt">1,749</div></div>
      <div class="f-row"><div class="f-lbl">Cita agendada</div><div class="f-track"><div class="f-bar" style="width:29.2%;background:#d97706"></div></div><div class="f-cnt">893</div></div>
      <div class="f-row"><div class="f-lbl">Es Luca ✓</div><div class="f-track"><div class="f-bar" style="width:4.6%;background:#059669"></div></div><div class="f-cnt">141</div></div>
      <div class="f-row"><div class="f-lbl">Rechazó ✗</div><div class="f-track"><div class="f-bar" style="width:100%;background:#dc2626"></div></div><div class="f-cnt">3,060</div></div>
      <div class="f-note">Conversión SDR: <b style="color:var(--green)">5.4%</b> (141 / 2,622 en seguimiento)</div>
    </div>
    <div class="panel">
      <div class="panel-lbl">💼 Funnel BDR — Oportunidades</div>
      <div class="f-row"><div class="f-lbl">Demo en Cita</div><div class="f-track"><div class="f-bar" style="width:35.1%;background:#2563eb"></div></div><div class="f-cnt">249</div></div>
      <div class="f-row"><div class="f-lbl">Negociación</div><div class="f-track"><div class="f-bar" style="width:100%;background:#d97706"></div></div><div class="f-cnt">709</div></div>
      <div class="f-row"><div class="f-lbl">Cerrada Ganada ✓</div><div class="f-track"><div class="f-bar" style="width:14.8%;background:#059669"></div></div><div class="f-cnt">105</div></div>
      <div class="f-row"><div class="f-lbl">Cerrada Perdida ✗</div><div class="f-track"><div class="f-bar" style="width:27.2%;background:#dc2626"></div></div><div class="f-cnt">193</div></div>
      <div class="f-note">Win rate: <b style="color:var(--green)">35.2%</b> (105/298 cerradas) · Pipeline activo: <b style="color:var(--amber)">709 opps</b></div>
    </div>
  </div>
</section>

<!-- CHART -->
<section>
  <div class="sec-title"><span class="sec-dot" style="background:#0891b2"></span>Tendencia Mensual</div>
  <div class="chart-box">
    <div class="chart-lbl">📈 Citas · Demos · Cierres por Mes</div>
    <canvas id="trendChart" height="170"></canvas>
  </div>
</section>

<!-- SDR TABLE -->
<section>
  <div class="sec-title"><span class="sec-dot"></span>Performance SDR — 24 representantes</div>
  <div class="scroll-hint">← Desliza para ver más →</div>
  <div class="t-wrap">
    <table id="sdr-table">
      <thead><tr>
        <th onclick="sort('sdr',0,false)" style="text-align:left">SDR <span class="sort-icon">↕</span></th>
        <th onclick="sort('sdr',1,true)">Citas <span class="sort-icon">↕</span></th>
        <th onclick="sort('sdr',2,true)">Video <span class="sort-icon">↕</span></th>
        <th onclick="sort('sdr',3,true)">Presencial <span class="sort-icon">↕</span></th>
        <th onclick="sort('sdr',4,true)">Seguimiento <span class="sort-icon">↕</span></th>
        <th onclick="sort('sdr',5,true)">Es Luca <span class="sort-icon">↕</span></th>
        <th onclick="sort('sdr',6,true)" class="sorted">Conv % <span class="sort-icon">↓</span></th>
      </tr></thead>
      <tbody id="sdr-body"></tbody>
    </table>
  </div>
</section>

<!-- BDR TABLE -->
<section>
  <div class="sec-title"><span class="sec-dot" style="background:var(--green)"></span>Performance BDR</div>
  <div class="scroll-hint">← Desliza para ver más →</div>
  <div class="t-wrap">
    <table id="bdr-table">
      <thead><tr>
        <th onclick="sort('bdr',0,false)" style="text-align:left">BDR <span class="sort-icon">↕</span></th>
        <th onclick="sort('bdr',1,true)">Demos <span class="sort-icon">↕</span></th>
        <th onclick="sort('bdr',2,true)">Negociación <span class="sort-icon">↕</span></th>
        <th onclick="sort('bdr',3,true)">Ganadas <span class="sort-icon">↕</span></th>
        <th onclick="sort('bdr',4,true)">Perdidas <span class="sort-icon">↕</span></th>
        <th onclick="sort('bdr',5,true)" class="sorted">Win Rate <span class="sort-icon">↓</span></th>
        <th onclick="sort('bdr',6,true)">Video <span class="sort-icon">↕</span></th>
        <th onclick="sort('bdr',7,true)">Presencial <span class="sort-icon">↕</span></th>
      </tr></thead>
      <tbody id="bdr-body"></tbody>
    </table>
  </div>
</section>

<!-- ATTRIBUTION -->
<section>
  <div class="sec-title"><span class="sec-dot" style="background:var(--amber)"></span>Atribución SDR → Cierres BDR</div>
  <div class="scroll-hint">← Desliza para ver más →</div>
  <div class="t-wrap">
    <table id="attr-table">
      <thead><tr>
        <th onclick="sort('attr',0,false)" style="text-align:left">SDR Soporte <span class="sort-icon">↕</span></th>
        <th onclick="sort('attr',1,true)">Demos <span class="sort-icon">↕</span></th>
        <th onclick="sort('attr',2,true)">Negociación <span class="sort-icon">↕</span></th>
        <th onclick="sort('attr',3,true)">Ganadas <span class="sort-icon">↕</span></th>
        <th onclick="sort('attr',4,true)">Perdidas <span class="sort-icon">↕</span></th>
        <th onclick="sort('attr',5,true)" class="sorted">Conv % <span class="sort-icon">↓</span></th>
        <th style="text-align:right">Diagnóstico</th>
      </tr></thead>
      <tbody id="attr-body"></tbody>
    </table>
  </div>
</section>

<!-- HEATMAP RITMO -->
<section>
  <div class="sec-title"><span class="sec-dot" style="background:var(--indigo)"></span>Ritmo de Asignación por SDR — Opps creadas por mes</div>
  <div class="hm-legend">
    <span class="hm-swatch hm-0"></span> Sin opps &nbsp;
    <span class="hm-swatch hm-1"></span> 1–2 &nbsp;
    <span class="hm-swatch hm-2"></span> 3–5 &nbsp;
    <span class="hm-swatch hm-3"></span> 6+ &nbsp;&nbsp;
    <span style="color:var(--muted)">· Ganadas = IsWon · % = Ganadas ÷ Total creadas por SDR</span>
  </div>
  <div class="scroll-hint">← Desliza para ver más →</div>
  <div class="t-wrap">
    <table>
      <thead><tr>
        <th style="text-align:left;min-width:145px">SDR Soporte</th>
        <th class="th-hm">Ago'25</th><th class="th-hm">Sep'25</th><th class="th-hm">Oct'25</th><th class="th-hm">Nov'25</th><th class="th-hm">Dic'25</th><th class="th-hm">Ene'26</th><th class="th-hm">Feb'26</th><th class="th-hm">Mar'26</th><th class="th-hm">Abr'26</th><th class="th-hm">May'26</th><th class="th-hm">Jun'26</th><th class="th-hm">Jul'26</th><th class="th-hm">Ago'26</th>
        <th class="th-hm th-total">Total</th><th class="th-hm th-total">Ganadas</th>
      </tr></thead>
      <tbody id="hm-body"></tbody>
    </table>
  </div>
</section>

</div>

<footer>
  <span>Luca Educación · Datos al 03 Ago 2026</span>
  <span>Fuente: Salesforce · lucaedu.my.salesforce.com</span>
</footer>

<script>
const SDR=[
  ["Milton Rodríguez",344,149,132,842,71,20.6,"warn"],
  ["Jessica Cárdenas Álvarez",56,12,32,36,10,17.9,""],
  ["Ulises Castañeda",55,32,10,49,2,3.6,""],
  ["Fernanda Armenta",51,26,7,29,11,21.6,""],
  ["Griselda Castañeda",41,6,27,120,5,12.2,""],
  ["Georgina Ruvalcaba",38,6,28,39,10,26.3,""],
  ["Rosa I. Hernandez Garcia",34,13,12,80,2,5.9,""],
  ["Veronica Guzman",34,5,17,46,4,11.8,""],
  ["Daniel Franco",34,6,13,24,3,8.8,""],
  ["Sandra Osuna",32,12,2,63,2,6.2,""],
  ["Isis Arriaga Ontiveros",28,4,14,61,5,17.9,""],
  ["Fabiola De Anda",27,9,8,71,3,11.1,""],
  ["Yvonne De La Vega",26,11,9,21,1,3.8,""],
  ["Samantha Garcia",22,11,5,97,2,9.1,""],
  ["Ileana Alvarez",15,4,3,56,1,6.7,""],
  ["Fatima Perez Cambero",9,1,8,67,4,44.4,""],
  ["Andrea Espinoza",4,3,0,5,0,0.0,""],
  ["Lydia Gonzalez Rangel",3,1,2,8,0,0.0,""],
  ["Dulce Perez",3,2,1,7,0,0.0,""],
  ["Zulma Dueñas",3,0,0,3,0,0.0,""],
  ["Yennifer Acevedo",3,1,0,8,3,100.0,""],
  ["Amanda Torres Romo",2,0,1,7,0,0.0,""],
  ["Mireya Gonzalez",2,0,1,7,0,0.0,""],
  ["Andrea L. Florencio Maciel",2,2,0,3,2,100.0,""],
];
const BDR=[
  ["Luis Dominguez",8,63,20,12,62.5,70,28],
  ["Marcos Gutiérrez",4,111,18,19,48.6,71,75],
  ["Luis Gómez",1,32,17,29,37.0,20,47],
  ["Fernando Carrillo",4,102,14,10,58.3,33,80],
  ["Omar Sanchez",0,61,11,10,52.4,25,57],
  ["Andres Reyes",7,63,8,13,38.1,44,44],
  ["Sergio Arenas",1,38,6,11,35.3,7,44],
  ["Jorge Fuentes",0,50,4,50,7.4,60,40],
  ["Alfonso Reyes",0,55,2,31,6.1,61,22],
  ["Alex Pérez",0,33,2,0,100.0,25,10],
  ["Frederico Bello",0,3,1,1,50.0,0,3],
  ["Angel Martinez",2,10,1,1,50.0,5,2],
  ["Arely Quintana",11,80,0,2,0.0,53,36],
];
const ATTR=[
  ["Jessica Cárdenas Álvarez",14,58,10,11,10.8],
  ["Veronica Guzman",12,24,9,4,18.4],
  ["Zulma Dueñas",6,36,8,13,12.7],
  ["Luis Dominguez",0,4,7,0,63.6],
  ["Milton Rodríguez",17,110,7,45,3.9],
  ["Rosa I. Hernandez Garcia",13,41,7,10,9.9],
  ["Fernanda Armenta",6,42,6,5,10.2],
  ["Ulises Castañeda",18,44,6,16,7.1],
  ["Dulce Perez",14,26,5,19,7.8],
  ["Griselda Castañeda",7,45,5,5,8.1],
  ["Mireya Gonzalez",12,22,4,4,9.5],
  ["Arely Quintana",2,4,3,1,30.0],
  ["Daniel Franco",10,16,3,6,8.6],
  ["Yennifer Acevedo",20,13,2,2,5.4],
  ["Andrea Espinoza",15,42,2,5,3.1],
  ["Sandra Osuna",17,33,2,13,3.1],
  ["Mayra Burgos",6,24,2,3,5.7],
  ["Luis Gómez",0,2,1,0,33.3],
  ["Mauricio Esquivel",3,20,1,2,3.8],
];
// Heatmap data: [name, ago25,sep25,oct25,nov25,dic25,ene26,feb26,mar26,abr26,may26,jun26,jul26,ago26, total, ganadas]
const HM=[
  ["Milton Rodríguez",0,0,0,174,0,0,0,2,2,0,0,1,0,179,7],
  ["Jessica Cárdenas Álvarez",0,0,0,20,14,15,13,8,11,3,4,3,2,93,10],
  ["Ulises Castañeda",0,0,0,10,11,13,16,11,5,8,3,6,1,84,6],
  ["Rosa I. Hernandez Garcia",0,0,0,15,5,15,13,10,8,4,0,1,0,71,7],
  ["Sandra Osuna",0,0,0,10,6,10,12,11,6,4,4,1,1,65,2],
  ["Andrea Espinoza",0,0,0,2,7,9,8,8,14,7,3,6,0,64,2],
  ["Dulce Perez",0,0,0,5,15,10,9,10,6,7,1,0,1,64,5],
  ["Zulma Dueñas",0,0,0,3,2,16,16,10,4,6,3,3,0,63,8],
  ["Griselda Castañeda",0,0,0,9,7,7,16,8,8,4,2,1,0,62,5],
  ["Fernanda Armenta",0,0,0,4,5,7,10,8,13,9,3,0,0,59,6],
  ["Veronica Guzman",0,0,0,5,3,6,15,9,6,2,0,1,2,49,9],
  ["Mireya Gonzalez",0,0,0,3,2,3,13,7,3,7,0,1,3,42,4],
  ["Yennifer Acevedo",0,0,0,6,2,5,7,3,5,6,3,0,0,37,2],
  ["Daniel Franco",0,0,0,5,6,8,5,3,5,2,1,0,0,35,3],
  ["Mayra Burgos",0,0,0,0,0,3,11,7,4,5,3,1,1,35,2],
  ["Lydia Gonzalez Rangel",0,0,0,1,2,3,5,5,4,2,1,3,1,27,0],
  ["Mauricio Esquivel",0,0,0,0,0,1,8,8,6,2,1,0,0,26,1],
  ["Blanca Adriana Pérez",0,0,0,0,0,1,6,6,12,0,0,0,0,25,0],
  ["Santiago Franco Zuno",0,0,0,0,0,0,2,10,5,5,0,0,0,22,0],
  ["Samantha Garcia",0,0,0,8,5,3,0,3,2,0,1,0,0,22,0],
  ["Andrea L. Florencio Maciel",0,0,0,8,3,0,0,0,0,0,0,0,0,11,0],
  ["Luis Dominguez",0,0,0,1,10,0,0,0,0,0,0,0,0,11,7],
  ["Arely Quintana",0,0,0,4,2,2,0,2,0,0,0,0,0,10,3],
  ["Fatima Perez Cambero",0,0,0,7,1,0,0,0,0,0,0,0,0,8,0],
  ["Ana Luisa Armadillo Hdz.",0,0,0,0,0,1,3,2,1,0,0,0,0,7,0],
  ["Fernando Carrillo",0,0,0,6,1,0,0,0,0,0,0,0,0,7,0],
  ["Luis Gómez",0,0,0,0,2,1,0,0,0,0,0,0,0,3,1],
  ["Sergio Arenas",0,0,0,0,0,0,0,3,0,0,0,0,0,3,0],
  ["Jorge Fuentes",0,0,0,0,0,2,0,0,0,0,0,0,0,2,0],
  ["Victor Ortiz",0,0,0,0,0,0,0,0,0,0,0,1,0,1,1],
];
const HM_TOTALS=[0,0,0,310,112,141,188,154,132,84,34,31,12,1198,92];

function hmClass(v){
  if(v===0) return 'hm-0';
  if(v<=2) return 'hm-1';
  if(v<=5) return 'hm-2';
  return 'hm-3';
}
function hmCell(v){
  return `<td class="td-hm ${hmClass(v)}">${v===0?'·':v}</td>`;
}

function renderHM(){
  const tb=document.getElementById('hm-body');
  tb.innerHTML=HM.map((r,i)=>{
    const [name,...rest]=r;
    const months=rest.slice(0,13);
    const total=rest[13], gan=rest[14];
    const pct=total>0?Math.round(gan/total*100):0;
    return `<tr class="${i%2?'tr-alt':''}">
      <td class="td-name">${name}</td>
      ${months.map(v=>hmCell(v)).join('')}
      <td class="td-hm td-total-val">${total}</td>
      <td class="td-hm td-ganadas-val">${gan} <span class="wr-pct">(${pct}%)</span></td>
    </tr>`;
  }).join('')+`<tr class="totals-row">
    <td class="td-name">TOTAL</td>
    ${HM_TOTALS.slice(0,13).map(v=>hmCell(v)).join('')}
    <td class="td-hm td-total-val">${HM_TOTALS[13]}</td>
    <td class="td-hm td-ganadas-val">${HM_TOTALS[14]} <span class="wr-pct">(${Math.round(HM_TOTALS[14]/HM_TOTALS[13]*100)}%)</span></td>
  </tr>`;
}

function cc(v){return v>=25?'cell-green':v>=10?'cell-amber':'cell-red'}
function wc(v){return v>=40?'cell-green':v>=20?'cell-amber':'cell-red'}

const tables={
  sdr:{data:[...SDR],col:6,asc:false},
  bdr:{data:[...BDR],col:5,asc:false},
  attr:{data:[...ATTR],col:5,asc:false},
};

function renderSDR(data){
  document.getElementById('sdr-body').innerHTML=data.map((r,i)=>{
    const [name,citas,video,pres,seg,luca,conv,flag]=r;
    const rc=i===0?'rank-1':i===1?'rank-2':i===2?'rank-3':'';
    const b=flag==='warn'?'<span class="badge badge-warn">Revisar</span>':'';
    return `<tr class="${i%2?'tr-alt':''}"><td class="td-name ${rc}">${b}${name}</td><td class="td-num">${citas}</td><td class="td-num">${video}</td><td class="td-num">${pres}</td><td class="td-num">${seg}</td><td class="td-num">${luca}</td><td class="td-num ${cc(conv)}">${conv.toFixed(1)}%</td></tr>`;
  }).join('');
}
function renderBDR(data){
  document.getElementById('bdr-body').innerHTML=data.map((r,i)=>{
    const [name,demos,neg,gan,per,wr,vid,pres]=r;
    const rc=i===0?'rank-1':i===1?'rank-2':i===2?'rank-3':'';
    const b=wr>=50?'<span class="badge badge-ok">Top</span>':wr<10&&(gan+per)>5?'<span class="badge badge-danger">Atención</span>':'';
    return `<tr class="${i%2?'tr-alt':''}"><td class="td-name ${rc}">${b}${name}</td><td class="td-num">${demos}</td><td class="td-num">${neg}</td><td class="td-num cell-green">${gan}</td><td class="td-num cell-red">${per}</td><td class="td-num ${wc(wr)}">${wr.toFixed(1)}%</td><td class="td-num">${vid}</td><td class="td-num">${pres}</td></tr>`;
  }).join('');
}
function renderAttr(data){
  document.getElementById('attr-body').innerHTML=data.map((r,i)=>{
    const [name,demos,neg,gan,per,conv]=r;
    const d=conv>=30?'<span class="badge badge-ok">Alto</span>':conv>=15?'<span class="badge badge-warn">Promedio</span>':'<span class="badge badge-danger">Bajo</span>';
    return `<tr class="${i%2?'tr-alt':''}"><td class="td-name">${name}</td><td class="td-num">${demos}</td><td class="td-num">${neg}</td><td class="td-num cell-green">${gan}</td><td class="td-num cell-red">${per}</td><td class="td-num ${cc(conv)}">${conv.toFixed(1)}%</td><td style="text-align:right">${d}</td></tr>`;
  }).join('');
}

function sort(key,col,num){
  const t=tables[key];
  if(t.col===col) t.asc=!t.asc; else{t.col=col;t.asc=false;}
  t.data.sort((a,b)=>{
    const av=num?+a[col]:a[col].toLowerCase();
    const bv=num?+b[col]:b[col].toLowerCase();
    return t.asc?(av>bv?1:-1):(av<bv?1:-1);
  });
  const tid=key+'-table';
  document.querySelectorAll(`#${tid} thead th`).forEach((th,i)=>{
    th.classList.remove('sorted');
    const ic=th.querySelector('.sort-icon');
    if(ic) ic.textContent='↕';
  });
  const ths=document.querySelectorAll(`#${tid} thead th`);
  if(ths[col]){ths[col].classList.add('sorted');const ic=ths[col].querySelector('.sort-icon');if(ic) ic.textContent=t.asc?'↑':'↓';}
  if(key==='sdr') renderSDR(t.data);
  else if(key==='bdr') renderBDR(t.data);
  else renderAttr(t.data);
}

renderSDR(tables.sdr.data);
renderBDR(tables.bdr.data);
renderAttr(tables.attr.data);
renderHM();

// Chart
(function(){
  var canvas=document.getElementById('trendChart');
  if(!canvas) return;
  var months=["Oct'25","Nov'25","Dic'25","Ene'26","Feb'26","Mar'26","Abr'26","May'26","Jun'26","Jul'26"];
  var citas=[97,81,53,118,151,108,101,81,34,8];
  var demos=[1,21,53,18,27,46,22,24,11,4];
  var closes=[0,0,0,0,0,12,3,13,22,55];
  var dpr=window.devicePixelRatio||1;
  var W=canvas.parentElement.clientWidth;
  var H=170;
  canvas.width=W*dpr;canvas.height=H*dpr;
  canvas.style.width=W+'px';canvas.style.height=H+'px';
  var ctx=canvas.getContext('2d');
  ctx.scale(dpr,dpr);
  var P={t:14,r:16,b:46,l:44};
  var CW=W-P.l-P.r, CH=H-P.t-P.b;
  var n=months.length;
  var mx=Math.max.apply(null,[].concat(citas,demos,closes));
  function xp(i){return P.l+i*CW/(n-1);}
  function yp(v){return P.t+CH*(1-v/mx);}
  for(var g=0;g<=4;g++){
    var gy=P.t+CH*g/4;
    ctx.strokeStyle='#e2e8f0';ctx.lineWidth=1;
    ctx.beginPath();ctx.moveTo(P.l,gy);ctx.lineTo(P.l+CW,gy);ctx.stroke();
    ctx.fillStyle='#94a3b8';ctx.font='10px sans-serif';ctx.textAlign='right';
    ctx.fillText(Math.round(mx*(1-g/4)),P.l-6,gy+3);
  }
  var bw=Math.max(3,CW/n*0.35);
  for(var i=0;i<citas.length;i++){
    if(!citas[i]) continue;
    var bh=CH*citas[i]/mx;
    ctx.fillStyle='rgba(37,99,235,.1)';
    ctx.fillRect(xp(i)-bw/2,P.t+CH-bh,bw,bh);
  }
  function line(data,color,dash){
    ctx.beginPath();ctx.setLineDash(dash||[]);ctx.strokeStyle=color;ctx.lineWidth=2;
    data.forEach(function(v,i){i===0?ctx.moveTo(xp(i),yp(v)):ctx.lineTo(xp(i),yp(v));});
    ctx.stroke();ctx.setLineDash([]);
    data.forEach(function(v,i){if(!v) return;ctx.beginPath();ctx.arc(xp(i),yp(v),3,0,Math.PI*2);ctx.fillStyle=color;ctx.fill();});
  }
  line(citas,'#2563eb');
  line(demos,'#d97706',[4,3]);
  line(closes,'#059669');
  ctx.fillStyle='#94a3b8';ctx.font='10px sans-serif';ctx.textAlign='center';
  months.forEach(function(m,i){ctx.fillText(m,xp(i),P.t+CH+16);});
  var LEG=[['Citas','#2563eb'],['Demos','#d97706'],['Cierres','#059669']];
  var lx=P.l;
  LEG.forEach(function(l){
    ctx.fillStyle=l[1];ctx.fillRect(lx,P.t+CH+28,12,5);
    ctx.fillStyle='#64748b';ctx.font='10px sans-serif';ctx.textAlign='left';
    ctx.fillText(l[0],lx+15,P.t+CH+34);
    lx+=l[0].length*5.5+30;
  });
})();
</script>
</body>
</html>
