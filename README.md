# 1st-Hackathon-Elara
1st Hackathon group project freight prototype
"""
ShipChat App — Conversational Shipping Service
Run: pip install flask && python shipchat_app.py
Then open: http://localhost:5000
"""

from flask import Flask, render_template_string

app = Flask(__name__)

HTML = """<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ShipChat — Ship anything, anywhere</title>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --pink:        #F4C0D1;
    --pink-dark:   #993556;
    --pink-light:  #FBEAF0;
    --blue:        #B5D4F4;
    --blue-dark:   #185FA5;
    --blue-light:  #E6F1FB;
    --purple:      #CECBF6;
    --purple-dark: #3C3489;
    --purple-mid:  #7F77DD;
    --purple-light:#EEEDFE;
    --bg:          #ffffff;
    --surface:     #f7f7f8;
    --text:        #1a1a1a;
    --muted:       #6b7280;
    --border:      rgba(0,0,0,0.10);
    --border-md:   rgba(0,0,0,0.18);
    --radius-sm:   8px;
    --radius-md:   12px;
    --radius-lg:   16px;
  }

  body {
    font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
    background: #f0f2f7;
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 24px;
  }

  /* ── App Shell ─────────────────────────────────────── */
  .app-shell {
    width: 100%;
    max-width: 860px;
    background: var(--bg);
    border-radius: var(--radius-lg);
    border: 0.5px solid var(--border);
    box-shadow: 0 8px 40px rgba(127,119,221,0.10), 0 2px 8px rgba(0,0,0,0.06);
    overflow: hidden;
    display: flex;
    flex-direction: column;
    height: 680px;
  }

  /* ── Top Nav ─────────────────────────────────────────*/
  .top-nav {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 12px 20px;
    border-bottom: 0.5px solid var(--border);
    background: var(--bg);
    flex-shrink: 0;
  }

  .logo {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-right: auto;
  }

  .logo-icon {
    width: 30px;
    height: 30px;
    border-radius: var(--radius-sm);
    background: linear-gradient(135deg, #AFA9EC, #ED93B1);
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .logo-icon svg { width: 14px; height: 14px; }

  .logo-name {
    font-size: 15px;
    font-weight: 600;
    color: var(--text);
    letter-spacing: -0.3px;
  }

  .logo-tagline {
    font-size: 10px;
    color: var(--muted);
  }

  .tab-btn {
    padding: 5px 14px;
    border-radius: 20px;
    border: 0.5px solid var(--border-md);
    background: none;
    color: var(--muted);
    font-size: 12px;
    cursor: pointer;
    font-family: inherit;
    transition: all 0.18s;
  }

  .tab-btn:hover { background: var(--surface); color: var(--text); }

  .tab-btn.active {
    background: linear-gradient(135deg, #AFA9EC, #D4537E);
    color: white;
    border-color: transparent;
    font-weight: 500;
  }

  /* ── Main Content ─────────────────────────────────── */
  .main { flex: 1; display: flex; overflow: hidden; }

  /* ── Sidebar ──────────────────────────────────────── */
  .sidebar {
    width: 220px;
    border-right: 0.5px solid var(--border);
    display: flex;
    flex-direction: column;
    background: var(--surface);
    flex-shrink: 0;
  }

  .sidebar-section { padding: 10px 8px; flex: 1; overflow-y: auto; }

  .section-label {
    font-size: 10px;
    font-weight: 600;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    padding: 4px 8px 6px;
  }

  .nav-item {
    display: flex;
    align-items: center;
    gap: 9px;
    padding: 7px 10px;
    border-radius: var(--radius-sm);
    cursor: pointer;
    margin-bottom: 2px;
    transition: background 0.15s;
  }

  .nav-item:hover { background: var(--bg); }
  .nav-item.active { background: var(--purple-light); }

  .nav-dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    flex-shrink: 0;
  }

  .dot-purple { background: var(--purple-mid); }
  .dot-blue   { background: #378ADD; }
  .dot-pink   { background: #D4537E; }
  .dot-green  { background: #639922; }

  .nav-label-text { flex: 1; }
  .nav-name  { font-size: 12px; color: var(--text); }
  .nav-time  { font-size: 10px; color: var(--muted); }

  .nav-badge {
    font-size: 10px;
    font-weight: 600;
    background: #D4537E;
    color: white;
    border-radius: 10px;
    padding: 1px 6px;
  }

  .new-ship-btn {
    margin: 8px 10px;
    padding: 8px;
    border-radius: var(--radius-sm);
    border: 0.5px dashed var(--border-md);
    background: none;
    color: var(--muted);
    font-size: 11px;
    cursor: pointer;
    font-family: inherit;
    text-align: center;
    transition: all 0.15s;
  }

  .new-ship-btn:hover { background: var(--bg); color: var(--text); }

  /* ── Chat Panel ───────────────────────────────────── */
  .chat-panel {
    flex: 1;
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }

  .chat-header {
    padding: 11px 16px;
    border-bottom: 0.5px solid var(--border);
    display: flex;
    align-items: center;
    gap: 10px;
    flex-shrink: 0;
  }

  .chat-avatar {
    width: 32px;
    height: 32px;
    border-radius: 50%;
    background: linear-gradient(135deg, #AFA9EC, #ED93B1);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 13px;
    font-weight: 600;
    color: white;
    flex-shrink: 0;
  }

  .chat-hdr-name  { font-size: 13px; font-weight: 500; color: var(--text); }

  .chat-online {
    font-size: 10px;
    color: #3B6D11;
    display: flex;
    align-items: center;
    gap: 4px;
  }

  .chat-online::before {
    content: '';
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: #639922;
    display: inline-block;
  }

  .hdr-actions { margin-left: auto; display: flex; gap: 6px; }

  .icon-btn {
    width: 28px;
    height: 28px;
    border-radius: 6px;
    border: 0.5px solid var(--border);
    background: none;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 0.14s;
  }

  .icon-btn:hover { background: var(--surface); }
  .icon-btn svg { width: 13px; height: 13px; stroke: var(--muted); fill: none; stroke-width: 1.5; }

  /* ── Messages ─────────────────────────────────────── */
  .messages {
    flex: 1;
    overflow-y: auto;
    padding: 14px 16px;
    display: flex;
    flex-direction: column;
    gap: 11px;
    scroll-behavior: smooth;
  }

  .msg { display: flex; flex-direction: column; max-width: 74%; }
  .msg.user { align-self: flex-end; align-items: flex-end; }
  .msg.bot  { align-self: flex-start; align-items: flex-start; }

  .bubble {
    padding: 9px 13px;
    border-radius: 14px;
    font-size: 13px;
    line-height: 1.55;
  }

  .msg.user .bubble {
    background: linear-gradient(135deg, #AFA9EC, #D4537E);
    color: white;
    border-bottom-right-radius: 4px;
  }

  .msg.bot .bubble {
    background: var(--blue-light);
    color: var(--blue-dark);
    border: 0.5px solid var(--blue);
    border-bottom-left-radius: 4px;
  }

  .msg-time { font-size: 10px; color: var(--muted); margin-top: 3px; padding: 0 3px; }

  /* Quote card */
  .quote-card {
    background: var(--bg);
    border: 0.5px solid var(--border);
    border-radius: var(--radius-md);
    padding: 11px;
    margin-top: 7px;
    width: 270px;
  }

  .quote-title { font-size: 10px; font-weight: 500; color: var(--muted); margin-bottom: 7px; }

  .progress-bar {
    height: 3px;
    background: var(--border);
    border-radius: 2px;
    margin-bottom: 8px;
    overflow: hidden;
  }

  .progress-fill {
    height: 100%;
    background: linear-gradient(90deg, #AFA9EC, #ED93B1);
    border-radius: 2px;
    width: 60%;
  }

  .quote-opt {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 7px 9px;
    border-radius: var(--radius-sm);
    border: 0.5px solid var(--border);
    margin-bottom: 5px;
    cursor: pointer;
    transition: all 0.15s;
  }

  .quote-opt:hover, .quote-opt.selected {
    border-color: #AFA9EC;
    background: var(--purple-light);
  }

  .carrier-badge {
    width: 26px;
    height: 26px;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 8px;
    font-weight: 700;
    flex-shrink: 0;
  }

  .c-ups   { background: #FAC775; color: #633806; }
  .c-fedex { background: #B5D4F4; color: #042C53; }
  .c-dhl   { background: #F4C0D1; color: #4B1528; }

  .q-info { flex: 1; }
  .q-name { font-size: 11px; font-weight: 500; color: var(--text); }
  .q-eta  { font-size: 10px; color: var(--muted); }
  .q-price{ font-size: 12px; font-weight: 600; color: var(--purple-mid); }

  /* Action chips under bot messages */
  .chips-row { display: flex; gap: 5px; margin-top: 7px; flex-wrap: wrap; }

  .action-chip {
    padding: 4px 9px;
    border-radius: 16px;
    font-size: 10px;
    font-weight: 500;
    cursor: pointer;
    border: 0.5px solid;
    transition: opacity 0.15s;
  }

  .chip-pink   { background: var(--pink-light);   color: var(--pink-dark);   border-color: var(--pink); }
  .chip-blue   { background: var(--blue-light);   color: var(--blue-dark);   border-color: var(--blue); }
  .chip-purple { background: var(--purple-light); color: var(--purple-dark); border-color: var(--purple); }

  /* Photo bubble */
  .img-bubble {
    width: 130px;
    height: 80px;
    border-radius: 10px;
    background: linear-gradient(135deg, #F4C0D1, #B5D4F4);
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 4px;
    border: 0.5px solid var(--border);
  }

  .img-bubble svg { width: 22px; height: 22px; stroke: white; fill: none; stroke-width: 1.5; }

  /* Typing indicator */
  .typing-wrap {
    background: var(--blue-light);
    border: 0.5px solid var(--blue);
    border-radius: 14px;
    border-bottom-left-radius: 4px;
    padding: 10px 14px;
    display: flex;
    gap: 4px;
    align-items: center;
  }

  .dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: var(--blue-dark);
    animation: bounce 1s infinite;
  }

  .dot:nth-child(2) { animation-delay: 0.15s; }
  .dot:nth-child(3) { animation-delay: 0.30s; }

  @keyframes bounce {
    0%, 60%, 100% { transform: translateY(0); }
    30%            { transform: translateY(-4px); }
  }

  /* ── Input Area ───────────────────────────────────── */
  .input-area {
    padding: 10px 14px;
    border-top: 0.5px solid var(--border);
    flex-shrink: 0;
  }

  .input-chips { display: flex; gap: 5px; margin-bottom: 7px; flex-wrap: wrap; }

  .input-chip {
    padding: 4px 10px;
    border-radius: 16px;
    font-size: 10px;
    border: 0.5px solid var(--border-md);
    color: var(--muted);
    cursor: pointer;
    background: var(--bg);
    font-family: inherit;
    transition: all 0.15s;
    white-space: nowrap;
  }

  .input-chip:hover {
    border-color: #AFA9EC;
    color: var(--purple-dark);
    background: var(--purple-light);
  }

  .input-row { display: flex; gap: 7px; align-items: flex-end; }

  .msg-input {
    flex: 1;
    padding: 9px 13px;
    border-radius: 20px;
    border: 0.5px solid var(--border-md);
    background: var(--surface);
    font-size: 13px;
    color: var(--text);
    resize: none;
    outline: none;
    line-height: 1.4;
    font-family: inherit;
    transition: border-color 0.15s, background 0.15s;
  }

  .msg-input:focus { border-color: #AFA9EC; background: var(--bg); }
  .msg-input::placeholder { color: var(--muted); }

  .attach-btn {
    width: 35px;
    height: 35px;
    border-radius: 50%;
    border: 0.5px solid var(--border-md);
    background: none;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    transition: background 0.14s;
  }

  .attach-btn:hover { background: var(--surface); }
  .attach-btn svg { width: 14px; height: 14px; stroke: var(--muted); fill: none; stroke-width: 1.5; }

  .send-btn {
    width: 35px;
    height: 35px;
    border-radius: 50%;
    border: none;
    background: linear-gradient(135deg, #AFA9EC, #D4537E);
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    transition: opacity 0.15s, transform 0.12s;
  }

  .send-btn:hover   { opacity: 0.88; }
  .send-btn:active  { transform: scale(0.94); }
  .send-btn svg { width: 14px; height: 14px; fill: white; }

  /* ── Screens (Track / History) ────────────────────── */
  .screen { display: none; flex: 1; padding: 16px; overflow-y: auto; flex-direction: column; gap: 12px; }
  .screen.active { display: flex; }

  .panel-card {
    border: 0.5px solid var(--border);
    border-radius: var(--radius-md);
    overflow: hidden;
    background: var(--bg);
  }

  .panel-card-header {
    padding: 11px 14px;
    border-bottom: 0.5px solid var(--border);
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .status-pill {
    padding: 2px 9px;
    border-radius: 12px;
    font-size: 10px;
    font-weight: 500;
  }

  .pill-transit  { background: #E6F1FB; color: #185FA5; border: 0.5px solid #B5D4F4; }
  .pill-done     { background: #EAF3DE; color: #3B6D11; border: 0.5px solid #C0DD97; }

  /* Track timeline */
  .track-body { padding: 14px; }

  .route-row {
    display: flex;
    gap: 10px;
    margin-bottom: 14px;
  }

  .route-box {
    flex: 1;
    border-radius: var(--radius-sm);
    padding: 10px 12px;
    border: 0.5px solid;
  }

  .route-from { background: var(--purple-light); border-color: var(--purple); }
  .route-to   { background: var(--pink-light);   border-color: var(--pink); }
  .route-arrow { display: flex; align-items: center; color: var(--muted); font-size: 16px; }

  .route-lbl { font-size: 9px; font-weight: 600; margin-bottom: 3px; }
  .route-from .route-lbl { color: var(--purple-dark); }
  .route-to   .route-lbl { color: var(--pink-dark); }
  .route-city { font-size: 12px; font-weight: 500; color: var(--text); }

  .timeline { position: relative; padding-left: 22px; }

  .timeline::before {
    content: '';
    position: absolute;
    left: 6px;
    top: 8px;
    bottom: 8px;
    width: 1px;
    background: var(--border-md);
  }

  .tl-item { position: relative; margin-bottom: 13px; }

  .tl-dot {
    position: absolute;
    left: -16px;
    top: 2px;
    width: 12px;
    height: 12px;
    border-radius: 50%;
  }

  .tl-dot.done    { background: var(--purple-mid); }
  .tl-dot.current { background: #378ADD; border: 2px solid var(--blue); }
  .tl-dot.pending { background: var(--border-md); }

  .tl-name  { font-size: 12px; font-weight: 500; color: var(--text); }
  .tl-name.pending { color: var(--muted); font-weight: 400; }
  .tl-name.current { color: #185FA5; }
  .tl-date  { font-size: 10px; color: var(--muted); }

  /* History stats */
  .stats-row {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 9px;
  }

  .stat-box {
    border-radius: var(--radius-sm);
    padding: 11px;
    text-align: center;
    border: 0.5px solid;
  }

  .stat-purple { background: var(--purple-light); border-color: var(--purple); }
  .stat-pink   { background: var(--pink-light);   border-color: var(--pink); }
  .stat-blue   { background: var(--blue-light);   border-color: var(--blue); }

  .stat-num {
    font-size: 22px;
    font-weight: 600;
    display: block;
    margin-bottom: 2px;
  }

  .stat-purple .stat-num { color: var(--purple-dark); }
  .stat-pink   .stat-num { color: var(--pink-dark); }
  .stat-blue   .stat-num { color: var(--blue-dark); }

  .stat-lbl { font-size: 10px; }
  .stat-purple .stat-lbl { color: var(--purple-dark); }
  .stat-pink   .stat-lbl { color: var(--pink-dark); }
  .stat-blue   .stat-lbl { color: var(--blue-dark); }

  /* History list rows */
  .hist-row {
    display: flex;
    align-items: center;
    gap: 9px;
    padding: 11px 14px;
    border-bottom: 0.5px solid var(--border);
  }

  .hist-row:last-child { border-bottom: none; }

  .hist-icon {
    width: 34px;
    height: 34px;
    border-radius: var(--radius-sm);
    background: var(--purple-light);
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }

  .hist-icon svg { width: 15px; height: 15px; stroke: var(--purple-mid); fill: none; stroke-width: 1.5; }
  .hist-name     { font-size: 12px; font-weight: 500; color: var(--text); }
  .hist-sub      { font-size: 10px; color: var(--muted); }
  .hist-price    { font-size: 12px; font-weight: 500; color: var(--text); text-align: right; }

  /* Scrollbar */
  ::-webkit-scrollbar { width: 4px; }
  ::-webkit-scrollbar-track { background: transparent; }
  ::-webkit-scrollbar-thumb { background: var(--border-md); border-radius: 4px; }
</style>
</head>
<body>

<div class="app-shell">

  <!-- Top nav -->
  <div class="top-nav">
    <div class="logo">
      <div class="logo-icon">
        <svg viewBox="0 0 16 16" fill="none" stroke="white" stroke-width="1.3">
          <path d="M2 5l6-3 6 3v6l-6 3-6-3z"/>
          <path d="M2 5l6 3 6-3M8 8v6"/>
        </svg>
      </div>
      <div>
        <div class="logo-name">ShipChat</div>
        <div class="logo-tagline">Ship anything, anywhere</div>
      </div>
    </div>
    <button class="tab-btn active" id="tab-chat"    onclick="showTab('chat')">Chat</button>
    <button class="tab-btn"        id="tab-track"   onclick="showTab('track')">Track</button>
    <button class="tab-btn"        id="tab-history" onclick="showTab('history')">History</button>
  </div>

  <div class="main">

    <!-- Sidebar (only visible on Chat tab) -->
    <div class="sidebar" id="sidebar">
      <div class="sidebar-section">
        <div class="section-label">Active</div>
        <div class="nav-item active">
          <div class="nav-dot dot-purple"></div>
          <div class="nav-label-text">
            <div class="nav-name">Projector</div>
            <div class="nav-time">Now</div>
          </div>
          <div class="nav-badge">3</div>
        </div>
        <div class="nav-item">
          <div class="nav-dot dot-blue"></div>
          <div class="nav-label-text">
            <div class="nav-name">Winter clothes</div>
            <div class="nav-time">2h ago</div>
          </div>
        </div>
        <div class="section-label" style="margin-top:8px;">Recent</div>
        <div class="nav-item">
          <div class="nav-dot dot-pink"></div>
          <div class="nav-label-text">
            <div class="nav-name">Laptop bag</div>
            <div class="nav-time">Yesterday</div>
          </div>
        </div>
        <div class="nav-item">
          <div class="nav-dot dot-green"></div>
          <div class="nav-label-text">
            <div class="nav-name">Books x4</div>
            <div class="nav-time">Apr 14</div>
          </div>
        </div>
      </div>
      <button class="new-ship-btn">+ New shipment</button>
    </div>

    <!-- ── CHAT SCREEN ──────────────────────────────── -->
    <div class="chat-panel" id="screen-chat">

      <div class="chat-header">
        <div class="chat-avatar">S</div>
        <div>
          <div class="chat-hdr-name">ShipChat Assistant</div>
          <div class="chat-online">Online</div>
        </div>
        <div class="hdr-actions">
          <button class="icon-btn">
            <svg viewBox="0 0 16 16"><circle cx="8" cy="8" r="6"/><path d="M8 5v4l2 2"/></svg>
          </button>
          <button class="icon-btn">
            <svg viewBox="0 0 16 16"><circle cx="4" cy="8" r="1"/><circle cx="8" cy="8" r="1"/><circle cx="12" cy="8" r="1"/></svg>
          </button>
        </div>
      </div>

      <div class="messages" id="message-area">

        <!-- Message 1: user -->
        <div class="msg bot">
          <div class="bubble">Hi! I want to ship a projector. 📦</div>
          <div class="msg-time">10:23 AM</div>
        </div>

        <!-- Message 2: bot -->
        <div class="msg user">
          <div class="bubble">Got it! Can you send me a photo and roughly how much does it weigh?</div>
          <div class="msg-time">10:23 AM</div>
        </div>

        <!-- Message 3: user with photo -->
        <div class="msg bot">
          <div class="img-bubble">
            <svg viewBox="0 0 24 24">
              <rect x="3" y="5" width="18" height="14" rx="2"/>
              <circle cx="8.5" cy="10" r="1.5"/>
              <path d="M21 15l-5-5L5 19"/>
            </svg>
          </div>
          <div class="bubble">Here's a photo — about 3.5 kg</div>
          <div class="msg-time">10:24 AM</div>
        </div>

        <!-- Message 4: bot with quote card -->
        <div class="msg user">
          <div class="bubble">Analyzing your item... Here are your best shipping options for NYC → LA:</div>
          <div class="quote-card">
            <div class="quote-title">3 quotes found for NYC → LA</div>
            <div class="progress-bar"><div class="progress-fill"></div></div>
            <div class="quote-opt selected" onclick="selectQuote(this)">
              <div class="carrier-badge c-ups">UPS</div>
              <div class="q-info">
                <div class="q-name">Standard Ground</div>
                <div class="q-eta">3–5 business days</div>
              </div>
              <div class="q-price">$24.90</div>
            </div>
            <div class="quote-opt" onclick="selectQuote(this)">
              <div class="carrier-badge c-fedex">FedEx</div>
              <div class="q-info">
                <div class="q-name">2-Day Air</div>
                <div class="q-eta">2 business days</div>
              </div>
              <div class="q-price">$41.50</div>
            </div>
            <div class="quote-opt" onclick="selectQuote(this)">
              <div class="carrier-badge c-dhl">DHL</div>
              <div class="q-info">
                <div class="q-name">Express</div>
                <div class="q-eta">1 business day</div>
              </div>
              <div class="q-price">$67.00</div>
            </div>
          </div>
          <div class="msg-time">10:24 AM</div>
        </div>

        <!-- Message 5: user reply -->
        <div class="msg bot">
          <div class="bubble">I'll go with UPS Standard please!</div>
          <div class="msg-time">10:25 AM</div>
        </div>

        <!-- Message 6: bot with chips -->
        <div class="msg user">
          <div class="bubble">Smart pick! I'd recommend adding insurance for electronics — covers up to $500 for just $4.99 extra. Add it?</div>
          <div class="chips-row">
            <span class="action-chip chip-pink" onclick="sendChip('Yes, add insurance')">Add insurance</span>
            <span class="action-chip chip-blue" onclick="sendChip('Skip insurance')">Skip for now</span>
          </div>
          <div class="msg-time">10:25 AM</div>
        </div>

        <!-- Message 7: user confirms -->
        <div class="msg bot">
          <div class="bubble">Yes, add insurance please!</div>
          <div class="msg-time">10:25 AM</div>
        </div>

        <!-- Typing indicator -->
        <div class="msg user" id="typing-indicator">
          <div class="typing-wrap">
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
          </div>
        </div>

      </div>

      <!-- Input area -->
      <div class="input-area">
        <div class="input-chips">
          <button class="input-chip" onclick="sendChip('Confirm order')">Confirm order</button>
          <button class="input-chip" onclick="sendChip('Change carrier')">Change carrier</button>
          <button class="input-chip" onclick="sendChip('Add address')">Add address</button>
          <button class="input-chip" onclick="sendChip('Pay now')">Pay now</button>
        </div>
        <div class="input-row">
          <button class="attach-btn">
            <svg viewBox="0 0 24 24">
              <path d="M21.44 11.05l-9.19 9.19a6 6 0 01-8.49-8.49l9.19-9.19a4 4 0 015.66 5.66l-9.2 9.19a2 2 0 01-2.83-2.83l8.49-8.48"/>
            </svg>
          </button>
          <textarea class="msg-input" id="chat-input" rows="1"
            placeholder="Message ShipChat..."
            onkeydown="handleKey(event)"></textarea>
          <button class="send-btn" onclick="sendMessage()">
            <svg viewBox="0 0 24 24"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
          </button>
        </div>
      </div>
    </div>

    <!-- ── TRACK SCREEN ─────────────────────────────── -->
    <div class="screen" id="screen-track">
      <div class="panel-card">
        <div class="panel-card-header">
          <div style="flex:1;">
            <div style="font-size:13px;font-weight:500;color:var(--text);">Projector — UPS Standard</div>
            <div style="font-size:10px;color:var(--muted);">Tracking: 1Z999AA10123456784</div>
          </div>
          <span class="status-pill pill-transit">In Transit</span>
        </div>
        <div class="track-body">
          <div class="route-row">
            <div class="route-box route-from">
              <div class="route-lbl">FROM</div>
              <div class="route-city">New York, NY</div>
            </div>
            <div class="route-arrow">→</div>
            <div class="route-box route-to">
              <div class="route-lbl">TO</div>
              <div class="route-city">Los Angeles, CA</div>
            </div>
          </div>
          <div class="timeline">
            <div class="tl-item">
              <div class="tl-dot done"></div>
              <div class="tl-name">Order confirmed</div>
              <div class="tl-date">Apr 18 — 10:26 AM</div>
            </div>
            <div class="tl-item">
              <div class="tl-dot done"></div>
              <div class="tl-name">Picked up by UPS</div>
              <div class="tl-date">Apr 18 — 2:14 PM</div>
            </div>
            <div class="tl-item">
              <div class="tl-dot current"></div>
              <div class="tl-name current">In transit — Chicago, IL</div>
              <div class="tl-date">Apr 19 — 8:40 AM · Current</div>
            </div>
            <div class="tl-item">
              <div class="tl-dot pending"></div>
              <div class="tl-name pending">Out for delivery</div>
              <div class="tl-date">Expected Apr 22</div>
            </div>
            <div class="tl-item">
              <div class="tl-dot pending"></div>
              <div class="tl-name pending">Delivered</div>
              <div class="tl-date">Expected Apr 22</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- ── HISTORY SCREEN ───────────────────────────── -->
    <div class="screen" id="screen-history">
      <div class="stats-row">
        <div class="stat-box stat-purple">
          <span class="stat-num">7</span>
          <span class="stat-lbl">Total shipped</span>
        </div>
        <div class="stat-box stat-pink">
          <span class="stat-num">$218</span>
          <span class="stat-lbl">Total spent</span>
        </div>
        <div class="stat-box stat-blue">
          <span class="stat-num">4.9</span>
          <span class="stat-lbl">Avg rating</span>
        </div>
      </div>

      <div class="panel-card">
        <div class="panel-card-header">
          <div style="font-size:13px;font-weight:500;color:var(--text);">Your shipments</div>
        </div>
        <div id="history-list"></div>
      </div>
    </div>

  </div><!-- .main -->
</div><!-- .app-shell -->

<script>
  /* ── Tab switching ──────────────────────────────────── */
  const TABS = ['chat', 'track', 'history'];

  function showTab(id) {
    TABS.forEach(t => {
      const btn = document.getElementById('tab-' + t);
      const scr = document.getElementById('screen-' + t);
      if (t === id) {
        btn.classList.add('active');
        if (scr) scr.classList.add('active');
      } else {
        btn.classList.remove('active');
        if (scr) scr.classList.remove('active');
      }
    });
    const sidebar = document.getElementById('sidebar');
    sidebar.style.display = (id === 'chat') ? 'flex' : 'none';
    const chatPanel = document.getElementById('screen-chat');
    chatPanel.style.display = (id === 'chat') ? 'flex' : 'none';
  }

  /* ── Quote selection ────────────────────────────────── */
  function selectQuote(el) {
    el.closest('.quote-card').querySelectorAll('.quote-opt').forEach(o => o.classList.remove('selected'));
    el.classList.add('selected');
  }

  /* ── Chat sending ───────────────────────────────────── */
  function sendChip(text) {
    addUserBubble(text);
    setTimeout(() => addBotResponse(text), 900);
  }

  function sendMessage() {
    const input = document.getElementById('chat-input');
    const text  = input.value.trim();
    if (!text) return;
    addUserBubble(text);
    input.value = '';
    setTimeout(() => addBotResponse(text), 900);
  }

  function handleKey(e) {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      sendMessage();
    }
  }

  function now() {
    const d = new Date();
    return d.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
  }

  function addUserBubble(text) {
    const area = document.getElementById('message-area');
    const typing = document.getElementById('typing-indicator');
    const div = document.createElement('div');
    div.className = 'msg bot';
    div.innerHTML = `<div class="bubble">${escHtml(text)}</div><div class="msg-time">${now()}</div>`;
    area.insertBefore(div, typing);
    area.scrollTop = area.scrollHeight;
  }

  const replies = {
    'confirm order':    'Your order is confirmed! You will receive a tracking number shortly. 🎉',
    'change carrier':   'Sure! You can choose from UPS, FedEx, or DHL above.',
    'add address':      'Please share your pickup address and destination.',
    'pay now':          'Redirecting you to our secure payment page... 💳',
    'yes, add insurance': 'Insurance added! Your item is now covered up to $500. Ready to confirm?',
    'skip insurance':   'Got it, skipping insurance. Ready to confirm your order?',
  };

  function addBotResponse(text) {
    const key = text.toLowerCase();
    const body = replies[key] || "Got it! Let me help you with that. Anything else you need?";
    const area = document.getElementById('message-area');
    const typing = document.getElementById('typing-indicator');
    const div = document.createElement('div');
    div.className = 'msg user';
    div.innerHTML = `<div class="bubble">${escHtml(body)}</div><div class="msg-time">${now()}</div>`;
    area.insertBefore(div, typing);
    area.scrollTop = area.scrollHeight;
  }

  function escHtml(s) {
    return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
  }

  /* ── History list ───────────────────────────────────── */
  const shipments = [
    { name: 'Projector',       carrier: 'UPS Ground',    date: 'Apr 18', price: '$29.89', status: 'In Transit', cls: 'pill-transit' },
    { name: 'Winter clothes',  carrier: 'FedEx 2-Day',   date: 'Apr 16', price: '$38.50', status: 'In Transit', cls: 'pill-transit' },
    { name: 'Laptop bag',      carrier: 'DHL Express',   date: 'Apr 10', price: '$54.00', status: 'Delivered',  cls: 'pill-done'    },
    { name: 'Books x4',        carrier: 'UPS Ground',    date: 'Apr 5',  price: '$18.20', status: 'Delivered',  cls: 'pill-done'    },
  ];

  const listEl = document.getElementById('history-list');
  listEl.innerHTML = shipments.map(s => `
    <div class="hist-row">
      <div class="hist-icon">
        <svg viewBox="0 0 24 24">
          <rect x="2" y="7" width="20" height="14" rx="2"/>
          <path d="M16 3v4M8 3v4M2 11h20"/>
        </svg>
      </div>
      <div style="flex:1;min-width:0;">
        <div class="hist-name">${s.name}</div>
        <div class="hist-sub">${s.carrier} · ${s.date}</div>
      </div>
      <div>
        <div class="hist-price">${s.price}</div>
        <span class="status-pill ${s.cls}">${s.status}</span>
      </div>
    </div>
  `).join('');
</script>
</body>
</html>"""


@app.route("/")
def index():
    return render_template_string(HTML)


if __name__ == "__main__":
    print("=" * 48)
    print("  ShipChat App")
    print("  Running at: http://localhost:5000")
    print("  Press Ctrl+C to stop")
    print("=" * 48)
    app.run(debug=True, port=5000)
