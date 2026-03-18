<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Museu Ecrom Busca</title>
<link href="https://fonts.googleapis.com/css2?family=Cinzel+Decorative:wght@400;700;900&family=Cinzel:wght@400;600;700&family=Share+Tech+Mono&display=swap" rel="stylesheet">
<style>
  :root {
    --gold: #c9a84c;
    --gold-bright: #f0cc6e;
    --gold-dim: #7a6030;
    --ember: #e84c1e;
    --ember-dim: #7a2810;
    --cyan: #00d4ff;
    --cyan-dim: #005566;
    --bg-void: #050508;
    --bg-dark: #0a0a10;
    --bg-panel: #0d0d18;
    --bg-card: #111120;
    --stone: #1a1a2e;
    --stone-light: #252540;
    --text-primary: #e8e0c8;
    --text-secondary: #9990aa;
    --text-rune: #c9a84c88;
    --stable: #4caf7d;
    --unstable: #f0a030;
    --broken: #e84c4c;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg-void);
    color: var(--text-primary);
    font-family: 'Cinzel', serif;
    min-height: 100vh;
    overflow-x: hidden;
    cursor: default;
  }
    
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(201,168,76,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(201,168,76,0.03) 1px, transparent 1px);
    background-size: 60px 60px;
    pointer-events: none;
    z-index: 0;
  }

  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background: radial-gradient(ellipse at 50% 0%, rgba(201,168,76,0.08) 0%, transparent 60%),
                radial-gradient(ellipse at 20% 80%, rgba(0,212,255,0.04) 0%, transparent 50%),
                radial-gradient(ellipse at 80% 70%, rgba(232,76,30,0.04) 0%, transparent 50%);
    pointer-events: none;
    z-index: 0;
  }


  .rune-left, .rune-right {
    position: fixed;
    top: 0; bottom: 0;
    width: 36px;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 80px 0;
    gap: 14px;
    z-index: 1;
    overflow: hidden;
  }
  .rune-left { left: 0; border-right: 1px solid rgba(201,168,76,0.12); }
  .rune-right { right: 0; border-left: 1px solid rgba(201,168,76,0.12); }
  .rune-glyph {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    color: var(--text-rune);
    writing-mode: vertical-rl;
    letter-spacing: 6px;
    animation: runeFlicker 4s infinite;
  }
  .rune-glyph:nth-child(2n) { animation-delay: 1.3s; color: rgba(0,212,255,0.2); }
  .rune-glyph:nth-child(3n) { animation-delay: 2.7s; }

  @keyframes runeFlicker {
    0%, 90%, 100% { opacity: 1; }
    92% { opacity: 0.2; }
    94% { opacity: 0.8; }
    96% { opacity: 0.1; }
  }


  #header {
    position: relative;
    z-index: 10;
    padding: 48px 80px 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0;
  }

  .header-ornament {
    display: flex;
    align-items: center;
    gap: 16px;
    margin-bottom: 12px;
  }
  .ornament-line {
    height: 1px;
    width: 120px;
    background: linear-gradient(90deg, transparent, var(--gold), transparent);
  }
  .ornament-diamond {
    width: 8px; height: 8px;
    background: var(--gold);
    transform: rotate(45deg);
    box-shadow: 0 0 12px var(--gold);
  }

  .site-badge {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    letter-spacing: 6px;
    color: var(--gold-dim);
    text-transform: uppercase;
    margin-bottom: 10px;
  }

  .site-title {
    font-family: 'Cinzel Decorative', serif;
    font-size: clamp(32px, 5vw, 64px);
    font-weight: 900;
    letter-spacing: 8px;
    color: transparent;
    background: linear-gradient(180deg, var(--gold-bright) 0%, var(--gold) 50%, var(--gold-dim) 100%);
    -webkit-background-clip: text;
    background-clip: text;
    text-shadow: none;
    filter: drop-shadow(0 0 30px rgba(201,168,76,0.5));
    position: relative;
  }

  .site-sub {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    letter-spacing: 10px;
    color: var(--cyan);
    opacity: 0.7;
    margin-top: 6px;
  }


  .corner-bracket {
    position: absolute;
    width: 20px; height: 20px;
  }
  .cb-tl { top: 0; left: 0; border-top: 2px solid var(--gold); border-left: 2px solid var(--gold); }
  .cb-tr { top: 0; right: 0; border-top: 2px solid var(--gold); border-right: 2px solid var(--gold); }
  .cb-bl { bottom: 0; left: 0; border-bottom: 2px solid var(--gold); border-left: 2px solid var(--gold); }
  .cb-br { bottom: 0; right: 0; border-bottom: 2px solid var(--gold); border-right: 2px solid var(--gold); }

  .title-wrap {
    position: relative;
    padding: 16px 40px;
  }

  .divider {
    position: relative;
    z-index: 10;
    margin: 24px 80px;
    height: 1px;
    background: linear-gradient(90deg, transparent 0%, var(--gold-dim) 20%, var(--gold) 50%, var(--gold-dim) 80%, transparent 100%);
  }
  .divider::after {
    content: '◆ NEXUS ARCHIVE ◆';
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: var(--bg-void);
    padding: 0 20px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    letter-spacing: 5px;
    color: var(--gold);
    white-space: nowrap;
  }

  #controls {
    position: relative;
    z-index: 10;
    padding: 16px 80px;
    display: flex;
    align-items: center;
    gap: 12px;
    flex-wrap: wrap;
  }

  .ctrl-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    letter-spacing: 3px;
    color: var(--gold-dim);
    text-transform: uppercase;
    margin-right: 4px;
  }

  .filter-btn {
    font-family: 'Cinzel', serif;
    font-size: 10px;
    letter-spacing: 2px;
    padding: 6px 16px;
    background: transparent;
    border: 1px solid var(--stone-light);
    color: var(--text-secondary);
    cursor: pointer;
    transition: all 0.2s;
    position: relative;
    text-transform: uppercase;
  }
  .filter-btn::before, .filter-btn::after {
    content: '';
    position: absolute;
    width: 5px; height: 5px;
  }
  .filter-btn::before { top: -1px; left: -1px; border-top: 1px solid; border-left: 1px solid; }
  .filter-btn::after { bottom: -1px; right: -1px; border-bottom: 1px solid; border-right: 1px solid; }
  .filter-btn:hover, .filter-btn.active {
    background: rgba(201,168,76,0.08);
    border-color: var(--gold);
    color: var(--gold);
  }
  .filter-btn::before, .filter-btn::after { border-color: inherit; }

  .filter-btn.f-stable:hover, .filter-btn.f-stable.active { border-color: var(--stable); color: var(--stable); background: rgba(76,175,125,0.08); }
  .filter-btn.f-unstable:hover, .filter-btn.f-unstable.active { border-color: var(--unstable); color: var(--unstable); background: rgba(240,160,48,0.08); }
  .filter-btn.f-broken:hover, .filter-btn.f-broken.active { border-color: var(--broken); color: var(--broken); background: rgba(232,76,76,0.08); }

  .spacer { flex: 1; }

  .sort-wrap {
    display: flex;
    align-items: center;
    gap: 8px;
  }

  #sort-select {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    background: var(--bg-panel);
    border: 1px solid var(--stone-light);
    color: var(--gold);
    padding: 6px 10px;
    letter-spacing: 1px;
    cursor: pointer;
    outline: none;
  }
  #sort-select option { background: var(--bg-dark); }

  #status-bar {
    position: relative;
    z-index: 10;
    padding: 0 80px 8px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    color: var(--text-secondary);
    letter-spacing: 2px;
    display: flex;
    align-items: center;
    gap: 16px;
  }
  .status-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--cyan);
    box-shadow: 0 0 6px var(--cyan);
    animation: pulse 2s infinite;
  }
  @keyframes pulse {
    0%, 100% { opacity: 1; box-shadow: 0 0 6px var(--cyan); }
    50% { opacity: 0.5; box-shadow: 0 0 2px var(--cyan); }
  }

  #releases-grid {
    position: relative;
    z-index: 10;
    padding: 16px 72px 80px;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(360px, 1fr));
    gap: 20px;
  }

  .release-card {
    background: var(--bg-card);
    border: 1px solid var(--stone-light);
    position: relative;
    padding: 0;
    overflow: hidden;
    transition: transform 0.2s, box-shadow 0.2s;
    animation: cardReveal 0.4s ease both;
  }
  .release-card:hover {
    transform: translateY(-3px);
    box-shadow: 0 8px 40px rgba(201,168,76,0.15), 0 0 1px var(--gold);
    border-color: var(--gold-dim);
  }

  @keyframes cardReveal {
    from { opacity: 0; transform: translateY(12px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .card-accent {
    height: 2px;
    width: 100%;
  }
  .status-estavel .card-accent { background: linear-gradient(90deg, transparent, var(--stable), transparent); }
  .status-problematica .card-accent { background: linear-gradient(90deg, transparent, var(--broken), transparent); }
  .status-nao-avaliavel .card-accent { background: linear-gradient(90deg, transparent, var(--unstable), transparent); }

  .card-corner {
    position: absolute;
    width: 14px; height: 14px;
  }
  .card-corner.tl { top: 2px; left: 0; border-top: 1px solid; border-left: 1px solid; }
  .card-corner.tr { top: 2px; right: 0; border-top: 1px solid; border-right: 1px solid; }
  .card-corner.bl { bottom: 0; left: 0; border-bottom: 1px solid; border-left: 1px solid; }
  .card-corner.br { bottom: 0; right: 0; border-bottom: 1px solid; border-right: 1px solid; }
  .status-estavel .card-corner { border-color: rgba(76,175,125,0.4); }
  .status-problematica .card-corner { border-color: rgba(232,76,76,0.4); }
  .status-nao-avaliavel .card-corner { border-color: rgba(240,160,48,0.4); }

  .card-body {
    padding: 20px 22px 18px;
  }

  .card-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 12px;
  }

  .card-title {
    font-family: 'Cinzel Decorative', serif;
    font-size: 13px;
    font-weight: 700;
    color: var(--gold-bright);
    letter-spacing: 1px;
    line-height: 1.3;
  }

  .card-tag {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    letter-spacing: 2px;
    color: var(--text-secondary);
    padding: 3px 8px;
    border: 1px solid var(--stone-light);
    white-space: nowrap;
    margin-left: 8px;
    flex-shrink: 0;
  }

  .status-badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    letter-spacing: 2px;
    padding: 4px 10px;
    margin-bottom: 14px;
    border: 1px solid;
    text-transform: uppercase;
    position: relative;
  }
  .badge-dot {
    width: 5px; height: 5px;
    border-radius: 50%;
    flex-shrink: 0;
  }
  .status-estavel .status-badge { color: var(--stable); border-color: rgba(76,175,125,0.4); background: rgba(76,175,125,0.08); }
  .status-estavel .badge-dot { background: var(--stable); box-shadow: 0 0 5px var(--stable); }
  .status-problematica .status-badge { color: var(--broken); border-color: rgba(232,76,76,0.4); background: rgba(232,76,76,0.08); }
  .status-problematica .badge-dot { background: var(--broken); box-shadow: 0 0 5px var(--broken); animation: pulse 1.5s infinite; }
  .status-nao-avaliavel .status-badge { color: var(--unstable); border-color: rgba(240,160,48,0.4); background: rgba(240,160,48,0.08); }
  .status-nao-avaliavel .badge-dot { background: var(--unstable); box-shadow: 0 0 5px var(--unstable); }

  .card-divider {
    height: 1px;
    background: linear-gradient(90deg, var(--stone-light), transparent);
    margin-bottom: 12px;
  }

  .card-desc {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10.5px;
    line-height: 1.7;
    color: var(--text-secondary);
    letter-spacing: 0.3px;
    max-height: 80px;
    overflow: hidden;
    position: relative;
    margin-bottom: 16px;
  }
  .card-desc.expanded { max-height: none; }
  .card-desc::after {
    content: '';
    position: absolute;
    bottom: 0; left: 0; right: 0;
    height: 30px;
    background: linear-gradient(transparent, var(--bg-card));
    pointer-events: none;
  }
  .card-desc.expanded::after { display: none; }

  .expand-btn {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    letter-spacing: 2px;
    color: var(--gold-dim);
    background: none;
    border: none;
    cursor: pointer;
    padding: 0;
    margin-bottom: 16px;
    display: block;
    transition: color 0.2s;
  }
  .expand-btn:hover { color: var(--gold); }

  .card-footer {
    display: flex;
    gap: 10px;
    align-items: center;
    padding-top: 12px;
    border-top: 1px solid var(--stone-light);
  }

  .card-date {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    color: var(--text-secondary);
    letter-spacing: 1px;
    flex: 1;
    opacity: 0.7;
  }

  .btn-download {
    font-family: 'Cinzel', serif;
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 3px;
    padding: 8px 20px;
    background: transparent;
    border: 1px solid;
    cursor: pointer;
    text-decoration: none;
    text-transform: uppercase;
    position: relative;
    transition: all 0.25s;
    display: inline-flex;
    align-items: center;
    gap: 6px;
    overflow: hidden;
  }
  .btn-download::before {
    content: '';
    position: absolute;
    inset: 0;
    opacity: 0;
    transition: opacity 0.25s;
  }
  .btn-download:hover::before { opacity: 1; }
  .btn-download:hover { transform: scale(1.02); }

  .status-estavel .btn-download {
    color: var(--stable);
    border-color: var(--stable);
  }
  .status-estavel .btn-download::before { background: rgba(76,175,125,0.12); }
  .status-estavel .btn-download:hover { box-shadow: 0 0 20px rgba(76,175,125,0.3); }

  .status-nao-avaliavel .btn-download {
    color: var(--unstable);
    border-color: var(--unstable);
  }
  .status-nao-avaliavel .btn-download::before { background: rgba(240,160,48,0.12); }
  .status-nao-avaliavel .btn-download:hover { box-shadow: 0 0 20px rgba(240,160,48,0.3); }

  .status-problematica .btn-download {
    color: var(--broken);
    border-color: var(--broken);
  }
  .status-problematica .btn-download::before { background: rgba(232,76,76,0.12); }
  .status-problematica .btn-download:hover { box-shadow: 0 0 20px rgba(232,76,76,0.3); }

  .btn-info {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    letter-spacing: 2px;
    padding: 8px 12px;
    background: transparent;
    border: 1px solid var(--stone-light);
    color: var(--text-secondary);
    cursor: pointer;
    text-decoration: none;
    transition: all 0.2s;
  }
  .btn-info:hover { border-color: var(--gold-dim); color: var(--gold); }

  /* === LATEST TAG === */
  .latest-tag {
    font-family: 'Share Tech Mono', monospace;
    font-size: 8px;
    letter-spacing: 2px;
    color: var(--cyan);
    border: 1px solid rgba(0,212,255,0.3);
    padding: 2px 8px;
    margin-left: 8px;
    vertical-align: middle;
    animation: pulse 2s infinite;
  }

  #loading-screen {
    position: fixed;
    inset: 0;
    background: var(--bg-void);
    z-index: 1000;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 24px;
    transition: opacity 0.5s, visibility 0.5s;
  }
  #loading-screen.hidden { opacity: 0; visibility: hidden; }

  .loading-title {
    font-family: 'Cinzel Decorative', serif;
    font-size: 28px;
    color: var(--gold);
    letter-spacing: 8px;
    filter: drop-shadow(0 0 20px var(--gold));
  }

  .loading-bar-wrap {
    width: 280px;
    height: 2px;
    background: var(--stone-light);
    position: relative;
    overflow: hidden;
  }
  .loading-bar {
    height: 100%;
    background: linear-gradient(90deg, var(--gold-dim), var(--gold-bright), var(--gold-dim));
    background-size: 200%;
    animation: loadSweep 1.2s ease-in-out forwards, shimmer 0.8s linear infinite;
    width: 0%;
    transition: width 0.1s;
  }
  @keyframes loadSweep { to { width: 100%; } }
  @keyframes shimmer { 0% { background-position: 0%; } 100% { background-position: 200%; } }

  .loading-text {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    letter-spacing: 4px;
    color: var(--text-secondary);
  }

  /* === HERO STATS === */
  #hero-stats {
    position: relative;
    z-index: 10;
    padding: 0 80px 0;
    display: flex;
    gap: 2px;
    margin-bottom: 8px;
  }
  .stat-block {
    flex: 1;
    padding: 14px 20px;
    background: var(--bg-panel);
    border: 1px solid var(--stone-light);
    display: flex;
    flex-direction: column;
    gap: 4px;
    position: relative;
    overflow: hidden;
  }
  .stat-block::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 1px;
  }
  .stat-block.s-stable::before { background: linear-gradient(90deg, transparent, var(--stable), transparent); }
  .stat-block.s-unstable::before { background: linear-gradient(90deg, transparent, var(--unstable), transparent); }
  .stat-block.s-broken::before { background: linear-gradient(90deg, transparent, var(--broken), transparent); }
  .stat-block.s-total::before { background: linear-gradient(90deg, transparent, var(--gold), transparent); }

  .stat-val {
    font-family: 'Cinzel Decorative', serif;
    font-size: 26px;
    font-weight: 900;
    line-height: 1;
  }
  .stat-block.s-stable .stat-val { color: var(--stable); }
  .stat-block.s-unstable .stat-val { color: var(--unstable); }
  .stat-block.s-broken .stat-val { color: var(--broken); }
  .stat-block.s-total .stat-val { color: var(--gold); }

  .stat-lbl {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    letter-spacing: 2px;
    color: var(--text-secondary);
    text-transform: uppercase;
  }

  #empty-state {
    grid-column: 1/-1;
    text-align: center;
    padding: 80px;
    display: none;
  }
  #empty-state.visible { display: block; }
  .empty-rune {
    font-family: 'Cinzel Decorative', serif;
    font-size: 40px;
    color: var(--stone-light);
    margin-bottom: 16px;
  }
  .empty-text {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    letter-spacing: 3px;
    color: var(--text-secondary);
  }

  ::-webkit-scrollbar { width: 6px; }
  ::-webkit-scrollbar-track { background: var(--bg-void); }
  ::-webkit-scrollbar-thumb { background: var(--gold-dim); border-radius: 0; }
  ::-webkit-scrollbar-thumb:hover { background: var(--gold); }

  #error-msg {
    position: relative;
    z-index: 10;
    margin: 20px 80px;
    padding: 20px 24px;
    background: rgba(232,76,30,0.08);
    border: 1px solid rgba(232,76,30,0.3);
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--ember);
    letter-spacing: 2px;
    display: none;
    align-items: center;
    gap: 12px;
  }
  #error-msg.visible { display: flex; }

  @media (max-width: 700px) {
    #header, #controls, #status-bar, #hero-stats, #releases-grid, #error-msg { padding-left: 24px; padding-right: 24px; }
    .divider { margin-left: 24px; margin-right: 24px; }
    #releases-grid { grid-template-columns: 1fr; }
    #hero-stats { flex-wrap: wrap; }
    .rune-left, .rune-right { display: none; }
    .site-title { font-size: 28px; }
  }
