<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Luca — SDR &amp; BDR Analytics</title>
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root{
  --navy:#060d1f;--panel:#0d1526;--panel2:#0f1c35;
  --border:#1a2d4a;--border2:#243c5c;
  --text:#dde6f0;--muted:#7a95b4;
  --blue:#3b82f6;--indigo:#6366f1;--green:#10b981;
  --amber:#f59e0b;--red:#ef4444;--cyan:#06b6d4;--purple:#a855f7;
}
html{font-size:15px}
body{background:var(--navy);color:var(--text);
  font-family:-apple-system,BlinkMacSystemFont,'Segoe UI','Inter',Helvetica,sans-serif;
  line-height:1.55;min-height:100vh;padding-bottom:60px}

/* Header */
.hdr{
  background:linear-gradient(160deg,#091320 0%,#060d1f 100%);
  border-bottom:1px solid var(--border);
  padding:32px 48px 26px;display:flex;
  align-items:flex-end;justify-content:space-between;flex-wrap:wrap;gap:16px;
}
.hdr-left{display:flex;align-items:center;gap:16px}
.logo{
  width:46px;height:46px;border-radius:10px;flex-shrink:0;
  background:linear-gradient(135deg,#3b82f6 0%,#6366f1 100%);
  display:flex;align-items:center;justify-content:center;
  font-weight:900;font-size:22px;color:#fff;letter-spacing:-1px;
  box-shadow:0 0 24px rgba(59,130,246,.4);
}
.hdr-title{
  font-size:1.55rem;font-weight:800;letter-spacing:-.5px;
  background:linear-gradient(90deg,#93c5fd 0%,#c4b5fd 100%);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;
}
.hdr-sub{font-size:.78rem;color:var(--muted);margin-top:2px}
.hdr-meta{font-size:.73rem;color:var(--muted);text-align:right;line-height:1.7}
.hdr-meta b{color:var(--text);font-weight:600}

/* Layout */
.wrap{max-width:1440px;margin:0 auto;padding:0 40px}
section{margin-top:52px}
.sec-title{
  font-size:.72rem;font-weight:700;letter-spacing:.12em;
  text-transform:uppercase;color:var(--muted);
  padding-bottom:12px;border-bottom:1px solid var(--border);
  margin-bottom:24px;display:flex;align-items:center;gap:10px;
}
.sec-dot{width:8px;height:8px;border-radius:50%;background:var(--blue);
  box-shadow:0 0 8px var(--blue);display:inline-block;flex-shrink:0}

/* KPI grid */
.kpi-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(165px,1fr));gap:14px}
.kpi{
  background:var(--panel);border:1px solid var(--border);border-radius:12px;
  padding:20px 18px 16px;position:relative;overflow:hidden;
  transition:border-color .18s,transform .15s;
}
.kpi::before{
  content:'';position:absolute;top:0;left:0;right:0;height:3px;
  border-radius:12px 12px 0 0;background:var(--c,var(--blue));
}
.kpi:hover{border-color:var(--border2);transform:translateY(-2px)}
.kpi-lbl{font-size:.68rem;font-weight:700;letter-spacing:.07em;
  text-transform:uppercase;color:var(--muted);margin-bottom:10px}
.kpi-val{font-size:2.1rem;font-weight:800;line-height:1;letter-spacing:-1.5px}
.kpi-note{font-size:.7rem;color:var(--muted);margin-top:6px}

/* Two-col */
.two-col{display:grid;grid-template-columns:1fr 1fr;gap:20px}
.panel{background:var(--panel);border:1px solid var(--border);border-radius:12px;padding:24px}
.panel-lbl{font-size:.72rem;font-weight:700;letter-spacing:.06em;
  text-transform:uppercase;color:var(--muted);margin-bottom:18px}

/* Funnel bars */
.f-row{display:flex;align-items:center;gap:14px;margin-bottom:12px}
.f-lbl{width:185px;font-size:.8rem;color:var(--text);flex-shrink:0;white-space:nowrap}
.f-track{flex:1;height:24px;background:rgba(255,255,255,.04);border-radius:6px;overflow:hidden}
.f-bar{height:100%;border-radius:6px;transition:width .5s cubic-bezier(.4,0,.2,1)}
.f-cnt{width:55px;text-align:right;font-size:.8rem;font-weight:600;color:var(--text);flex-shrink:0}

/* Tables */
.t-wrap{overflow-x:auto;border-radius:10px;border:1px solid var(--border)}
table{border-collapse:collapse;width:100%;font-size:.81rem}
thead th{
  background:var(--panel2);color:var(--muted);
  font-size:.67rem;font-weight:700;letter-spacing:.09em;text-transform:uppercase;
  padding:11px 14px;border-bottom:1px solid var(--border2);
  white-space:nowrap;text-align:right;
}
thead th:first-child{text-align:left}
tbody tr{border-bottom:1px solid var(--border);transition:background .1s}
tbody tr:last-child{border-bottom:none}
tbody tr:hover{background:rgba(59,130,246,.06)}
.tr-alt{background:rgba(255,255,255,.016)}
td{padding:10px 14px;vertical-align:middle}
.td-name{color:var(--text);font-weight:500;white-space:nowrap;min-width:160px}
.td-num{text-align:right;font-variant-numeric:tabular-nums;color:var(--muted)}
.cell-green{color:#34d399!important;font-weight:600}
.cell-amber{color:#fbbf24!important;font-weight:600}
.cell-red{color:#f87171!important;font-weight:600}

/* Chart */
.chart-box{padding:24px;background:var(--panel);border:1px solid var(--border);border-radius:12px}
.chart-box canvas{display:block;width:100%!important}

/* Footer */
footer{
  margin-top:72px;border-top:1px solid var(--border);
  padding:20px 48px;display:flex;justify-content:space-between;
  font-size:.7rem;color:var(--muted);flex-wrap:wrap;gap:6px;
}

@media(max-width:860px){
  .hdr{padding:20px 18px}
  .wrap{padding:0 16px}
  .two-col{grid-template-columns:1fr}
  .kpi-grid{grid-template-columns:repeat(auto-fill,minmax(140px,1fr))}
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
    Generado: <b>03 Aug 2026, 16:13</b><br>
    Org: <b>lucaedu.my.salesforce.com</b><br>
    <b>24</b> SDRs &nbsp;·&nbsp; <b>31</b> BDRs
  </div>
</header>

<div class="wrap">

<!-- KPIs -->
<section>
  <div class="sec-title"><span class="sec-dot"></span>KPIs Globales</div>
  <div class="kpi-grid">
    <div class="kpi" style="--c:#3b82f6">
      <div class="kpi-lbl">Citas Generadas</div>
      <div class="kpi-val">868</div>
      <div class="kpi-note">con fecha confirmada</div>
    </div>
    <div class="kpi" style="--c:#06b6d4">
      <div class="kpi-lbl">Videollamadas</div>
      <div class="kpi-val">316</div>
      <div class="kpi-note">36% de citas</div>
    </div>
    <div class="kpi" style="--c:#a855f7">
      <div class="kpi-lbl">Presenciales</div>
      <div class="kpi-val">332</div>
      <div class="kpi-note">38% de citas</div>
    </div>
    <div class="kpi" style="--c:#10b981">
      <div class="kpi-lbl">Es Luca</div>
      <div class="kpi-val">141</div>
      <div class="kpi-note">16% conv. SDR</div>
    </div>
    <div class="kpi" style="--c:#6366f1">
      <div class="kpi-lbl">Demos BDR</div>
      <div class="kpi-val">249</div>
      <div class="kpi-note">en cita</div>
    </div>
    <div class="kpi" style="--c:#f59e0b">
      <div class="kpi-lbl">En Negociación</div>
      <div class="kpi-val">709</div>
      <div class="kpi-note">propuestas activas</div>
    </div>
    <div class="kpi" style="--c:#10b981">
      <div class="kpi-lbl">Cerrada Ganada</div>
      <div class="kpi-val">105</div>
      <div class="kpi-note">win rate 35%</div>
    </div>
    <div class="kpi" style="--c:#ef4444">
      <div class="kpi-lbl">Cerrada Perdida</div>
      <div class="kpi-val">193</div>
      <div class="kpi-note">65% de cerradas</div>
    </div>
  </div>
</section>

<!-- Funnels -->
<section>
  <div class="sec-title"><span class="sec-dot" style="background:var(--indigo);box-shadow:0 0 8px var(--indigo)"></span>Funnels de Conversión</div>
  <div class="two-col">
    <div class="panel">
      <div class="panel-lbl">🎯 Funnel SDR — Cuentas por Etapa</div>
      
      <div class="f-row">
        <div class="f-lbl">Entrada</div>
        <div class="f-track"><div class="f-bar" style="width:85.7%;background:#3b82f6;box-shadow:0 0 10px #3b82f655"></div></div>
        <div class="f-cnt">2,622</div>
      </div>
      <div class="f-row">
        <div class="f-lbl">En seguimiento</div>
        <div class="f-track"><div class="f-bar" style="width:57.2%;background:#6366f1;box-shadow:0 0 10px #6366f155"></div></div>
        <div class="f-cnt">1,749</div>
      </div>
      <div class="f-row">
        <div class="f-lbl">Cita agendada</div>
        <div class="f-track"><div class="f-bar" style="width:29.2%;background:#f59e0b;box-shadow:0 0 10px #f59e0b55"></div></div>
        <div class="f-cnt">893</div>
      </div>
      <div class="f-row">
        <div class="f-lbl">Es Luca</div>
        <div class="f-track"><div class="f-bar" style="width:4.6%;background:#10b981;box-shadow:0 0 10px #10b98155"></div></div>
        <div class="f-cnt">141</div>
      </div>
      <div class="f-row">
        <div class="f-lbl">Rechazó</div>
        <div class="f-track"><div class="f-bar" style="width:100.0%;background:#ef4444;box-shadow:0 0 10px #ef444455"></div></div>
        <div class="f-cnt">3,060</div>
      </div>
    </div>
    <div class="panel">
      <div class="panel-lbl">💼 Funnel BDR — Oportunidades por Etapa</div>
      
      <div class="f-row">
        <div class="f-lbl">Demo en Cita</div>
        <div class="f-track"><div class="f-bar" style="width:35.1%;background:#3b82f6;box-shadow:0 0 10px #3b82f655"></div></div>
        <div class="f-cnt">249</div>
      </div>
      <div class="f-row">
        <div class="f-lbl">Negociación de Propuesta</div>
        <div class="f-track"><div class="f-bar" style="width:100.0%;background:#f59e0b;box-shadow:0 0 10px #f59e0b55"></div></div>
        <div class="f-cnt">709</div>
      </div>
      <div class="f-row">
        <div class="f-lbl">Cerrada ganada</div>
        <div class="f-track"><div class="f-bar" style="width:14.8%;background:#10b981;box-shadow:0 0 10px #10b98155"></div></div>
        <div class="f-cnt">105</div>
      </div>
      <div class="f-row">
        <div class="f-lbl">Cerrada perdida</div>
        <div class="f-track"><div class="f-bar" style="width:27.2%;background:#ef4444;box-shadow:0 0 10px #ef444455"></div></div>
        <div class="f-cnt">193</div>
      </div>
    </div>
  </div>
</section>

<!-- Monthly Trend -->
<section>
  <div class="sec-title"><span class="sec-dot" style="background:var(--cyan);box-shadow:0 0 8px var(--cyan)"></span>Tendencia Mensual</div>
  <div class="chart-box">
    <div class="panel-lbl" style="margin-bottom:16px">📈 Citas · Demos · Cierres por Mes</div>
    <canvas id="trendChart" height="190"></canvas>
  </div>
</section>

<!-- SDR Table -->
<section>
  <div class="sec-title"><span class="sec-dot" style="background:var(--blue)"></span>Performance SDR — 24 representantes</div>
  <div class="t-wrap">
    <table>
      <thead><tr>
        <th style="text-align:left">SDR</th>
        <th>Citas<br>Confirmadas</th>
        <th>Video&shy;llamadas</th>
        <th>Presenciales</th>
        <th>En<br>Seguimiento</th>
        <th>Es Luca</th>
        <th>Conv %</th>
      </tr></thead>
      <tbody><tr class="">
          <td class="td-name">Milton Rodríguez</td>
          <td class="td-num">344</td>
          <td class="td-num">149</td>
          <td class="td-num">132</td>
          <td class="td-num">842</td>
          <td class="td-num">71</td>
          <td class="td-num cell-green">20.6%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Jessica Cárdenas Álvarez</td>
          <td class="td-num">56</td>
          <td class="td-num">12</td>
          <td class="td-num">32</td>
          <td class="td-num">36</td>
          <td class="td-num">10</td>
          <td class="td-num cell-amber">17.9%</td>
        </tr><tr class="">
          <td class="td-name">Ulises Castañeda</td>
          <td class="td-num">55</td>
          <td class="td-num">32</td>
          <td class="td-num">10</td>
          <td class="td-num">49</td>
          <td class="td-num">2</td>
          <td class="td-num cell-red">3.6%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Fernanda Armenta</td>
          <td class="td-num">51</td>
          <td class="td-num">26</td>
          <td class="td-num">7</td>
          <td class="td-num">29</td>
          <td class="td-num">11</td>
          <td class="td-num cell-green">21.6%</td>
        </tr><tr class="">
          <td class="td-name">Griselda Castañeda</td>
          <td class="td-num">41</td>
          <td class="td-num">6</td>
          <td class="td-num">27</td>
          <td class="td-num">120</td>
          <td class="td-num">5</td>
          <td class="td-num cell-amber">12.2%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Georgina Ruvalcaba</td>
          <td class="td-num">38</td>
          <td class="td-num">6</td>
          <td class="td-num">28</td>
          <td class="td-num">39</td>
          <td class="td-num">10</td>
          <td class="td-num cell-green">26.3%</td>
        </tr><tr class="">
          <td class="td-name">ROSA ISABEL HERNANDEZ GARCIA</td>
          <td class="td-num">34</td>
          <td class="td-num">13</td>
          <td class="td-num">12</td>
          <td class="td-num">80</td>
          <td class="td-num">2</td>
          <td class="td-num cell-amber">5.9%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Veronica Guzman</td>
          <td class="td-num">34</td>
          <td class="td-num">5</td>
          <td class="td-num">17</td>
          <td class="td-num">46</td>
          <td class="td-num">4</td>
          <td class="td-num cell-amber">11.8%</td>
        </tr><tr class="">
          <td class="td-name">Daniel Franco</td>
          <td class="td-num">34</td>
          <td class="td-num">6</td>
          <td class="td-num">13</td>
          <td class="td-num">24</td>
          <td class="td-num">3</td>
          <td class="td-num cell-amber">8.8%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Sandra Osuna</td>
          <td class="td-num">32</td>
          <td class="td-num">12</td>
          <td class="td-num">2</td>
          <td class="td-num">63</td>
          <td class="td-num">2</td>
          <td class="td-num cell-amber">6.2%</td>
        </tr><tr class="">
          <td class="td-name">Isis Arriaga Ontiveros</td>
          <td class="td-num">28</td>
          <td class="td-num">4</td>
          <td class="td-num">14</td>
          <td class="td-num">61</td>
          <td class="td-num">5</td>
          <td class="td-num cell-amber">17.9%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Fabiola De Anda</td>
          <td class="td-num">27</td>
          <td class="td-num">9</td>
          <td class="td-num">8</td>
          <td class="td-num">71</td>
          <td class="td-num">3</td>
          <td class="td-num cell-amber">11.1%</td>
        </tr><tr class="">
          <td class="td-name">Yvonne De La Vega</td>
          <td class="td-num">26</td>
          <td class="td-num">11</td>
          <td class="td-num">9</td>
          <td class="td-num">21</td>
          <td class="td-num">1</td>
          <td class="td-num cell-red">3.8%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Samantha Garcia</td>
          <td class="td-num">22</td>
          <td class="td-num">11</td>
          <td class="td-num">5</td>
          <td class="td-num">97</td>
          <td class="td-num">2</td>
          <td class="td-num cell-amber">9.1%</td>
        </tr><tr class="">
          <td class="td-name">ileana alvarez</td>
          <td class="td-num">15</td>
          <td class="td-num">4</td>
          <td class="td-num">3</td>
          <td class="td-num">56</td>
          <td class="td-num">1</td>
          <td class="td-num cell-amber">6.7%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Fatima perez cambero</td>
          <td class="td-num">9</td>
          <td class="td-num">1</td>
          <td class="td-num">8</td>
          <td class="td-num">67</td>
          <td class="td-num">4</td>
          <td class="td-num cell-green">44.4%</td>
        </tr><tr class="">
          <td class="td-name">Andrea Espinoza</td>
          <td class="td-num">4</td>
          <td class="td-num">3</td>
          <td class="td-num">0</td>
          <td class="td-num">5</td>
          <td class="td-num">0</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Lydia Gonzalez Rangel</td>
          <td class="td-num">3</td>
          <td class="td-num">1</td>
          <td class="td-num">2</td>
          <td class="td-num">8</td>
          <td class="td-num">0</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="">
          <td class="td-name">Dulce Perez</td>
          <td class="td-num">3</td>
          <td class="td-num">2</td>
          <td class="td-num">1</td>
          <td class="td-num">7</td>
          <td class="td-num">0</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">ZULMA DUEÑAS</td>
          <td class="td-num">3</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
          <td class="td-num">3</td>
          <td class="td-num">0</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="">
          <td class="td-name">Yennifer Acevedo</td>
          <td class="td-num">3</td>
          <td class="td-num">1</td>
          <td class="td-num">0</td>
          <td class="td-num">8</td>
          <td class="td-num">3</td>
          <td class="td-num cell-green">100.0%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Amanda Torres Romo</td>
          <td class="td-num">2</td>
          <td class="td-num">0</td>
          <td class="td-num">1</td>
          <td class="td-num">7</td>
          <td class="td-num">0</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="">
          <td class="td-name">Mireya Gonzalez</td>
          <td class="td-num">2</td>
          <td class="td-num">0</td>
          <td class="td-num">1</td>
          <td class="td-num">7</td>
          <td class="td-num">0</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Andrea lorena Florencio Maciel</td>
          <td class="td-num">2</td>
          <td class="td-num">2</td>
          <td class="td-num">0</td>
          <td class="td-num">3</td>
          <td class="td-num">2</td>
          <td class="td-num cell-green">100.0%</td>
        </tr></tbody>
    </table>
  </div>
</section>

<!-- BDR Table -->
<section>
  <div class="sec-title"><span class="sec-dot" style="background:var(--green);box-shadow:0 0 8px var(--green)"></span>Performance BDR — 31 representantes</div>
  <div class="t-wrap">
    <table>
      <thead><tr>
        <th style="text-align:left">BDR</th>
        <th>Demos</th>
        <th>Negociación</th>
        <th>Ganadas</th>
        <th>Perdidas</th>
        <th>Win Rate</th>
        <th>Video&shy;llamadas</th>
        <th>Presenciales</th>
      </tr></thead>
      <tbody><tr class="">
          <td class="td-name">Luis Dominguez</td>
          <td class="td-num">8</td>
          <td class="td-num">63</td>
          <td class="td-num cell-green">20</td>
          <td class="td-num cell-red">12</td>
          <td class="td-num cell-green">62.5%</td>
          <td class="td-num">70</td>
          <td class="td-num">28</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Marcos Gutiérrez</td>
          <td class="td-num">4</td>
          <td class="td-num">111</td>
          <td class="td-num cell-green">18</td>
          <td class="td-num cell-red">19</td>
          <td class="td-num cell-green">48.6%</td>
          <td class="td-num">71</td>
          <td class="td-num">75</td>
        </tr><tr class="">
          <td class="td-name">Luis Gómez</td>
          <td class="td-num">1</td>
          <td class="td-num">32</td>
          <td class="td-num cell-green">17</td>
          <td class="td-num cell-red">29</td>
          <td class="td-num cell-amber">37.0%</td>
          <td class="td-num">20</td>
          <td class="td-num">47</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Fernando Carrillo</td>
          <td class="td-num">4</td>
          <td class="td-num">102</td>
          <td class="td-num cell-green">14</td>
          <td class="td-num cell-red">10</td>
          <td class="td-num cell-green">58.3%</td>
          <td class="td-num">33</td>
          <td class="td-num">80</td>
        </tr><tr class="">
          <td class="td-name">Omar Sanchez</td>
          <td class="td-num">0</td>
          <td class="td-num">61</td>
          <td class="td-num cell-green">11</td>
          <td class="td-num cell-red">10</td>
          <td class="td-num cell-green">52.4%</td>
          <td class="td-num">25</td>
          <td class="td-num">57</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Andres Reyes</td>
          <td class="td-num">7</td>
          <td class="td-num">63</td>
          <td class="td-num cell-green">8</td>
          <td class="td-num cell-red">13</td>
          <td class="td-num cell-amber">38.1%</td>
          <td class="td-num">44</td>
          <td class="td-num">44</td>
        </tr><tr class="">
          <td class="td-name">Sergio Arenas</td>
          <td class="td-num">1</td>
          <td class="td-num">38</td>
          <td class="td-num cell-green">6</td>
          <td class="td-num cell-red">11</td>
          <td class="td-num cell-amber">35.3%</td>
          <td class="td-num">7</td>
          <td class="td-num">44</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Jorge Fuentes</td>
          <td class="td-num">0</td>
          <td class="td-num">50</td>
          <td class="td-num cell-green">4</td>
          <td class="td-num cell-red">50</td>
          <td class="td-num cell-red">7.4%</td>
          <td class="td-num">60</td>
          <td class="td-num">40</td>
        </tr><tr class="">
          <td class="td-name">Alfonso Reyes</td>
          <td class="td-num">0</td>
          <td class="td-num">55</td>
          <td class="td-num cell-green">2</td>
          <td class="td-num cell-red">31</td>
          <td class="td-num cell-red">6.1%</td>
          <td class="td-num">61</td>
          <td class="td-num">22</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Alex Pérez</td>
          <td class="td-num">0</td>
          <td class="td-num">33</td>
          <td class="td-num cell-green">2</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-green">100.0%</td>
          <td class="td-num">25</td>
          <td class="td-num">10</td>
        </tr><tr class="">
          <td class="td-name">Frederico Bello</td>
          <td class="td-num">0</td>
          <td class="td-num">3</td>
          <td class="td-num cell-green">1</td>
          <td class="td-num cell-red">1</td>
          <td class="td-num cell-green">50.0%</td>
          <td class="td-num">0</td>
          <td class="td-num">3</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Angel Martinez</td>
          <td class="td-num">2</td>
          <td class="td-num">10</td>
          <td class="td-num cell-green">1</td>
          <td class="td-num cell-red">1</td>
          <td class="td-num cell-green">50.0%</td>
          <td class="td-num">5</td>
          <td class="td-num">2</td>
        </tr><tr class="">
          <td class="td-name">Victor Ortiz</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">1</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-green">100.0%</td>
          <td class="td-num">1</td>
          <td class="td-num">0</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Arely Quintana</td>
          <td class="td-num">11</td>
          <td class="td-num">80</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">2</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">53</td>
          <td class="td-num">36</td>
        </tr><tr class="">
          <td class="td-name">Andrea lorena Florencio Maciel</td>
          <td class="td-num">1</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Milton Rodríguez</td>
          <td class="td-num">179</td>
          <td class="td-num">4</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">3</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">95</td>
          <td class="td-num">60</td>
        </tr><tr class="">
          <td class="td-name">Ulises Castañeda</td>
          <td class="td-num">2</td>
          <td class="td-num">1</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">2</td>
          <td class="td-num">0</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Sandra Osuna</td>
          <td class="td-num">1</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">1</td>
          <td class="td-num">0</td>
        </tr><tr class="">
          <td class="td-name">Daniel False</td>
          <td class="td-num">0</td>
          <td class="td-num">2</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">2</td>
          <td class="td-num">0</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Mireya Gonzalez</td>
          <td class="td-num">1</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
        </tr><tr class="">
          <td class="td-name">Samantha Garcia</td>
          <td class="td-num">3</td>
          <td class="td-num">1</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">1</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">2</td>
          <td class="td-num">0</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Veronica Guzman</td>
          <td class="td-num">1</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">0</td>
          <td class="td-num">1</td>
        </tr><tr class="">
          <td class="td-name">Amanda Torres Romo</td>
          <td class="td-num">1</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
        </tr><tr class="tr-alt">
          <td class="td-name">ROSA ISABEL HERNANDEZ GARCIA</td>
          <td class="td-num">1</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
        </tr><tr class="">
          <td class="td-name">Daniel Franco</td>
          <td class="td-num">2</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
        </tr><tr class="tr-alt">
          <td class="td-name">ZULMA DUEÑAS</td>
          <td class="td-num">1</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
        </tr><tr class="">
          <td class="td-name">Fernanda Armenta</td>
          <td class="td-num">1</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Fatima perez cambero</td>
          <td class="td-num">5</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
        </tr><tr class="">
          <td class="td-name">Yennifer Acevedo</td>
          <td class="td-num">5</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Jessica Cárdenas Álvarez</td>
          <td class="td-num">6</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
        </tr><tr class="">
          <td class="td-name">Griselda Castañeda</td>
          <td class="td-num">1</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
          <td class="td-num">0</td>
          <td class="td-num">1</td>
        </tr></tbody>
    </table>
  </div>
</section>

<!-- Attribution -->
<section>
  <div class="sec-title"><span class="sec-dot" style="background:var(--amber);box-shadow:0 0 8px var(--amber)"></span>Atribución SDR → BDR — 38 SDRs con oportunidades</div>
  <div class="t-wrap">
    <table>
      <thead><tr>
        <th style="text-align:left">SDR (Soporte)</th>
        <th>Demos</th>
        <th>Negociación</th>
        <th>Ganadas</th>
        <th>Perdidas</th>
        <th>Conv %</th>
      </tr></thead>
      <tbody><tr class="">
          <td class="td-name">Jessica Cárdenas Álvarez</td>
          <td class="td-num">14</td>
          <td class="td-num">58</td>
          <td class="td-num cell-green">10</td>
          <td class="td-num cell-red">11</td>
          <td class="td-num cell-amber">10.8%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Veronica Guzman</td>
          <td class="td-num">12</td>
          <td class="td-num">24</td>
          <td class="td-num cell-green">9</td>
          <td class="td-num cell-red">4</td>
          <td class="td-num cell-amber">18.4%</td>
        </tr><tr class="">
          <td class="td-name">ZULMA DUEÑAS</td>
          <td class="td-num">6</td>
          <td class="td-num">36</td>
          <td class="td-num cell-green">8</td>
          <td class="td-num cell-red">13</td>
          <td class="td-num cell-amber">12.7%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Luis Dominguez</td>
          <td class="td-num">0</td>
          <td class="td-num">4</td>
          <td class="td-num cell-green">7</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-green">63.6%</td>
        </tr><tr class="">
          <td class="td-name">Milton Rodríguez</td>
          <td class="td-num">17</td>
          <td class="td-num">110</td>
          <td class="td-num cell-green">7</td>
          <td class="td-num cell-red">45</td>
          <td class="td-num cell-red">3.9%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">ROSA ISABEL HERNANDEZ GARCIA</td>
          <td class="td-num">13</td>
          <td class="td-num">41</td>
          <td class="td-num cell-green">7</td>
          <td class="td-num cell-red">10</td>
          <td class="td-num cell-red">9.9%</td>
        </tr><tr class="">
          <td class="td-name">Fernanda Armenta</td>
          <td class="td-num">6</td>
          <td class="td-num">42</td>
          <td class="td-num cell-green">6</td>
          <td class="td-num cell-red">5</td>
          <td class="td-num cell-amber">10.2%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Ulises Castañeda</td>
          <td class="td-num">18</td>
          <td class="td-num">44</td>
          <td class="td-num cell-green">6</td>
          <td class="td-num cell-red">16</td>
          <td class="td-num cell-red">7.1%</td>
        </tr><tr class="">
          <td class="td-name">Dulce Perez</td>
          <td class="td-num">14</td>
          <td class="td-num">26</td>
          <td class="td-num cell-green">5</td>
          <td class="td-num cell-red">19</td>
          <td class="td-num cell-red">7.8%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Griselda Castañeda</td>
          <td class="td-num">7</td>
          <td class="td-num">45</td>
          <td class="td-num cell-green">5</td>
          <td class="td-num cell-red">5</td>
          <td class="td-num cell-red">8.1%</td>
        </tr><tr class="">
          <td class="td-name">Mireya Gonzalez</td>
          <td class="td-num">12</td>
          <td class="td-num">22</td>
          <td class="td-num cell-green">4</td>
          <td class="td-num cell-red">4</td>
          <td class="td-num cell-red">9.5%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Arely Quintana</td>
          <td class="td-num">2</td>
          <td class="td-num">4</td>
          <td class="td-num cell-green">3</td>
          <td class="td-num cell-red">1</td>
          <td class="td-num cell-green">30.0%</td>
        </tr><tr class="">
          <td class="td-name">Daniel Franco</td>
          <td class="td-num">10</td>
          <td class="td-num">16</td>
          <td class="td-num cell-green">3</td>
          <td class="td-num cell-red">6</td>
          <td class="td-num cell-red">8.6%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Yennifer Acevedo</td>
          <td class="td-num">20</td>
          <td class="td-num">13</td>
          <td class="td-num cell-green">2</td>
          <td class="td-num cell-red">2</td>
          <td class="td-num cell-red">5.4%</td>
        </tr><tr class="">
          <td class="td-name">Andrea Espinoza</td>
          <td class="td-num">15</td>
          <td class="td-num">42</td>
          <td class="td-num cell-green">2</td>
          <td class="td-num cell-red">5</td>
          <td class="td-num cell-red">3.1%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Sandra Osuna</td>
          <td class="td-num">17</td>
          <td class="td-num">33</td>
          <td class="td-num cell-green">2</td>
          <td class="td-num cell-red">13</td>
          <td class="td-num cell-red">3.1%</td>
        </tr><tr class="">
          <td class="td-name">Mayra Burgos</td>
          <td class="td-num">6</td>
          <td class="td-num">24</td>
          <td class="td-num cell-green">2</td>
          <td class="td-num cell-red">3</td>
          <td class="td-num cell-red">5.7%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Luis Gómez</td>
          <td class="td-num">0</td>
          <td class="td-num">2</td>
          <td class="td-num cell-green">1</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-green">33.3%</td>
        </tr><tr class="">
          <td class="td-name">Mauricio Esquivel</td>
          <td class="td-num">3</td>
          <td class="td-num">20</td>
          <td class="td-num cell-green">1</td>
          <td class="td-num cell-red">2</td>
          <td class="td-num cell-red">3.8%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Victor Ortiz</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">1</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-green">100.0%</td>
        </tr><tr class="">
          <td class="td-name">Daniela Soto</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">1</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-green">100.0%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Samantha Garcia</td>
          <td class="td-num">7</td>
          <td class="td-num">9</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">6</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="">
          <td class="td-name">Amanda Torres Romo</td>
          <td class="td-num">1</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Betzabeth Hernández</td>
          <td class="td-num">1</td>
          <td class="td-num">1</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="">
          <td class="td-name">Lydia Gonzalez Rangel</td>
          <td class="td-num">7</td>
          <td class="td-num">12</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">8</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Fernando Carrillo</td>
          <td class="td-num">0</td>
          <td class="td-num">6</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">1</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="">
          <td class="td-name">Axel Rangel</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">1</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Jorge Fuentes</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">2</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="">
          <td class="td-name">Santiago Franco Zuno</td>
          <td class="td-num">7</td>
          <td class="td-num">11</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">4</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Andrea lorena Florencio Maciel</td>
          <td class="td-num">5</td>
          <td class="td-num">5</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">1</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="">
          <td class="td-name">Ana Luisa Armadillo Hernández</td>
          <td class="td-num">4</td>
          <td class="td-num">3</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Tania Molina</td>
          <td class="td-num">0</td>
          <td class="td-num">1</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="">
          <td class="td-name">Blanca Adriana Pérez</td>
          <td class="td-num">14</td>
          <td class="td-num">9</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">2</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Fatima perez cambero</td>
          <td class="td-num">7</td>
          <td class="td-num">1</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="">
          <td class="td-name">Fabiola De Anda</td>
          <td class="td-num">0</td>
          <td class="td-num">1</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Sergio Arenas</td>
          <td class="td-num">0</td>
          <td class="td-num">3</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="">
          <td class="td-name">Daniel False</td>
          <td class="td-num">0</td>
          <td class="td-num">2</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">0</td>
          <td class="td-num cell-red">0.0%</td>
        </tr><tr class="tr-alt">
          <td class="td-name">Yvonne De La Vega</td>
          <td class="td-num">0</td>
          <td class="td-num">0</td>
          <td class="td-num cell-green">0</td>
          <td class="td-num cell-red">2</td>
          <td class="td-num cell-red">0.0%</td>
        </tr></tbody>
    </table>
  </div>
</section>

</div>

<footer>
  <span>Luca Educación · Datos al 03 Aug 2026, 16:13</span>
  <span>Fuente: Salesforce · lucaedu.my.salesforce.com</span>
</footer>

<script>
(function(){
  var canvas = document.getElementById('trendChart');
  if (!canvas) return;
  var months = ["2025-10", "2025-11", "2025-12", "2026-01", "2026-02", "2026-03", "2026-04", "2026-05", "2026-06", "2026-07", "2026-08", "2026-11"];
  var citas  = [97, 81, 53, 118, 151, 108, 101, 81, 34, 8, 0, 0];
  var demos  = [1, 21, 53, 18, 27, 46, 22, 24, 11, 4, 20, 2];
  var closes = [0, 0, 0, 0, 0, 12, 3, 13, 22, 55, 0, 0];
  var dpr = window.devicePixelRatio||1;
  var W = canvas.parentElement.clientWidth;
  var H = 190;
  canvas.width  = W*dpr; canvas.height = H*dpr;
  canvas.style.width = W+'px'; canvas.style.height = H+'px';
  var c = canvas.getContext('2d');
  c.scale(dpr, dpr);
  var PAD={t:18,r:20,b:52,l:50};
  var CW=W-PAD.l-PAD.r, CH=H-PAD.t-PAD.b;
  var n=months.length;
  var all=[].concat(citas,demos,closes).filter(function(v){return v>0;});
  var mx=all.length?Math.max.apply(null,all):1;
  function xp(i){return PAD.l+i*CW/(n-1||1);}
  function yp(v){return PAD.t+CH*(1-v/mx);}
  /* grid lines */
  for(var g=0;g<=4;g++){
    var gy=PAD.t+CH*g/4;
    c.strokeStyle='rgba(255,255,255,.05)'; c.lineWidth=1;
    c.beginPath(); c.moveTo(PAD.l,gy); c.lineTo(PAD.l+CW,gy); c.stroke();
    c.fillStyle='rgba(122,149,180,.65)'; c.font='10px sans-serif'; c.textAlign='right';
    c.fillText(Math.round(mx*(1-g/4)),PAD.l-8,gy+4);
  }
  /* bar background for citas */
  var bw=Math.max(4,CW/n*0.4);
  for(var i=0;i<citas.length;i++){
    if(!citas[i]) continue;
    var bh=CH*citas[i]/mx;
    c.fillStyle='rgba(59,130,246,.18)';
    c.fillRect(xp(i)-bw/2, PAD.t+CH-bh, bw, bh);
  }
  /* line drawing helper */
  function line(data, color, dash){
    c.beginPath(); c.setLineDash(dash||[]); c.strokeStyle=color; c.lineWidth=2.5;
    var started=false;
    for(var i=0;i<data.length;i++){
      if(!started){c.moveTo(xp(i),yp(data[i]));started=true;}
      else c.lineTo(xp(i),yp(data[i]));
    }
    c.stroke(); c.setLineDash([]);
    for(var i=0;i<data.length;i++){
      if(!data[i]) continue;
      c.beginPath(); c.arc(xp(i),yp(data[i]),3.5,0,Math.PI*2);
      c.fillStyle=color; c.fill();
    }
  }
  line(citas, '#3b82f6');
  line(demos,  '#f59e0b', [5,3]);
  line(closes, '#10b981');
  /* x labels */
  var MN=['','Ene','Feb','Mar','Abr','May','Jun','Jul','Ago','Sep','Oct','Nov','Dic'];
  c.fillStyle='rgba(122,149,180,.8)'; c.font='10px sans-serif'; c.textAlign='center';
  for(var i=0;i<months.length;i++){
    var parts=months[i].split('-');
    c.fillText(MN[+parts[1]]+" '"+parts[0].slice(2), xp(i), PAD.t+CH+18);
  }
  /* legend */
  var LEG=[['Citas','#3b82f6'],['Demos BDR','#f59e0b'],['Cierres','#10b981']];
  var lx=PAD.l;
  for(var j=0;j<LEG.length;j++){
    c.fillStyle=LEG[j][1]; c.fillRect(lx,PAD.t+CH+34,14,6);
    c.fillStyle='rgba(200,215,230,.75)'; c.font='10px sans-serif'; c.textAlign='left';
    c.fillText(LEG[j][0],lx+18,PAD.t+CH+42);
    lx+=LEG[j][0].length*6.5+34;
  }
})();
</script>
</body>
</html>
