<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Luca — SDR &amp; BDR Analytics</title>
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#f0f4f8;
  --surface:#ffffff;
  --surface2:#f7f9fc;
  --border:#e2e8f0;
  --border2:#cbd5e1;
  --text:#1e293b;
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
  --purple:#7c3aed;
  --radius:10px;
}
html{font-size:15px;-webkit-text-size-adjust:100%}
body{background:var(--bg);color:var(--text);
  font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',system-ui,Helvetica,sans-serif;
  line-height:1.6;min-height:100vh;padding-bottom:48px}

/* HEADER */
.hdr{
  background:var(--surface);
  border-bottom:1px solid var(--border);
  padding:20px 24px;
  display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:14px;
}
.hdr-left{display:flex;align-items:center;gap:14px}
.logo{
  width:42px;height:42px;border-radius:10px;flex-shrink:0;
  background:linear-gradient(135deg,#2563eb 0%,#4f46e5 100%);
  display:flex;align-items:center;justify-content:center;
  font-weight:900;font-size:20px;color:#fff;letter-spacing:-1px;
}
.hdr-title{font-size:1.25rem;font-weight:700;color:var(--text);letter-spacing:-.3px}
.hdr-sub{font-size:.78rem;color:var(--muted);margin-top:1px}
.hdr-meta{font-size:.72rem;color:var(--muted);text-align:right;line-height:1.8}
.hdr-meta b{color:var(--text);font-weight:600}

/* LAYOUT */
.wrap{max-width:1200px;margin:0 auto;padding:0 16px}
section{margin-top:36px}
.sec-title{
  font-size:.7rem;font-weight:700;letter-spacing:.1em;text-transform:uppercase;
  color:var(--muted);padding-bottom:10px;border-bottom:2px solid var(--border);
  margin-bottom:18px;display:flex;align-items:center;gap:8px;
}
.sec-dot{width:7px;height:7px;border-radius:50%;background:var(--blue);flex-shrink:0}

/* KPI GRID */
.kpi-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(140px,1fr));gap:12px}
.kpi{
  background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);
  padding:16px 14px 13px;position:relative;overflow:hidden;
}
.kpi::before{
  content:'';position:absolute;top:0;left:0;right:0;height:3px;
  border-radius:var(--radius) var(--radius) 0 0;background:var(--c,var(--blue));
}
.kpi-lbl{font-size:.65rem;font-weight:700;letter-spacing:.07em;
  text-transform:uppercase;color:var(--muted);margin-bottom:8px}
.kpi-val{font-size:1.9rem;font-weight:800;line-height:1;letter-spacing:-1px;color:var(--text)}
.kpi-note{font-size:.68rem;color:var(--light);margin-top:5px}

/* INSIGHTS */
.insights-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(260px,1fr));gap:12px}
.insight-card{
  background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);
  padding:16px;border-left:4px solid var(--c,var(--blue));
}
.insight-card.good{--c:var(--green)}
.insight-card.warn{--c:var(--amber)}
.insight-card.danger{--c:var(--red)}
.insight-card.info{--c:var(--blue)}
.insight-title{font-size:.82rem;font-weight:700;color:var(--text);margin-bottom:5px}
.insight-body{font-size:.76rem;color:var(--muted);line-height:1.55}
.insight-body b{color:var(--text)}
.insight-icon{font-size:.9rem;margin-bottom:6px}