</style>
</head>
<body>

<div id="loading-screen">
  <div class="loading-title">MUSEU-ECROM</div>
  <div style="font-family:'Share Tech Mono',monospace;font-size:10px;letter-spacing:5px;color:var(--gold-dim)">Busca Museu</div>
  <div class="loading-bar-wrap">
    <div class="loading-bar" id="loading-bar"></div>
  </div>
  <div class="loading-text" id="loading-text">INICIALIZANDO...</div>
</div>

<!-- RUNE SIDE BARS -->
<div class="rune-left">
  <span class="rune-glyph">V8.6AW • V8.5AW • V8.4AW</span>
  <span class="rune-glyph">MUSEU ECROM</span>
  <span class="rune-glyph">0xEC30M • ARCH</span>
  <span class="rune-glyph">GAMESISTEM • RELEASE</span>
</div>
<div class="rune-right">
  <span class="rune-glyph">ESTÁVEL • PROBLEMÁTICA</span>
  <span class="rune-glyph">ECROM ARCHIVE v0.0</span>
  <span class="rune-glyph">NÃO AVALIÁVEL</span>
  <span class="rune-glyph">BUILD • TAG • RELEASE</span>
</div>

<!-- HEADER -->
<div id="header">
  <div class="site-badge">◈ SISTEMA DE BUSCA DG ◈</div>
  <div class="header-ornament">
    <div class="ornament-line"></div>
    <div class="ornament-diamond"></div>
    <div class="ornament-line" style="transform:scaleX(-1)"></div>
  </div>
  <div class="title-wrap">
    <div class="cb-tl corner-bracket"></div>
    <div class="cb-tr corner-bracket"></div>
    <div class="cb-bl corner-bracket"></div>
    <div class="cb-br corner-bracket"></div>
    <div class="site-title">MUSEU ECROM</div>
  </div>
  <div class="site-sub">ARQUIVO · VERSÕES · DOWNLOADS</div>
