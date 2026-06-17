<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Email Template Designer</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --navy: #0F1729;
    --navy-mid: #1A2744;
    --blue: #2563EB;
    --blue-light: #3B82F6;
    --gold: #F59E0B;
    --white: #FFFFFF;
    --gray-50: #F8FAFC;
    --gray-100: #F1F5F9;
    --gray-200: #E2E8F0;
    --gray-400: #94A3B8;
    --gray-600: #475569;
    --gray-800: #1E293B;
  }

  body {
    font-family: 'Inter', sans-serif;
    background: var(--navy);
    color: var(--white);
    min-height: 100vh;
  }

  /* ── APP SHELL ── */
  .app {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
  }

  /* ── TOP BAR ── */
  .topbar {
    background: var(--navy-mid);
    border-bottom: 1px solid rgba(255,255,255,0.08);
    padding: 14px 28px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: sticky;
    top: 0;
    z-index: 100;
  }
  .topbar-brand {
    display: flex;
    align-items: center;
    gap: 10px;
    font-weight: 700;
    font-size: 1rem;
    letter-spacing: -0.02em;
  }
  .topbar-brand .dot {
    width: 10px; height: 10px;
    background: var(--gold);
    border-radius: 50%;
    box-shadow: 0 0 10px var(--gold);
  }
  .topbar-controls {
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .device-btn {
    background: transparent;
    border: 1px solid rgba(255,255,255,0.15);
    color: var(--gray-400);
    padding: 7px 14px;
    border-radius: 8px;
    font-size: 0.8rem;
    font-family: 'Inter', sans-serif;
    cursor: pointer;
    transition: all 0.2s;
    display: flex;
    align-items: center;
    gap: 6px;
  }
  .device-btn:hover { border-color: var(--blue-light); color: var(--white); }
  .device-btn.active {
    background: var(--blue);
    border-color: var(--blue);
    color: var(--white);
  }
  .btn-send {
    background: linear-gradient(135deg, var(--blue), var(--blue-light));
    border: none;
    color: var(--white);
    padding: 8px 20px;
    border-radius: 8px;
    font-size: 0.85rem;
    font-weight: 600;
    font-family: 'Inter', sans-serif;
    cursor: pointer;
    margin-left: 8px;
    transition: opacity 0.2s;
  }
  .btn-send:hover { opacity: 0.9; }

  /* ── MAIN LAYOUT ── */
  .main {
    display: flex;
    flex: 1;
    gap: 0;
  }

  /* ── SIDEBAR ── */
  .sidebar {
    width: 260px;
    min-width: 260px;
    background: var(--navy-mid);
    border-right: 1px solid rgba(255,255,255,0.08);
    padding: 20px 16px;
    display: flex;
    flex-direction: column;
    gap: 6px;
  }
  .sidebar-label {
    font-size: 0.68rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--gray-400);
    padding: 12px 10px 6px;
  }
  .sidebar-item {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 9px 12px;
    border-radius: 8px;
    cursor: pointer;
    font-size: 0.85rem;
    color: var(--gray-400);
    transition: all 0.15s;
    border: 1px solid transparent;
  }
  .sidebar-item:hover { background: rgba(255,255,255,0.05); color: var(--white); }
  .sidebar-item.active {
    background: rgba(37,99,235,0.18);
    border-color: rgba(37,99,235,0.4);
    color: var(--blue-light);
  }
  .sidebar-icon { font-size: 1rem; width: 20px; text-align: center; }
  .sidebar-badge {
    margin-left: auto;
    background: var(--gold);
    color: var(--navy);
    font-size: 0.65rem;
    font-weight: 700;
    padding: 2px 7px;
    border-radius: 20px;
  }

  /* ── CANVAS ── */
  .canvas {
    flex: 1;
    display: flex;
    align-items: flex-start;
    justify-content: center;
    padding: 36px 24px;
    overflow-y: auto;
    background: radial-gradient(ellipse at 30% 20%, rgba(37,99,235,0.08) 0%, transparent 60%),
                var(--navy);
  }

  .preview-wrapper {
    transition: width 0.4s cubic-bezier(0.4,0,0.2,1);
    width: 680px;
    max-width: 100%;
  }
  .preview-wrapper.tablet { width: 480px; }
  .preview-wrapper.mobile { width: 375px; }

  /* Browser chrome */
  .browser-chrome {
    background: #1E293B;
    border-radius: 12px 12px 0 0;
    padding: 12px 16px 10px;
    display: flex;
    align-items: center;
    gap: 10px;
    border: 1px solid rgba(255,255,255,0.08);
    border-bottom: none;
  }
  .chrome-dots { display: flex; gap: 6px; }
  .chrome-dot {
    width: 11px; height: 11px; border-radius: 50%;
  }
  .chrome-dot.red { background: #FF5F57; }
  .chrome-dot.yellow { background: #FEBC2E; }
  .chrome-dot.green { background: #28C840; }
  .chrome-bar {
    flex: 1;
    background: rgba(255,255,255,0.06);
    border-radius: 5px;
    height: 22px;
    display: flex;
    align-items: center;
    padding: 0 10px;
    font-size: 0.72rem;
    color: var(--gray-400);
    gap: 6px;
  }
  .chrome-bar .lock { font-size: 0.65rem; color: #4ade80; }

  /* ── EMAIL BODY ── */
  .email-outer {
    border: 1px solid rgba(255,255,255,0.08);
    border-top: none;
    border-radius: 0 0 12px 12px;
    background: var(--gray-100);
    overflow: hidden;
  }

  /* Email Client Header */
  .email-client-header {
    background: var(--white);
    padding: 16px 24px;
    border-bottom: 1px solid var(--gray-200);
  }
  .email-meta { display: flex; align-items: flex-start; gap: 12px; }
  .avatar {
    width: 40px; height: 40px; min-width: 40px;
    background: linear-gradient(135deg, var(--blue), var(--gold));
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-weight: 700; font-size: 0.85rem; color: white;
  }
  .email-meta-info { flex: 1; }
  .from-name { font-size: 0.9rem; font-weight: 600; color: var(--gray-800); }
  .from-addr { font-size: 0.75rem; color: var(--gray-400); }
  .subject-line { font-size: 0.82rem; color: var(--gray-600); margin-top: 4px; }
  .email-date { font-size: 0.72rem; color: var(--gray-400); white-space: nowrap; }

  /* ── THE ACTUAL EMAIL TEMPLATE ── */
  .email-template {
    font-family: 'Inter', -apple-system, sans-serif;
    background: var(--gray-50);
    padding: 28px 16px;
  }

  .et-container {
    max-width: 600px;
    margin: 0 auto;
    background: var(--white);
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 4px 40px rgba(0,0,0,0.08);
  }

  /* Email Header with gradient */
  .et-header {
    background: linear-gradient(135deg, #0F1729 0%, #1A2F6B 50%, #1e3a8a 100%);
    padding: 40px 36px 36px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  .et-header::before {
    content: '';
    position: absolute;
    top: -60px; right: -60px;
    width: 200px; height: 200px;
    background: radial-gradient(circle, rgba(245,158,11,0.25) 0%, transparent 70%);
    border-radius: 50%;
  }
  .et-header::after {
    content: '';
    position: absolute;
    bottom: -40px; left: -40px;
    width: 160px; height: 160px;
    background: radial-gradient(circle, rgba(37,99,235,0.3) 0%, transparent 70%);
    border-radius: 50%;
  }
  .et-logo {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    background: rgba(255,255,255,0.1);
    border: 1px solid rgba(255,255,255,0.2);
    border-radius: 30px;
    padding: 6px 16px;
    margin-bottom: 24px;
    position: relative;
    z-index: 1;
  }
  .et-logo-dot { width: 8px; height: 8px; background: var(--gold); border-radius: 50%; }
  .et-logo-text { font-size: 0.8rem; font-weight: 600; color: rgba(255,255,255,0.9); letter-spacing: 0.05em; }
  .et-header h1 {
    font-size: 1.75rem;
    font-weight: 800;
    color: var(--white);
    letter-spacing: -0.03em;
    line-height: 1.2;
    margin-bottom: 10px;
    position: relative;
    z-index: 1;
  }
  .et-header h1 span {
    background: linear-gradient(90deg, #F59E0B, #FBBF24);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  .et-header p {
    font-size: 0.9rem;
    color: rgba(255,255,255,0.65);
    line-height: 1.6;
    max-width: 380px;
    margin: 0 auto;
    position: relative;
    z-index: 1;
  }

  /* Hero image block */
  .et-hero-img {
    width: 100%;
    height: 200px;
    background: linear-gradient(135deg, #1E3A8A, #1D4ED8, #2563EB);
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
  }
  .et-hero-img::before {
    content: '';
    position: absolute;
    inset: 0;
    background: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23ffffff' fill-opacity='0.04'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
  }
  .hero-icon-wrap {
    position: relative;
    z-index: 1;
    text-align: center;
  }
  .hero-icon {
    width: 80px; height: 80px;
    background: rgba(255,255,255,0.12);
    border: 2px solid rgba(255,255,255,0.2);
    border-radius: 24px;
    display: flex; align-items: center; justify-content: center;
    font-size: 2.2rem;
    margin: 0 auto 12px;
    backdrop-filter: blur(10px);
  }
  .hero-caption {
    color: rgba(255,255,255,0.8);
    font-size: 0.8rem;
    font-weight: 500;
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }

  /* Body */
  .et-body { padding: 36px; }
  .et-greeting {
    font-size: 1rem;
    font-weight: 600;
    color: var(--gray-800);
    margin-bottom: 12px;
  }
  .et-text {
    font-size: 0.9rem;
    color: var(--gray-600);
    line-height: 1.75;
    margin-bottom: 16px;
  }

  /* Feature cards */
  .et-features {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin: 24px 0;
  }
  .et-feature-card {
    background: var(--gray-50);
    border: 1px solid var(--gray-200);
    border-radius: 12px;
    padding: 16px;
  }
  .et-feature-icon {
    font-size: 1.4rem;
    margin-bottom: 8px;
    display: block;
  }
  .et-feature-title {
    font-size: 0.82rem;
    font-weight: 700;
    color: var(--gray-800);
    margin-bottom: 4px;
  }
  .et-feature-desc {
    font-size: 0.75rem;
    color: var(--gray-400);
    line-height: 1.5;
  }

  /* Highlight box */
  .et-highlight {
    background: linear-gradient(135deg, rgba(37,99,235,0.06), rgba(245,158,11,0.06));
    border: 1px solid rgba(37,99,235,0.2);
    border-left: 3px solid var(--blue);
    border-radius: 10px;
    padding: 16px 20px;
    margin: 24px 0;
  }
  .et-highlight p {
    font-size: 0.875rem;
    color: var(--gray-800);
    font-weight: 500;
    line-height: 1.6;
  }
  .et-highlight strong { color: var(--blue); }

  /* CTA Button */
  .et-cta-wrap {
    text-align: center;
    margin: 28px 0;
  }
  .et-cta {
    display: inline-block;
    background: linear-gradient(135deg, #2563EB, #1D4ED8);
    color: white;
    text-decoration: none;
    padding: 14px 36px;
    border-radius: 10px;
    font-size: 0.9rem;
    font-weight: 700;
    letter-spacing: -0.01em;
    box-shadow: 0 4px 20px rgba(37,99,235,0.35);
    transition: transform 0.2s, box-shadow 0.2s;
    cursor: pointer;
    border: none;
  }
  .et-cta:hover {
    transform: translateY(-1px);
    box-shadow: 0 6px 28px rgba(37,99,235,0.45);
  }
  .et-cta-sub {
    margin-top: 10px;
    font-size: 0.75rem;
    color: var(--gray-400);
  }

  /* Stats row */
  .et-stats {
    display: flex;
    gap: 0;
    border: 1px solid var(--gray-200);
    border-radius: 12px;
    overflow: hidden;
    margin: 24px 0;
  }
  .et-stat {
    flex: 1;
    text-align: center;
    padding: 16px 12px;
    border-right: 1px solid var(--gray-200);
  }
  .et-stat:last-child { border-right: none; }
  .et-stat-num {
    font-size: 1.4rem;
    font-weight: 800;
    color: var(--gray-800);
    letter-spacing: -0.04em;
  }
  .et-stat-num span { color: var(--blue); }
  .et-stat-label {
    font-size: 0.7rem;
    color: var(--gray-400);
    margin-top: 2px;
    font-weight: 500;
  }

  /* Divider */
  .et-divider {
    height: 1px;
    background: var(--gray-200);
    margin: 28px 0;
  }

  /* Footer */
  .et-footer {
    background: var(--gray-50);
    border-top: 1px solid var(--gray-200);
    padding: 28px 36px;
    text-align: center;
  }
  .et-footer-links {
    display: flex;
    justify-content: center;
    gap: 20px;
    margin-bottom: 16px;
    flex-wrap: wrap;
  }
  .et-footer-links a {
    font-size: 0.75rem;
    color: var(--gray-400);
    text-decoration: none;
    cursor: pointer;
    transition: color 0.15s;
  }
  .et-footer-links a:hover { color: var(--blue); }
  .et-footer-social {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-bottom: 16px;
  }
  .social-btn {
    width: 32px; height: 32px;
    background: var(--gray-200);
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 0.9rem;
    cursor: pointer;
    transition: background 0.15s;
  }
  .social-btn:hover { background: var(--blue); }
  .et-footer-copy {
    font-size: 0.72rem;
    color: var(--gray-400);
    line-height: 1.6;
  }
  .et-footer-copy a { color: var(--gray-400); text-decoration: underline; cursor: pointer; }
  .et-footer-addr {
    font-size: 0.72rem;
    color: var(--gray-400);
    margin-top: 8px;
  }

  /* ── RIGHT PANEL ── */
  .right-panel {
    width: 240px;
    min-width: 240px;
    background: var(--navy-mid);
    border-left: 1px solid rgba(255,255,255,0.08);
    padding: 20px 16px;
    overflow-y: auto;
  }
  .panel-section { margin-bottom: 24px; }
  .panel-title {
    font-size: 0.68rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--gray-400);
    margin-bottom: 12px;
  }
  .color-row { display: flex; gap: 8px; flex-wrap: wrap; }
  .color-swatch {
    width: 28px; height: 28px;
    border-radius: 8px;
    cursor: pointer;
    border: 2px solid transparent;
    transition: border-color 0.15s;
  }
  .color-swatch:hover, .color-swatch.active { border-color: white; }
  .stat-mini {
    display: flex; justify-content: space-between;
    font-size: 0.78rem;
    color: var(--gray-400);
    padding: 6px 0;
    border-bottom: 1px solid rgba(255,255,255,0.05);
  }
  .stat-mini span:last-child { color: var(--white); font-weight: 500; }
  .stat-mini-green span:last-child { color: #4ade80; }
  .toggle-row {
    display: flex; justify-content: space-between; align-items: center;
    font-size: 0.78rem; color: var(--gray-400);
    padding: 6px 0;
  }
  .toggle {
    width: 34px; height: 18px;
    background: var(--blue);
    border-radius: 9px;
    position: relative;
    cursor: pointer;
  }
  .toggle::after {
    content: '';
    width: 14px; height: 14px;
    background: white;
    border-radius: 50%;
    position: absolute;
    right: 2px; top: 2px;
    transition: transform 0.2s;
  }
  .toggle.off { background: rgba(255,255,255,0.15); }
  .toggle.off::after { transform: translateX(-16px); }

  /* Responsive adjustments */
  @media (max-width: 1100px) {
    .right-panel { display: none; }
  }
  @media (max-width: 820px) {
    .sidebar { display: none; }
    .canvas { padding: 20px 12px; }
  }
  @media (max-width: 600px) {
    .topbar { padding: 12px 16px; }
    .topbar-brand span { display: none; }
    .device-btn span { display: none; }
    .preview-wrapper { width: 100% !important; }
  }

  /* Mobile email adjustments */
  .preview-wrapper.mobile .et-body { padding: 24px 20px; }
  .preview-wrapper.mobile .et-features { grid-template-columns: 1fr; }
  .preview-wrapper.mobile .et-header { padding: 28px 20px; }
  .preview-wrapper.mobile .et-header h1 { font-size: 1.35rem; }
  .preview-wrapper.mobile .et-stats { flex-direction: column; }
  .preview-wrapper.mobile .et-stat { border-right: none; border-bottom: 1px solid var(--gray-200); }
  .preview-wrapper.mobile .et-stat:last-child { border-bottom: none; }
  .preview-wrapper.mobile .et-footer { padding: 20px; }

  /* Tablet adjustments */
  .preview-wrapper.tablet .et-header h1 { font-size: 1.5rem; }
</style>
</head>
<body>
<div class="app">

  <!-- TOP BAR -->
  <div class="topbar">
    <div class="topbar-brand">
      <div class="dot"></div>
      <span>MailCraft Studio</span>
    </div>
    <div class="topbar-controls">
      <button class="device-btn active" onclick="setDevice('desktop', this)">
        🖥️ <span>Desktop</span>
      </button>
      <button class="device-btn" onclick="setDevice('tablet', this)">
        📱 <span>Tablet</span>
      </button>
      <button class="device-btn" onclick="setDevice('mobile', this)">
        📲 <span>Mobile</span>
      </button>
      <button class="btn-send">↗ Export HTML</button>
    </div>
  </div>

  <div class="main">

    <!-- SIDEBAR -->
    <div class="sidebar">
      <div class="sidebar-label">Templates</div>
      <div class="sidebar-item active">
        <span class="sidebar-icon">🚀</span> Product Launch
        <span class="sidebar-badge">Active</span>
      </div>
      <div class="sidebar-item">
        <span class="sidebar-icon">📰</span> Newsletter
      </div>
      <div class="sidebar-item">
        <span class="sidebar-icon">🎉</span> Welcome Email
      </div>
      <div class="sidebar-item">
        <span class="sidebar-icon">💳</span> Transactional
      </div>
      <div class="sidebar-item">
        <span class="sidebar-icon">🛒</span> Abandoned Cart
      </div>

      <div class="sidebar-label">Elements</div>
      <div class="sidebar-item">
        <span class="sidebar-icon">🖼️</span> Header Block
      </div>
      <div class="sidebar-item">
        <span class="sidebar-icon">📝</span> Text Block
      </div>
      <div class="sidebar-item">
        <span class="sidebar-icon">🔳</span> Feature Grid
      </div>
      <div class="sidebar-item">
        <span class="sidebar-icon">🔗</span> CTA Button
      </div>
      <div class="sidebar-item">
        <span class="sidebar-icon">📊</span> Stats Row
      </div>
      <div class="sidebar-item">
        <span class="sidebar-icon">🦶</span> Footer
      </div>
    </div>

    <!-- CANVAS -->
    <div class="canvas">
      <div class="preview-wrapper" id="previewWrapper">

        <!-- Browser Chrome -->
        <div class="browser-chrome">
          <div class="chrome-dots">
            <div class="chrome-dot red"></div>
            <div class="chrome-dot yellow"></div>
            <div class="chrome-dot green"></div>
          </div>
          <div class="chrome-bar">
            <span class="lock">🔒</span>
            mail.google.com/inbox/product-launch
          </div>
        </div>

        <!-- Email Outer -->
        <div class="email-outer">

          <!-- Email Client Header -->
          <div class="email-client-header">
            <div class="email-meta">
              <div class="avatar">NX</div>
              <div class="email-meta-info">
                <div class="from-name">Nexus Team <span style="font-weight:400;color:#94A3B8;">via mailcraft.io</span></div>
                <div class="from-addr">hello@nexus.io · to me</div>
                <div class="subject-line">🚀 Introducing Nexus 2.0 — Everything changes today</div>
              </div>
              <div class="email-date">Jun 16</div>
            </div>
          </div>

          <!-- THE EMAIL TEMPLATE -->
          <div class="email-template">
            <div class="et-container">

              <!-- Header -->
              <div class="et-header">
                <div class="et-logo">
                  <div class="et-logo-dot"></div>
                  <span class="et-logo-text">NEXUS</span>
                </div>
                <h1>Everything changes<br>with <span>Nexus 2.0</span></h1>
                <p>The most powerful update we've ever shipped. Faster, smarter, and built for teams that move at the speed of ideas.</p>
              </div>

              <!-- Hero visual -->
              <div class="et-hero-img">
                <div class="hero-icon-wrap">
                  <div class="hero-icon">⚡</div>
                  <div class="hero-caption">Version 2.0 · Now Live</div>
                </div>
              </div>

              <!-- Body -->
              <div class="et-body">
                <div class="et-greeting">Hi Priya 👋</div>
                <p class="et-text">
                  We've been working on this for months, and today is finally the day. Nexus 2.0 isn't just an update — it's a complete reimagining of how your team collaborates, ships, and grows together.
                </p>

                <!-- Stats -->
                <div class="et-stats">
                  <div class="et-stat">
                    <div class="et-stat-num"><span>3</span>×</div>
                    <div class="et-stat-label">Faster</div>
                  </div>
                  <div class="et-stat">
                    <div class="et-stat-num"><span>50</span>%</div>
                    <div class="et-stat-label">Less Clicks</div>
                  </div>
                  <div class="et-stat">
                    <div class="et-stat-num"><span>∞</span></div>
                    <div class="et-stat-label">Integrations</div>
                  </div>
                </div>

                <p class="et-text">Here's what's new in this release:</p>

                <!-- Features -->
                <div class="et-features">
                  <div class="et-feature-card">
                    <span class="et-feature-icon">🧠</span>
                    <div class="et-feature-title">AI Copilot</div>
                    <div class="et-feature-desc">Smart suggestions that learn your workflow and reduce busywork.</div>
                  </div>
                  <div class="et-feature-card">
                    <span class="et-feature-icon">⚡</span>
                    <div class="et-feature-title">Instant Sync</div>
                    <div class="et-feature-desc">Real-time updates across all devices with zero latency.</div>
                  </div>
                  <div class="et-feature-card">
                    <span class="et-feature-icon">🔒</span>
                    <div class="et-feature-title">Zero-Trust Security</div>
                    <div class="et-feature-desc">Enterprise-grade protection built into every layer.</div>
                  </div>
                  <div class="et-feature-card">
                    <span class="et-feature-icon">🎨</span>
                    <div class="et-feature-title">New Design</div>
                    <div class="et-feature-desc">A completely redesigned interface that gets out of your way.</div>
                  </div>
                </div>

                <!-- Highlight -->
                <div class="et-highlight">
                  <p>As an early supporter, you get <strong>3 months of Pro — free</strong>. No credit card required. Just log in and your plan is already upgraded.</p>
                </div>

                <!-- CTA -->
                <div class="et-cta-wrap">
                  <a class="et-cta">Explore Nexus 2.0 →</a>
                  <div class="et-cta-sub">Takes less than 60 seconds to get started</div>
                </div>

                <div class="et-divider"></div>

                <p class="et-text" style="font-size:0.82rem;">
                  Questions? Reply to this email and a human on our team (not a bot) will get back to you within a few hours. We read everything.
                </p>
                <p class="et-text" style="font-size:0.82rem;margin-bottom:0;">
                  With love,<br>
                  <strong style="color:#1E293B;">The Nexus Team</strong>
                </p>
              </div>

              <!-- Footer -->
              <div class="et-footer">
                <div class="et-footer-social">
                  <div class="social-btn">𝕏</div>
                  <div class="social-btn">in</div>
                  <div class="social-btn">▶</div>
                  <div class="social-btn">📸</div>
                </div>
                <div class="et-footer-links">
                  <a>Unsubscribe</a>
                  <a>Manage Preferences</a>
                  <a>Privacy Policy</a>
                  <a>View in Browser</a>
                </div>
                <div class="et-footer-copy">
                  You're receiving this because you signed up for Nexus updates.<br>
                  <a>Unsubscribe</a> anytime — no hard feelings.
                </div>
                <div class="et-footer-addr">
                  Nexus Inc. · 123 Innovation Drive, Bangalore 560001, India
                </div>
              </div>

            </div>
          </div><!-- /email-template -->

        </div><!-- /email-outer -->
      </div><!-- /preview-wrapper -->
    </div><!-- /canvas -->

    <!-- RIGHT PANEL -->
    <div class="right-panel">
      <div class="panel-section">
        <div class="panel-title">Color Palette</div>
        <div class="color-row">
          <div class="color-swatch active" style="background:#0F1729;" title="Navy"></div>
          <div class="color-swatch" style="background:#2563EB;" title="Blue"></div>
          <div class="color-swatch" style="background:#F59E0B;" title="Gold"></div>
          <div class="color-swatch" style="background:#F8FAFC;" title="Light"></div>
          <div class="color-swatch" style="background:#4ade80;" title="Green"></div>
          <div class="color-swatch" style="background:#F43F5E;" title="Red"></div>
        </div>
      </div>
      <div class="panel-section">
        <div class="panel-title">Analytics</div>
        <div class="stat-mini stat-mini-green">
          <span>Open Rate</span><span>42.8%</span>
        </div>
        <div class="stat-mini stat-mini-green">
          <span>Click Rate</span><span>18.3%</span>
        </div>
        <div class="stat-mini">
          <span>Bounce Rate</span><span>1.2%</span>
        </div>
        <div class="stat-mini">
          <span>Unsubscribes</span><span>0.4%</span>
        </div>
        <div class="stat-mini">
          <span>Delivered</span><span>12,481</span>
        </div>
      </div>
      <div class="panel-section">
        <div class="panel-title">Settings</div>
        <div class="toggle-row">
          Dark Mode Preview <div class="toggle off"></div>
        </div>
        <div class="toggle-row">
          Show Guides <div class="toggle"></div>
        </div>
        <div class="toggle-row">
          Auto-save <div class="toggle"></div>
        </div>
        <div class="toggle-row">
          Spam Check <div class="toggle off"></div>
        </div>
      </div>
    </div>

  </div>
</div>

<script>
  function setDevice(device, btn) {
    const wrapper = document.getElementById('previewWrapper');
    wrapper.className = 'preview-wrapper ' + (device === 'desktop' ? '' : device);
    document.querySelectorAll('.device-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
  }
</script>
</body>
</html>