/* TWO COL */
.two-col{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.panel{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:20px}
.panel-lbl{font-size:.7rem;font-weight:700;letter-spacing:.06em;
  text-transform:uppercase;color:var(--muted);margin-bottom:16px}

/* FUNNEL BARS */
.f-row{display:flex;align-items:center;gap:10px;margin-bottom:10px}
.f-lbl{width:175px;font-size:.79rem;color:var(--text);flex-shrink:0;white-space:nowrap}
.f-track{flex:1;height:20px;background:var(--bg);border-radius:5px;overflow:hidden;border:1px solid var(--border)}
.f-bar{height:100%;border-radius:4px;transition:width .5s cubic-bezier(.4,0,.2,1)}
.f-cnt{width:50px;text-align:right;font-size:.79rem;font-weight:600;color:var(--text);flex-shrink:0}
.f-note{font-size:.72rem;color:var(--muted);margin-top:12px;padding-top:10px;border-top:1px solid var(--border)}

/* TABLES */
.t-wrap{overflow-x:auto;border-radius:var(--radius);border:1px solid var(--border);-webkit-overflow-scrolling:touch}
table{border-collapse:collapse;width:100%;font-size:.8rem;min-width:500px}
thead th{
  background:var(--surface2);color:var(--muted);
  font-size:.65rem;font-weight:700;letter-spacing:.08em;text-transform:uppercase;
  padding:10px 12px;border-bottom:1px solid var(--border2);
  white-space:nowrap;text-align:right;cursor:pointer;user-select:none;
  transition:background .1s,color .1s;
}
thead th:hover{background:var(--blue-mid);color:var(--blue)}
thead th:first-child{text-align:left}
thead th.sorted{background:var(--blue-mid);color:var(--blue)}
.sort-icon{margin-left:3px;opacity:.4;font-size:.75em}
thead th.sorted .sort-icon,thead th:hover .sort-icon{opacity:1}
tbody tr{border-bottom:1px solid var(--border);transition:background .1s}
tbody tr:last-child{border-bottom:none}
tbody tr:hover{background:var(--blue-soft)}
.tr-alt{background:var(--surface2)}
td{padding:9px 12px;vertical-align:middle}
.td-name{color:var(--text);font-weight:500;white-space:nowrap}
.td-num{text-align:right;font-variant-numeric:tabular-nums;color:var(--muted)}

/* STATUS COLORS */
.cell-green{color:var(--green)!important;font-weight:600}
.cell-amber{color:var(--amber)!important;font-weight:600}
.cell-red{color:var(--red)!important;font-weight:600}
.rank-1{color:var(--amber);font-weight:700}
.rank-2{color:var(--muted);font-weight:600}
.rank-3{color:#92400e;font-weight:600}

/* BADGES */
.badge{display:inline-block;padding:2px 7px;border-radius:4px;font-size:.65rem;font-weight:700;margin-right:4px}
.badge-ok{background:var(--green-soft);color:var(--green)}
.badge-warn{background:var(--amber-soft);color:var(--amber)}
.badge-danger{background:var(--red-soft);color:var(--red)}
.badge-info{background:var(--blue-soft);color:var(--blue)}

/* SCROLL HINT on mobile */
.scroll-hint{display:none;font-size:.7rem;color:var(--light);text-align:right;margin-bottom:6px;padding-right:2px}

footer{margin-top:48px;border-top:1px solid var(--border);background:var(--surface);
  padding:16px 24px;display:flex;justify-content:space-between;
  font-size:.7rem;color:var(--muted);flex-wrap:wrap;gap:6px;}

/* MOBILE */
@media(max-width:700px){
  .hdr{padding:16px}
  .hdr-meta{display:none}
  .wrap{padding:0 12px}
  .two-col{grid-template-columns:1fr}
  .kpi-grid{grid-template-columns:repeat(2,1fr)}
  .kpi-val{font-size:1.6rem}
  .f-lbl{width:130px;font-size:.72rem}
  .insights-grid{grid-template-columns:1fr}
  .scroll-hint{display:block}
  section{margin-top:28px}
}
@media(max-width:380px){
  .kpi-grid{grid-template-columns:repeat(2,1fr)}
  .kpi-val{font-size:1.4rem}
}
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
    Generado: <b>03 Aug 2026</b><br>
    Org: <b>lucaedu.my.salesforce.com</b><br>
    <b>24</b> SDRs &nbsp;·&nbsp; <b>31</b> BDRs
  </div>
</header>

<div class="wrap">

<!-- KPIs -->
<section>
  <div class="sec-title"><span class="sec-dot"></span>KPIs Globales</div>
  <div class="kpi-grid">
    <div class="kpi" style="--c:#2563eb">
      <div class="kpi-lbl">Citas Generadas</div>
      <div class="kpi-val">868</div>
      <div class="kpi-note">con fecha confirmada</div>
    </div>
    <div class="kpi" style="--c:#0891b2">
      <div class="kpi-lbl">Videollamadas</div>
      <div class="kpi-val">316</div>
      <div class="kpi-note">36% de citas</div>
    </div>
    <div class="kpi" style="--c:#7c3aed">
      <div class="kpi-lbl">Presenciales</div>
      <div class="kpi-val">332</div>
      <div class="kpi-note">38% de citas</div>
    </div>
    <div class="kpi" style="--c:#059669">
      <div class="kpi-lbl">Es Luca</div>
      <div class="kpi-val">141</div>
      <div class="kpi-note">16.2% conv. SDR</div>
    </div>
    <div class="kpi" style="--c:#4f46e5">
      <div class="kpi-lbl">Demos BDR</div>
      <div class="kpi-val">249</div>
      <div class="kpi-note">en cita</div>
    </div>
    <div class="kpi" style="--c:#d97706">
      <div class="kpi-lbl">En Negociación</div>
      <div class="kpi-val">709</div>
      <div class="kpi-note">propuestas activas</div>
    </div>
    <div class="kpi" style="--c:#059669">
      <div class="kpi-lbl">Cerrada Ganada</div>
      <div class="kpi-val">105</div>
      <div class="kpi-note">win rate 35.2%</div>
    </div>
    <div class="kpi" style="--c:#dc2626">
      <div class="kpi-lbl">Cerrada Perdida</div>
      <div class="kpi-val">193</div>
      <div class="kpi-note">64.8% de cerradas</div>
    </div>
  </div>
</section>

<!-- INSIGHTS -->
<section>
  <div class="sec-title"><span class="sec-dot" style="background:var(--amber)"></span>Hallazgos Clave</div>
  <div class="insights-grid">
    <div class="insight-card good">
      <div class="insight-icon">🏆</div>
      <div class="insight-title">Top BDR: Luis Dominguez — 62.5% win rate</div>
      <div class="insight-body">Con <b>20 cierres de 32 oportunidades</b>, es el BDR más efectivo. Perfil presencial fuerte (28 vs. 70 video).</div>
    </div>
    <div class="insight-card good">
      <div class="insight-icon">⭐</div>
      <div class="insight-title">SDR destaque: Georgina Ruvalcaba — 26.3% conv.</div>
      <div class="insight-body"><b>38 citas, 10 clientes</b>. Mejor conversión SDR con volumen real. Candidata a mentora.</div>
    </div>
    <div class="insight-card warn">
      <div class="insight-icon">⚠️</div>
      <div class="insight-title">Milton figura como SDR y BDR — revisar datos</div>
      <div class="insight-body"><b>344 citas como SDR</b> (40% del equipo) + <b>179 demos como BDR con 0 cierres</b>. Posible rol duplicado en Salesforce.</div>
    </div>
    <div class="insight-card warn">
      <div class="insight-icon">📊</div>
      <div class="insight-title">709 opps en negociación sin resolución</div>
      <div class="insight-body">Pipeline grande sin limpiar. Si hay opps stale, el win rate real podría ser muy diferente.</div>
    </div>
    <div class="insight-card danger">
      <div class="insight-icon">🔴</div>
      <div class="insight-title">Jorge Fuentes: 54 opps — 7.4% win rate</div>
      <div class="insight-body"><b>50 en negociación, solo 4 ganadas</b>. Mayor volumen con menor efectividad. Requiere coaching.</div>
    </div>
    <div class="insight-card danger">
      <div class="insight-icon">🔴</div>
      <div class="insight-title">Alfonso Reyes: 88 opps — 6.1% win rate</div>
      <div class="insight-body"><b>55 negociación, 31 perdidas, 2 ganadas</b>. Junto a Jorge, los dos BDRs con mayor brecha volumen/efectividad.</div>
    </div>
    <div class="insight-card info">
      <div class="insight-icon">💡</div>
      <div class="insight-title">Luis Dominguez genera sus propias opps (63.6%)</div>
      <div class="insight-body">Aparece también como SDR Soporte con la conversión más alta. Maneja ciclo completo o tiene cuentas directas.</div>
    </div>
    <div class="insight-card info">
      <div class="insight-icon">📋</div>
      <div class="insight-title">332 presenciales a cruzar con gastos</div>
      <div class="insight-body">Cada visita presencial debe tener respaldo. Recomendable auditar gastos vs. registro en Salesforce mensualmente.</div>
    </div>
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
      <div class="f-note">Conversión total SDR: <b style="color:var(--green)">5.4%</b> (141 / 2,622 en seguimiento)</div>
    </div>
    <div class="panel">
      <div class="panel-lbl">💼 Funnel BDR — Oportunidades</div>
      <div class="f-row"><div class="f-lbl">Demo en Cita</div><div class="f-track"><div class="f-bar" style="width:35.1%;background:#2563eb"></div></div><div class="f-cnt">249</div></div>
      <div class="f-row"><div class="f-lbl">Negociación</div><div class="f-track"><div class="f-bar" style="width:100%;background:#d97706"></div></div><div class="f-cnt">709</div></div>
      <div class="f-row"><div class="f-lbl">Cerrada Ganada ✓</div><div class="f-track"><div class="f-bar" style="width:14.8%;background:#059669"></div></div><div class="f-cnt">105</div></div>
      <div class="f-row"><div class="f-lbl">Cerrada Perdida ✗</div><div class="f-track"><div class="f-bar" style="width:27.2%;background:#dc2626"></div></div><div class="f-cnt">193</div></div>
      <div class="f-note">Win rate: <b style="color:var(--green)">35.2%</b> (105/298 cerradas) · Pipeline: <b style="color:var(--amber)">709 opps activas</b></div>
    </div>
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
  <div class="sec-title"><span class="sec-dot" style="background:var(--green)"></span>Performance BDR — Top representantes</div>
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

</div>

<footer>
  <span>Luca Educación · Datos al 03 Aug 2026</span>
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

function cc(v){return v>=25?'cell-green':v>=10?'cell-amber':'cell-red'}
function wc(v){return v>=40?'cell-green':v>=20?'cell-amber':'cell-red'}

let tables={
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
  const tableId=key+'-table';
  document.querySelectorAll(`#${tableId} thead th`).forEach((th,i)=>{
    th.classList.remove('sorted');
    const ic=th.querySelector('.sort-icon');
    if(ic) ic.textContent='↕';
  });
  const ths=document.querySelectorAll(`#${tableId} thead th`);
  if(ths[col]){
    ths[col].classList.add('sorted');
    const ic=ths[col].querySelector('.sort-icon');
    if(ic) ic.textContent=t.asc?'↑':'↓';
  }
  if(key==='sdr') renderSDR(t.data);
  else if(key==='bdr') renderBDR(t.data);
  else renderAttr(t.data);
}

renderSDR(tables.sdr.data);
renderBDR(tables.bdr.data);
renderAttr(tables.attr.data);
</script>
</body>
</html>