</div>

<!-- STATS -->
<div id="hero-stats">
  <div class="stat-block s-total">
    <div class="stat-val" id="stat-total">—</div>
    <div class="stat-lbl">Total de Versões</div>
  </div>
  <div class="stat-block s-stable">
    <div class="stat-val" id="stat-stable">—</div>
    <div class="stat-lbl">Estáveis</div>
  </div>
  <div class="stat-block s-unstable">
    <div class="stat-val" id="stat-unstable">—</div>
    <div class="stat-lbl">Não Avaliáveis</div>
  </div>
  <div class="stat-block s-broken">
    <div class="stat-val" id="stat-broken">—</div>
    <div class="stat-lbl">Problemáticas</div>
  </div>
</div>

<div class="divider"></div>

<!-- CONTROLS -->
<div id="controls">
  <span class="ctrl-label">FILTRAR:</span>
  <button class="filter-btn active" data-filter="all">Todos</button>
  <button class="filter-btn f-stable" data-filter="estavel">⬥ Estável</button>
  <button class="filter-btn f-unstable" data-filter="nao-avaliavel">⬥ Não Avaliável</button>
  <button class="filter-btn f-broken" data-filter="problematica">⬥ Problemática</button>
  <div class="spacer"></div>
  <div class="sort-wrap">
    <span class="ctrl-label">ORDEM:</span>
    <select id="sort-select">
      <option value="newest">MAIS RECENTE</option>
      <option value="oldest">MAIS ANTIGO</option>
      <option value="name">NOME</option>
    </select>
  </div>
