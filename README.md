# Trading-Journal-
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FuturesEdge — Trading Journal</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Sora:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0d0f1a;
    --surface: #141725;
    --surface2: #1c2035;
    --surface3: #242840;
    --border: rgba(255,255,255,0.07);
    --text: #e8eaf6;
    --muted: #7b82a8;
    --cyan: #00e5ff;
    --green: #00e676;
    --red: #ff1744;
    --orange: #ff9100;
    --purple: #d500f9;
    --yellow: #ffea00;
    --blue: #448aff;
    --pink: #f50057;
    --font: 'Sora', sans-serif;
    --mono: 'Space Mono', monospace;
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  body { background:var(--bg); color:var(--text); font-family:var(--font); min-height:100vh; overflow-x:hidden; }

  /* SCROLLBAR */
  ::-webkit-scrollbar { width:6px; height:6px; }
  ::-webkit-scrollbar-track { background:var(--surface); }
  ::-webkit-scrollbar-thumb { background:var(--surface3); border-radius:3px; }

  /* LAYOUT */
  .app { display:flex; min-height:100vh; }
  .sidebar { width:220px; background:var(--surface); border-right:1px solid var(--border); display:flex; flex-direction:column; position:fixed; top:0; left:0; height:100vh; z-index:100; }
  .main { margin-left:220px; flex:1; display:flex; flex-direction:column; min-height:100vh; }

  /* SIDEBAR */
  .logo { padding:24px 20px 20px; border-bottom:1px solid var(--border); }
  .logo-text { font-size:18px; font-weight:800; letter-spacing:-0.5px; }
  .logo-text span { color:var(--cyan); }
  .logo-sub { font-size:10px; color:var(--muted); letter-spacing:2px; text-transform:uppercase; margin-top:2px; font-family:var(--mono); }
  .nav { padding:16px 0; flex:1; }
  .nav-item { display:flex; align-items:center; gap:10px; padding:10px 20px; cursor:pointer; font-size:13px; font-weight:500; color:var(--muted); transition:all .2s; border-left:2px solid transparent; }
  .nav-item:hover { color:var(--text); background:var(--surface2); }
  .nav-item.active { color:var(--cyan); border-left-color:var(--cyan); background:rgba(0,229,255,0.06); }
  .nav-icon { font-size:16px; width:20px; text-align:center; }
  .nav-badge { margin-left:auto; background:var(--cyan); color:#000; font-size:10px; font-weight:700; padding:1px 6px; border-radius:10px; font-family:var(--mono); }
  .sidebar-footer { padding:16px 20px; border-top:1px solid var(--border); }
  .account-pill { display:flex; align-items:center; gap:8px; }
  .account-avatar { width:28px; height:28px; border-radius:50%; background:linear-gradient(135deg, var(--cyan), var(--purple)); display:flex; align-items:center; justify-content:center; font-size:11px; font-weight:700; color:#000; }
  .account-info { font-size:11px; }
  .account-name { font-weight:600; color:var(--text); }
  .account-balance { color:var(--green); font-family:var(--mono); font-size:10px; }

  /* TOP BAR */
  .topbar { background:var(--surface); border-bottom:1px solid var(--border); padding:12px 28px; display:flex; align-items:center; justify-content:space-between; }
  .page-title { font-size:20px; font-weight:700; }
  .page-title span { color:var(--cyan); }
  .topbar-actions { display:flex; gap:10px; align-items:center; }
  .btn { display:inline-flex; align-items:center; gap:6px; padding:8px 16px; border-radius:8px; font-size:13px; font-weight:600; cursor:pointer; border:none; transition:all .2s; font-family:var(--font); }
  .btn-primary { background:var(--cyan); color:#000; }
  .btn-primary:hover { background:#33ebff; transform:translateY(-1px); box-shadow:0 4px 20px rgba(0,229,255,0.3); }
  .btn-ghost { background:transparent; color:var(--muted); border:1px solid var(--border); }
  .btn-ghost:hover { color:var(--text); border-color:rgba(255,255,255,0.15); }
  .btn-danger { background:rgba(255,23,68,0.15); color:var(--red); border:1px solid rgba(255,23,68,0.3); }
  .btn-danger:hover { background:rgba(255,23,68,0.25); }

  /* CONTENT */
  .content { padding:24px 28px; flex:1; }

  /* PAGES */
  .page { display:none; animation:fadeIn .3s ease; }
  .page.active { display:block; }
  @keyframes fadeIn { from{opacity:0;transform:translateY(8px)} to{opacity:1;transform:none} }

  /* STATS GRID */
  .stats-grid { display:grid; grid-template-columns:repeat(4, 1fr); gap:16px; margin-bottom:24px; }
  .stat-card { background:var(--surface); border:1px solid var(--border); border-radius:14px; padding:18px 20px; position:relative; overflow:hidden; }
  .stat-card::before { content:''; position:absolute; top:0; left:0; right:0; height:2px; }
  .stat-card.cyan::before { background:var(--cyan); }
  .stat-card.green::before { background:var(--green); }
  .stat-card.red::before { background:var(--red); }
  .stat-card.orange::before { background:var(--orange); }
  .stat-card.purple::before { background:var(--purple); }
  .stat-card.blue::before { background:var(--blue); }
  .stat-label { font-size:11px; color:var(--muted); text-transform:uppercase; letter-spacing:1px; font-family:var(--mono); }
  .stat-value { font-size:26px; font-weight:800; font-family:var(--mono); margin:6px 0 4px; }
  .stat-value.pos { color:var(--green); }
  .stat-value.neg { color:var(--red); }
  .stat-value.neu { color:var(--cyan); }
  .stat-sub { font-size:11px; color:var(--muted); }
  .stat-icon { position:absolute; top:16px; right:16px; font-size:22px; opacity:.3; }

  /* CHART AREA */
  .chart-row { display:grid; grid-template-columns:2fr 1fr; gap:16px; margin-bottom:24px; }
  .card { background:var(--surface); border:1px solid var(--border); border-radius:14px; padding:20px; }
  .card-header { display:flex; align-items:center; justify-content:space-between; margin-bottom:16px; }
  .card-title { font-size:13px; font-weight:700; text-transform:uppercase; letter-spacing:1px; color:var(--muted); font-family:var(--mono); }
  .card-title span { color:var(--text); }

  /* EQUITY CHART */
  .equity-chart { width:100%; height:160px; position:relative; }
  .equity-chart canvas { width:100%!important; height:100%!important; }
  svg.chart { width:100%; height:160px; overflow:visible; }
  .chart-line { fill:none; stroke:var(--cyan); stroke-width:2.5; }
  .chart-area { fill:url(#chartGrad); }
  .chart-dot { fill:var(--cyan); }

  /* DONUT */
  .donut-wrap { display:flex; align-items:center; gap:20px; }
  .donut-svg { flex-shrink:0; }
  .donut-legend { font-size:12px; }
  .donut-item { display:flex; align-items:center; gap:6px; margin-bottom:8px; }
  .donut-dot { width:8px; height:8px; border-radius:50%; }

  /* TRADES TABLE */
  .table-wrap { overflow-x:auto; }
  table { width:100%; border-collapse:collapse; font-size:13px; }
  thead tr { border-bottom:1px solid var(--border); }
  th { padding:10px 12px; text-align:left; font-size:10px; text-transform:uppercase; letter-spacing:1px; color:var(--muted); font-family:var(--mono); font-weight:400; }
  td { padding:12px 12px; border-bottom:1px solid rgba(255,255,255,0.03); }
  tr:hover td { background:rgba(255,255,255,0.02); }
  tr.last-added td { background:rgba(0,229,255,0.04); }
  .badge { display:inline-flex; align-items:center; padding:3px 8px; border-radius:6px; font-size:11px; font-weight:600; font-family:var(--mono); }
  .badge-long { background:rgba(0,230,118,.15); color:var(--green); }
  .badge-short { background:rgba(255,23,68,.15); color:var(--red); }
  .badge-win { background:rgba(0,230,118,.12); color:var(--green); }
  .badge-loss { background:rgba(255,23,68,.12); color:var(--red); }
  .badge-be { background:rgba(255,145,0,.12); color:var(--orange); }
  .market-tag { display:inline-flex; align-items:center; gap:4px; font-size:12px; font-weight:600; }
  .market-dot { width:6px; height:6px; border-radius:50%; }
  .pnl-pos { color:var(--green); font-family:var(--mono); font-weight:700; }
  .pnl-neg { color:var(--red); font-family:var(--mono); font-weight:700; }
  .action-btn { background:none; border:1px solid var(--border); color:var(--muted); padding:4px 8px; border-radius:6px; cursor:pointer; font-size:11px; transition:all .2s; }
  .action-btn:hover { color:var(--text); border-color:rgba(255,255,255,0.2); }

  /* MODAL */
  .modal-overlay { position:fixed; inset:0; background:rgba(0,0,0,.7); z-index:1000; display:flex; align-items:center; justify-content:center; backdrop-filter:blur(4px); opacity:0; pointer-events:none; transition:opacity .25s; }
  .modal-overlay.open { opacity:1; pointer-events:all; }
  .modal { background:var(--surface); border:1px solid var(--border); border-radius:18px; width:640px; max-width:95vw; max-height:90vh; overflow-y:auto; padding:28px; transform:scale(.95) translateY(10px); transition:transform .25s; box-shadow:0 20px 60px rgba(0,0,0,.5); }
  .modal-overlay.open .modal { transform:scale(1) translateY(0); }
  .modal-title { font-size:18px; font-weight:800; margin-bottom:20px; display:flex; align-items:center; gap:10px; }
  .modal-title .icon { width:32px; height:32px; background:rgba(0,229,255,.1); border-radius:8px; display:flex; align-items:center; justify-content:center; font-size:16px; }
  .form-grid { display:grid; grid-template-columns:1fr 1fr; gap:14px; }
  .form-group { display:flex; flex-direction:column; gap:6px; }
  .form-group.full { grid-column:1/-1; }
  label { font-size:11px; color:var(--muted); text-transform:uppercase; letter-spacing:1px; font-family:var(--mono); }
  input, select, textarea { background:var(--surface2); border:1px solid var(--border); color:var(--text); padding:10px 12px; border-radius:8px; font-size:13px; font-family:var(--font); width:100%; transition:border-color .2s; outline:none; }
  input:focus, select:focus, textarea:focus { border-color:rgba(0,229,255,.4); }
  select option { background:var(--surface2); }
  textarea { resize:vertical; min-height:70px; }
  .emotion-grid { display:grid; grid-template-columns:repeat(5, 1fr); gap:8px; }
  .emotion-btn { background:var(--surface2); border:1px solid var(--border); border-radius:8px; padding:10px 6px; cursor:pointer; text-align:center; transition:all .2s; }
  .emotion-btn:hover { border-color:rgba(255,255,255,0.2); }
  .emotion-btn.selected { border-color:var(--cyan); background:rgba(0,229,255,.1); }
  .emotion-emoji { font-size:20px; display:block; margin-bottom:3px; }
  .emotion-label { font-size:10px; color:var(--muted); }
  .modal-actions { display:flex; gap:10px; justify-content:flex-end; margin-top:20px; padding-top:16px; border-top:1px solid var(--border); }

  /* RATING STARS */
  .rating { display:flex; gap:4px; }
  .star { font-size:18px; cursor:pointer; color:var(--surface3); transition:color .15s; }
  .star.on { color:var(--yellow); }

  /* IMAGE UPLOAD */
  .img-upload { background:var(--surface2); border:2px dashed var(--border); border-radius:10px; padding:24px; text-align:center; cursor:pointer; transition:all .2s; }
  .img-upload:hover { border-color:rgba(0,229,255,.3); background:rgba(0,229,255,.03); }
  .img-upload-text { font-size:12px; color:var(--muted); margin-top:6px; }
  .img-preview { max-width:100%; border-radius:8px; margin-top:10px; display:none; }
  #screenshotInput { display:none; }

  /* STATS PAGE */
  .stats-hero { display:grid; grid-template-columns:repeat(3, 1fr); gap:16px; margin-bottom:24px; }
  .hero-card { background:var(--surface); border:1px solid var(--border); border-radius:14px; padding:20px; text-align:center; }
  .hero-big { font-size:36px; font-weight:800; font-family:var(--mono); }
  .hero-label { font-size:11px; color:var(--muted); text-transform:uppercase; letter-spacing:1px; font-family:var(--mono); margin-top:4px; }

  .metrics-grid { display:grid; grid-template-columns:1fr 1fr; gap:16px; }
  .metric-row { display:flex; align-items:center; justify-content:space-between; padding:10px 0; border-bottom:1px solid var(--border); }
  .metric-row:last-child { border-bottom:none; }
  .metric-name { font-size:13px; color:var(--muted); }
  .metric-val { font-size:13px; font-weight:700; font-family:var(--mono); }

  /* CALENDAR */
  .cal-grid { display:grid; grid-template-columns:repeat(7,1fr); gap:6px; }
  .cal-header { display:grid; grid-template-columns:repeat(7,1fr); gap:6px; margin-bottom:6px; }
  .cal-day-name { text-align:center; font-size:10px; color:var(--muted); font-family:var(--mono); padding:4px; }
  .cal-cell { aspect-ratio:1; border-radius:8px; display:flex; flex-direction:column; align-items:center; justify-content:center; font-size:11px; cursor:pointer; transition:all .2s; background:var(--surface2); border:1px solid transparent; }
  .cal-cell:hover { border-color:rgba(255,255,255,.15); }
  .cal-cell.win { background:rgba(0,230,118,.12); border-color:rgba(0,230,118,.2); color:var(--green); }
  .cal-cell.loss { background:rgba(255,23,68,.12); border-color:rgba(255,23,68,.2); color:var(--red); }
  .cal-cell.be { background:rgba(255,145,0,.1); border-color:rgba(255,145,0,.2); color:var(--orange); }
  .cal-cell.empty { background:transparent; border:none; cursor:default; }
  .cal-cell.today { border-color:var(--cyan)!important; }
  .cal-pnl { font-size:9px; font-family:var(--mono); margin-top:1px; }

  /* PROGRESS BARS */
  .progress-bar { height:6px; background:var(--surface3); border-radius:3px; overflow:hidden; margin-top:6px; }
  .progress-fill { height:100%; border-radius:3px; transition:width .6s ease; }

  /* EMOTION CHART */
  .emotion-stats { display:grid; grid-template-columns:repeat(5, 1fr); gap:10px; text-align:center; }
  .emotion-stat { background:var(--surface2); border-radius:10px; padding:12px 8px; }
  .emotion-stat-emoji { font-size:24px; }
  .emotion-stat-count { font-size:18px; font-weight:800; font-family:var(--mono); margin:4px 0 2px; }
  .emotion-stat-label { font-size:10px; color:var(--muted); }

  /* TAGS */
  .tag { display:inline-block; background:var(--surface3); padding:2px 7px; border-radius:5px; font-size:11px; color:var(--muted); margin:2px; }

  /* EMPTY STATE */
  .empty-state { text-align:center; padding:60px 20px; color:var(--muted); }
  .empty-icon { font-size:48px; margin-bottom:12px; opacity:.4; }
  .empty-text { font-size:14px; }

  /* TOAST */
  .toast { position:fixed; bottom:24px; right:24px; background:var(--surface); border:1px solid var(--border); border-radius:10px; padding:12px 18px; font-size:13px; z-index:2000; display:flex; align-items:center; gap:8px; box-shadow:0 8px 30px rgba(0,0,0,.4); transform:translateY(20px); opacity:0; transition:all .3s; pointer-events:none; }
  .toast.show { transform:none; opacity:1; }
  .toast.success { border-left:3px solid var(--green); }
  .toast.error { border-left:3px solid var(--red); }

  /* FILTER BAR */
  .filter-bar { display:flex; gap:8px; flex-wrap:wrap; margin-bottom:16px; align-items:center; }
  .filter-chip { padding:6px 12px; border-radius:20px; font-size:12px; cursor:pointer; border:1px solid var(--border); background:transparent; color:var(--muted); transition:all .2s; font-family:var(--font); }
  .filter-chip:hover { color:var(--text); }
  .filter-chip.active { background:var(--cyan); color:#000; border-color:var(--cyan); font-weight:600; }

  /* RESPONSIVE */
  @media(max-width:900px) {
    .sidebar { width:60px; }
    .sidebar .logo-text, .sidebar .logo-sub, .nav-item span, .account-info, .nav-badge { display:none; }
    .main { margin-left:60px; }
    .stats-grid { grid-template-columns:repeat(2,1fr); }
    .chart-row { grid-template-columns:1fr; }
    .form-grid { grid-template-columns:1fr; }
  }
</style>
</head>
<body>
<div class="app">

  <aside class="sidebar">
    <div class="logo">
      <div class="logo-text">Futures<span>Edge</span></div>
      <div class="logo-sub">Trading Journal</div>
    </div>
    <nav class="nav">
      <div class="nav-item active" onclick="showPage('dashboard', this)">
        <span class="nav-icon">📊</span><span>Dashboard</span>
      </div>
      <div class="nav-item" onclick="showPage('trades', this)">
        <span class="nav-icon">📋</span><span>Trades</span>
        <span class="nav-badge" id="tradesCount">0</span>
      </div>
      <div class="nav-item" onclick="showPage('stats', this)">
        <span class="nav-icon">📈</span><span>Estadísticas</span>
      </div>
      <div class="nav-item" onclick="showPage('calendar', this)">
        <span class="nav-icon">📅</span><span>Calendario</span>
      </div>
    </nav>
    <div class="sidebar-footer">
      <div class="account-pill">
        <div class="account-avatar">FE</div>
        <div class="account-info">
          <div class="account-name">Mi Cuenta</div>
          <div class="account-balance" id="sidebarBalance">$0.00</div>
        </div>
      </div>
    </div>
  </aside>

  <main class="main">
    <div class="topbar">
      <div class="page-title" id="pageTitle">Dashboard <span>General</span></div>
      <div class="topbar-actions">
        <button class="btn btn-ghost" onclick="openImportModal()">⬆ Importar</button>
        <button class="btn btn-primary" onclick="openTradeModal()">+ Nuevo Trade</button>
      </div>
    </div>

    <div class="content">

      <div class="page active" id="page-dashboard">
        <div class="stats-grid">
          <div class="stat-card cyan">
            <div class="stat-icon">💰</div>
            <div class="stat-label">P&L Total</div>
            <div class="stat-value neu" id="dash-pnl">$0.00</div>
            <div class="stat-sub" id="dash-pnl-sub">0 trades registrados</div>
          </div>
          <div class="stat-card green">
            <div class="stat-icon">🎯</div>
            <div class="stat-label">Win Rate</div>
            <div class="stat-value pos" id="dash-wr">0%</div>
            <div class="stat-sub" id="dash-wr-sub">0W / 0L</div>
          </div>
          <div class="stat-card orange">
            <div class="stat-icon">⚖️</div>
            <div class="stat-label">Profit Factor</div>
            <div class="stat-value neu" id="dash-pf">—</div>
            <div class="stat-sub">Ganancias / Pérdidas</div>
          </div>
          <div class="stat-card purple">
            <div class="stat-icon">📐</div>
            <div class="stat-label">R:R Promedio</div>
            <div class="stat-value neu" id="dash-rr">—</div>
            <div class="stat-sub">Ratio riesgo/beneficio</div>
          </div>
        </div>

        <div class="chart-row">
          <div class="card">
            <div class="card-header">
              <span class="card-title"><span>Curva de Equity</span></span>
              <span style="font-size:11px;color:var(--muted);font-family:var(--mono)">P&L acumulado</span>
            </div>
            <svg class="chart" id="equitySvg" viewBox="0 0 600 160" preserveAspectRatio="none">
              <defs>
                <linearGradient id="chartGrad" x1="0" y1="0" x2="0" y2="1">
                  <stop offset="0%" stop-color="#00e5ff" stop-opacity="0.25"/>
                  <stop offset="100%" stop-color="#00e5ff" stop-opacity="0"/>
                </linearGradient>
              </defs>
              <path class="chart-area" id="chartArea" d=""/>
              <path class="chart-line" id="chartLine" d=""/>
            </svg>
          </div>
          <div class="card">
            <div class="card-header"><span class="card-title"><span>Por Mercado</span></span></div>
            <div class="donut-wrap">
              <svg class="donut-svg" width="100" height="100" viewBox="0 0 100 100" id="donutSvg">
                <circle cx="50" cy="50" r="38" fill="none" stroke="var(--surface3)" stroke-width="14"/>
                <text x="50" y="55" text-anchor="middle" font-size="12" fill="var(--muted)" font-family="Space Mono">—</text>
              </svg>
              <div class="donut-legend" id="donutLegend">
                <div style="font-size:12px;color:var(--muted)">Sin trades aún</div>
              </div>
            </div>
          </div>
        </div>

        <div class="card">
          <div class="card-header">
            <span class="card-title"><span>Últimos Trades</span></span>
            <button class="btn btn-ghost" style="padding:5px 10px;font-size:11px" onclick="showPage('trades', document.querySelector('.nav-item:nth-child(2)'))">Ver todos →</button>
          </div>
          <div class="table-wrap" id="recentTradesTable">
            <div class="empty-state"><div class="empty-icon">📭</div><div class="empty-text">No hay trades todavía. ¡Añade el primero!</div></div>
          </div>
        </div>
      </div>

      <div class="page" id="page-trades">
        <div class="filter-bar">
          <span style="font-size:12px;color:var(--muted);font-family:var(--mono)">FILTRAR:</span>
          <button class="filter-chip active" onclick="filterTrades('all',this)">Todos</button>
          <button class="filter-chip" onclick="filterTrades('long',this)">Long</button>
          <button class="filter-chip" onclick="filterTrades('short',this)">Short</button>
          <button class="filter-chip" onclick="filterTrades('win',this)">Wins</button>
          <button class="filter-chip" onclick="filterTrades('loss',this)">Losses</button>
          <button class="filter-chip" onclick="filterTrades('crypto',this)">Crypto</button>
          <button class="filter-chip" onclick="filterTrades('indices',this)">Índices</button>
          <button class="filter-chip" onclick="filterTrades('forex',this)">Forex</button>
          <button class="filter-chip" onclick="filterTrades('commodities',this)">Commodities</button>
        </div>
        <div class="card">
          <div class="table-wrap" id="allTradesTable">
            <div class="empty-state"><div class="empty-icon">📋</div><div class="empty-text">Aún no hay trades. Empieza registrando uno.</div></div>
          </div>
        </div>
      </div>

      <div class="page" id="page-stats">
        <div class="stats-hero">
          <div class="hero-card">
            <div class="hero-big pos" id="st-best">—</div>
            <div class="hero-label">🏆 Mejor trade</div>
          </div>
          <div class="hero-card">
            <div class="hero-big neg" id="st-worst">—</div>
            <div class="hero-label">💥 Peor trade</div>
          </div>
          <div class="hero-card">
            <div class="hero-big neu" id="st-streak">0</div>
            <div class="hero-label">🔥 Racha ganadora</div>
          </div>
        </div>

        <div class="metrics-grid">
          <div class="card">
            <div class="card-header"><span class="card-title"><span>Métricas Clave</span></span></div>
            <div id="keyMetrics">
              <div style="color:var(--muted);font-size:13px">Sin datos aún</div>
            </div>
          </div>
          <div class="card">
            <div class="card-header"><span class="card-title"><span>P&L por Mercado</span></span></div>
            <div id="marketBreakdown">
              <div style="color:var(--muted);font-size:13px">Sin datos aún</div>
            </div>
          </div>
          <div class="card" style="grid-column:1/-1">
            <div class="card-header"><span class="card-title"><span>Estado Emocional</span></span></div>
            <div class="emotion-stats" id="emotionStats">
              <div class="emotion-stat"><div class="emotion-stat-emoji">😤</div><div class="emotion-stat-count" id="em-revenge">0</div><div class="emotion-stat-label">Revenge</div></div>
              <div class="emotion-stat"><div class="emotion-stat-emoji">😰</div><div class="emotion-stat-count" id="em-fear">0</div><div class="emotion-stat-label">Miedo</div></div>
              <div class="emotion-stat"><div class="emotion-stat-emoji">😐</div><div class="emotion-stat-count" id="em-neutral">0</div><div class="emotion-stat-label">Neutral</div></div>
              <div class="emotion-stat"><div class="emotion-stat-emoji">😊</div><div class="emotion-stat-count" id="em-confident">0</div><div class="emotion-stat-label">Confiado</div></div>
              <div class="emotion-stat"><div class="emotion-stat-emoji">🤩</div><div class="emotion-stat-count" id="em-fomo">0</div><div class="emotion-stat-label">FOMO</div></div>
            </div>
          </div>
        </div>
      </div>

      <div class="page" id="page-calendar">
        <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:20px;">
          <div style="display:flex;align-items:center;gap:16px;">
            <button class="btn btn-ghost" style="padding:6px 12px" onclick="changeMonth(-1)">←</button>
            <h2 style="font-size:18px;font-weight:700" id="calMonthLabel">—</h2>
            <button class="btn btn-ghost" style="padding:6px 12px" onclick="changeMonth(1)">→</button>
          </div>
          <div style="display:flex;gap:12px;font-size:12px;">
            {% if result == "win" %}
✅ Win
{% elsif result == "loss" %}
❌ Loss
{% elsif result == "be" %}
➖ BE
{% endif %}         
        </div>
        </div>
        <div class="card">
          <div class="cal-header">
            <div class="cal-day-name">LUN</div><div class="cal-day-name">MAR</div><div class="cal-day-name">MIÉ</div>
            <div class="cal-day-name">JUE</div><div class="cal-day-name">VIE</div><div class="cal-day-name">SÁB</div><div class="cal-day-name">DOM</div>
          </div>
          <div class="cal-grid" id="calGrid"></div>
        </div>
        <div style="margin-top:16px;display:grid;grid-template-columns:repeat(4,1fr);gap:12px" id="calMonthStats"></div>
      </div>

    </div></main>
</div><div class="modal-overlay" id="tradeModal">
  <div class="modal">
    <div class="modal-title">
      <div class="icon">📝</div>
      <span id="modalTitleText">Registrar Trade</span>
    </div>

    <div class="form-grid">
      <div class="form-group">
        <label>Fecha & Hora Entrada</label>
        <input type="datetime-local" id="f-date">
      </div>
      <div class="form-group">
        <label>Fecha & Hora Salida</label>
        <input type="datetime-local" id="f-dateClose">
      </div>
      <div class="form-group">
        <label>Instrumento</label>
        <input type="text" id="f-symbol" placeholder="BTC-PERP, ES, XAUUSD...">
      </div>
      <div class="form-group">
        <label>Mercado</label>
        <select id="f-market">
          <option value="crypto">🟣 Crypto</option>
          <option value="indices">🔵 Índices</option>
          <option value="forex">🟡 Forex</option>
          <option value="commodities">🟠 Commodities</option>
        </select>
      </div>
      <div class="form-group">
        <label>Dirección</label>
        <select id="f-side">
          <option value="long">Long 📈</option>
          <option value="short">Short 📉</option>
        </select>
      </div>
      <div class="form-group">
        <label>Tamaño / Contratos</label>
        <input type="number" id="f-size" placeholder="1" min="0" step="0.01">
      </div>
      <div class="form-group">
        <label>Precio Entrada</label>
        <input type="number" id="f-entry" placeholder="0.00" step="0.01">
      </div>
      <div class="form-group">
        <label>Precio Salida</label>
        <input type="number" id="f-exit" placeholder="0.00" step="0.01">
      </div>
      <div class="form-group">
        <label>Stop Loss</label>
        <input type="number" id="f-sl" placeholder="0.00" step="0.01">
      </div>
      <div class="form-group">
        <label>Take Profit</label>
        <input type="number" id="f-tp" placeholder="0.00" step="0.01">
      </div>
      <div class="form-group">
        <label>P&L ($)</label>
        <input type="number" id="f-pnl" placeholder="250.00" step="0.01">
      </div>
      <div class="form-group">
        <label>Comisiones ($)</label>
        <input type="number" id="f-fees" placeholder="0.00" step="0.01">
      </div>
      <div class="form-group">
        <label>Setup / Estrategia</label>
        <input type="text" id="f-setup" placeholder="Breakout, Mean Reversion, ICT...">
      </div>
      <div class="form-group">
        <label>Resultado</label>
        <select id="f-result">
          <option value="win">✅ Win</option>
          <option value="loss">❌ Loss</option>
          <option value="be">➖ Break Even</option>
        </select>
      </div>
      <div class="form-group">
        <label>Calidad del Trade (1-5)</label>
        <div class="rating" id="ratingStars">
          <span class="star" onclick="setRating(1)">★</span>
          <span class="star" onclick="setRating(2)">★</span>
          <span class="star" onclick="setRating(3)">★</span>
          <span class="star" onclick="setRating(4)">★</span>
          <span class="star" onclick="setRating(5)">★</span>
        </div>
      </div>
      <div class="form-group">
        <label>R:R Real</label>
        <input type="number" id="f-rr" placeholder="2.5" step="0.1">
      </div>

      <div class="form-group full">
        <label>Estado Emocional</label>
        <div class="emotion-grid">
          <div class="emotion-btn" onclick="selectEmotion('revenge', this)"><span class="emotion-emoji">😤</span><span class="emotion-label">Revenge</span></div>
          <div class="emotion-btn" onclick="selectEmotion('fear', this)"><span class="emotion-emoji">😰</span><span class="emotion-label">Miedo</span></div>
          <div class="emotion-btn" onclick="selectEmotion('neutral', this)"><span class="emotion-emoji">😐</span><span class="emotion-label">Neutral</span></div>
          <div class="emotion-btn" onclick="selectEmotion('confident', this)"><span class="emotion-emoji">😊</span><span class="emotion-label">Confiado</span></div>
          <div class="emotion-btn" onclick="selectEmotion('fomo', this)"><span class="emotion-emoji">🤩</span><span class="emotion-label">FOMO</span></div>
        </div>
      </div>

      <div class="form-group full">
        <label>Captura de Pantalla</label>
        <div class="img-upload" onclick="document.getElementById('screenshotInput').click()">
          <div style="font-size:28px">📸</div>
          <div class="img-upload-text">Click para subir captura del trade</div>
          <img id="imgPreview" class="img-preview">
        </div>
        <input type="file" id="screenshotInput" accept="image/*" onchange="previewImage(event)">
      </div>

      <div class="form-group full">
        <label>Notas / Análisis Post-Trade</label>
        <textarea id="f-notes" placeholder="¿Por qué entraste? ¿Qué salió bien o mal? ¿Qué aprenderías?"></textarea>
      </div>
    </div>

    <div class="modal-actions">
      <button class="btn btn-ghost" onclick="closeTradeModal()">Cancelar</button>
      <button class="btn btn-primary" onclick="saveTrade()">💾 Guardar Trade</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="detailModal">
  <div class="modal">
    <div class="modal-title"><div class="icon">🔍</div><span>Detalle del Trade</span></div>
    <div id="detailContent"></div>
    <div class="modal-actions">
      <button class="btn btn-danger" id="detailDeleteBtn">🗑 Eliminar</button>
      <button class="btn btn-ghost" onclick="closeDetailModal()">Cerrar</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
// ============================================================
//  DATA
// ============================================================
let trades = JSON.parse(localStorage.getItem('fe_trades') || '[]');
let currentFilter = 'all';
let editingIndex = -1;
let selectedEmotion = '';
let currentRating = 0;
let calendarDate = new Date();
let screenshotData = '';

const MARKET_COLORS = { crypto:'#d500f9', indices:'#448aff', forex:'#ffea00', commodities:'#ff9100' };
const MARKET_LABELS = { crypto:'Crypto', indices:'Índices', forex:'Forex', commodities:'Commodities' };
const MARKET_DOTS = { crypto:'#d500f9', indices:'#448aff', forex:'#ffea00', commodities:'#ff9100' };

function save() {
  localStorage.setItem('fe_trades', JSON.stringify(trades));
}

// ============================================================
//  NAVIGATION
// ============================================================
function showPage(id, el) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.getElementById('page-' + id).classList.add('active');
  document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
  if (el) el.classList.add('active');
  const titles = { dashboard:'Dashboard <span>General</span>', trades:'Mis <span>Trades</span>', stats:'<span>Estadísticas</span>', calendar:'<span>Calendario</span>' };
  document.getElementById('pageTitle').innerHTML = titles[id];
  if (id === 'stats') renderStats();
  if (id === 'calendar') renderCalendar();
  if (id === 'trades') renderAllTrades();
}

// ============================================================
//  MODAL
// ============================================================
function openTradeModal(idx = -1) {
  editingIndex = idx;
  selectedEmotion = '';
  currentRating = 0;
  screenshotData = '';
  document.getElementById('imgPreview').style.display = 'none';
  document.querySelectorAll('.emotion-btn').forEach(b => b.classList.remove('selected'));
  setRating(0);
  document.getElementById('modalTitleText').textContent = idx >= 0 ? 'Editar Trade' : 'Registrar Trade';

  const now = new Date();
  const toLocalDT = d => { const o = new Date(d - d.getTimezoneOffset()*60000); return o.toISOString().slice(0,16); };

  if (idx >= 0) {
    const t = trades[idx];
    document.getElementById('f-date').value = t.date || '';
    document.getElementById('f-dateClose').value = t.dateClose || '';
    document.getElementById('f-symbol').value = t.symbol || '';
    document.getElementById('f-market').value = t.market || 'crypto';
    document.getElementById('f-side').value = t.side || 'long';
    document.getElementById('f-size').value = t.size || '';
    document.getElementById('f-entry').value = t.entry || '';
    document.getElementById('f-exit').value = t.exit || '';
    document.getElementById('f-sl').value = t.sl || '';
    document.getElementById('f-tp').value = t.tp || '';
    document.getElementById('f-pnl').value = t.pnl || '';
    document.getElementById('f-fees').value = t.fees || '';
    document.getElementById('f-setup').value = t.setup || '';
    document.getElementById('f-result').value = t.result || 'win';
    document.getElementById('f-rr').value = t.rr || '';
    document.getElementById('f-notes').value = t.notes || '';
    selectedEmotion = t.emotion || '';
    setRating(t.rating || 0);
    if (t.emotion) {
      document.querySelectorAll('.emotion-btn').forEach(b => { if (b.getAttribute('onclick').includes("'"+t.emotion+"'")) b.classList.add('selected'); });
    }
    if (t.screenshot) { document.getElementById('imgPreview').src = t.screenshot; document.getElementById('imgPreview').style.display = 'block'; screenshotData = t.screenshot; }
  } else {
    ['f-date','f-dateClose','f-symbol','f-size','f-entry','f-exit','f-sl','f-tp','f-pnl','f-fees','f-setup','f-rr','f-notes'].forEach(id => document.getElementById(id).value = '');
    document.getElementById('f-date').value = toLocalDT(now);
    document.getElementById('f-market').value = 'crypto';
    document.getElementById('f-side').value = 'long';
    document.getElementById('f-result').value = 'win';
  }
  document.getElementById('tradeModal').classList.add('open');
}

function closeTradeModal() { document.getElementById('tradeModal').classList.remove('open'); }

function saveTrade() {
  const sym = document.getElementById('f-symbol').value.trim();
  if (!sym) { showToast('⚠️ El instrumento es obligatorio', 'error'); return; }

  const pnlRaw = parseFloat(document.getElementById('f-pnl').value) || 0;
  const fees = parseFloat(document.getElementById('f-fees').value) || 0;

  const trade = {
    id: editingIndex >= 0 ? trades[editingIndex].id : Date.now(),
    date: document.getElementById('f-date').value,
    dateClose: document.getElementById('f-dateClose').value,
    symbol: sym.toUpperCase(),
    market: document.getElementById('f-market').value,
    side: document.getElementById('f-side').value,
    size: parseFloat(document.getElementById('f-size').value) || 1,
    entry: parseFloat(document.getElementById('f-entry').value) || 0,
    exit: parseFloat(document.getElementById('f-exit').value) || 0,
    sl: parseFloat(document.getElementById('f-sl').value) || 0,
    tp: parseFloat(document.getElementById('f-tp').value) || 0,
    pnl: pnlRaw - fees,
    fees,
    setup: document.getElementById('f-setup').value.trim(),
    result: document.getElementById('f-result').value,
    rr: parseFloat(document.getElementById('f-rr').value) || 0,
    notes: document.getElementById('f-notes').value.trim(),
    emotion: selectedEmotion,
    rating: currentRating,
    screenshot: screenshotData,
    createdAt: Date.now()
  };

  if (editingIndex >= 0) trades[editingIndex] = trade;
  else trades.unshift(trade);

  save();
  closeTradeModal();
  renderAll();
  showToast(editingIndex >= 0 ? '✅ Trade actualizado' : '✅ Trade guardado', 'success');
}

function deleteTrade(idx) {
  if (!confirm('¿Eliminar este trade?')) return;
  trades.splice(idx, 1);
  save();
  renderAll();
  closeDetailModal();
  showToast('🗑 Trade eliminado', 'error');
}

// ============================================================
//  EMOTIONS & RATING
// ============================================================
function selectEmotion(val, el) {
  selectedEmotion = val;
  document.querySelectorAll('.emotion-btn').forEach(b => b.classList.remove('selected'));
  el.classList.add('selected');
}

function setRating(n) {
  currentRating = n;
  document.querySelectorAll('.star').forEach((s, i) => { s.classList.toggle('on', i < n); });
}

// ============================================================
//  IMAGE
// ============================================================
function previewImage(e) {
  const file = e.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = ev => {
    screenshotData = ev.target.result;
    const img = document.getElementById('imgPreview');
    img.src = screenshotData;
    img.style.display = 'block';
  };
  reader.readAsDataURL(file);
}

// ============================================================
//  RENDER ALL
// ============================================================
function renderAll() {
  renderDashboard();
  renderAllTrades();
  document.getElementById('tradesCount').textContent = trades.length;
  const total = trades.reduce((a, t) => a + t.pnl, 0);
  document.getElementById('sidebarBalance').textContent = fmt(total);
  document.getElementById('sidebarBalance').className = 'account-balance ' + (total >= 0 ? 'pos' : '');
  if (total < 0) document.getElementById('sidebarBalance').style.color = 'var(--red)';
}

// ============================================================
//  DASHBOARD
// ============================================================
function renderDashboard() {
  const total = trades.reduce((a, t) => a + t.pnl, 0);
  const wins = trades.filter(t => t.result === 'win');
  const losses = trades.filter(t => t.result === 'loss');
  const wr = trades.length ? (wins.length / trades.length * 100) : 0;
  const grossWin = wins.reduce((a, t) => a + t.pnl, 0);
  const grossLoss = Math.abs(losses.reduce((a, t) => a + t.pnl, 0));
  const pf = grossLoss > 0 ? grossWin / grossLoss : grossWin > 0 ? '∞' : '—';
  const rrTrades = trades.filter(t => t.rr > 0);
  const avgRR = rrTrades.length ? (rrTrades.reduce((a, t) => a + t.rr, 0) / rrTrades.length) : 0;

  document.getElementById('dash-pnl').textContent = fmt(total);
  document.getElementById('dash-pnl').className = 'stat-value ' + (total >= 0 ? 'pos' : 'neg');
  document.getElementById('dash-pnl-sub').textContent = trades.length + ' trades registrados';
  document.getElementById('dash-wr').textContent = wr.toFixed(1) + '%';
  document.getElementById('dash-wr-sub').textContent = wins.length + 'W / ' + losses.length + 'L';
  document.getElementById('dash-pf').textContent = typeof pf === 'number' ? pf.toFixed(2) : pf;
  document.getElementById('dash-rr').textContent = avgRR > 0 ? avgRR.toFixed(2) : '—';

  renderEquityChart();
  renderDonut();
  renderRecentTrades();
}

function renderEquityChart() {
  const svg = document.getElementById('equitySvg');
  const W = 600, H = 160, PAD = 20;
  if (trades.length < 2) {
    document.getElementById('chartLine').setAttribute('d', '');
    document.getElementById('chartArea').setAttribute('d', '');
    return;
  }
  const sorted = [...trades].sort((a, b) => new Date(a.date) - new Date(b.date));
  let cum = 0;
  const pts = [{ x: 0, y: 0 }, ...sorted.map((t, i) => { cum += t.pnl; return { x: i + 1, y: cum }; })];
  const minY = Math.min(...pts.map(p => p.y));
  const maxY = Math.max(...pts.map(p => p.y));
  const rng = maxY - minY || 1;
  const toX = x => PAD + x / pts.length * (W - PAD * 2);
  const toY = y => H - PAD - ((y - minY) / rng) * (H - PAD * 2);
  const line = pts.map((p, i) => (i === 0 ? 'M' : 'L') + toX(p.x).toFixed(1) + ',' + toY(p.y).toFixed(1)).join(' ');
  const area = line + ` L${toX(pts.length - 1).toFixed(1)},${H - PAD} L${toX(0).toFixed(1)},${H - PAD} Z`;
  document.getElementById('chartLine').setAttribute('d', line);
  document.getElementById('chartArea').setAttribute('d', area);
}

function renderDonut() {
  const counts = {};
  trades.forEach(t => { counts[t.market] = (counts[t.market] || 0) + 1; });
  const total = trades.length;
  if (!total) return;
  const svg = document.getElementById('donutSvg');
  const markets = Object.keys(counts);
  let offset = 0;
  let circles = '<circle cx="50" cy="50" r="38" fill="none" stroke="var(--surface3)" stroke-width="14"/>';
  const circ = 2 * Math.PI * 38;
  markets.forEach(m => {
    const pct = counts[m] / total;
    const dash = circ * pct;
    const gap = circ - dash;
    circles += `<circle cx="50" cy="50" r="38" fill="none" stroke="${MARKET_COLORS[m]||'#fff'}" stroke-width="14" stroke-dasharray="${dash.toFixed(2)} ${gap.toFixed(2)}" stroke-dashoffset="${-offset.toFixed(2)}" transform="rotate(-90 50 50)"/>`;
    offset += dash;
  });
  circles += `<text x="50" y="52" text-anchor="middle" font-size="11" fill="var(--text)" font-family="Space Mono" font-weight="700">${total}</text><text x="50" y="63" text-anchor="middle" font-size="8" fill="var(--muted)" font-family="Space Mono">trades</text>`;
  svg.innerHTML = `<defs><linearGradient id="chartGrad" x1="0" y1="0" x2="0" y2="1"><stop offset="0%" stop-color="#00e5ff" stop-opacity="0.25"/><stop offset="100%" stop-color="#00e5ff" stop-opacity="0"/></linearGradient></defs>` + circles;

  const legend = markets.map(m =>
    `<div class="donut-item"><div class="donut-dot" style="background:${MARKET_COLORS[m]}"></div><span style="color:var(--text)">${MARKET_LABELS[m]}</span> <span style="color:var(--muted);margin-left:4px">${counts[m]}</span></div>`
  ).join('');
  document.getElementById('donutLegend').innerHTML = legend || '<div style="color:var(--muted);font-size:12px">Sin datos</div>';
}

function renderRecentTrades() {
  const el = document.getElementById('recentTradesTable');
  const recent = trades.slice(0, 5);
  if (!recent.length) { el.innerHTML = `<div class="empty-state"><div class="empty-icon">📭</div><div class="empty-text">No hay trades todavía</div></div>`; return; }
  el.innerHTML = tradeTable(recent, true);
}

// ============================================================
//  ALL TRADES
// ============================================================
function renderAllTrades() {
  const el = document.getElementById('allTradesTable');
  let filtered = trades;
  if (currentFilter !== 'all') {
    if (['long','short'].includes(currentFilter)) filtered = trades.filter(t => t.side === currentFilter);
    else if (['win','loss','be'].includes(currentFilter)) filtered = trades.filter(t => t.result === currentFilter);
    else filtered = trades.filter(t => t.market === currentFilter);
  }
  if (!filtered.length) { el.innerHTML = `<div class="empty-state"><div class="empty-icon">📋</div><div class="empty-text">Sin resultados para este filtro</div></div>`; return; }
  el.innerHTML = tradeTable(filtered, false);
}

function filterTrades(f, el) {
  currentFilter = f;
  document.querySelectorAll('.filter-chip').forEach(c => c.classList.remove('active'));
  el.classList.add('active');
  renderAllTrades();
}

function tradeTable(list, compact) {
  const header = `<table><thead><tr>
    <th>Fecha</th><th>Símbolo</th><th>Dir</th><th>P&L</th><th>R:R</th><th>Setup</th><th>Estado</th><th>Rating</th><th></th>
  </tr></thead><tbody>`;
  const rows = list.map((t, i) => {
    const idx = trades.indexOf(t);
    const dateStr = t.date ? new Date(t.date).toLocaleDateString('es-ES', { day:'2-digit', month:'short' }) : '—';
    const stars = '★'.repeat(t.rating || 0) + '☆'.repeat(5 - (t.rating || 0));
    const emojiMap = { revenge:'😤', fear:'😰', neutral:'😐', confident:'😊', fomo:'🤩' };
    return `<tr>
      <td><span style="font-family:var(--mono);font-size:12px;color:var(--muted)">${dateStr}</span></td>
      <td><div class="market-tag"><div class="market-dot" style="background:${MARKET_DOTS[t.market]}"></div>${t.symbol}</div></td>
      <td><span class="badge badge-${t.side}">${t.side.toUpperCase()}</span></td>
      <td class="${t.pnl >= 0 ? 'pnl-pos' : 'pnl-neg'}">${fmt(t.pnl)}</td>
      <td style="font-family:var(--mono);font-size:12px">${t.rr > 0 ? t.rr.toFixed(2) + 'R' : '—'}</td>
      <td><span class="tag">${t.setup || '—'}</span></td>
      <td><span class="badge badge-${t.result}">${{win:'✅ Win',loss:'❌ Loss',be:'➖ BE'}[t.result]}</span> ${t.emotion ? emojiMap[t.emotion] : ''}</td>
      <td style="color:var(--yellow);font-size:12px;letter-spacing:-2px">${stars}</td>
      <td><button class="action-btn" onclick="openDetail(${idx})">Ver</button></td>
    </tr>`;
  }).join('');
  return header + rows + '</tbody></table>';
}

// ============================================================
//  DETAIL MODAL
// ============================================================
function openDetail(idx) {
  const t = trades[idx];
  const emojiMap = { revenge:'😤 Revenge', fear:'😰 Miedo', neutral:'😐 Neutral', confident:'😊 Confiado', fomo:'🤩 FOMO' };
  const stars = '★'.repeat(t.rating || 0) + '☆'.repeat(5 - (t.rating || 0));

  let html = `
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:16px">
      <div><div class="stat-label">Instrumento</div><div style="font-size:20px;font-weight:800;margin-top:4px">${t.symbol}</div></div>
      <div><div class="stat-label">P&L Neto</div><div style="font-size:20px;font-weight:800;margin-top:4px;color:${t.pnl >= 0 ? 'var(--green)' : 'var(--red)'}">${fmt(t.pnl)}</div></div>
      <div><div class="stat-label">Dirección</div><div class="badge badge-${t.side}" style="margin-top:6px">${t.side.toUpperCase()}</div></div>
      <div><div class="stat-label">Resultado</div><div class="badge badge-${t.result}" style="margin-top:6px">${{win:'✅ Win',loss:'❌ Loss',be:'➖ Break Even'}[t.result]}</div></div>
      <div><div class="stat-label">Entrada</div><div style="font-family:var(--mono);margin-top:4px">${t.entry || '—'}</div></div>
      <div><div class="stat-label">Salida</div><div style="font-family:var(--mono);margin-top:4px">${t.exit || '—'}</div></div>
      <div><div class="stat-label">Stop Loss</div><div style="font-family:var(--mono);margin-top:4px;color:var(--red)">${t.sl || '—'}</div></div>
      <div><div class="stat-label">Take Profit</div><div style="font-family:var(--mono);margin-top:4px;color:var(--green)">${t.tp || '—'}</div></div>
      <div><div class="stat-label">R:R Real</div><div style="font-family:var(--mono);margin-top:4px">${t.rr > 0 ? t.rr.toFixed(2) + 'R' : '—'}</div></div>
      <div><div class="stat-label">Setup</div><div class="tag" style="margin-top:6px;display:inline-block">${t.setup || '—'}</div></div>
      <div><div class="stat-label">Calidad</div><div style="color:var(--yellow);letter-spacing:-2px;margin-top:4px">${stars}</div></div>
      <div><div class="stat-label">Emoción</div><div style="margin-top:4px">${t.emotion ? emojiMap[t.emotion] : '—'}</div></div>
    </div>`;

  if (t.notes) html += `<div style="margin-bottom:16px"><div class="stat-label" style="margin-bottom:6px">Notas</div><div style="background:var(--surface2);padding:12px;border-radius:8px;font-size:13px;line-height:1.6">${t.notes}</div></div>`;

  if (t.screenshot) html += `<div style="margin-bottom:16px"><div class="stat-label" style="margin-bottom:6px">Captura</div><img src="${t.screenshot}" style="width:100%;border-radius:10px"></div>`;

  document.getElementById('detailContent').innerHTML = html;
  document.getElementById('detailDeleteBtn').onclick = () => deleteTrade(idx);
  document.getElementById('detailModal').classList.add('open');

  // Edit button in footer
  const actions = document.querySelector('#detailModal .modal-actions');
  const existingEdit = document.getElementById('detailEditBtn');
  if (!existingEdit) {
    const editBtn = document.createElement('button');
    editBtn.id = 'detailEditBtn';
    editBtn.className = 'btn btn-ghost';
    editBtn.textContent = '✏️ Editar';
    editBtn.onclick = () => { closeDetailModal(); openTradeModal(idx); };
    actions.insertBefore(editBtn, actions.querySelector('.btn-ghost'));
  } else {
    existingEdit.onclick = () => { closeDetailModal(); openTradeModal(idx); };
  }
}

function closeDetailModal() { document.getElementById('detailModal').classList.remove('open'); }

// ============================================================
//  STATS
// ============================================================
function renderStats() {
  if (!trades.length) return;
  const wins = trades.filter(t => t.result === 'win');
  const losses = trades.filter(t => t.result === 'loss');
  const pnls = trades.map(t => t.pnl);
  const best = Math.max(...pnls);
  const worst = Math.min(...pnls);
  const rrTrades = trades.filter(t => t.rr > 0);
  const avgRR = rrTrades.length ? rrTrades.reduce((a, t) => a + t.rr, 0) / rrTrades.length : 0;
  const avgWin = wins.length ? wins.reduce((a, t) => a + t.pnl, 0) / wins.length : 0;
  const avgLoss = losses.length ? losses.reduce((a, t) => a + t.pnl, 0) / losses.length : 0;
  const grossWin = wins.reduce((a, t) => a + t.pnl, 0);
  const grossLoss = Math.abs(losses.reduce((a, t) => a + t.pnl, 0));
  const pf = grossLoss > 0 ? (grossWin / grossLoss).toFixed(2) : '∞';
  const wr = (wins.length / trades.length * 100).toFixed(1);

  // Streak
  let maxStreak = 0, streak = 0;
  const sorted = [...trades].sort((a, b) => new Date(a.date) - new Date(b.date));
  sorted.forEach(t => { if (t.result === 'win') { streak++; maxStreak = Math.max(maxStreak, streak); } else streak = 0; });

  document.getElementById('st-best').textContent = fmt(best);
  document.getElementById('st-worst').textContent = fmt(worst);
  document.getElementById('st-streak').textContent = maxStreak;

  document.getElementById('keyMetrics').innerHTML = `
    <div class="metric-row"><span class="metric-name">Total Trades</span><span class="metric-val neu">${trades.length}</span></div>
    <div class="metric-row"><span class="metric-name">Win Rate</span><span class="metric-val pos">${wr}%</span></div>
    <div class="metric-row"><span class="metric-name">Profit Factor</span><span class="metric-val neu">${pf}</span></div>
    <div class="metric-row"><span class="metric-name">R:R Promedio</span><span class="metric-val neu">${avgRR > 0 ? avgRR.toFixed(2) : '—'}</span></div>
    <div class="metric-row"><span class="metric-name">Trade Promedio Win</span><span class="metric-val pos">${fmt(avgWin)}</span></div>
    <div class="metric-row"><span class="metric-name">Trade Promedio Loss</span><span class="metric-val neg">${fmt(avgLoss)}</span></div>
    <div class="metric-row"><span class="metric-name">Comisiones Totales</span><span class="metric-val neg">${fmt(-trades.reduce((a, t) => a + (t.fees || 0), 0))}</span></div>
  `;

  const mkts = {};
  trades.forEach(t => { if (!mkts[t.market]) mkts[t.market] = { pnl: 0, count: 0 }; mkts[t.market].pnl += t.pnl; mkts[t.market].count++; });
  const maxPnl = Math.max(...Object.values(mkts).map(v => Math.abs(v.pnl)));
  document.getElementById('marketBreakdown').innerHTML = Object.entries(mkts).map(([m, v]) => `
    <div style="margin-bottom:14px">
      <div style="display:flex;justify-content:space-between;margin-bottom:4px">
        <span style="font-size:13px"><span style="color:${MARKET_COLORS[m]};margin-right:4px">●</span>${MARKET_LABELS[m]}</span>
        <span class="${v.pnl >= 0 ? 'pnl-pos' : 'pnl-neg'}" style="font-size:13px">${fmt(v.pnl)}</span>
      </div>
      <div class="progress-bar"><div class="progress-fill" style="width:${(Math.abs(v.pnl)/maxPnl*100).toFixed(0)}%;background:${MARKET_COLORS[m]}"></div></div>
      <div style="font-size:11px;color:var(--muted);margin-top:2px">${v.count} trades</div>
    </div>`).join('');

  const emCount = { revenge: 0, fear: 0, neutral: 0, confident: 0, fomo: 0 };
  trades.forEach(t => { if (t.emotion && emCount.hasOwnProperty(t.emotion)) emCount[t.emotion]++; });
  Object.entries(emCount).forEach(([k, v]) => { const el = document.getElementById('em-' + k); if (el) el.textContent = v; });
}

// ============================================================
//  CALENDAR
// ============================================================
function renderCalendar() {
  const y = calendarDate.getFullYear(), m = calendarDate.getMonth();
  document.getElementById('calMonthLabel').textContent = new Date(y, m, 1).toLocaleDateString('es-ES', { month:'long', year:'numeric' });

  const firstDay = new Date(y, m, 1).getDay();
  const daysInMonth = new Date(y, m + 1, 0).getDate();
  const offset = firstDay === 0 ? 6 : firstDay - 1; // Mon-based

  // Group trades by date
  const byDate = {};
  trades.forEach(t => {
    if (!t.date) return;
    const d = t.date.slice(0, 10);
    if (!byDate[d]) byDate[d] = { pnl: 0, wins: 0, losses: 0 };
    byDate[d].pnl += t.pnl;
    if (t.result === 'win') byDate[d].wins++;
    else if (t.result === 'loss') byDate[d].losses++;
  });

  const today = new Date();
  let cells = '';
  for (let i = 0; i < offset; i++) cells += '<div class="cal-cell empty"></div>';
  for (let d = 1; d <= daysInMonth; d++) {
    const dateStr = `${y}-${String(m+1).padStart(2,'0')}-${String(d).padStart(2,'0')}`;
    const data = byDate[dateStr];
    const isToday = today.getFullYear() === y && today.getMonth() === m && today.getDate() === d;
    let cls = 'cal-cell';
    let pnlHtml = '';
    if (data) {
      if (data.pnl > 0) cls += ' win';
      else if (data.pnl < 0) cls += ' loss';
      else cls += ' be';
      pnlHtml = `<div class="cal-pnl">${data.pnl >= 0 ? '+' : ''}${(data.pnl/1000).toFixed(1)}k</div>`;
    }
    if (isToday) cls += ' today';
    cells += `<div class="${cls}" title="${dateStr}"><div>${d}</div>${pnlHtml}</div>`;
  }
  document.getElementById('calGrid').innerHTML = cells;

  // Month stats
  const monthKey = `${y}-${String(m+1).padStart(2,'0')}`;
  const monthTrades = trades.filter(t => t.date && t.date.startsWith(monthKey));
  const mPnl = monthTrades.reduce((a, t) => a + t.pnl, 0);
  const mWr = monthTrades.length ? (monthTrades.filter(t => t.result === 'win').length / monthTrades.length * 100) : 0;
  document.getElementById('calMonthStats').innerHTML = `
    <div class="stat-card cyan"><div class="stat-label">P&L del mes</div><div class="stat-value ${mPnl >= 0 ? 'pos' : 'neg'}" style="font-size:20px">${fmt(mPnl)}</div></div>
    <div class="stat-card green"><div class="stat-label">Trades</div><div class="stat-value neu" style="font-size:20px">${monthTrades.length}</div></div>
    <div class="stat-card orange"><div class="stat-label">Win Rate mes</div><div class="stat-value pos" style="font-size:20px">${mWr.toFixed(0)}%</div></div>
    <div class="stat-card purple"><div class="stat-label">Días con trades</div><div class="stat-value neu" style="font-size:20px">${Object.keys(byDate).filter(d => d.startsWith(monthKey)).length}</div></div>`;
}

function changeMonth(dir) {
  calendarDate = new Date(calendarDate.getFullYear(), calendarDate.getMonth() + dir, 1);
  renderCalendar();
}

// ============================================================
//  IMPORT MODAL (simple CSV hint)
// ============================================================
function openImportModal() {
  alert('💡 Función de importación CSV próximamente.\nPor ahora añade trades manualmente.');
}

// ============================================================
//  UTILS
// ============================================================
function fmt(n) {
  const abs = Math.abs(n);
  const s = abs >= 1000 ? abs.toFixed(0).replace(/\B(?=(\d{3})+(?!\d))/g, ',') : abs.toFixed(2);
  return (n < 0 ? '-' : '+') + '$' + s;
}

function showToast(msg, type = 'success') {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.className = 'toast ' + type + ' show';
  setTimeout(() => t.classList.remove('show'), 3000);
}

// Close modals on overlay click
document.getElementById('tradeModal').addEventListener('click', e => { if (e.target === e.currentTarget) closeTradeModal(); });
document.getElementById('detailModal').addEventListener('click', e => { if (e.target === e.currentTarget) closeDetailModal(); });

// ============================================================
//  INIT WITH SAMPLE DATA
// ============================================================
if (!trades.length) {
  const sampleTrades = [
    { id:1, date:'2026-04-01T09:30', dateClose:'2026-04-01T10:15', symbol:'BTC-PERP', market:'crypto', side:'long', size:0.5, entry:68500, exit:70200, sl:67800, tp:71000, pnl:850, fees:12, setup:'Breakout', result:'win', rr:2.4, notes:'Setup limpio en apertura de NY', emotion:'confident', rating:4, screenshot:'', createdAt:Date.now() },
    { id:2, date:'2026-04-02T14:00', dateClose:'2026-04-02T14:45', symbol:'ES1!', market:'indices', side:'short', size:2, entry:5420, exit:5408, sl:5430, tp:5400, pnl:480, fees:8, setup:'Mean Reversion', result:'win', rr:2.0, notes:'Rechazo en resistencia clave', emotion:'neutral', rating:3, screenshot:'', createdAt:Date.now() },
    { id:3, date:'2026-04-03T08:00', dateClose:'2026-04-03T08:30', symbol:'XAUUSD', market:'commodities', side:'long', size:1, entry:2320, exit:2308, sl:2315, tp:2340, pnl:-240, fees:6, setup:'ICT Order Block', result:'loss', rr:0, notes:'Stop justo. Mercado giró sin confirmación', emotion:'fear', rating:2, screenshot:'', createdAt:Date.now() },
    { id:4, date:'2026-04-04T10:00', dateClose:'2026-04-04T11:00', symbol:'NQ1!', market:'indices', side:'long', size:1, entry:19850, exit:19920, sl:19800, tp:19950, pnl:700, fees:10, setup:'Opening Drive', result:'win', rr:1.4, notes:'Good entry on VWAP reclaim', emotion:'confident', rating:4, screenshot:'', createdAt:Date.now() },
    { id:5, date:'2026-04-07T09:00', dateClose:'2026-04-07T09:20', symbol:'EURUSD', market:'forex', side:'short', size:3, entry:1.0850, exit:1.0862, sl:1.0840, tp:1.0820, pnl:-180, fees:5, setup:'Liquidity Sweep', result:'loss', rr:0, notes:'FOMO entry. Esperé tarde el setup', emotion:'fomo', rating:1, screenshot:'', createdAt:Date.now() },
    { id:6, date:'2026-04-08T15:30', dateClose:'2026-04-08T16:00', symbol:'ETH-PERP', market:'crypto', side:'short', size:2, entry:3450, exit:3390, sl:3490, tp:3380, pnl:1200, fees:18, setup:'Distribution', result:'win', rr:1.75, notes:'Seguí el plan al 100%', emotion:'neutral', rating:5, screenshot:'', createdAt:Date.now() },
  ];
  trades = sampleTrades;
  save();
}

// INIT
renderAll();
</script>
</body>
</html>
