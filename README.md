
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Delete Account</title>
  <link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet"/>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg: #0d0d0f;
      --surface: #16161a;
      --border: rgba(255,255,255,0.07);
      --text: #e8e6e0;
      --muted: #6b6a72;
      --accent: #e05a5a;
      --accent-soft: rgba(224, 90, 90, 0.12);
      --step-line: rgba(255,255,255,0.08);
    }

    html { scroll-behavior: smooth; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'DM Sans', sans-serif;
      font-weight: 300;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 0 20px 80px;
      overflow-x: hidden;
    }

    /* Background grain */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
      pointer-events: none;
      z-index: 0;
      opacity: 0.6;
    }

    /* Glow blob */
    body::after {
      content: '';
      position: fixed;
      top: -120px;
      left: 50%;
      transform: translateX(-50%);
      width: 600px;
      height: 400px;
      background: radial-gradient(ellipse, rgba(224,90,90,0.08) 0%, transparent 70%);
      pointer-events: none;
      z-index: 0;
    }

    .wrapper {
      position: relative;
      z-index: 1;
      width: 100%;
      max-width: 560px;
    }

    /* Header */
    header {
      padding: 64px 0 48px;
      text-align: center;
    }

    .app-badge {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 100px;
      padding: 6px 16px 6px 8px;
      margin-bottom: 36px;
      font-size: 12px;
      color: var(--muted);
      letter-spacing: 0.04em;
      text-transform: uppercase;
    }

    .app-badge .dot {
      width: 6px;
      height: 6px;
      border-radius: 50%;
      background: var(--accent);
      animation: pulse 2s ease-in-out infinite;
    }

    @keyframes pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.3; }
    }

    h1 {
      font-family: 'DM Serif Display', serif;
      font-size: clamp(32px, 6vw, 44px);
      font-weight: 400;
      line-height: 1.15;
      letter-spacing: -0.02em;
      margin-bottom: 16px;
      color: var(--text);
    }

    h1 em {
      font-style: italic;
      color: var(--accent);
    }

    .subtitle {
      font-size: 15px;
      color: var(--muted);
      line-height: 1.7;
      max-width: 400px;
      margin: 0 auto;
    }

    /* Card */
    .card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 36px;
      margin-bottom: 16px;
      animation: fadeUp 0.6s ease both;
    }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(20px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    .card:nth-child(2) { animation-delay: 0.1s; }
    .card:nth-child(3) { animation-delay: 0.2s; }

    .card-label {
      font-size: 10px;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: var(--muted);
      margin-bottom: 24px;
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .card-label::after {
      content: '';
      flex: 1;
      height: 1px;
      background: var(--border);
    }

    /* Steps */
    .steps { display: flex; flex-direction: column; gap: 0; }

    .step {
      display: flex;
      gap: 20px;
      position: relative;
    }

    .step:not(:last-child)::before {
      content: '';
      position: absolute;
      left: 17px;
      top: 40px;
      bottom: -8px;
      width: 1px;
      background: var(--step-line);
    }

    .step + .step { margin-top: 8px; }

    .step-num {
      flex-shrink: 0;
      width: 36px;
      height: 36px;
      border-radius: 50%;
      background: var(--bg);
      border: 1px solid var(--border);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 13px;
      font-weight: 500;
      color: var(--muted);
      margin-top: 2px;
      transition: border-color 0.2s, color 0.2s;
    }

    .step:hover .step-num {
      border-color: var(--accent);
      color: var(--accent);
    }

    .step-body { padding: 6px 0 28px; }

    .step-title {
      font-size: 15px;
      font-weight: 500;
      color: var(--text);
      margin-bottom: 4px;
      line-height: 1.4;
    }

    .step-desc {
      font-size: 13px;
      color: var(--muted);
      line-height: 1.6;
    }

    .step-tag {
      display: inline-block;
      background: var(--accent-soft);
      color: var(--accent);
      border-radius: 6px;
      padding: 1px 8px;
      font-size: 12px;
      font-weight: 500;
      margin-top: 6px;
    }

    /* Warning card */
    .warning-card {
      background: rgba(224, 90, 90, 0.06);
      border: 1px solid rgba(224, 90, 90, 0.2);
      border-radius: 14px;
      padding: 20px 24px;
      margin-bottom: 16px;
      display: flex;
      gap: 14px;
      align-items: flex-start;
      animation: fadeUp 0.6s 0.3s ease both;
    }

    .warning-icon {
      font-size: 18px;
      flex-shrink: 0;
      margin-top: 1px;
    }

    .warning-text {
      font-size: 13px;
      color: var(--muted);
      line-height: 1.7;
    }

    .warning-text strong {
      color: var(--accent);
      font-weight: 500;
    }

    /* Contact card */
    .contact-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 16px;
      flex-wrap: wrap;
    }

    .contact-text { font-size: 14px; color: var(--muted); line-height: 1.6; }
    .contact-text strong { color: var(--text); font-weight: 500; }

    .contact-link {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 10px 20px;
      border-radius: 100px;
      border: 1px solid var(--border);
      background: var(--bg);
      color: var(--text);
      font-size: 13px;
      font-weight: 500;
      text-decoration: none;
      white-space: nowrap;
      transition: border-color 0.2s, background 0.2s;
    }

    .contact-link:hover {
      border-color: rgba(255,255,255,0.2);
      background: rgba(255,255,255,0.04);
    }

    /* Footer */
    footer {
      text-align: center;
      margin-top: 48px;
      font-size: 12px;
      color: var(--muted);
      opacity: 0.5;
      animation: fadeUp 0.6s 0.5s ease both;
    }
  </style>
</head>
<body>
  <div class="wrapper">
    <header>
      <div class="app-badge">
        <span class="dot"></span>
        Account Management
      </div>
      <h1>Delete Your<br/><em>Account</em></h1>
      <p class="subtitle">Follow the steps below to permanently remove your account and all associated data from our platform.</p>
    </header>

    <div class="card" style="animation-delay:0s">
      <div class="card-label">Steps to delete</div>
      <div class="steps">

        <div class="step">
          <div class="step-num">1</div>
          <div class="step-body">
            <div class="step-title">Open the App</div>
            <div class="step-desc">Launch the application on your device and make sure you're signed into your account.</div>
          </div>
        </div>

        <div class="step">
          <div class="step-num">2</div>
          <div class="step-body">
            <div class="step-title">Tap Your Profile Image</div>
            <div class="step-desc">Find your profile picture in the <strong style="color:var(--text)">top-left corner</strong> of the screen and tap it to open your profile menu.</div>
          </div>
        </div>

        <div class="step">
          <div class="step-num">3</div>
          <div class="step-body">
            <div class="step-title">Go to Settings</div>
            <div class="step-desc">From the profile menu, navigate to <strong style="color:var(--text)">Settings</strong> and scroll down to the bottom of the page.</div>
          </div>
        </div>

        <div class="step">
          <div class="step-num">4</div>
          <div class="step-body">
            <div class="step-title">Tap "Delete Account"</div>
            <div class="step-desc">Tap the <strong style="color:var(--text)">Delete Account</strong> button and confirm your choice when prompted.</div>
            <span class="step-tag">Permanent action</span>
          </div>
        </div>

      </div>
    </div>

    <div class="warning-card">
      <div class="warning-icon">⚠️</div>
      <div class="warning-text">
        <strong>This action is irreversible.</strong> Once deleted, your account and all associated data will be permanently removed within <strong>30 days</strong> and cannot be recovered.
      </div>
    </div>



    <footer>
      &copy; 2026 3ala Gamb · All rights reserved
    </footer>
  </div>
</body>
</html>