</div>

<!-- STATUS BAR -->
<div id="status-bar">
  <div class="status-dot"></div>
  <span id="status-text">CARREGANDO DADOS DO ARQUIVO...</span>
</div>

<!-- ERROR -->
<div id="error-msg">
  ⚠ FALHA AO CONECTAR AO RPOSITÓRIO GITHUB. VERIFIQUE SUA CONEXÃO.
</div>

<!-- GRID -->
<div id="releases-grid">
  <div id="empty-state">
    <div class="empty-rune">◈</div>
    <div class="empty-text">NENHUMA VERSÃO ENCONTRADA PARA ESTE FILTRO</div>
  </div>
</div>

<script>
const REPO = 'gamesistem/Museu-Ecrom';
const API = `https://api.github.com/repos/${REPO}/releases?per_page=100`;
const RELEASE_BASE = `https://github.com/${REPO}/releases/tag/`;

let allReleases = [];
let currentFilter = 'all';
let currentSort = 'newest';

function detectStatus(body) {
  if (!body) return 'nao-avaliavel';
  const b = body.toLowerCase();
  if (b.includes('problemática') || b.includes('problematica')) return 'problematica';
  if (b.includes('estável') || b.includes('estavel')) return 'estavel';
  if (b.includes('não avaliável') || b.includes('nao avaliavel') || b.includes('nao-avaliavel')) return 'nao-avaliavel';
  return 'nao-avaliavel';
}

function statusLabel(s) {
  if (s === 'estavel') return 'ESTÁVEL';
  if (s === 'problematica') return 'PROBLEMÁTICA';
  return 'NÃO AVALIÁVEL';
}

function formatDate(iso) {
  const d = new Date(iso);
  return d.toLocaleDateString('pt-BR', { day: '2-digit', month: '2-digit', year: 'numeric' });
}

function setLoadingText(t) {
  document.getElementById('loading-text').textContent = t;
}

async function fetchReleases() {
  try {
    setLoadingText('ACESSANDO ARQUIVO NEXUS...');
    const pages = [];
    let page = 1;
    while (true) {
      const res = await fetch(`https://api.github.com/repos/${REPO}/releases?per_page=100&page=${page}`);
      if (!res.ok) throw new Error('API error');
      const data = await res.json();
      if (data.length === 0) break;
      pages.push(...data);
      if (data.length < 100) break;
      page++;
    }
    setLoadingText('PROCESSANDO REGISTROS...');
    allReleases = pages.map(r => ({
      id: r.id,
      name: r.name || r.tag_name,
      tag: r.tag_name,
      body: r.body || '',
      status: detectStatus(r.body),
      date: r.published_at,
      url: r.html_url,
      assets: r.assets || [],
      latest: r.prerelease === false && page === 1
    }));
    // Mark first as latest
    if (allReleases.length > 0) allReleases[0].isLatest = true;
    updateStats();
    renderReleases();
    setTimeout(() => {
      document.getElementById('loading-screen').classList.add('hidden');
    }, 600);
    document.getElementById('status-text').textContent =
      `${allReleases.length} VERSÕES CARREGADAS · ARQUIVO SINCRONIZADO`;
  } catch (e) {
    document.getElementById('loading-screen').classList.add('hidden');
    document.getElementById('error-msg').classList.add('visible');
    document.getElementById('status-text').textContent = 'ERRO DE CONEXÃO COM O ARQUIVO';
  }
}

function updateStats() {
  const stable = allReleases.filter(r => r.status === 'estavel').length;
  const unstable = allReleases.filter(r => r.status === 'nao-avaliavel').length;
  const broken = allReleases.filter(r => r.status === 'problematica').length;
  document.getElementById('stat-total').textContent = allReleases.length;
  document.getElementById('stat-stable').textContent = stable;
  document.getElementById('stat-unstable').textContent = unstable;
  document.getElementById('stat-broken').textContent = broken;
}

function getSorted(list) {
  const s = currentSort;
  const arr = [...list];
  if (s === 'newest') arr.sort((a, b) => new Date(b.date) - new Date(a.date));
  else if (s === 'oldest') arr.sort((a, b) => new Date(a.date) - new Date(b.date));
  else if (s === 'name') arr.sort((a, b) => a.name.localeCompare(b.name));
  return arr;
}

function renderReleases() {
  const grid = document.getElementById('releases-grid');
  const empty = document.getElementById('empty-state');

  const filtered = currentFilter === 'all'
    ? allReleases
    : allReleases.filter(r => r.status === currentFilter);

  const sorted = getSorted(filtered);

  // remove old cards
  grid.querySelectorAll('.release-card').forEach(c => c.remove());
  empty.classList.remove('visible');

  if (sorted.length === 0) {
    empty.classList.add('visible');
    return;
  }

  sorted.forEach((r, i) => {
    const card = buildCard(r, i);
    grid.appendChild(card);
  });
}

function buildCard(r, idx) {
  const div = document.createElement('div');
  div.className = `release-card status-${r.status}`;
  div.style.animationDelay = `${idx * 0.04}s`;

  const sLabel = statusLabel(r.status);
  const latestBadge = r.isLatest ? `<span class="latest-tag">LATEST</span>` : '';
  const assetCount = r.assets.length;

  const zipAsset = r.assets.find(a => a.name.endsWith('.zip') || a.name.endsWith('.rar'));
  const downloadUrl = zipAsset ? zipAsset.browser_download_url : r.url;
  const downloadLabel = zipAsset ? `⬇ BAIXAR` : `⬹ VER VERSÃO`;

  const truncBody = r.body ? r.body.replace(/\r\n/g, '\n').trim() : 'Sem descrição.';

  div.innerHTML = `
    <div class="card-accent"></div>
    <div class="card-corner tl"></div>
    <div class="card-corner tr"></div>
    <div class="card-corner bl"></div>
    <div class="card-corner br"></div>
    <div class="card-body">
      <div class="card-header">
        <div>
          <div class="card-title">${escHtml(r.name)}${latestBadge}</div>
        </div>
        <div class="card-tag">${escHtml(r.tag)}</div>
      </div>
      <div class="status-badge">
        <div class="badge-dot"></div>
        ${sLabel}
      </div>
      <div class="card-divider"></div>
      <div class="card-desc" id="desc-${r.id}">${escHtml(truncBody)}</div>
      <button class="expand-btn" data-id="${r.id}" onclick="toggleDesc('${r.id}', this)">▼ LER MAIS</button>
      <div class="card-footer">
        <div class="card-date">📅 ${formatDate(r.date)}${assetCount > 0 ? ` · ${assetCount} ASSET${assetCount > 1 ? 'S' : ''}` : ''}</div>
        <a href="${downloadUrl}" target="_blank" class="btn-download">${downloadLabel}</a>
        <a href="${r.url}" target="_blank" class="btn-info">[ ↗ ]</a>
      </div>
    </div>
  `;
  return div;
}

function toggleDesc(id, btn) {
  const desc = document.getElementById(`desc-${id}`);
  if (desc.classList.contains('expanded')) {
    desc.classList.remove('expanded');
    btn.textContent = '▼ LER MAIS';
  } else {
    desc.classList.add('expanded');
    btn.textContent = '▲ RECOLHER';
  }
}

function escHtml(s) {
  return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

document.querySelectorAll('.filter-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    currentFilter = btn.dataset.filter;
    renderReleases();
  });
});

document.getElementById('sort-select').addEventListener('change', e => {
  currentSort = e.target.value;
  renderReleases();
});

fetchReleases();
</script>
</body>
<!-- Porfavor não copia esse código seja humano!!! Contato GameSistem515@gmail.com -->
</html>
