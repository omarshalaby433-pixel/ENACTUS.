<html lang="en" dir="ltr" class="dark">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Enactus Video Team — Director's Canvas Portal</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Google Fonts: Inter & JetBrains Mono -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&family=JetBrains+Mono:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          fontFamily: {
            sans: ['Inter', 'sans-serif'],
            mono: ['JetBrains Mono', 'monospace'],
          },
          colors: {
            canvas: {
              bg: '#090B10',
              panel: '#111520',
              card: '#151A28',
              elevated: '#1B2234',
              border: '#262F45',
              gold: '#FFC222',
              blue: '#38BDF8',
              emerald: '#10B981',
              crimson: '#EF4444'
            }
          }
        }
      }
    }
  </script>
  <style>
    body {
      background-color: #090B10;
      color: #F8FAFC;
      font-family: 'Inter', sans-serif;
      overflow-x: hidden;
    }

    /* Interactive Cursor Spotlight Overlay */
    #cursorSpotlight {
      position: fixed;
      inset: 0;
      pointer-events: none;
      z-index: 1;
      background: radial-gradient(
        650px circle at var(--cursor-x, 50vw) var(--cursor-y, 50vh),
        rgba(255, 194, 34, 0.09),
        rgba(56, 189, 248, 0.04) 40%,
        transparent 75%
      );
      transition: background 0.05s linear;
    }

    /* Glassmorphic Director Cards so the Background Shows Through */
    .bg-canvas-panel {
      background-color: rgba(17, 21, 32, 0.78) !important;
      backdrop-filter: blur(14px);
      -webkit-backdrop-filter: blur(14px);
    }

    /* Trendy Mouse-Reactive Card Glow & Lift */
    .vf-frame, .interactive-card {
      position: relative;
      transition: transform 0.28s cubic-bezier(0.22, 1, 0.36, 1), border-color 0.28s ease, box-shadow 0.28s ease;
    }
    .vf-frame:hover, .interactive-card:hover {
      border-color: rgba(255, 194, 34, 0.45);
      box-shadow: 0 18px 45px -15px rgba(0, 0, 0, 0.7), 0 0 28px -8px rgba(255, 194, 34, 0.14);
    }
    .vf-frame::before {
      content: "";
      position: absolute;
      inset: 0;
      border-radius: inherit;
      pointer-events: none;
      background: radial-gradient(
        480px circle at var(--card-mouse-x, -500px) var(--card-mouse-y, -500px),
        rgba(255, 194, 34, 0.11),
        transparent 60%
      );
      z-index: 0;
    }
    .vf-frame > * {
      position: relative;
      z-index: 1;
    }

    /* Viewfinder Corner Brackets */
    .vf-corner {
      position: absolute;
      width: 14px;
      height: 14px;
      border-color: #FFC222;
      pointer-events: none;
      opacity: 0.8;
      transition: all 0.25s ease;
      z-index: 2;
    }
    .vf-frame:hover .vf-corner {
      width: 18px;
      height: 18px;
      opacity: 1;
    }
    .vf-tl { top: 10px; left: 10px; border-top: 2px solid; border-left: 2px solid; }
    .vf-tr { top: 10px; right: 10px; border-top: 2px solid; border-right: 2px solid; }
    .vf-bl { bottom: 10px; left: 10px; border-bottom: 2px solid; border-left: 2px solid; }
    .vf-br { bottom: 10px; right: 10px; border-bottom: 2px solid; border-right: 2px solid; }

    /* Custom Scrollbars */
    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: #090B10;
    }
    ::-webkit-scrollbar-thumb {
      background: #262F45;
      border-radius: 9999px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: #FFC222;
    }

    /* Force dark tables (some hosts/themes paint table rows white) */
    main table { background: transparent !important; color: #F8FAFC; }
    main tbody tr { background: transparent !important; }
    main tbody tr:nth-child(even) { background: rgba(27, 34, 52, 0.55) !important; }
    main tbody tr:hover { background: rgba(255, 194, 34, 0.07) !important; }
    main thead tr, main thead th { background: #1B2234 !important; }
    main tbody td { background: transparent !important; }
    main td, main th { border-color: #262F45; }
    main td:not([class*="text-"]) { color: #F8FAFC; }

    @keyframes pulseRec {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.35; transform: scale(0.92); }
    }
    .rec-dot {
      animation: pulseRec 1.6s infinite ease-in-out;
    }

    @keyframes floatSlow {
      0%, 100% { transform: translateY(0px); }
      50% { transform: translateY(-6px); }
    }
    .logo-float {
      animation: floatSlow 5s ease-in-out infinite;
    }
  </style>
</head>
<body class="min-h-screen flex flex-col justify-between selection:bg-canvas-gold selection:text-canvas-bg relative">

  <!-- ================= INTERACTIVE CINEMATIC CANVAS BACKGROUND ================= -->
  <canvas id="cinemaBgCanvas" class="fixed inset-0 pointer-events-none z-0"></canvas>
  <div id="cursorSpotlight"></div>

  <!-- ================= VISIBLE PARALLAX CAMERA & ENACTUS ORIGAMI WATERMARK ================= -->
  <div id="parallaxDecor" class="fixed inset-0 pointer-events-none z-0 overflow-hidden">
    <!-- Left Enactus Origami Giant Ambient Glow -->
    <div id="parallaxLogoLeft" class="absolute -left-16 top-24 opacity-[0.07] transition-transform duration-200 ease-out hidden md:block">
      <svg width="380" height="380" viewBox="0 0 260 240" fill="none" xmlns="http://www.w3.org/2000/svg">
        <polygon points="48,132 118,132 118,214" fill="#FFC222"/>
        <polygon points="104,132 118,132 118,214" fill="#B45309"/>
        <polygon points="118,12 118,214 216,132" fill="#FFC222"/>
        <polygon points="164,175 196,108 182,160" fill="#92400E"/>
        <polygon points="182,160 196,108 228,132" fill="#FFC222"/>
      </svg>
    </div>

    <!-- Right Cinema Camera Rig Watermark -->
    <div id="parallaxCameraRight" class="absolute bottom-20 right-6 opacity-[0.13] transition-transform duration-200 ease-out hidden sm:block">
      <svg width="340" height="340" viewBox="0 0 240 240" fill="none" xmlns="http://www.w3.org/2000/svg">
        <!-- Film Reels -->
        <circle cx="75" cy="58" r="28" stroke="#FFC222" stroke-width="5"/>
        <circle cx="75" cy="58" r="8" fill="#FFC222"/>
        <circle cx="135" cy="52" r="34" stroke="#38BDF8" stroke-width="5"/>
        <circle cx="135" cy="52" r="10" fill="#38BDF8"/>
        <!-- Camera Body -->
        <rect x="48" y="88" width="115" height="68" rx="10" stroke="#FFC222" stroke-width="5" fill="#111520"/>
        <!-- Matte Box / Lens -->
        <polygon points="163,104 205,86 205,158 163,140" stroke="#38BDF8" stroke-width="5" fill="#090B10"/>
        <!-- Viewfinder Eyepiece -->
        <path d="M48 105 L24 95 L24 118 L48 115" stroke="#FFC222" stroke-width="4"/>
        <!-- Tripod Head & Legs -->
        <rect x="88" y="156" width="36" height="12" rx="3" fill="#FFC222"/>
        <line x1="96" y1="168" x2="54" y2="232" stroke="#FFC222" stroke-width="5" stroke-linecap="round"/>
        <line x1="106" y1="168" x2="106" y2="235" stroke="#38BDF8" stroke-width="5" stroke-linecap="round"/>
        <line x1="116" y1="168" x2="158" y2="232" stroke="#FFC222" stroke-width="5" stroke-linecap="round"/>
      </svg>
    </div>
  </div>

  <!-- ================= TOAST NOTIFICATION ================= -->
  <div id="toastBox" class="fixed top-20 right-5 z-50 hidden bg-canvas-card border-2 border-canvas-gold text-white px-4 py-3 rounded-xl shadow-2xl flex items-center gap-3 text-xs sm:text-sm font-semibold transition-all">
    <span id="toastIcon" class="w-6 h-6 rounded-lg bg-canvas-gold text-canvas-bg flex items-center justify-center font-black">✓</span>
    <span id="toastMsg">Action completed</span>
  </div>

  <!-- ================= TOP DIRECTOR'S MONITOR HUD BAR ================= -->
  <header class="sticky top-0 z-40 bg-canvas-bg/90 backdrop-blur-xl border-b border-canvas-border">
    <div class="max-w-7xl mx-auto px-3 sm:px-6 h-16 flex items-center justify-between gap-3">
      
      <!-- Left: REC Indicator + Official Enactus Menufiya University Logo -->
      <div class="flex items-center gap-3">
        <div class="flex items-center gap-2 bg-canvas-panel border border-canvas-border px-2.5 py-1.5 rounded-lg">
          <span class="w-2.5 h-2.5 rounded-full bg-canvas-crimson rec-dot"></span>
          <span class="font-mono text-[11px] font-bold tracking-wider text-white">REC</span>
        </div>

        <!-- Enactus Menufiya University Compact Brand Lockup -->
        <div class="flex items-center gap-2.5 group cursor-pointer" onclick="switchScene('submit')">
          <div class="w-10 h-10 rounded-xl bg-canvas-panel border border-canvas-gold/40 flex items-center justify-center p-1 shadow-lg group-hover:border-canvas-gold transition">
            <!-- Exact Origami Logo SVG -->
            <svg viewBox="0 0 120 110" class="w-8 h-8" fill="none" xmlns="http://www.w3.org/2000/svg">
              <polygon points="18,62 54,62 54,102" fill="#FBBF24"/>
              <polygon points="47,62 54,62 54,102" fill="#B45309"/>
              <polygon points="54,4 54,102 100,62" fill="#FBBF24"/>
              <polygon points="75,84 91,51 84,76" fill="#92400E"/>
              <polygon points="84,76 91,51 107,62" fill="#FBBF24"/>
            </svg>
          </div>
          <div>
            <div class="flex items-baseline gap-1.5">
              <span class="font-extrabold text-sm sm:text-base tracking-tight text-slate-100 uppercase leading-none">ENACTUS<sup class="text-[8px] font-semibold text-slate-400 ml-0.5">TM</sup></span>
              <span class="text-canvas-gold font-mono text-xs">//</span>
              <span class="font-mono text-xs font-bold text-canvas-gold">VIDEO TEAM</span>
            </div>
            <div class="text-[10px] font-medium tracking-widest text-slate-300 uppercase mt-0.5">
              MENUFIYA UNIVERSITY
            </div>
          </div>
        </div>
      </div>

      <!-- Center: Active Task Countdown HUD -->
      <div class="hidden lg:flex items-center gap-3 bg-canvas-panel border border-canvas-border px-3.5 py-1.5 rounded-xl">
        <span class="text-[10px] font-mono uppercase tracking-wider text-slate-400">Active Deadline</span>
        <span id="hudCountdown" class="font-mono text-xs font-extrabold text-canvas-gold">00d : 00h : 00m : 00s</span>
        <span id="hudStatusBadge" class="text-[10px] font-mono font-bold px-2 py-0.5 rounded bg-emerald-500/15 text-emerald-400 border border-emerald-500/30">
          OPEN
        </span>
      </div>

      <!-- Right: Team Badge -->
      <div class="flex items-center gap-2">
        <span id="syncStatus" class="font-mono text-[11px] px-2.5 py-1.5 rounded-lg border bg-canvas-panel text-slate-400 border-canvas-border">…</span>
        <div class="hidden sm:flex items-center gap-1.5 bg-canvas-panel border border-canvas-border px-2.5 py-1.5 rounded-lg font-mono text-[11px] text-canvas-blue">
          <span>TEAM:</span>
          <strong class="text-white">VIDEO</strong>
        </div>
      </div>

    </div>
  </header>

  <!-- ================= MAIN DIRECTOR'S VIEWPORT ================= -->
  <main class="flex-1 max-w-7xl w-full mx-auto px-3 sm:px-6 pt-5 pb-28 relative z-10">

    <!-- SETUP BANNER (shows only until a Google Sheet backend is connected) -->
    <div id="setupBanner" class="hidden mb-5 bg-canvas-panel border-2 border-canvas-gold/60 rounded-2xl p-4 sm:p-5 space-y-2">
      <div class="text-sm font-extrabold text-white">⚙️ Connect your Google Sheet</div>
      <p class="text-xs text-slate-300">Paste the Apps Script <strong>Web App URL</strong> (ends with <code class="font-mono text-canvas-gold">/exec</code>). Until then, data is saved only in this browser.</p>
      <div class="flex gap-2">
        <input id="setupUrlInput" type="url" placeholder="https://script.google.com/macros/s/.../exec" class="flex-1 bg-canvas-bg border border-canvas-border focus:border-canvas-gold rounded-xl px-3.5 py-2 text-xs font-mono text-white focus:outline-none" />
        <button onclick="connectBackend('setupUrlInput')" class="bg-canvas-gold hover:bg-amber-400 text-canvas-bg font-extrabold text-xs px-4 py-2 rounded-xl transition">Connect</button>
      </div>
    </div>

    <!-- ================= ENACTUS MENUFIYA UNIVERSITY HERO LOGO SHOWCASE BANNER ================= -->
    <div class="mb-5 vf-frame bg-canvas-panel border border-canvas-border rounded-2xl px-5 py-4 flex flex-col sm:flex-row items-center justify-between gap-4 shadow-xl">
      <div class="vf-corner vf-tl"></div>
      <div class="vf-corner vf-tr"></div>
      <div class="vf-corner vf-bl"></div>
      <div class="vf-corner vf-br"></div>

      <!-- Full Official Enactus Menufiya University Lockup -->
      <div class="flex items-center gap-4">
        <div class="logo-float w-16 h-16 sm:w-20 sm:h-20 rounded-2xl bg-canvas-bg/90 border border-canvas-gold/40 flex items-center justify-center p-2 shadow-inner shrink-0">
          <svg viewBox="0 0 120 110" class="w-full h-full drop-shadow-[0_4px_12px_rgba(251,191,36,0.3)]" fill="none" xmlns="http://www.w3.org/2000/svg">
            <!-- Left Wing -->
            <polygon points="18,62 54,62 54,102" fill="#FBBF24"/>
            <!-- Left Wing Inner Fold Shadow -->
            <polygon points="47,62 54,62 54,102" fill="#B45309"/>
            <!-- Main Tall Sail -->
            <polygon points="54,4 54,102 100,62" fill="#FBBF24"/>
            <!-- Right Fold Crease Shadow -->
            <polygon points="75,84 91,51 84,76" fill="#92400E"/>
            <!-- Right Wing Tip -->
            <polygon points="84,76 91,51 107,62" fill="#FBBF24"/>
          </svg>
        </div>

        <div>
          <div class="flex items-baseline gap-1">
            <span class="text-2xl sm:text-3xl font-extrabold tracking-tight text-slate-100 uppercase leading-none">ENACTUS</span>
            <span class="text-[10px] font-bold text-slate-400 -translate-y-3">TM</span>
          </div>
          <div class="text-xs sm:text-sm font-medium tracking-[0.16em] text-slate-300 uppercase mt-0.5">
            MENUFIYA UNIVERSITY
          </div>
          <div class="inline-flex items-center gap-2 mt-1.5 px-2.5 py-0.5 rounded-md bg-canvas-gold/15 border border-canvas-gold/30 text-[10px] font-mono font-bold text-canvas-gold">
            <span>🎬 VIDEO TEAM PRODUCTION &amp; EVALUATION PORTAL</span>
          </div>
        </div>
      </div>

      <!-- Right Studio Telemetry Badge -->
      <div class="flex sm:flex-col items-center sm:items-end justify-between w-full sm:w-auto border-t sm:border-t-0 border-canvas-border pt-2.5 sm:pt-0 text-right">
        <div class="font-mono text-[11px] text-slate-400">
          BOARD SUPERVISION
        </div>
        <div class="font-mono text-xs font-bold text-white mt-0.5">
          OMAR SHALABY <span class="text-canvas-gold">//</span> ALI AFIFY
        </div>
        <div class="text-[10px] font-mono text-canvas-blue mt-0.5 hidden sm:block">
          SEASON 2026 — DIRECTOR'S CANVAS
        </div>
      </div>
    </div>

    <!-- =================================================================
         SCENE 01: 01_SUBMIT_TASK.mp4 (Member Task Submission Bay)
         ================================================================= -->
    <section id="scene-submit" class="space-y-5">
      
      <!-- Top Monitor Frame Header -->
      <div class="vf-frame bg-canvas-panel border border-canvas-border rounded-2xl p-5 sm:p-6 shadow-xl">
        <div class="vf-corner vf-tl"></div>
        <div class="vf-corner vf-tr"></div>
        <div class="vf-corner vf-bl"></div>
        <div class="vf-corner vf-br"></div>

        <div class="flex flex-col lg:flex-row lg:items-center justify-between gap-4">
          <div class="space-y-1.5">
            <div class="flex flex-wrap items-center gap-2">
              <span class="font-mono text-[11px] font-bold px-2.5 py-0.5 rounded bg-canvas-gold text-canvas-bg">
                SCENE 01 // SUBMIT BAY
              </span>
              <span id="submitStatusPill" class="inline-flex items-center gap-1.5 font-mono text-xs font-bold px-2.5 py-0.5 rounded bg-emerald-500/15 text-emerald-400 border border-emerald-500/30">
                ● SUBMISSIONS OPEN
              </span>
              <span id="submitTaskPointsBadge" class="font-mono text-xs text-canvas-blue bg-canvas-bg px-2.5 py-0.5 rounded border border-canvas-border">
                Max Score: 100 PTS
              </span>
            </div>
            <h1 id="submitBannerTitle" class="text-xl sm:text-2xl font-extrabold text-white tracking-tight">
              Task 01: File Organization &amp; Rough Cut
            </h1>
            <p id="submitBannerBrief" class="text-xs sm:text-sm text-slate-300 max-w-2xl">
              Upload your organized project folder and rough cut video to your personal Google Drive, enable link sharing, and submit the link below.
            </p>
          </div>

          <!-- Digital Timecode Countdown Box -->
          <div class="bg-canvas-bg border border-canvas-border rounded-xl px-4 py-3 text-left lg:text-right shrink-0">
            <div class="text-[10px] font-mono text-slate-400 uppercase tracking-widest">DEADLINE TIMECODE</div>
            <div id="mainCountdownDisplay" class="font-mono text-lg sm:text-xl font-black text-canvas-gold tracking-wider mt-0.5">
              02D : 14H : 22M : 19S
            </div>
            <div id="mainDeadlineDateText" class="text-[11px] font-mono text-slate-400 mt-0.5">
              Closes: Nov 20, 2026 — 11:59 PM
            </div>
          </div>
        </div>
      </div>

      <!-- DEADLINE EXPIRED LOCKED SCREEN -->
      <div id="deadlineLockedBanner" class="hidden vf-frame bg-canvas-panel border-2 border-red-500/50 rounded-2xl p-8 text-center space-y-3 shadow-2xl">
        <div class="vf-corner vf-tl"></div>
        <div class="vf-corner vf-tr"></div>
        <div class="vf-corner vf-bl"></div>
        <div class="vf-corner vf-br"></div>
        <div class="w-14 h-14 rounded-2xl bg-red-500/15 border border-red-500/40 text-red-400 flex items-center justify-center mx-auto text-2xl font-black">
          🔒
        </div>
        <div class="font-mono text-xs font-bold uppercase tracking-widest text-red-400">TIMECODE LOCKOUT ACTIVE</div>
        <h2 class="text-xl sm:text-2xl font-extrabold text-white">SUBMISSION CLOSED — DEADLINE EXPIRED</h2>
        <p class="text-xs sm:text-sm text-slate-400 max-w-md mx-auto">
          The deadline for this task has passed and new submissions are locked. Please select another open task or contact the Video Team Board.
        </p>
      </div>

      <!-- ACTIVE SUBMISSION FORM -->
      <div id="submitFormCard" class="vf-frame bg-canvas-panel border border-canvas-border rounded-2xl p-5 sm:p-7 space-y-5 shadow-xl">
        <div class="vf-corner vf-tl"></div>
        <div class="vf-corner vf-tr"></div>
        <div class="vf-corner vf-bl"></div>
        <div class="vf-corner vf-br"></div>

        <div class="flex flex-wrap items-center justify-between gap-2 border-b border-canvas-border pb-4">
          <div>
            <h2 class="text-base sm:text-lg font-bold text-white">Task Delivery Console</h2>
            <p class="text-xs text-slate-400">Select the task, choose your name from the list, and paste your Google Drive link.</p>
          </div>
          <span class="font-mono text-xs font-semibold px-3 py-1 rounded-lg bg-canvas-card text-canvas-gold border border-canvas-border">
            Role: Video Team
          </span>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <!-- 1. Select Task (Unlimited Tasks) -->
          <div>
            <label class="block text-xs font-mono uppercase tracking-wider text-slate-300 mb-1.5">
              1. Select Task <span class="text-canvas-gold">*</span>
            </label>
            <select id="submitTaskSelect" onchange="handleTaskSelectChange(this.value)" class="w-full bg-canvas-bg border border-canvas-border focus:border-canvas-gold rounded-xl px-3.5 py-2.5 text-sm font-semibold text-white focus:outline-none">
              <!-- Populated dynamically -->
            </select>
          </div>

          <!-- 2. Select Member Name ONLY (Secret Code is completely hidden) -->
          <div>
            <label class="block text-xs font-mono uppercase tracking-wider text-slate-300 mb-1.5">
              2. Select Your Name <span class="text-canvas-gold">*</span>
            </label>
            <select id="submitMemberDropdown" onchange="handleSubmitMemberDropdown(this.value)" class="w-full bg-canvas-bg border border-canvas-border focus:border-canvas-gold rounded-xl px-3.5 py-2.5 text-sm font-semibold text-white focus:outline-none">
              <option value="">Choose your name...</option>
            </select>
          </div>
        </div>

        <!-- Selected Member Confirmation Badge (Shows Name & Team ONLY — No Secret Code) -->
        <div id="submitVerifiedMemberBar" class="hidden bg-canvas-bg border border-emerald-500/40 rounded-xl px-4 py-3 flex flex-wrap items-center justify-between gap-2">
          <div class="flex items-center gap-2.5">
            <span class="w-6 h-6 rounded-lg bg-emerald-500/20 text-emerald-400 flex items-center justify-center text-xs font-bold">✓</span>
            <div>
              <span class="text-xs text-slate-400">Selected Member:</span>
              <strong id="verifiedMemberName" class="text-sm font-bold text-white ml-1">—</strong>
            </div>
          </div>
          <span class="font-mono text-xs font-bold px-2.5 py-0.5 rounded bg-canvas-card text-canvas-blue border border-canvas-border">
            Team: Video
          </span>
        </div>

        <!-- Resubmit Notice (Does NOT leak previous link or secret code) -->
        <div id="resubmitAlertBox" class="hidden bg-amber-500/10 border border-amber-500/30 rounded-xl px-4 py-2.5 text-xs text-amber-300 flex items-center justify-between gap-2">
          <span>⚠️ A submission is already recorded under your name for this task. Submitting again before the deadline will update your Google Drive link.</span>
        </div>

        <!-- 3. Google Drive Link -->
        <div>
          <div class="flex items-center justify-between mb-1.5">
            <label class="text-xs font-mono uppercase tracking-wider text-slate-300">
              3. Google Drive Link (Video / Folder) <span class="text-red-400">*</span>
            </label>
            <span id="driveUrlStatusText" class="text-[11px] font-mono text-slate-400">Paste drive.google.com link</span>
          </div>
          <input id="submitDriveUrlInput" type="url" oninput="validateDriveUrlLive(this.value)" placeholder="https://drive.google.com/drive/folders/..." class="w-full bg-canvas-bg border border-canvas-border focus:border-canvas-gold rounded-xl px-3.5 py-2.5 text-sm font-mono text-white focus:outline-none" />

          <label class="mt-3 flex items-center gap-2.5 cursor-pointer select-none">
            <input id="submitShareConfirm" type="checkbox" class="w-4 h-4 accent-canvas-gold rounded cursor-pointer" />
            <span class="text-xs text-slate-300">
              I confirm that Google Drive link sharing is set to <strong class="text-canvas-gold">"Anyone with the link can view"</strong>
            </span>
          </label>
        </div>

        <!-- 4. Optional Notes -->
        <div>
          <label class="block text-xs font-mono uppercase tracking-wider text-slate-300 mb-1.5">
            4. Delivery Note (Optional)
          </label>
          <input id="submitNoteInput" type="text" placeholder="Any short note for the Board about your edit or footage..." class="w-full bg-canvas-bg border border-canvas-border focus:border-canvas-gold rounded-xl px-3.5 py-2.5 text-sm text-white focus:outline-none" />
        </div>

        <!-- Submit Action Button -->
        <div class="pt-2 flex justify-end">
          <button id="submitTaskBtn" onclick="handleMemberTaskSubmit()" class="w-full sm:w-auto bg-canvas-gold hover:bg-amber-400 text-canvas-bg font-extrabold text-sm px-7 py-3 rounded-xl transition shadow-lg flex items-center justify-center gap-2">
            <span>🎬 Submit Task to Board</span>
          </button>
        </div>
      </div>

      <!-- SUBMISSION RECEIPT CARD (Shows Name, Task, Timestamp, Status ONLY — No Secret Code) -->
      <div id="submissionReceiptBox" class="hidden vf-frame bg-canvas-panel border-2 border-emerald-500/50 rounded-2xl p-5 sm:p-6 space-y-3 shadow-xl">
        <div class="flex flex-wrap items-center justify-between gap-2">
          <div class="flex items-center gap-2.5">
            <span class="w-7 h-7 rounded-lg bg-emerald-500/20 text-emerald-400 flex items-center justify-center font-bold">✓</span>
            <div>
              <h3 class="text-sm font-bold text-white">Task Submission Logged</h3>
              <p class="text-[11px] text-slate-400">Your Google Drive link is now visible in the Board Grading Portal.</p>
            </div>
          </div>
          <span id="receiptRefBadge" class="font-mono text-xs font-bold px-2.5 py-1 rounded bg-emerald-500/15 text-emerald-400 border border-emerald-500/30">
            ON-TIME ✓
          </span>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-4 gap-3 bg-canvas-bg border border-canvas-border rounded-xl p-3.5 text-xs">
          <div>
            <div class="text-slate-500 font-mono">MEMBER NAME</div>
            <div id="receiptMemberVal" class="font-bold text-white mt-0.5">—</div>
          </div>
          <div>
            <div class="text-slate-500 font-mono">TASK</div>
            <div id="receiptTaskVal" class="font-semibold text-canvas-gold mt-0.5">—</div>
          </div>
          <div>
            <div class="text-slate-500 font-mono">TIMESTAMP</div>
            <div id="receiptTimeVal" class="font-mono text-emerald-400 mt-0.5">—</div>
          </div>
          <div>
            <div class="text-slate-500 font-mono">STATUS</div>
            <div id="receiptStatusVal" class="font-mono font-bold text-canvas-blue mt-0.5">ON-TIME // DELIVERED</div>
          </div>
        </div>
      </div>

    </section>

    <!-- =================================================================
         SCENE 02: 02_TASK_GRADE.mp4 (Task-Specific Grade & Feedback Lookup)
         ================================================================= -->
    <section id="scene-grade" class="hidden space-y-5">
      
      <div class="max-w-2xl mx-auto vf-frame bg-canvas-panel border border-canvas-border rounded-2xl p-6 sm:p-8 space-y-6 shadow-2xl">
        <div class="vf-corner vf-tl"></div>
        <div class="vf-corner vf-tr"></div>
        <div class="vf-corner vf-bl"></div>
        <div class="vf-corner vf-br"></div>

        <div class="text-center space-y-1.5">
          <span class="font-mono text-[11px] font-bold px-2.5 py-0.5 rounded bg-canvas-gold text-canvas-bg">
            SCENE 02 // PRIVATE GRADE &amp; FEEDBACK
          </span>
          <h1 class="text-xl sm:text-2xl font-extrabold text-white pt-1">
            Check Your Task Evaluation
          </h1>
          <p class="text-xs sm:text-sm text-slate-400">
            Select the task and enter the private <strong class="text-slate-200">Secret Code</strong> sent to you by the Board to view your personal score and feedback.
          </p>
        </div>

        <!-- Step 1: Select Task + Step 2: Enter Private Secret Code (With Eye Toggle & NO Public Code Hints) -->
        <div class="grid grid-cols-1 sm:grid-cols-12 gap-3 pt-1">
          <div class="sm:col-span-5">
            <label class="block text-[11px] font-mono uppercase text-slate-400 mb-1">1. Select Task</label>
            <select id="lookupTaskSelect" class="w-full bg-canvas-bg border border-canvas-border focus:border-canvas-gold rounded-xl px-3.5 py-3 text-xs sm:text-sm font-semibold text-white focus:outline-none">
              <!-- Populated dynamically from tasks list -->
            </select>
          </div>

          <div class="sm:col-span-4">
            <label class="block text-[11px] font-mono uppercase text-slate-400 mb-1">2. Your Private Secret Code</label>
            <div class="relative flex items-center">
              <input id="lookupMemberCodeInput" type="password" placeholder="Enter secret code..." onkeydown="if(event.key==='Enter') lookupSingleTaskGrade()" class="w-full bg-canvas-bg border border-canvas-border focus:border-canvas-gold rounded-xl pl-3.5 pr-9 py-3 text-xs sm:text-sm font-mono font-bold text-canvas-gold uppercase focus:outline-none" />
              <button type="button" onclick="toggleLookupSecretCodeVisibility()" id="lookupEyeBtn" class="absolute right-2.5 text-slate-400 hover:text-white text-xs font-mono" title="Show/Hide Secret Code">
                👁️
              </button>
            </div>
          </div>

          <div class="sm:col-span-3 flex items-end">
            <button onclick="lookupSingleTaskGrade()" class="w-full bg-canvas-gold hover:bg-amber-400 text-canvas-bg font-extrabold text-xs sm:text-sm py-3 px-4 rounded-xl transition shadow-md">
              Show Grade
            </button>
          </div>
        </div>

        <!-- Result Display Area (Shows ONLY the selected task's grade & feedback) -->
        <div id="lookupSingleTaskResult" class="pt-1">
          <div class="bg-canvas-bg/70 border border-dashed border-canvas-border rounded-xl p-6 text-center text-xs text-slate-500 font-mono">
            Select a task above and enter your private Secret Code to view your personal task score and Board feedback.
          </div>
        </div>

      </div>

    </section>

    <!-- =================================================================
         SCENE 03: 03_BOARD_PORTAL.lock (Password-Protected Board Portal)
         ================================================================= -->
    <!-- =================================================================
         SCENE: SESSIONS SCHEDULE (everyone can view — Board edits with password)
         ================================================================= -->
    <section id="scene-sessions" class="hidden space-y-5">
      <div class="bg-canvas-panel border border-canvas-border rounded-2xl p-4 sm:p-6 space-y-4">
        <div class="border-b border-canvas-border pb-3">
          <span class="font-mono text-[10px] font-bold uppercase px-2 py-0.5 rounded bg-canvas-gold/15 text-canvas-gold border border-canvas-gold/30">SESSIONS</span>
          <h2 class="text-lg sm:text-xl font-extrabold text-white mt-1">📅 Sessions Schedule</h2>
          <p class="text-xs text-slate-400">Every session with its day &amp; date, and the Google Drive folder that holds that day's material.</p>
        </div>

        <!-- Board-only editor -->
        <div id="sessionsAdminBox" class="hidden bg-canvas-bg border border-canvas-gold/40 rounded-xl p-4 space-y-3">
          <div id="sessionFormTitle" class="text-xs font-mono font-bold uppercase text-canvas-gold">➕ Add Session (Board only)</div>
          <div class="grid grid-cols-1 md:grid-cols-12 gap-3">
            <div class="md:col-span-4">
              <label class="block text-[10px] font-mono text-slate-400 mb-1">Session name *</label>
              <input id="sessionTitleInput" type="text" maxlength="120" placeholder="Session 1: Intro to Cinematography" class="w-full bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3 py-2 text-xs text-white focus:outline-none" />
            </div>
            <div class="md:col-span-3">
              <label class="block text-[10px] font-mono text-slate-400 mb-1">Date *</label>
              <input id="sessionDateInput" type="date" class="w-full bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3 py-2 text-xs font-mono text-white focus:outline-none" />
            </div>
            <div class="md:col-span-5">
              <label class="block text-[10px] font-mono text-slate-400 mb-1">Google Drive link (that day's data)</label>
              <input id="sessionLinkInput" type="url" placeholder="https://drive.google.com/drive/folders/..." class="w-full bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3 py-2 text-xs font-mono text-white focus:outline-none" />
            </div>
            <div class="md:col-span-12">
              <label class="block text-[10px] font-mono text-slate-400 mb-1">Note (optional)</label>
              <input id="sessionNoteInput" type="text" maxlength="300" placeholder="Bring your camera / Location: ..." class="w-full bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3 py-2 text-xs text-white focus:outline-none" />
            </div>
          </div>
          <div class="flex flex-wrap justify-end gap-2">
            <button id="sessionCancelBtn" onclick="cancelEditSession()" class="hidden text-slate-300 hover:bg-canvas-elevated font-mono text-xs font-bold px-4 py-2 rounded-xl transition">Cancel</button>
            <button id="sessionSaveBtn" onclick="saveSessionFromForm()" class="bg-canvas-gold hover:bg-amber-400 text-canvas-bg font-extrabold text-xs px-5 py-2 rounded-xl transition">Add Session</button>
          </div>
        </div>

        <div id="sessionsList" class="space-y-3"></div>
        <p id="sessionsBoardHint" class="text-[11px] font-mono text-slate-500">Board members: unlock the <strong class="text-slate-300">Board Portal</strong> tab with the password to add or edit sessions.</p>
      </div>
    </section>

    <!-- =================================================================
         SCENE: TEAM MEMORIES (everyone can view — Board edits with password)
         ================================================================= -->
    <section id="scene-memories" class="hidden space-y-5">
      <div class="bg-canvas-panel border border-canvas-border rounded-2xl p-4 sm:p-6 space-y-4">
        <div class="border-b border-canvas-border pb-3">
          <span class="font-mono text-[10px] font-bold uppercase px-2 py-0.5 rounded bg-canvas-gold/15 text-canvas-gold border border-canvas-gold/30">MEMORIES</span>
          <h2 class="text-lg sm:text-xl font-extrabold text-white mt-1">📸 Team Photos &amp; Memories</h2>
          <p class="text-xs text-slate-400">Albums from our events and sessions. Tap any photo to enlarge it.</p>
        </div>

        <div id="memoriesAdminBox" class="hidden bg-canvas-bg border border-canvas-gold/40 rounded-xl p-4 space-y-3">
          <div id="memoryFormTitle" class="text-xs font-mono font-bold uppercase text-canvas-gold">➕ Add Album (Board only)</div>
          <div class="grid grid-cols-1 md:grid-cols-12 gap-3">
            <div class="md:col-span-5">
              <label class="block text-[10px] font-mono text-slate-400 mb-1">Album title *</label>
              <input id="memoryTitleInput" type="text" maxlength="120" placeholder="Team trip / Workshop day" class="w-full bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3 py-2 text-xs text-white focus:outline-none" />
            </div>
            <div class="md:col-span-3">
              <label class="block text-[10px] font-mono text-slate-400 mb-1">Event date</label>
              <input id="memoryDateInput" type="date" class="w-full bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3 py-2 text-xs font-mono text-white focus:outline-none" />
            </div>
            <div class="md:col-span-4">
              <label class="block text-[10px] font-mono text-slate-400 mb-1">Full album folder link (optional)</label>
              <input id="memoryLinkInput" type="url" placeholder="https://drive.google.com/drive/folders/..." class="w-full bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3 py-2 text-xs font-mono text-white focus:outline-none" />
            </div>
            <div class="md:col-span-12">
              <label class="block text-[10px] font-mono text-slate-400 mb-1">Text shown above the photos (optional)</label>
              <textarea id="memoryNoteInput" rows="3" maxlength="600" placeholder="Write a few words about this day..." class="w-full bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3 py-2 text-xs text-white focus:outline-none"></textarea>
            </div>
            <div class="md:col-span-12">
              <label class="block text-[10px] font-mono text-slate-400 mb-1">Photos — paste one Google Drive photo link per line (each photo shared as "Anyone with the link")</label>
              <textarea id="memoryPhotosInput" rows="5" placeholder="https://drive.google.com/file/d/.../view&#10;https://drive.google.com/file/d/.../view" class="w-full bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3 py-2 text-xs font-mono text-white focus:outline-none"></textarea>
            </div>
          </div>
          <div class="flex flex-wrap justify-end gap-2">
            <button id="memoryCancelBtn" onclick="resetMemoryForm()" class="hidden text-slate-300 hover:bg-canvas-elevated font-mono text-xs font-bold px-4 py-2 rounded-xl transition">Cancel</button>
            <button id="memorySaveBtn" onclick="saveMemoryFromForm()" class="bg-canvas-gold hover:bg-amber-400 text-canvas-bg font-extrabold text-xs px-5 py-2 rounded-xl transition">Add Album</button>
          </div>
        </div>

        <div id="memoriesList" class="space-y-5"></div>
        <p id="memoriesBoardHint" class="text-[11px] font-mono text-slate-500">Board members: unlock the <strong class="text-slate-300">Board Portal</strong> tab with the password to add or edit albums.</p>
      </div>
    </section>

    <section id="scene-board" class="hidden space-y-5">

      <!-- LOCKED SCREEN (Requires Password) -->
      <div id="boardLockCard" class="max-w-md mx-auto vf-frame bg-canvas-panel border border-canvas-border rounded-2xl p-6 sm:p-8 text-center space-y-5 my-4 shadow-2xl">
        <div class="vf-corner vf-tl"></div>
        <div class="vf-corner vf-tr"></div>
        <div class="vf-corner vf-bl"></div>
        <div class="vf-corner vf-br"></div>

        <!-- Enactus Menufiya University Logo Inside Lock Screen -->
        <div class="w-20 h-20 rounded-2xl bg-canvas-bg border border-canvas-gold/40 flex flex-col items-center justify-center mx-auto p-2.5 shadow-lg logo-float">
          <svg viewBox="0 0 120 110" class="w-12 h-12" fill="none" xmlns="http://www.w3.org/2000/svg">
            <polygon points="18,62 54,62 54,102" fill="#FBBF24"/>
            <polygon points="47,62 54,62 54,102" fill="#B45309"/>
            <polygon points="54,4 54,102 100,62" fill="#FBBF24"/>
            <polygon points="75,84 91,51 84,76" fill="#92400E"/>
            <polygon points="84,76 91,51 107,62" fill="#FBBF24"/>
          </svg>
        </div>
        <div class="space-y-1">
          <div class="text-xs font-extrabold tracking-tight text-slate-200 uppercase">
            ENACTUS<sup class="text-[7px] text-slate-400 ml-0.5">TM</sup> <span class="uppercase tracking-widest text-[10px] text-canvas-gold font-mono ml-1">MENUFIYA UNIVERSITY</span>
          </div>
          <h2 class="text-lg sm:text-xl font-extrabold text-white">Video Team Board Portal 🔒</h2>
          <p class="text-xs text-slate-400">
            Authorized Board Members: <strong class="text-slate-200">Omar Shalaby</strong> &amp; <strong class="text-slate-200">Ali Afify</strong>
          </p>
        </div>

        <div class="flex gap-2">
          <input id="boardPasswordInput" type="password" placeholder="Enter Board Password" onkeydown="if(event.key==='Enter') unlockBoardPortal()" class="flex-1 bg-canvas-bg border border-canvas-border focus:border-canvas-gold rounded-xl px-4 py-2.5 text-sm font-mono text-center text-white focus:outline-none" />
          <button onclick="unlockBoardPortal()" class="bg-canvas-gold hover:bg-amber-400 text-canvas-bg font-extrabold text-sm px-5 py-2.5 rounded-xl transition">
            Unlock
          </button>
        </div>

      </div>

      <!-- UNLOCKED BOARD COMMAND SUITE -->
      <div id="boardUnlockedSuite" class="hidden space-y-5">

        <!-- Board Top Bar & 6 Sub-Tabs -->
        <div class="vf-frame bg-canvas-panel border border-canvas-border rounded-2xl p-4 sm:p-5 flex flex-col xl:flex-row xl:items-center justify-between gap-4">
          <div>
            <div class="flex items-center gap-2">
              <span class="font-mono text-[11px] font-bold px-2 py-0.5 rounded bg-canvas-gold text-canvas-bg">BOARD PORTAL UNLOCKED</span>
              <span class="text-xs font-mono text-slate-400">Omar Shalaby &amp; Ali Afify</span>
            </div>
            <h2 class="text-lg font-extrabold text-white mt-1">
              Tasks Grading, Multi-Criteria Matrix, Rankings &amp; Private Member Codes
            </h2>
          </div>

          <!-- Sub-Navigation Buttons (6 Board Tabs + Board-Only Test Lock) -->
          <div class="flex flex-wrap items-center gap-1.5 bg-canvas-bg p-1.5 rounded-xl border border-canvas-border">
            <button id="btab-grading" onclick="switchBoardTab('grading')" class="px-3 py-1.5 rounded-lg text-xs font-bold bg-canvas-gold text-canvas-bg transition">
              🎬 1. Grade Tasks &amp; Drive Links
            </button>
            <button id="btab-matrix" onclick="switchBoardTab('matrix')" class="px-3 py-1.5 rounded-lg text-xs font-bold text-slate-300 hover:text-white transition">
              📊 2. Evaluation Matrix
            </button>
            <button id="btab-rankings" onclick="switchBoardTab('rankings')" class="px-3 py-1.5 rounded-lg text-xs font-bold text-slate-300 hover:text-white transition">
              🏆 3. Auto Rankings
            </button>
            <button id="btab-tasks" onclick="switchBoardTab('tasks')" class="px-3 py-1.5 rounded-lg text-xs font-bold text-slate-300 hover:text-white transition">
              ➕ 4. Unlimited Tasks
            </button>
            <button id="btab-roster" onclick="switchBoardTab('roster')" class="px-3 py-1.5 rounded-lg text-xs font-bold text-slate-300 hover:text-white transition">
              👥 5. Team Roster (Private Codes)
            </button>
            <button id="btab-security" onclick="switchBoardTab('security')" class="px-3 py-1.5 rounded-lg text-xs font-bold text-slate-300 hover:text-white transition">
              🔐 6. Security &amp; Sheet
            </button>
            <button onclick="exportEverythingMasterExcel()" class="px-3 py-1.5 rounded-lg text-xs font-mono font-bold bg-emerald-500/20 hover:bg-emerald-500 hover:text-canvas-bg text-emerald-400 border border-emerald-500/40 transition flex items-center gap-1" title="Export All Data (Tasks, Drive Links, Evaluations & Rankings) to Excel">
              <span>⬇ Export All (Excel)</span>
            </button>
            <button onclick="refreshFromServer()" class="px-3 py-1.5 rounded-lg text-xs font-mono font-bold bg-canvas-blue/15 hover:bg-canvas-blue hover:text-canvas-bg text-canvas-blue border border-canvas-blue/40 transition" title="Reload latest submissions from the Google Sheet">🔄 Sync</button>
            <button onclick="lockBoardPortal()" class="px-2.5 py-1.5 rounded-lg text-xs font-bold text-red-400 hover:bg-red-500/10 transition">
              Lock 🔒
            </button>
          </div>
        </div>

        <!-- =============================================================
             BOARD SUB-TAB 1: GRADE TASKS & VIEW DRIVE LINKS
             ============================================================= -->
        <div id="bpanel-grading" class="space-y-4">
          <div class="bg-canvas-panel border border-canvas-border rounded-2xl p-4 sm:p-5 flex flex-col lg:flex-row lg:items-center justify-between gap-4">
            <div class="flex flex-wrap items-center gap-3">
              <div>
                <label class="block text-[10px] font-mono uppercase text-slate-400 mb-1">Select Task to Grade</label>
                <select id="boardGradeTaskSelect" onchange="handleBoardGradeTaskChange(this.value)" class="bg-canvas-bg border border-canvas-gold rounded-xl px-3.5 py-2 text-xs sm:text-sm font-bold text-white focus:outline-none">
                </select>
              </div>
              <div>
                <label class="block text-[10px] font-mono uppercase text-slate-400 mb-1">Active Board Evaluator</label>
                <select id="boardActiveEvaluator" class="bg-canvas-bg border border-canvas-border rounded-xl px-3.5 py-2 text-xs sm:text-sm font-bold text-canvas-gold focus:outline-none">
                  <option value="Omar Shalaby">Omar Shalaby (Board)</option>
                  <option value="Ali Afify">Ali Afify (Board)</option>
                </select>
              </div>
            </div>

            <div class="flex flex-wrap items-center gap-2 text-xs font-mono">
              <span id="boardGradeSummaryPill" class="px-3 py-1.5 rounded-xl bg-canvas-bg border border-canvas-border text-slate-300">
                Submitted: 0 / 0
              </span>
              <button onclick="exportSingleTaskSubmissionsExcel()" class="bg-canvas-gold hover:bg-amber-400 text-canvas-bg font-extrabold text-xs px-3 py-1.5 rounded-xl transition shadow">
                ⬇ Export Current Task &amp; Drive Links (Excel)
              </button>
              <button onclick="exportAllTasksSubmissionsExcel()" class="bg-canvas-blue hover:bg-sky-300 text-canvas-bg font-extrabold text-xs px-3 py-1.5 rounded-xl transition shadow">
                ⬇ Export All Tasks &amp; Links (Excel)
              </button>
            </div>
          </div>

          <div id="boardGradingCardsContainer" class="grid grid-cols-1 lg:grid-cols-2 gap-4">
            <!-- Populated dynamically by JS -->
          </div>
        </div>

        <!-- =============================================================
             BOARD SUB-TAB 2: EVALUATION MATRIX (MIND MAP + 6 FIXED PILLARS)
             ============================================================= -->
        <div id="bpanel-matrix" class="hidden space-y-5">
          <div class="bg-canvas-panel border border-canvas-border rounded-2xl p-4 sm:p-6 space-y-5">
            
            <!-- Header & Action Buttons -->
            <div class="flex flex-col lg:flex-row lg:items-center justify-between gap-3 border-b border-canvas-border pb-4">
              <div>
                <div class="flex flex-wrap items-center gap-2">
                  <span class="font-mono text-[10px] font-bold uppercase px-2.5 py-0.5 rounded bg-canvas-gold text-canvas-bg">
                    INTERACTIVE EVALUATION MIND MAP
                  </span>
                  <span class="font-mono text-[10px] font-bold uppercase px-2.5 py-0.5 rounded bg-canvas-blue/15 text-canvas-blue border border-canvas-blue/30">
                    🎬 6 CORE PILLARS = 100% VIDEO SHARE (45% OF ENACTUS TOTAL • HR = 55%)
                  </span>
                </div>
                <h3 class="text-base sm:text-lg font-extrabold text-white mt-1.5">
                  Video Team Evaluation Mind Map &amp; Drill-Down Matrix
                </h3>
                <p class="text-xs text-slate-400">
                  Click any of the <strong>6 Core Pillars</strong> in the Mind Map below to open its sub-columns, add new items or tasks inside it, and calculate each member's <strong>Total Score</strong> for that pillar and overall.
                </p>
              </div>

              <div class="flex flex-wrap items-center gap-2">
                <button onclick="selectMindMapPillar('ALL')" id="btnMasterOverview" class="bg-canvas-gold text-canvas-bg font-extrabold text-xs px-3.5 py-2 rounded-xl transition shadow">
                  🗺️ Master 6-Pillars Summary
                </button>
                <button onclick="exportActivePillarExcel()" class="bg-canvas-bg hover:bg-canvas-elevated text-canvas-gold border border-canvas-gold/50 font-extrabold text-xs px-3.5 py-2 rounded-xl transition">
                  ⬇ Export Opened Section (Excel)
                </button>
                <button onclick="exportFullMatrixCSV()" class="bg-canvas-blue hover:bg-sky-300 text-canvas-bg font-extrabold text-xs px-3.5 py-2 rounded-xl transition">
                  ⬇ Export 6-Pillars Summary (Excel)
                </button>
                <button onclick="exportEverythingMasterExcel()" class="bg-emerald-500 hover:bg-emerald-400 text-canvas-bg font-extrabold text-xs px-3.5 py-2 rounded-xl transition shadow">
                  ⬇ Export Everything All-in-One (Excel)
                </button>
                <button onclick="exportEvaluationDetailedExcel()" class="bg-canvas-bg hover:bg-canvas-elevated text-emerald-400 border border-emerald-500/50 font-extrabold text-xs px-3.5 py-2 rounded-xl transition">
                  ⬇ Extra: Row-per-item (Points &amp; %)
                </button>
              </div>
            </div>

            <!-- INTERACTIVE MIND MAP VISUAL TREE -->
            <div class="bg-canvas-bg/90 border border-canvas-border rounded-2xl p-4 sm:p-6 space-y-4">
              <!-- Root Node -->
              <div class="flex flex-col items-center text-center">
                <div onclick="selectMindMapPillar('ALL')" class="cursor-pointer group inline-flex flex-col items-center px-6 py-3 rounded-2xl bg-canvas-panel border-2 border-canvas-gold shadow-lg hover:scale-[1.02] transition">
                  <span class="font-mono text-[10px] font-bold uppercase tracking-widest text-canvas-gold">ROOT NODE // CLICK FOR MASTER SUMMARY</span>
                  <div class="text-sm sm:text-base font-extrabold text-white mt-0.5">
                    🎬 Video Team Total Evaluation (100% Scale = 45% Official Share)
                  </div>
                  <div class="text-[11px] font-mono text-slate-400 mt-0.5">
                    Weights: Tasks 30% (Deadline 10 + Creativity 10 + Quality 10) + 5 pillars × 3% = 45% (+ 55% HR)
                  </div>
                </div>
                <!-- Vertical Stem Line -->
                <div class="w-0.5 h-5 bg-gradient-to-b from-canvas-gold to-canvas-blue"></div>
                <div class="w-4/5 h-0.5 bg-gradient-to-r from-transparent via-canvas-border to-transparent hidden lg:block"></div>
              </div>

              <!-- 6 Fixed Pillar Nodes Grid -->
              <div id="mindMapPillarsGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-6 gap-3">
                <!-- Populated dynamically by JS -->
              </div>
            </div>

            <!-- ACTIVE DRILL-DOWN WORKSPACE (Opens when any of the 6 Pillars is clicked, or shows Master Summary) -->
            <div id="pillarDrilldownToolbar" class="bg-canvas-bg border border-canvas-border rounded-xl p-3.5 flex flex-col md:flex-row md:items-center justify-between gap-3">
              <!-- Populated dynamically by JS -->
            </div>

            <!-- Dynamic Table (Either Active Pillar Sub-Columns Table OR Master 6-Pillars Summary Table) -->
            <div class="overflow-x-auto">
              <table class="w-full text-left border-collapse text-xs">
                <thead id="matrixTableHead"></thead>
                <tbody id="matrixTableBody" class="divide-y divide-canvas-border"></tbody>
              </table>
            </div>

          </div>
        </div>

        <!-- =============================================================
             BOARD SUB-TAB 3: AUTO RANKINGS DASHBOARD
             ============================================================= -->
        <div id="bpanel-rankings" class="hidden space-y-4">
          <div class="bg-canvas-panel border border-canvas-border rounded-2xl p-4 sm:p-5 space-y-5">
            <div class="flex flex-col md:flex-row md:items-center justify-between gap-3 border-b border-canvas-border pb-4">
              <div>
                <span class="font-mono text-[10px] font-bold uppercase px-2 py-0.5 rounded bg-canvas-gold/15 text-canvas-gold border border-canvas-gold/30">
                  AUTOMATIC LEADERBOARD &amp; CATEGORY RANKS
                </span>
                <h3 class="text-base sm:text-lg font-extrabold text-white mt-1">
                  Board Rankings Dashboard
                </h3>
              </div>

              <div class="flex flex-wrap items-center gap-2">
                <label class="text-xs font-mono text-slate-400">Sort By:</label>
                <select id="rankingSortSelector" onchange="renderBoardRankingsDashboard()" class="bg-canvas-bg border border-canvas-gold rounded-xl px-3 py-2 text-xs font-bold text-white focus:outline-none">
                </select>
                <button onclick="exportRankingsExcel()" class="bg-canvas-gold hover:bg-amber-400 text-canvas-bg font-extrabold text-xs px-3.5 py-2 rounded-xl transition shadow">
                  ⬇ Export Rankings (Excel)
                </button>
              </div>
            </div>

            <div id="rankingsTop3Podium" class="grid grid-cols-1 md:grid-cols-3 gap-4"></div>
            <div class="overflow-x-auto">
              <table class="w-full text-left border-collapse text-xs">
                <thead id="rankingsTableHead"></thead>
                <tbody id="rankingsTableBody" class="divide-y divide-canvas-border"></tbody>
              </table>
            </div>
          </div>
        </div>

        <!-- =============================================================
             BOARD SUB-TAB 4: UNLIMITED TASKS MANAGER
             ============================================================= -->
        <div id="bpanel-tasks" class="hidden space-y-4">
          <div class="bg-canvas-panel border border-canvas-border rounded-2xl p-4 sm:p-5 space-y-4">
            <div class="flex items-center justify-between gap-2 border-b border-canvas-border pb-3">
              <div>
                <span class="font-mono text-[10px] font-bold uppercase px-2 py-0.5 rounded bg-canvas-gold/15 text-canvas-gold border border-canvas-gold/30">TASK MANAGER</span>
                <h3 class="text-base sm:text-lg font-extrabold text-white mt-1">Create &amp; Edit Tasks</h3>
              </div>
              <span id="tasksTotalCountBadge" class="font-mono text-xs font-bold text-canvas-blue">0 Tasks</span>
            </div>

            <div class="bg-canvas-bg border border-canvas-gold/30 rounded-xl p-4 space-y-3">
              <div class="text-xs font-mono font-bold uppercase text-canvas-gold">➕ Add New Task</div>
              <div class="grid grid-cols-1 md:grid-cols-12 gap-3">
                <div class="md:col-span-4">
                  <label class="block text-[10px] font-mono text-slate-400 mb-1">Title *</label>
                  <input id="newTaskTitleInput" type="text" placeholder="Task 04: ..." class="w-full bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3 py-2 text-xs text-white focus:outline-none" />
                </div>
                <div class="md:col-span-4">
                  <label class="block text-[10px] font-mono text-slate-400 mb-1">Brief</label>
                  <input id="newTaskBriefInput" type="text" placeholder="What members should deliver" class="w-full bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3 py-2 text-xs text-white focus:outline-none" />
                </div>
                <div class="md:col-span-2">
                  <label class="block text-[10px] font-mono text-slate-400 mb-1">Score Out Of</label>
                  <input id="newTaskMaxInput" type="number" value="30" readonly title="Fixed: Deadline 10 + Creativity 10 + Quality 10" class="w-full bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3 py-2 text-xs font-mono text-white focus:outline-none" />
                </div>
                <div class="md:col-span-2">
                  <label class="block text-[10px] font-mono text-slate-400 mb-1">Deadline</label>
                  <input id="newTaskDeadlineInput" type="datetime-local" class="w-full bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-2.5 py-2 text-xs font-mono text-white focus:outline-none" />
                </div>
              </div>
              <div class="flex justify-end">
                <button onclick="createNewTaskItem()" class="bg-canvas-gold hover:bg-amber-400 text-canvas-bg font-extrabold text-xs px-5 py-2 rounded-xl transition">Create Task</button>
              </div>
            </div>

            <div id="boardTasksManagerList" class="space-y-3"></div>
          </div>
        </div>

        <!-- =============================================================
             BOARD SUB-TAB 5: TEAM ROSTER & PRIVATE SECRET CODES MANAGER
             ============================================================= -->
        <div id="bpanel-roster" class="hidden space-y-5">
          
          <div class="bg-canvas-panel border border-canvas-border rounded-2xl p-5 sm:p-6 space-y-4">
            <div class="flex flex-col lg:flex-row lg:items-center justify-between gap-3 border-b border-canvas-border pb-4">
              <div>
                <div class="flex items-center gap-2">
                  <span class="font-mono text-[10px] font-bold uppercase px-2 py-0.5 rounded bg-canvas-gold/15 text-canvas-gold border border-canvas-gold/30">
                    BOARD EYES ONLY // PRIVATE SECRET CODES
                  </span>
                  <span id="rosterTotalBadge" class="font-mono text-xs font-bold text-canvas-blue">10 Members</span>
                </div>
                <h3 class="text-base sm:text-lg font-extrabold text-white mt-1">
                  Video Team Roster &amp; Private Member Secret Codes
                </h3>
                <p class="text-xs text-slate-400">
                  Members submit tasks by choosing their <strong>Name only</strong>. Send each member their private <strong>Secret Code</strong> below so only they can check their own grades.
                </p>
              </div>

              <div class="flex flex-wrap items-center gap-2">
                <button onclick="regenerateAllRandomSecretCodes()" class="bg-canvas-bg hover:bg-canvas-elevated text-canvas-gold border border-canvas-gold/40 font-mono text-xs font-bold px-3.5 py-2 rounded-xl transition">
                  🎲 Randomize All Codes
                </button>
                <button onclick="copyAllMemberCodesList()" class="bg-canvas-blue hover:bg-sky-300 text-canvas-bg font-extrabold text-xs px-3.5 py-2 rounded-xl transition shadow">
                  📋 Copy All Codes List
                </button>
                <button onclick="exportRosterCodesExcel()" class="bg-emerald-500 hover:bg-emerald-400 text-canvas-bg font-extrabold text-xs px-3.5 py-2 rounded-xl transition shadow">
                  ⬇ Export Roster &amp; Codes (Excel)
                </button>
              </div>
            </div>

            <!-- ADD MEMBER FORM -->
            <div class="bg-canvas-bg border border-canvas-gold/30 rounded-xl p-4 space-y-3">
              <div class="text-xs font-mono font-bold uppercase text-canvas-gold">➕ Add Member</div>
              <div class="grid grid-cols-1 sm:grid-cols-12 gap-2">
                <input id="rosterNewName" type="text" placeholder="Member full name" onkeydown="if(event.key==='Enter') addNewMemberToRoster()" class="sm:col-span-6 bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3.5 py-2.5 text-sm text-white focus:outline-none" />
                <input id="rosterNewCode" type="text" placeholder="Secret code (auto)" class="sm:col-span-3 bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3.5 py-2.5 text-sm font-mono font-bold text-canvas-gold uppercase focus:outline-none" />
                <button onclick="addNewMemberToRoster()" class="sm:col-span-3 bg-canvas-gold hover:bg-amber-400 text-canvas-bg font-extrabold text-sm py-2.5 rounded-xl transition">Add Member</button>
              </div>
              <details class="text-xs text-slate-300">
                <summary class="cursor-pointer font-mono text-canvas-blue">Add many at once (one name per line)</summary>
                <textarea id="rosterBulkNames" rows="5" placeholder="Ahmed Ali&#10;Sara Mohamed&#10;..." class="mt-2 w-full bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded-xl px-3.5 py-2.5 text-sm text-white focus:outline-none"></textarea>
                <button onclick="addBulkMembersToRoster()" class="mt-2 bg-canvas-blue hover:bg-sky-300 text-canvas-bg font-extrabold text-xs px-4 py-2 rounded-xl transition">Add All Names</button>
              </details>
            </div>

            <!-- Roster Table with Inline Editable Secret Codes & Copy Individual WhatsApp Message -->
            <div class="overflow-x-auto">
              <table class="w-full text-left border-collapse text-xs">
                <thead>
                  <tr class="bg-canvas-elevated text-slate-300 border-b border-canvas-border">
                    <th class="py-3 px-3 font-mono">#</th>
                    <th class="py-3 px-3">Member Name (Shown in Submit List)</th>
                    <th class="py-3 px-3">Team</th>
                    <th class="py-3 px-3 font-mono text-canvas-gold">Private Secret Code (Editable)</th>
                    <th class="py-3 px-3 text-right">Send Code / Actions</th>
                  </tr>
                </thead>
                <tbody id="rosterTableBody" class="divide-y divide-canvas-border">
                  <!-- Populated by JS -->
                </tbody>
              </table>
            </div>
          </div>

        </div>

        <!-- =============================================================
             BOARD SUB-TAB 6: SECURITY (PASSWORD CHANGE) & GOOGLE SHEETS SYNC
             ============================================================= -->
        <div id="bpanel-security" class="hidden space-y-5">
          
          <div class="grid grid-cols-1 lg:grid-cols-12 gap-5">
            
            <!-- Left 5 Cols: Change Board Password -->
            <div class="lg:col-span-5 bg-canvas-panel border border-canvas-border rounded-2xl p-5 space-y-4">
              <div class="border-b border-canvas-border pb-3">
                <span class="font-mono text-[10px] font-bold uppercase px-2 py-0.5 rounded bg-canvas-gold/15 text-canvas-gold border border-canvas-gold/30">
                  ACCESS CONTROL
                </span>
                <h3 class="text-base font-extrabold text-white mt-1">🔑 Change Board Portal Password</h3>
                <p class="text-xs text-slate-400">Enter the current password to set your new Board password.</p>
              </div>

              <div class="space-y-3">
                <div>
                  <label class="block text-[11px] font-mono text-slate-400 mb-1">Current Password *</label>
                  <input id="oldPassInput" type="password" placeholder="Enter current password..." class="w-full bg-canvas-bg border border-canvas-border focus:border-canvas-gold rounded-xl px-3.5 py-2.5 text-xs font-mono text-white focus:outline-none" />
                </div>
                <div>
                  <label class="block text-[11px] font-mono text-slate-400 mb-1">New Password *</label>
                  <input id="newPassInput" type="password" placeholder="Enter new password..." class="w-full bg-canvas-bg border border-canvas-border focus:border-canvas-gold rounded-xl px-3.5 py-2.5 text-xs font-mono text-white focus:outline-none" />
                </div>
                <div>
                  <label class="block text-[11px] font-mono text-slate-400 mb-1">Confirm New Password *</label>
                  <input id="confirmPassInput" type="password" placeholder="Confirm new password..." class="w-full bg-canvas-bg border border-canvas-border focus:border-canvas-gold rounded-xl px-3.5 py-2.5 text-xs font-mono text-white focus:outline-none" />
                </div>

                <button onclick="updateBoardPassword()" class="w-full bg-canvas-gold hover:bg-amber-400 text-canvas-bg font-extrabold text-xs py-3 rounded-xl transition shadow">
                  Update Board Password
                </button>
              </div>
            </div>

            <!-- Right 7 Cols: Google Sheet status -->
            <div class="lg:col-span-7 bg-canvas-panel border border-canvas-border rounded-2xl p-5 space-y-3">
              <div class="border-b border-canvas-border pb-3">
                <span class="font-mono text-[10px] font-bold uppercase px-2 py-0.5 rounded bg-canvas-blue/15 text-canvas-blue border border-canvas-blue/30">
                  GOOGLE SHEET DATABASE
                </span>
                <h3 class="text-base font-extrabold text-white mt-1">☁️ Data is stored in your Google Sheet</h3>
                <p class="text-xs text-slate-400">Every submission and every Board edit is saved automatically. Open the Sheet to see tabs: <code class="text-canvas-gold font-mono">Submissions</code>, <code class="text-canvas-gold font-mono">Members</code>, <code class="text-canvas-gold font-mono">Tasks</code>, <code class="text-canvas-gold font-mono">Pillar Scores</code>. To get an Excel file: Google Sheet → File → Download → .xlsx.</p>
              </div>
              <div class="space-y-2">
                <label class="block text-[11px] font-mono text-slate-400">Web App URL (current: <span id="currentApiLabel" class="text-canvas-blue"></span>)</label>
                <div class="flex flex-wrap gap-2">
                  <input id="settingsUrlInput" type="url" placeholder="https://script.google.com/macros/s/.../exec" class="flex-1 min-w-[200px] bg-canvas-bg border border-canvas-border focus:border-canvas-blue rounded-xl px-3.5 py-2 text-xs font-mono text-white focus:outline-none" />
                  <button onclick="connectBackend('settingsUrlInput')" class="bg-canvas-blue hover:bg-sky-300 text-canvas-bg font-extrabold text-xs px-4 py-2 rounded-xl transition">Save &amp; Reconnect</button>
                </div>
                <div class="flex flex-wrap gap-2">
                  <button onclick="copyShareLink()" class="bg-canvas-gold hover:bg-amber-400 text-canvas-bg font-extrabold text-xs px-4 py-2 rounded-xl transition">🔗 Copy Member Share Link</button>
                  <button onclick="disconnectBackend()" class="text-red-400 hover:bg-red-500/10 font-mono text-xs font-bold px-3 py-2 rounded-xl transition">Disconnect</button>
                </div>
                <p class="text-[11px] text-slate-500">The share link contains the backend URL, so members just open it once — no code editing needed.</p>
              </div>
              <p class="text-xs text-slate-300">Board password and member secret codes are checked on the server — they are never sent to visitors.</p>
              <p class="text-xs text-slate-400">Use <strong class="text-canvas-blue">🔄 Sync</strong> at the top to pull submissions that arrived while you were on this page.</p>
            </div>

          </div>

        </div>

      </div>

    </section>

  </main>

  <!-- ================= BOTTOM NLE TIMELINE NAVIGATION BAR (Track V1) ================= -->
  <footer class="fixed bottom-0 left-0 right-0 z-40 bg-canvas-bg/95 backdrop-blur-md border-t border-canvas-border">
    <div class="max-w-7xl mx-auto px-3 sm:px-6 h-16 flex items-center justify-between gap-2">
      
      <!-- Left Track Label -->
      <div class="hidden md:flex items-center gap-2 font-mono text-[11px] text-slate-400 shrink-0">
        <span class="px-2 py-1 rounded bg-canvas-panel border border-canvas-border text-canvas-gold font-bold">V1</span>
        <span>NLE TIMELINE</span>
      </div>

      <!-- Timeline Clips Navigation -->
      <div class="flex-1 grid grid-cols-2 sm:grid-cols-5 gap-2 max-w-5xl mx-auto">
        <button id="tclip-submit" onclick="switchScene('submit')" class="px-3 py-2 rounded-xl font-mono text-xs font-bold border transition flex items-center justify-center gap-2 bg-canvas-gold text-canvas-bg border-canvas-gold shadow">
          <span>🎬 01_SUBMIT_TASK.mp4</span>
        </button>
        <button id="tclip-grade" onclick="switchScene('grade')" class="px-3 py-2 rounded-xl font-mono text-xs font-bold border transition flex items-center justify-center gap-2 bg-canvas-panel text-slate-300 border-canvas-border hover:border-canvas-gold/50">
          <span>🎯 02_TASK_GRADE.mp4</span>
        </button>
        <button id="tclip-sessions" onclick="switchScene('sessions')" class="px-3 py-2 rounded-xl font-mono text-xs font-bold border transition flex items-center justify-center gap-2 bg-canvas-panel text-slate-300 border-canvas-border hover:border-canvas-gold/50">
          <span>📅 SESSIONS_SCHEDULE.cal</span>
        </button>
        <button id="tclip-memories" onclick="switchScene('memories')" class="px-3 py-2 rounded-xl font-mono text-xs font-bold border transition flex items-center justify-center gap-2 bg-canvas-panel text-slate-300 border-canvas-border hover:border-canvas-gold/50">
          <span>📸 TEAM_MEMORIES.jpg</span>
        </button>
        <button id="tclip-board" onclick="switchScene('board')" class="px-3 py-2 rounded-xl font-mono text-xs font-bold border transition flex items-center justify-center gap-2 bg-canvas-panel text-slate-300 border-canvas-border hover:border-canvas-gold/50">
          <span>🔒 03_BOARD_PORTAL.lock</span>
        </button>
      </div>

      <!-- Right Board Credit -->
      <div class="hidden lg:block font-mono text-[11px] text-slate-400 shrink-0">
        BOARD: <span class="text-white font-semibold">OMAR SHALABY</span> &amp; <span class="text-white font-semibold">ALI AFIFY</span>
      </div>

    </div>
  </footer>

  <!-- ================= APPLICATION JAVASCRIPT ================= -->
  <script>
    // -----------------------------------------------------------------
    // 1. FIXED 6 CORE PILLARS & INITIAL SAMPLE DATA
    //    Video Evaluation = 45% of Enactus Total (HR = 55%):
    //      Tasks 30% (Deadline 10% + Creativity 10% + Quality 10%) + 5 pillars x 3% = 15%  ->  45%
    //    weight100 below = the pillar's weight as "% of the 45%" (so 30 + 3x5 = 45).
    // -----------------------------------------------------------------
    const VIDEO_OFFICIAL_SHARE = 45; // 45% Video Team Share (HR = 55%)

    const FIXED_PILLARS = [
      {
        id: "PIL-TASKS",
        title: "TASK QUALITY AND RELIABILITY",
        shortTitle: "TASK QUALITY & RELIABILITY",
        weight100: 30, // 30% of the 45%
        icon: "🎬",
        accent: "gold",
        isTasksPillar: true
      },
      {
        id: "PIL-ALIGN",
        title: "ALIGNMENT TEAM GOALS AND IMPACT",
        shortTitle: "ALIGNMENT & IMPACT",
        weight100: 3, // 3% of the 45%
        icon: "🎯",
        accent: "blue",
        isTasksPillar: false
      },
      {
        id: "PIL-PROB",
        title: "PROBLEM SOLVING",
        shortTitle: "PROBLEM SOLVING",
        weight100: 3, // 3% of the 45%
        icon: "🧩",
        accent: "emerald",
        isTasksPillar: false
      },
      {
        id: "PIL-COLLAB",
        title: "COLLABORATION",
        shortTitle: "COLLABORATION",
        weight100: 3, // 3% of the 45%
        icon: "🤝",
        accent: "gold",
        isTasksPillar: false
      },
      {
        id: "PIL-GROWTH",
        title: "GROWTH MINDSET",
        shortTitle: "GROWTH MINDSET",
        weight100: 3, // 3% of the 45%
        icon: "🚀",
        accent: "blue",
        isTasksPillar: false
      },
      {
        id: "PIL-LEAD",
        title: "LEADERSHIP",
        shortTitle: "LEADERSHIP",
        weight100: 3, // 3% of the 45%
        icon: "👑",
        accent: "emerald",
        isTasksPillar: false
      }
    ];

    // Every task is graded on 3 criteria. Together they make the 30% Tasks pillar (10% each).
    const TASK_CRITERIA = [
      { id: "deadline", title: "Deadline", max: 10, weight: 10 },
      { id: "creativity", title: "Creativity", max: 10, weight: 10 },
      { id: "quality", title: "Quality", max: 10, weight: 10 }
    ];
    const TASK_MAX_TOTAL = TASK_CRITERIA.reduce((a, c) => a + c.max, 0); // 30 points per task

    const initialTasks = [
      {
        id: "TASK-01",
        title: "Task 01: File Organization & Rough Cut",
        brief: "Organize your project folders (Footage, Audio, SFX) and assemble a clean 45-second rough cut on the timeline.",
        maxScore: 30,
        deadlineISO: "2026-11-22T23:59",
        forceLocked: false
      },
      {
        id: "TASK-02",
        title: "Task 02: Cinematography & Composition Frames",
        brief: "Film 5 purposeful shots applying Focus, Rule of Thirds, Low/High Angles, and 45° Key Lighting.",
        maxScore: 30,
        deadlineISO: "2026-11-29T23:59",
        forceLocked: false
      },
      {
        id: "TASK-03",
        title: "Task 03: Reels Pacing & Sound Design",
        brief: "Edit a fast-paced Enactus Reel with a strong 3-second hook, B-Roll coverage, and balanced SFX.",
        maxScore: 30,
        deadlineISO: "2026-12-06T23:59",
        forceLocked: false
      }
    ];

    // Sub-columns inside each of the 5 non-task Pillars (Board can rename, change max score, delete, or add unlimited sub-columns)
    const initialPillarSubColumns = {
      "PIL-ALIGN": [
        { id: "SUB-ALIGN-01", title: "Brand & Goal Alignment", maxScore: 100 },
        { id: "SUB-ALIGN-02", title: "Impact Storytelling", maxScore: 100 }
      ],
      "PIL-PROB": [
        { id: "SUB-PROB-01", title: "Situation / Crisis Fix", maxScore: 100 },
        { id: "SUB-PROB-02", title: "Technical Troubleshooting", maxScore: 100 }
      ],
      "PIL-COLLAB": [
        { id: "SUB-COLLAB-01", title: "Team Spirit & Support", maxScore: 100 },
        { id: "SUB-COLLAB-02", title: "Media Sector Synergy", maxScore: 100 }
      ],
      "PIL-GROWTH": [
        { id: "SUB-GROWTH-01", title: "Applying Board Feedback", maxScore: 100 },
        { id: "SUB-GROWTH-02", title: "Skill Improvement", maxScore: 100 }
      ],
      "PIL-LEAD": [
        { id: "SUB-LEAD-01", title: "Initiative & Ownership", maxScore: 100 },
        { id: "SUB-LEAD-02", title: "On-Ground Responsibility", maxScore: 100 }
      ]
    };

    // Unified Video Team Roster (Empty by default so the Board adds members freely)
    const initialMembers = [];

    // Task Submissions & Task Grades (Empty by default)
    const initialSubmissions = [];

    // Member scores for sub-columns inside the 5 non-task Pillars: { [memberCode]: { [subColId]: number } }
    const initialPillarScores = {};

    // Runtime State
    let tasks = [];
    let pillarSubColumns = {};
    let members = [];
    let submissions = [];
    let sessions = [];            // مواعيد السيشنز: {id,title,dateISO,driveLink,note}
    let editingSessionId = null;
    let memories = [];            // ألبومات الذكريات: {id,title,dateISO,driveLink,coverLink,note}
    let editingMemoryId = null;
    let pillarScores = {};
    let activeMindMapPillarId = "PIL-TASKS"; // Can be 'ALL' or one of the 6 FIXED_PILLARS IDs
    let activeSubmitTaskId = "TASK-01";
    let activeBoardGradeTaskId = "TASK-01";
    let testDeadlineLockActive = false;
    let boardUnlocked = false;
    let boardPassword = "2026";

    const STORAGE_KEY = "enactus_director_canvas_portal_v8";

    // -----------------------------------------------------------------
    // 2. INITIALIZATION & STORAGE
    // -----------------------------------------------------------------
    // ===== BACKEND CONFIG =====
    // الصق هنا رابط الـ Web App بتاع Google Apps Script (بينتهي بـ /exec).
    // لو سبته فاضي، الموقع بيشتغل محلياً على المتصفح بس (localStorage) للتجربة.
    const BAKED_API_URL = "https://script.google.com/macros/s/AKfycbzIO8D1BMn7raIvpXtuT1VyevFBpydc6FU8OYnH6AQSaBPxa0_ZAFPybqVhMKk7uXeY/exec"; // اختياري: لو لصقت الرابط هنا مش هتحتاج ?api= ولا الإعداد من الصفحة
    let API_URL = BAKED_API_URL;
    try {
      const q = new URLSearchParams(location.search).get("api");
      if (q && /^https:\/\/script\.google\.com\//.test(q)) localStorage.setItem("portal_api_url", q);
      if (!API_URL) API_URL = localStorage.getItem("portal_api_url") || "";
    } catch (e) {}

    function connectBackend(inputId) {
      const v = (document.getElementById(inputId)?.value || "").trim();
      if (!/^https:\/\/script\.google\.com\/.+\/exec$/.test(v)) {
        showToast("Paste the Web App URL (ends with /exec)", "!");
        return;
      }
      try { localStorage.setItem("portal_api_url", v); } catch (e) {}
      location.href = location.pathname + "?api=" + encodeURIComponent(v);
    }

    function disconnectBackend() {
      try { localStorage.removeItem("portal_api_url"); } catch (e) {}
      location.href = location.pathname;
    }

    function copyShareLink() {
      if (!API_URL) { showToast("Connect a Google Sheet first", "!"); return; }
      copyTextSafe(location.origin + location.pathname + "?api=" + encodeURIComponent(API_URL), "Share link copied — send it to members");
    }

    let boardPasswordSession = ""; // الباسورد بيفضل في الذاكرة بس بعد الفتح
    let syncTimer = null, syncInFlight = false, syncDirty = false;

    async function api(payload) {
      const res = await fetch(API_URL, {
        method: "POST",
        headers: { "Content-Type": "text/plain;charset=utf-8" }, // بدون preflight
        body: JSON.stringify(payload)
      });
      return await res.json();
    }

    function setSyncStatus(kind, text) {
      const el = document.getElementById("syncStatus");
      if (!el) return;
      const cls = {
        ok: "bg-emerald-500/15 text-emerald-400 border-emerald-500/30",
        busy: "bg-amber-500/15 text-amber-300 border-amber-500/30",
        err: "bg-red-500/15 text-red-400 border-red-500/40",
        off: "bg-canvas-panel text-slate-400 border-canvas-border"
      }[kind];
      el.className = "font-mono text-[11px] px-2.5 py-1.5 rounded-lg border " + cls;
      el.textContent = text;
    }

    function applyDefaultsAfterLoad() {
      if (!tasks.length) tasks = JSON.parse(JSON.stringify(initialTasks));
      members.forEach((m, idx) => { if (!m.id) m.id = `MEM-${String(idx + 1).padStart(2, "0")}`; });
      tasks.forEach((t) => { t.maxScore = TASK_MAX_TOTAL; });
      FIXED_PILLARS.forEach((p) => {
        if (!p.isTasksPillar && !Array.isArray(pillarSubColumns[p.id])) {
          pillarSubColumns[p.id] = JSON.parse(JSON.stringify(initialPillarSubColumns[p.id] || []));
        }
      });
      if (!tasks.some((t) => t.id === activeSubmitTaskId)) activeSubmitTaskId = tasks[0]?.id || "TASK-01";
      if (!tasks.some((t) => t.id === activeBoardGradeTaskId)) activeBoardGradeTaskId = tasks[0]?.id || "TASK-01";
    }

    async function initApp() {
      resetToDefaults();
      if (API_URL) {
        setSyncStatus("busy", "⏳ Connecting…");
        try {
          const r = await api({ action: "load" });
          if (!r.ok) throw new Error(r.error);
          const st = r.state;
          tasks = st.tasks || [];
          members = st.members || [];
          submissions = st.submissions || [];
          sessions = st.sessions || [];
          memories = st.memories || [];
          pillarSubColumns = {};
          pillarScores = {};
          setSyncStatus("ok", "☁ Online");
        } catch (e) {
          setSyncStatus("err", "⚠ Server unreachable");
          const why = e instanceof SyntaxError ? "Server returned a login/HTML page — set Who has access = Anyone, then redeploy"
            : (e && e.message && e.message !== "Failed to fetch" ? "Server error: " + e.message : "Request blocked — check the /exec URL and that the deployment is public");
          showToast(why, "!");
          console.error("Backend load failed:", e);
        }
      } else {
        try {
          const raw = localStorage.getItem(STORAGE_KEY);
          if (raw) {
            const p = JSON.parse(raw);
            tasks = p.tasks || []; pillarSubColumns = p.pillarSubColumns || {}; members = p.members || [];
            submissions = p.submissions || []; pillarScores = p.pillarScores || {}; sessions = p.sessions || []; memories = p.memories || [];
            boardPassword = p.boardPassword || "2026";
          }
        } catch (e) {}
        setSyncStatus("off", "💾 Local mode");
      }
      applyDefaultsAfterLoad();
      document.getElementById("setupBanner")?.classList.toggle("hidden", !!API_URL);
      const lbl = document.getElementById("currentApiLabel");
      if (lbl) lbl.textContent = API_URL ? API_URL.slice(0, 48) + "…" : "not connected";
      fillRandomNewMemberCode();
      refreshAllSelectorsAndViews();
      tickDeadlineClock();
      setInterval(tickDeadlineClock, 1000);
    }

    function resetToDefaults() {
      tasks = JSON.parse(JSON.stringify(initialTasks));
      pillarSubColumns = JSON.parse(JSON.stringify(initialPillarSubColumns));
      members = JSON.parse(JSON.stringify(initialMembers));
      submissions = JSON.parse(JSON.stringify(initialSubmissions));
      sessions = [];
      memories = [];
      pillarScores = JSON.parse(JSON.stringify(initialPillarScores));
      boardPassword = "2026";
    }

    // saveState بيتنادى من كل تعديل بيعمله البورد. بيتجمّع ويتبعت للشيت بعد ثانية.
    function saveState() {
      if (!API_URL) {
        try {
          localStorage.setItem(STORAGE_KEY, JSON.stringify({ tasks, pillarSubColumns, members, submissions, pillarScores, sessions, memories, boardPassword }));
        } catch (e) {}
        return;
      }
      if (!boardUnlocked || !boardPasswordSession) return; // الزوار مبيكتبوش غير عن طريق submit
      syncDirty = true;
      setSyncStatus("busy", "⏳ Saving…");
      clearTimeout(syncTimer);
      syncTimer = setTimeout(flushToServer, 900);
    }

    async function flushToServer() {
      if (syncInFlight) { syncTimer = setTimeout(flushToServer, 500); return; }
      if (!syncDirty) return;
      syncInFlight = true; syncDirty = false;
      try {
        const r = await api({
          action: "boardSave",
          password: boardPasswordSession,
          state: { tasks, pillarSubColumns, members, submissions, pillarScores, sessions, memories }
        });
        if (!r.ok) throw new Error(r.error);
        // نعتمد التسليمات المدموجة (ممكن أعضاء سلّموا في الوقت ده) بدون إعادة رسم
        if (Array.isArray(r.submissions) && !syncDirty) submissions = r.submissions;
        setSyncStatus("ok", "☁ Saved " + new Date().toLocaleTimeString([], { hour: "2-digit", minute: "2-digit" }));
      } catch (e) {
        syncDirty = true;
        setSyncStatus("err", "⚠ Save failed — retrying");
        syncTimer = setTimeout(flushToServer, 4000);
      } finally {
        syncInFlight = false;
      }
    }

    async function refreshFromServer(silent) {
      if (!API_URL || !boardPasswordSession) return;
      if (syncDirty || syncInFlight) await flushToServer();
      const r = await api({ action: "boardLoad", password: boardPasswordSession });
      if (!r.ok) { showToast("Refresh failed", "✕"); return; }
      const st = r.state;
      tasks = st.tasks?.length ? st.tasks : tasks;
      members = st.members || [];
      submissions = st.submissions || [];
          sessions = st.sessions || [];
          memories = st.memories || [];
      pillarScores = st.pillarScores || {};
      if (st.pillarSubColumns && Object.keys(st.pillarSubColumns).length) pillarSubColumns = st.pillarSubColumns;
      applyDefaultsAfterLoad();
      refreshAllSelectorsAndViews();
      renderBoardAllPanels();
      setSyncStatus("ok", "☁ Synced");
      if (!silent) showToast("Synced from Google Sheet", "☁");
    }

    // -----------------------------------------------------------------
    // 3. UI HELPERS, SCENE NAVIGATION & DEADLINE COUNTDOWN
    // -----------------------------------------------------------------
    function showToast(msg, icon = "✓") {
      const box = document.getElementById("toastBox");
      const iconEl = document.getElementById("toastIcon");
      const msgEl = document.getElementById("toastMsg");
      if (!box || !msgEl) return;
      iconEl.textContent = icon;
      msgEl.textContent = msg;
      box.classList.remove("hidden");
      clearTimeout(window.__toastTimer);
      window.__toastTimer = setTimeout(() => {
        box.classList.add("hidden");
      }, 3200);
    }

    function copyTextSafe(text, successMessage = "Copied to clipboard") {
      const ta = document.createElement("textarea");
      ta.value = text;
      document.body.appendChild(ta);
      ta.select();
      try {
        document.execCommand("copy");
        showToast(successMessage, "📋");
      } catch (e) {
        showToast("Copy failed", "!");
      }
      document.body.removeChild(ta);
    }

    // ===== SESSIONS (read by everyone, written only by the Board after the password) =====
    function escSession(t) {
      return String(t ?? "").replace(/[&<>"']/g, (c) => ({ "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#39;" }[c]));
    }

    function isDriveLink(v) {
      return /^https?:\/\/(drive\.google\.com|docs\.google\.com)\/\S+$/i.test(v);
    }

    function localDateFromISO(iso) {
      const [y, m, d] = String(iso).split("-").map(Number);
      return new Date(y, (m || 1) - 1, d || 1);
    }

    function renderSessions() {
      const list = document.getElementById("sessionsList");
      if (!list) return;
      document.getElementById("sessionsAdminBox")?.classList.toggle("hidden", !boardUnlocked);
      document.getElementById("sessionsBoardHint")?.classList.toggle("hidden", boardUnlocked);

      const sorted = [...sessions].sort((a, b) => String(a.dateISO).localeCompare(String(b.dateISO)));
      if (!sorted.length) {
        list.innerHTML = `<div class="text-center text-xs font-mono text-slate-400 py-8 border border-dashed border-canvas-border rounded-xl">No sessions have been scheduled yet.</div>`;
        return;
      }
      const t = new Date();
      const todayISO = `${t.getFullYear()}-${String(t.getMonth() + 1).padStart(2, "0")}-${String(t.getDate()).padStart(2, "0")}`;

      list.innerHTML = sorted
        .map((x) => {
          const d = localDateFromISO(x.dateISO);
          const dayEn = d.toLocaleDateString("en-GB", { weekday: "long" });
          const dayAr = d.toLocaleDateString("ar-EG", { weekday: "long" });
          const dateTxt = d.toLocaleDateString("en-GB", { day: "numeric", month: "long", year: "numeric" });
          const state = x.dateISO === todayISO ? "today" : x.dateISO > todayISO ? "upcoming" : "past";
          const badge = {
            today: `<span class="font-mono text-[10px] font-bold px-2 py-0.5 rounded bg-emerald-500/20 text-emerald-400 border border-emerald-500/40">TODAY</span>`,
            upcoming: `<span class="font-mono text-[10px] font-bold px-2 py-0.5 rounded bg-canvas-blue/15 text-canvas-blue border border-canvas-blue/30">UPCOMING</span>`,
            past: `<span class="font-mono text-[10px] font-bold px-2 py-0.5 rounded bg-slate-500/15 text-slate-400 border border-slate-500/30">DONE</span>`
          }[state];
          const link = x.driveLink && isDriveLink(x.driveLink)
            ? `<a href="${escSession(x.driveLink)}" target="_blank" rel="noopener noreferrer" class="inline-flex items-center gap-1.5 bg-canvas-blue/15 hover:bg-canvas-blue hover:text-canvas-bg text-canvas-blue border border-canvas-blue/40 font-mono text-xs font-bold px-3.5 py-2 rounded-xl transition">📂 Open Drive Data</a>`
            : `<span class="font-mono text-[11px] text-slate-500">Drive link not added yet</span>`;
          const admin = boardUnlocked
            ? `<div class="flex gap-1.5">
                 <button onclick="editSession('${escSession(x.id)}')" class="text-canvas-gold hover:bg-canvas-gold/10 font-mono text-[11px] font-bold px-2.5 py-1.5 rounded-lg transition">✎ Edit</button>
                 <button onclick="deleteSession('${escSession(x.id)}')" class="text-red-400 hover:bg-red-500/10 font-mono text-[11px] font-bold px-2.5 py-1.5 rounded-lg transition">✕ Delete</button>
               </div>`
            : "";
          return `
            <div class="bg-canvas-bg border ${state === "today" ? "border-emerald-500/50" : "border-canvas-border"} rounded-xl p-4 flex flex-col md:flex-row md:items-center gap-3 md:gap-5">
              <div class="shrink-0 md:w-48">
                <div class="font-mono text-[10px] text-slate-500 uppercase">${escSession(dayEn)} · ${escSession(dayAr)}</div>
                <div class="text-sm font-extrabold text-canvas-gold">${escSession(dateTxt)}</div>
              </div>
              <div class="flex-1 min-w-0">
                <div class="flex flex-wrap items-center gap-2">
                  <span class="text-sm font-bold text-white">${escSession(x.title)}</span>${badge}
                </div>
                ${x.note ? `<div class="text-xs text-slate-400 mt-1">${escSession(x.note)}</div>` : ""}
              </div>
              <div class="flex flex-wrap items-center gap-2 md:justify-end">${link}${admin}</div>
            </div>`;
        })
        .join("");
    }

    function resetSessionForm() {
      editingSessionId = null;
      ["sessionTitleInput", "sessionDateInput", "sessionLinkInput", "sessionNoteInput"].forEach((id) => {
        const el = document.getElementById(id);
        if (el) el.value = "";
      });
      document.getElementById("sessionFormTitle").textContent = "➕ Add Session (Board only)";
      document.getElementById("sessionSaveBtn").textContent = "Add Session";
      document.getElementById("sessionCancelBtn").classList.add("hidden");
    }

    function cancelEditSession() {
      resetSessionForm();
    }

    function editSession(id) {
      if (!boardUnlocked) return;
      const x = sessions.find((s) => s.id === id);
      if (!x) return;
      editingSessionId = id;
      document.getElementById("sessionTitleInput").value = x.title || "";
      document.getElementById("sessionDateInput").value = x.dateISO || "";
      document.getElementById("sessionLinkInput").value = x.driveLink || "";
      document.getElementById("sessionNoteInput").value = x.note || "";
      document.getElementById("sessionFormTitle").textContent = "✎ Edit Session";
      document.getElementById("sessionSaveBtn").textContent = "Save Changes";
      document.getElementById("sessionCancelBtn").classList.remove("hidden");
      document.getElementById("sessionsAdminBox").scrollIntoView({ behavior: "smooth", block: "center" });
    }

    function saveSessionFromForm() {
      if (!boardUnlocked) { showToast("Board password required", "✕"); return; }
      const title = document.getElementById("sessionTitleInput").value.trim();
      const dateISO = document.getElementById("sessionDateInput").value;
      const driveLink = document.getElementById("sessionLinkInput").value.trim();
      const note = document.getElementById("sessionNoteInput").value.trim();

      if (!title) { showToast("Session name is required", "!"); return; }
      if (!/^\d{4}-\d{2}-\d{2}$/.test(dateISO)) { showToast("Pick the session date", "!"); return; }
      if (driveLink && !isDriveLink(driveLink)) { showToast("Use a Google Drive / Docs link", "!"); return; }

      if (editingSessionId) {
        const x = sessions.find((s) => s.id === editingSessionId);
        if (x) Object.assign(x, { title, dateISO, driveLink, note });
      } else {
        sessions.push({ id: "SES-" + Date.now().toString(36), title, dateISO, driveLink, note });
      }
      const wasEditing = !!editingSessionId;
      resetSessionForm();
      saveState();
      renderSessions();
      showToast(wasEditing ? "Session updated" : "Session added", "📅");
    }

    function deleteSession(id) {
      if (!boardUnlocked) return;
      const x = sessions.find((s) => s.id === id);
      if (!x || !confirm(`Delete "${x.title}"?`)) return;
      sessions = sessions.filter((s) => s.id !== id);
      saveState();
      renderSessions();
      showToast("Session deleted", "🗑");
    }

    // ===== TEAM MEMORIES (read by everyone, written only by the Board after the password) =====
    function driveThumb(link, size) {
      // Only single-file links can be shown as an image (folders cannot)
      const m = String(link || "").match(/\/file\/d\/([\w-]+)/) || String(link || "").match(/[?&]id=([\w-]+)/);
      return m ? `https://drive.google.com/thumbnail?id=${m[1]}&sz=w${size || 800}` : "";
    }

    function memoryPhotos(x) {
      if (Array.isArray(x.photos)) return x.photos;
      return x.coverLink ? [x.coverLink] : []; // old albums had a single cover photo
    }

    function renderMemories() {
      const list = document.getElementById("memoriesList");
      if (!list) return;
      document.getElementById("memoriesAdminBox")?.classList.toggle("hidden", !boardUnlocked);
      document.getElementById("memoriesBoardHint")?.classList.toggle("hidden", boardUnlocked);

      const sorted = [...memories].sort((a, b) => String(b.dateISO || "").localeCompare(String(a.dateISO || "")));
      if (!sorted.length) {
        list.innerHTML = `<div class="text-center text-xs font-mono text-slate-400 py-8 border border-dashed border-canvas-border rounded-xl">No albums yet.</div>`;
        return;
      }
      list.innerHTML = sorted
        .map((x) => {
          const dateTxt = x.dateISO
            ? localDateFromISO(x.dateISO).toLocaleDateString("en-GB", { weekday: "long", day: "numeric", month: "long", year: "numeric" })
            : "";
          const photos = memoryPhotos(x).filter((u) => driveThumb(u));
          const gallery = photos.length
            ? `<div class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 gap-2.5">
                ${photos
                  .map(
                    (u, i) => `
                  <button onclick="openPhoto('${escSession(x.id)}', ${i})" class="relative aspect-square rounded-xl overflow-hidden bg-canvas-elevated border border-canvas-border hover:border-canvas-gold transition flex items-center justify-center text-2xl">
                    <span>📷</span>
                    <img src="${escSession(driveThumb(u, 600))}" alt="" loading="lazy" referrerpolicy="no-referrer" class="absolute inset-0 w-full h-full object-cover" onerror="this.remove()" />
                  </button>`
                  )
                  .join("")}
              </div>`
            : "";
          const open = x.driveLink && isDriveLink(x.driveLink)
            ? `<a href="${escSession(x.driveLink)}" target="_blank" rel="noopener noreferrer" class="inline-flex items-center gap-1.5 bg-canvas-blue/15 hover:bg-canvas-blue hover:text-canvas-bg text-canvas-blue border border-canvas-blue/40 font-mono text-xs font-bold px-3.5 py-2 rounded-xl transition">📂 Open Full Album</a>`
            : "";
          const admin = boardUnlocked
            ? `<div class="flex gap-1.5">
                 <button onclick="editMemory('${escSession(x.id)}')" class="text-canvas-gold hover:bg-canvas-gold/10 font-mono text-[11px] font-bold px-2.5 py-1.5 rounded-lg transition">✎ Edit</button>
                 <button onclick="deleteMemory('${escSession(x.id)}')" class="text-red-400 hover:bg-red-500/10 font-mono text-[11px] font-bold px-2.5 py-1.5 rounded-lg transition">✕ Delete</button>
               </div>`
            : "";
          return `
            <div class="bg-canvas-bg border border-canvas-border rounded-xl p-4 sm:p-5 space-y-3">
              <div>
                <div class="flex flex-wrap items-center gap-x-3 gap-y-1">
                  <h3 class="text-base font-extrabold text-white">${escSession(x.title)}</h3>
                  ${dateTxt ? `<span class="font-mono text-[11px] text-canvas-gold">${escSession(dateTxt)}</span>` : ""}
                </div>
                ${x.note ? `<p class="text-sm text-slate-300 mt-2 whitespace-pre-line leading-relaxed">${escSession(x.note)}</p>` : ""}
              </div>
              ${gallery}
              ${open || admin ? `<div class="flex flex-wrap items-center gap-2">${open}${admin}</div>` : ""}
            </div>`;
        })
        .join("");
    }

    // ----- Photo viewer (lightbox) -----
    let viewerList = [];
    let viewerIndex = 0;

    function openPhoto(memId, idx) {
      const x = memories.find((m) => m.id === memId);
      if (!x) return;
      viewerList = memoryPhotos(x).filter((u) => driveThumb(u));
      viewerIndex = idx;
      let box = document.getElementById("photoViewer");
      if (!box) {
        box = document.createElement("div");
        box.id = "photoViewer";
        box.className = "fixed inset-0 z-[100] bg-black/90 flex items-center justify-center p-4";
        box.innerHTML = `
          <button onclick="closePhoto()" class="absolute top-4 right-4 text-white text-2xl font-bold w-10 h-10 rounded-full bg-white/10 hover:bg-white/20" aria-label="Close">✕</button>
          <button onclick="stepPhoto(-1)" class="absolute left-3 top-1/2 -translate-y-1/2 text-white text-3xl w-11 h-11 rounded-full bg-white/10 hover:bg-white/20" aria-label="Previous">‹</button>
          <button onclick="stepPhoto(1)" class="absolute right-3 top-1/2 -translate-y-1/2 text-white text-3xl w-11 h-11 rounded-full bg-white/10 hover:bg-white/20" aria-label="Next">›</button>
          <div class="max-w-5xl w-full text-center">
            <img id="photoViewerImg" alt="" referrerpolicy="no-referrer" class="max-h-[80vh] max-w-full mx-auto rounded-lg object-contain" />
            <div class="mt-3 flex items-center justify-center gap-4 font-mono text-xs text-slate-300">
              <span id="photoViewerCount"></span>
              <a id="photoViewerOpen" target="_blank" rel="noopener noreferrer" class="text-canvas-blue hover:underline">Open in Drive ↗</a>
            </div>
          </div>`;
        box.addEventListener("click", (e) => { if (e.target === box) closePhoto(); });
        document.body.appendChild(box);
        document.addEventListener("keydown", (e) => {
          if (!document.getElementById("photoViewer")) return;
          if (e.key === "Escape") closePhoto();
          if (e.key === "ArrowLeft") stepPhoto(-1);
          if (e.key === "ArrowRight") stepPhoto(1);
        });
      }
      showPhoto();
    }

    function showPhoto() {
      const u = viewerList[viewerIndex];
      if (!u) return;
      document.getElementById("photoViewerImg").src = driveThumb(u, 1600);
      document.getElementById("photoViewerCount").textContent = `${viewerIndex + 1} / ${viewerList.length}`;
      document.getElementById("photoViewerOpen").href = u;
    }

    function stepPhoto(d) {
      if (!viewerList.length) return;
      viewerIndex = (viewerIndex + d + viewerList.length) % viewerList.length;
      showPhoto();
    }

    function closePhoto() {
      document.getElementById("photoViewer")?.remove();
    }

    function resetMemoryForm() {
      editingMemoryId = null;
      ["memoryTitleInput", "memoryDateInput", "memoryLinkInput", "memoryPhotosInput", "memoryNoteInput"].forEach((id) => {
        const el = document.getElementById(id);
        if (el) el.value = "";
      });
      const t = document.getElementById("memoryFormTitle");
      if (t) t.textContent = "➕ Add Album (Board only)";
      const b = document.getElementById("memorySaveBtn");
      if (b) b.textContent = "Add Album";
      document.getElementById("memoryCancelBtn")?.classList.add("hidden");
    }

    function editMemory(id) {
      if (!boardUnlocked) return;
      const x = memories.find((m) => m.id === id);
      if (!x) return;
      editingMemoryId = id;
      document.getElementById("memoryTitleInput").value = x.title || "";
      document.getElementById("memoryDateInput").value = x.dateISO || "";
      document.getElementById("memoryLinkInput").value = x.driveLink || "";
      document.getElementById("memoryPhotosInput").value = memoryPhotos(x).join("\n");
      document.getElementById("memoryNoteInput").value = x.note || "";
      document.getElementById("memoryFormTitle").textContent = "✎ Edit Album";
      document.getElementById("memorySaveBtn").textContent = "Save Changes";
      document.getElementById("memoryCancelBtn").classList.remove("hidden");
      document.getElementById("memoriesAdminBox").scrollIntoView({ behavior: "smooth", block: "center" });
    }

    function saveMemoryFromForm() {
      if (!boardUnlocked) { showToast("Board password required", "✕"); return; }
      const title = document.getElementById("memoryTitleInput").value.trim();
      const dateISO = document.getElementById("memoryDateInput").value;
      const driveLink = document.getElementById("memoryLinkInput").value.trim();
      const note = document.getElementById("memoryNoteInput").value.trim();
      const rawPhotos = document.getElementById("memoryPhotosInput").value.split(/[\s,]+/).map((u) => u.trim()).filter(Boolean);

      if (!title) { showToast("Album title is required", "!"); return; }
      const badPhoto = rawPhotos.find((u) => !isDriveLink(u) || !driveThumb(u));
      if (badPhoto) { showToast("Photo links must be Google Drive file links (…/file/d/…)", "!"); return; }
      if (driveLink && !isDriveLink(driveLink)) { showToast("Album link must be a Google Drive link", "!"); return; }
      if (!rawPhotos.length && !driveLink && !note) { showToast("Add some text, photos, or an album link", "!"); return; }
      const photos = [...new Set(rawPhotos)].slice(0, 60);

      if (editingMemoryId) {
        const x = memories.find((m) => m.id === editingMemoryId);
        if (x) { Object.assign(x, { title, dateISO, driveLink, photos, note }); delete x.coverLink; }
      } else {
        memories.push({ id: "MEM-" + Date.now().toString(36), title, dateISO, driveLink, photos, note });
      }
      const wasEditing = !!editingMemoryId;
      resetMemoryForm();
      saveState();
      renderMemories();
      showToast(wasEditing ? "Album updated" : "Album added", "📸");
    }

    function deleteMemory(id) {
      if (!boardUnlocked) return;
      const x = memories.find((m) => m.id === id);
      if (!x || !confirm(`Delete "${x.title}"?`)) return;
      memories = memories.filter((m) => m.id !== id);
      saveState();
      renderMemories();
      showToast("Album deleted", "🗑");
    }

    function switchScene(sceneKey) {
      const scenes = ["submit", "grade", "sessions", "memories", "board"];
      scenes.forEach((sc) => {
        const sec = document.getElementById(`scene-${sc}`);
        const btn = document.getElementById(`tclip-${sc}`);
        if (sec) sec.classList.toggle("hidden", sc !== sceneKey);
        if (btn) {
          if (sc === sceneKey) {
            btn.className =
              "px-3 py-2 rounded-xl font-mono text-xs font-bold border transition flex items-center justify-center gap-2 bg-canvas-gold text-canvas-bg border-canvas-gold shadow";
          } else {
            btn.className =
              "px-3 py-2 rounded-xl font-mono text-xs font-bold border transition flex items-center justify-center gap-2 bg-canvas-panel text-slate-300 border-canvas-border hover:border-canvas-gold/50";
          }
        }
      });
      refreshAllSelectorsAndViews();
      renderSessions();
    }

    function getActiveSubmitTask() {
      return tasks.find((t) => t.id === activeSubmitTaskId) || tasks[0];
    }

    function isTaskLocked(task) {
      if (!task) return false;
      if (testDeadlineLockActive || task.forceLocked) return true;
      const dl = new Date(task.deadlineISO).getTime();
      return !isNaN(dl) && Date.now() > dl;
    }

    function toggleTestDeadlineLock() {
      testDeadlineLockActive = !testDeadlineLockActive;
      const btn = document.getElementById("testLockBtn");
      if (btn) {
        btn.innerHTML = testDeadlineLockActive ? "<span>🔓 Restore Open</span>" : "<span>⚡ Test Lock</span>";
      }
      tickDeadlineClock();
      updateSubmitSceneUI();
      showToast(testDeadlineLockActive ? "Simulating Deadline Lockout" : "Restored Active Deadline");
    }

    function tickDeadlineClock() {
      const task = getActiveSubmitTask();
      if (!task) return;

      const hudCd = document.getElementById("hudCountdown");
      const hudBadge = document.getElementById("hudStatusBadge");
      const mainCd = document.getElementById("mainCountdownDisplay");
      const mainDate = document.getElementById("mainDeadlineDateText");

      const dlDate = new Date(task.deadlineISO);
      if (mainDate && !isNaN(dlDate.getTime())) {
        mainDate.textContent = "Closes: " + dlDate.toLocaleString("en-US", {
          month: "short",
          day: "numeric",
          year: "numeric",
          hour: "2-digit",
          minute: "2-digit"
        });
      }

      const locked = isTaskLocked(task);
      const diff = dlDate.getTime() - Date.now();

      if (locked || diff <= 0) {
        if (hudCd) hudCd.textContent = "00d : 00h : 00m : 00s";
        if (mainCd) mainCd.textContent = "00D : 00H : 00M : 00S";
        if (hudBadge) {
          hudBadge.textContent = "CLOSED";
          hudBadge.className =
            "text-[10px] font-mono font-bold px-2 py-0.5 rounded bg-red-500/20 text-red-400 border border-red-500/40";
        }
        updateSubmitLockStateUI(true);
        return;
      }

      const d = Math.floor(diff / (1000 * 60 * 60 * 24));
      const h = Math.floor((diff / (1000 * 60 * 60)) % 24);
      const m = Math.floor((diff / (1000 * 60)) % 60);
      const s = Math.floor((diff / 1000) % 60);

      const pad = (n) => String(n).padStart(2, "0");
      if (hudCd) hudCd.textContent = `${pad(d)}d : ${pad(h)}h : ${pad(m)}m : ${pad(s)}s`;
      if (mainCd) mainCd.textContent = `${pad(d)}D : ${pad(h)}H : ${pad(m)}M : ${pad(s)}S`;
      if (hudBadge) {
        hudBadge.textContent = "OPEN";
        hudBadge.className =
          "text-[10px] font-mono font-bold px-2 py-0.5 rounded bg-emerald-500/15 text-emerald-400 border border-emerald-500/30";
      }
      updateSubmitLockStateUI(false);
    }

    function updateSubmitLockStateUI(locked) {
      const lockedBanner = document.getElementById("deadlineLockedBanner");
      const formCard = document.getElementById("submitFormCard");
      const pill = document.getElementById("submitStatusPill");

      if (lockedBanner) lockedBanner.classList.toggle("hidden", !locked);
      if (formCard) formCard.classList.toggle("hidden", locked);
      if (pill) {
        if (locked) {
          pill.textContent = "🔒 SUBMISSION CLOSED";
          pill.className =
            "inline-flex items-center gap-1.5 font-mono text-xs font-bold px-2.5 py-0.5 rounded bg-red-500/20 text-red-400 border border-red-500/40";
        } else {
          pill.textContent = "● SUBMISSIONS OPEN";
          pill.className =
            "inline-flex items-center gap-1.5 font-mono text-xs font-bold px-2.5 py-0.5 rounded bg-emerald-500/15 text-emerald-400 border border-emerald-500/30";
        }
      }
    }

    // -----------------------------------------------------------------
    // 4. REFRESH SELECTORS & SCENE 01 (SUBMIT TASK BY NAME ONLY)
    // -----------------------------------------------------------------
    function refreshAllSelectorsAndViews() {
      renderSessions();
      renderMemories();
      const submitTaskSel = document.getElementById("submitTaskSelect");
      const lookupTaskSel = document.getElementById("lookupTaskSelect");
      const boardTaskSel = document.getElementById("boardGradeTaskSelect");

      const taskOptionsHtml = tasks
        .map((t) => `<option value="${t.id}">${t.title} (Max: ${t.maxScore} PTS)</option>`)
        .join("");

      if (submitTaskSel) {
        submitTaskSel.innerHTML = taskOptionsHtml;
        submitTaskSel.value = activeSubmitTaskId;
      }
      if (lookupTaskSel) {
        const prevLookup = lookupTaskSel.value;
        lookupTaskSel.innerHTML = taskOptionsHtml;
        if (prevLookup && tasks.some((t) => t.id === prevLookup)) {
          lookupTaskSel.value = prevLookup;
        }
      }
      if (boardTaskSel) {
        boardTaskSel.innerHTML = taskOptionsHtml;
        boardTaskSel.value = activeBoardGradeTaskId;
      }

      // Member Dropdown in Submit Page shows Member Name ONLY (No Secret Code)
      const memDrop = document.getElementById("submitMemberDropdown");
      if (memDrop) {
        const currentMemId = memDrop.value;
        memDrop.innerHTML =
          '<option value="">Choose your name...</option>' +
          members.map((m) => `<option value="${m.id}">${m.name}</option>`).join("");
        if (currentMemId && members.some((m) => m.id === currentMemId)) {
          memDrop.value = currentMemId;
        }
      }

      updateSubmitSceneUI();
      populateRankingSortOptions();
      if (boardUnlocked) {
        renderBoardAllPanels();
      }
    }

    function updateSubmitSceneUI() {
      const task = getActiveSubmitTask();
      if (!task) return;

      const titleEl = document.getElementById("submitBannerTitle");
      const briefEl = document.getElementById("submitBannerBrief");
      const ptsEl = document.getElementById("submitTaskPointsBadge");

      if (titleEl) titleEl.textContent = task.title;
      if (briefEl) briefEl.textContent = task.brief;
      if (ptsEl) ptsEl.textContent = `Max Score: ${task.maxScore} PTS`;

      tickDeadlineClock();
      const currentMemberId = document.getElementById("submitMemberDropdown")?.value || "";
      if (currentMemberId) {
        handleSubmitMemberDropdown(currentMemberId);
      }
    }

    function handleTaskSelectChange(taskId) {
      activeSubmitTaskId = taskId;
      updateSubmitSceneUI();
    }

    function handleSubmitMemberDropdown(memberId) {
      const bar = document.getElementById("submitVerifiedMemberBar");
      const nameEl = document.getElementById("verifiedMemberName");
      const resubmitBox = document.getElementById("resubmitAlertBox");

      const member = members.find((m) => m.id === memberId);
      if (!member) {
        if (bar) bar.classList.add("hidden");
        if (resubmitBox) resubmitBox.classList.add("hidden");
        return;
      }

      if (nameEl) nameEl.textContent = member.name;
      if (bar) bar.classList.remove("hidden");

      const existing = submissions.find(
        (s) =>
          s.taskId === activeSubmitTaskId &&
          (s.memberId === member.id || s.memberCode === member.code) &&
          s.driveLink
      );
      if (resubmitBox) {
        resubmitBox.classList.toggle("hidden", !existing);
      }
    }

    function validateDriveUrlLive(url) {
      const statusText = document.getElementById("driveUrlStatusText");
      if (!statusText) return false;
      const clean = url.trim();
      if (!clean) {
        statusText.textContent = "Paste drive.google.com link";
        statusText.className = "text-[11px] font-mono text-slate-400";
        return false;
      }
      const isValid = /^https?:\/\/(drive\.google\.com|docs\.google\.com)\/\S+/i.test(clean);
      if (isValid) {
        statusText.textContent = "✓ Valid Google Drive URL";
        statusText.className = "text-[11px] font-mono font-bold text-emerald-400";
      } else {
        statusText.textContent = "✕ Must be a valid drive.google.com link";
        statusText.className = "text-[11px] font-mono font-bold text-red-400";
      }
      return isValid;
    }

    async function handleMemberTaskSubmit() {
      const task = getActiveSubmitTask();
      if (!task || isTaskLocked(task)) {
        showToast("Submissions are locked for this task", "🔒");
        return;
      }

      const memberId = document.getElementById("submitMemberDropdown")?.value || "";
      const member = members.find((m) => m.id === memberId);
      if (!member) {
        showToast("Please select your name from the list", "!");
        return;
      }

      const driveUrl = document.getElementById("submitDriveUrlInput")?.value.trim() || "";
      if (!validateDriveUrlLive(driveUrl)) {
        showToast("Please enter a valid Google Drive link", "!");
        return;
      }

      const shareChecked = document.getElementById("submitShareConfirm")?.checked;
      if (!shareChecked) {
        showToast("Please confirm 'Anyone with the link can view'", "!");
        return;
      }

      const note = document.getElementById("submitNoteInput")?.value.trim() || "";
      const btn = document.getElementById("submitTaskBtn");
      if (btn) { btn.disabled = true; btn.classList.add("opacity-60"); }

      let nowStr = new Date().toISOString().slice(0, 16).replace("T", " ");
      let ok = false;
      try {
        if (API_URL) {
          const r = await api({ action: "submit", taskId: task.id, memberId: member.id, driveLink: driveUrl, note });
          if (!r.ok) {
            const msgs = { locked: "Submissions are locked for this task", bad_link: "Invalid Google Drive link", busy: "Server busy — try again" };
            showToast(msgs[r.error] || "Submission failed: " + r.error, "✕");
            return;
          }
          nowStr = r.timestamp;
          // نعكس التسليم محلياً عشان رسالة "تم التسليم قبل كده" تظهر
          if (!submissions.some((x) => x.taskId === task.id && x.memberId === member.id)) {
            submissions.push({ taskId: task.id, memberId: member.id, driveLink: "1" });
          }
        } else {
          let sub = submissions.find((s) => s.taskId === task.id && (s.memberId === member.id || s.memberCode === member.code));
          if (sub) {
            Object.assign(sub, { driveLink: driveUrl, note, timestamp: nowStr, memberId: member.id, memberCode: member.code });
          } else {
            submissions.push({
              refCode: "REF-" + Math.floor(1000 + Math.random() * 9000), taskId: task.id, memberId: member.id,
              memberCode: member.code, driveLink: driveUrl, note, timestamp: nowStr, score: null, evaluator: "", feedback: ""
            });
          }
          saveState();
        }
        ok = true;
      } catch (e) {
        showToast("Network error — your submission was NOT saved, try again", "✕");
        return;
      } finally {
        if (btn) { btn.disabled = false; btn.classList.remove("opacity-60"); }
      }
      if (!ok) return;

      // Show Receipt without exposing Secret Code
      const receiptBox = document.getElementById("submissionReceiptBox");
      if (receiptBox) {
        document.getElementById("receiptMemberVal").textContent = member.name;
        document.getElementById("receiptTaskVal").textContent = task.title;
        document.getElementById("receiptTimeVal").textContent = nowStr;
        receiptBox.classList.remove("hidden");
      }

      document.getElementById("submitDriveUrlInput").value = "";
      document.getElementById("submitNoteInput").value = "";
      handleSubmitMemberDropdown(member.id);
      showToast(`Submission saved for ${member.name}`);
    }

    // -----------------------------------------------------------------
    // 5. SCENE 02: PRIVATE SECRET CODE LOOKUP (SINGLE TASK GRADE)
    // -----------------------------------------------------------------
    function toggleLookupSecretCodeVisibility() {
      const input = document.getElementById("lookupMemberCodeInput");
      if (!input) return;
      input.type = input.type === "password" ? "text" : "password";
    }

    async function lookupSingleTaskGrade() {
      const taskId = document.getElementById("lookupTaskSelect")?.value;
      const codeRaw = document.getElementById("lookupMemberCodeInput")?.value.trim().toUpperCase();
      const resBox = document.getElementById("lookupSingleTaskResult");
      if (!resBox) return;

      if (!codeRaw) {
        showToast("Please enter your private Secret Code", "!");
        return;
      }

      let member = null, task = null, sub = null;
      if (API_URL) {
        resBox.innerHTML = '<div class="text-center text-xs font-mono text-slate-400 p-5">⏳ Checking…</div>';
        try {
          const r = await api({ action: "grade", taskId, code: codeRaw });
          if (r.ok) { member = r.member; task = r.task; sub = r.sub; }
          else if (r.error !== "invalid_code") { showToast("Server error: " + r.error, "✕"); resBox.innerHTML = ""; return; }
        } catch (e) { showToast("Network error — try again", "✕"); resBox.innerHTML = ""; return; }
      } else {
        member = members.find((m) => m.code.toUpperCase() === codeRaw);
        task = tasks.find((t) => t.id === taskId);
        if (member && task) sub = submissions.find((s) => s.taskId === task.id && (s.memberCode === member.code || s.memberId === member.id));
      }

      if (!member || !task) {
        resBox.innerHTML = `
          <div class="bg-red-500/10 border border-red-500/40 rounded-xl p-5 text-center space-y-1">
            <div class="text-sm font-extrabold text-red-400">✕ Invalid Secret Code</div>
            <p class="text-xs text-slate-300">No Video Team member matches this Secret Code. Please verify the private code sent to you by the Board.</p>
          </div>
        `;
        return;
      }

      if (!sub || (!sub.driveLink && sub.score === null)) {
        resBox.innerHTML = `
          <div class="bg-canvas-bg border border-amber-500/40 rounded-2xl p-5 space-y-2">
            <div class="flex items-center justify-between">
              <span class="text-sm font-extrabold text-white">${member.name}</span>
              <span class="font-mono text-xs px-2.5 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30">NOT SUBMITTED YET</span>
            </div>
            <div class="text-xs text-slate-400">Task: <strong class="text-canvas-gold">${task.title}</strong></div>
            <p class="text-xs text-slate-300 pt-1">No submission or evaluation recorded for this task yet.</p>
          </div>
        `;
        return;
      }

      const isGraded = sub.score !== null && sub.score !== "";
      const critHtml = sub.criteria
        ? `<div class="grid grid-cols-3 gap-3 text-xs">${TASK_CRITERIA.map((c) => {
            const v = sub.criteria[c.id];
            return `<div class="bg-canvas-panel border border-canvas-border rounded-xl p-3 text-center">
              <div class="text-[10px] font-mono text-slate-400 uppercase">${c.title}</div>
              <div class="font-mono font-black text-canvas-gold mt-0.5">${v === null || v === undefined ? "—" : v} <span class="text-slate-500 font-normal">/ ${c.max}</span></div>
            </div>`;
          }).join("")}</div>`
        : "";
      resBox.innerHTML = `
        <div class="bg-canvas-bg border-2 ${isGraded ? "border-canvas-gold" : "border-canvas-blue/50"} rounded-2xl p-5 space-y-4 shadow-xl">
          <div class="flex flex-wrap items-center justify-between gap-2 border-b border-canvas-border pb-3">
            <div>
              <div class="text-xs font-mono text-slate-400">VERIFIED VIDEO TEAM MEMBER</div>
              <div class="text-lg font-extrabold text-white">${member.name}</div>
            </div>
            <div class="text-right">
              <div class="text-[11px] font-mono text-slate-400">TASK SCORE</div>
              <div class="font-mono text-2xl font-black ${isGraded ? "text-canvas-gold" : "text-canvas-blue"}">
                ${isGraded ? `${sub.score} / ${task.maxScore}` : "PENDING REVIEW ⏳"}
              </div>
            </div>
          </div>

          ${critHtml}
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 text-xs">
            <div class="bg-canvas-panel border border-canvas-border rounded-xl p-3">
              <div class="text-[10px] font-mono text-slate-400 uppercase">Selected Task</div>
              <div class="font-bold text-white mt-0.5">${task.title}</div>
            </div>
            <div class="bg-canvas-panel border border-canvas-border rounded-xl p-3">
              <div class="text-[10px] font-mono text-slate-400 uppercase">Reviewed By Board</div>
              <div class="font-bold text-canvas-gold mt-0.5">${sub.evaluator || "Pending Board Review"}</div>
            </div>
          </div>

          <div class="bg-canvas-panel border border-canvas-border rounded-xl p-4 space-y-1">
            <div class="text-[10px] font-mono uppercase text-canvas-blue font-bold">Board Feedback</div>
            <p class="text-xs sm:text-sm text-slate-200 leading-relaxed">
              ${sub.feedback ? sub.feedback : "Your task is logged and awaiting Board evaluation feedback."}
            </p>
          </div>

          ${
            sub.driveLink
              ? `<div class="flex items-center justify-between bg-canvas-panel border border-canvas-border rounded-xl px-3.5 py-2.5 text-xs">
                  <span class="font-mono text-slate-400 truncate max-w-[70%]">Submitted Link: ${sub.driveLink}</span>
                  <a href="${sub.driveLink}" target="_blank" rel="noopener noreferrer" class="font-mono font-bold text-canvas-blue hover:underline shrink-0">Open Drive ↗</a>
                </div>`
              : ""
          }
        </div>
      `;
    }

    // -----------------------------------------------------------------
    // 6. SCENE 03: BOARD PORTAL LOCK, TABS & SECURITY
    // -----------------------------------------------------------------
    async function unlockBoardPortal() {
      const passIn = document.getElementById("boardPasswordInput");
      if (!passIn) return;
      const pw = passIn.value;
      if (API_URL) {
        try {
          showToast("Checking password…", "⏳");
          const r = await api({ action: "boardLoad", password: pw });
          if (!r.ok) {
            showToast(r.error === "too_many_attempts" ? "Too many attempts — wait 10 minutes" : "Incorrect Board Password", "✕");
            return;
          }
          boardPasswordSession = pw;
          const st = r.state;
          tasks = st.tasks?.length ? st.tasks : tasks;
          members = st.members || [];
          submissions = st.submissions || [];
          sessions = st.sessions || [];
          memories = st.memories || [];
          pillarScores = st.pillarScores || {};
          pillarSubColumns = st.pillarSubColumns || {};
          applyDefaultsAfterLoad();
          boardUnlocked = true;
          if (!st.configured) { syncDirty = true; flushToServer(); } // أول مرة: نكتب الإعدادات الافتراضية في الشيت
        } catch (e) {
          showToast("Could not reach the server", "✕");
          return;
        }
      } else {
        if (pw !== boardPassword) { showToast("Incorrect Board Password", "✕"); return; }
        boardUnlocked = true;
      }
      passIn.value = "";
      document.getElementById("boardLockCard")?.classList.add("hidden");
      document.getElementById("boardUnlockedSuite")?.classList.remove("hidden");
      refreshAllSelectorsAndViews();
      renderBoardAllPanels();
      showToast("Board Portal Unlocked", "🔓");
    }

    function lockBoardPortal() {
      boardUnlocked = false;
      boardPasswordSession = "";
      resetSessionForm();
      resetMemoryForm();
      renderSessions();
      renderMemories();
      document.getElementById("boardUnlockedSuite")?.classList.add("hidden");
      document.getElementById("boardLockCard")?.classList.remove("hidden");
      showToast("Board Portal Locked", "🔒");
    }

    function switchBoardTab(tabKey) {
      const tabs = ["grading", "matrix", "rankings", "tasks", "roster", "security"];
      tabs.forEach((k) => {
        const panel = document.getElementById(`bpanel-${k}`);
        const btn = document.getElementById(`btab-${k}`);
        if (panel) panel.classList.toggle("hidden", k !== tabKey);
        if (btn) {
          if (k === tabKey) {
            btn.className = "px-3 py-1.5 rounded-lg text-xs font-bold bg-canvas-gold text-canvas-bg transition";
          } else {
            btn.className = "px-3 py-1.5 rounded-lg text-xs font-bold text-slate-300 hover:text-white transition";
          }
        }
      });
      renderBoardAllPanels();
    }

    function renderBoardAllPanels() {
      renderBoardGradingCards();
      renderEvaluationMatrix();
      populateRankingSortOptions();
      renderBoardRankingsDashboard();
      renderUnlimitedTasksManager();
      renderRosterTable();
    }

    async function updateBoardPassword() {
      const oldP = document.getElementById("oldPassInput")?.value || "";
      const newP = document.getElementById("newPassInput")?.value.trim() || "";
      const confP = document.getElementById("confirmPassInput")?.value.trim() || "";

      if (oldP !== (API_URL ? boardPasswordSession : boardPassword)) {
        showToast("Current password is incorrect", "✕");
        return;
      }
      if (!newP || newP.length < 3) {
        showToast("New password must be at least 3 characters", "!");
        return;
      }
      if (newP !== confP) {
        showToast("New passwords do not match", "!");
        return;
      }

      if (API_URL) {
        try {
          const r = await api({ action: "changePassword", password: boardPasswordSession, newPassword: newP });
          if (!r.ok) { showToast("Could not change password: " + r.error, "✕"); return; }
          boardPasswordSession = newP;
        } catch (e) { showToast("Network error — password NOT changed", "✕"); return; }
      } else {
        boardPassword = newP;
        saveState();
      }
      document.getElementById("oldPassInput").value = "";
      document.getElementById("newPassInput").value = "";
      document.getElementById("confirmPassInput").value = "";
      showToast("Board password updated successfully", "🔑");
    }

    // -----------------------------------------------------------------
    // 7. BOARD SUB-TAB 1: GRADE SUBMISSIONS & VIEW GOOGLE DRIVE LINKS
    // -----------------------------------------------------------------
    function handleBoardGradeTaskChange(taskId) {
      activeBoardGradeTaskId = taskId;
      renderBoardGradingCards();
    }

    function renderBoardGradingCards() {
      const container = document.getElementById("boardGradingCardsContainer");
      const summaryPill = document.getElementById("boardGradeSummaryPill");
      if (!container) return;

      const task = tasks.find((t) => t.id === activeBoardGradeTaskId) || tasks[0];
      if (!task) return;

      if (members.length === 0) {
        if (summaryPill) summaryPill.textContent = "Submitted: 0 / 0";
        container.innerHTML = "";
        return;
      }

      let submittedCount = 0;

      container.innerHTML = members
        .map((m) => {
          const sub = submissions.find(
            (s) => s.taskId === task.id && (s.memberCode === m.code || s.memberId === m.id)
          );
          const hasLink = !!(sub && sub.driveLink);
          if (hasLink) submittedCount++;
          const scoreVal = sub && sub.score !== null && sub.score !== "" ? sub.score : "";
          const fbVal = sub?.feedback || "";
          const linkVal = sub?.driveLink || "";

          return `
            <div class="bg-canvas-panel border ${
              hasLink ? "border-canvas-gold/50" : "border-canvas-border"
            } rounded-2xl p-4 sm:p-5 space-y-3.5 shadow-lg">
              <div class="flex items-center justify-between gap-2 border-b border-canvas-border pb-2.5">
                <div>
                  <div class="flex items-center gap-2">
                    <span class="text-sm font-extrabold text-white">${m.name}</span>
                    <span class="font-mono text-[11px] font-bold px-2 py-0.5 rounded bg-canvas-bg text-canvas-gold border border-canvas-border">${m.code}</span>
                  </div>
                  <div class="text-[11px] font-mono text-slate-400 mt-0.5">
                    ${sub?.timestamp ? `Submitted: ${sub.timestamp}` : "No submission timestamp yet"}
                  </div>
                </div>
                <span class="font-mono text-[11px] font-bold px-2.5 py-1 rounded ${
                  scoreVal !== ""
                    ? "bg-emerald-500/15 text-emerald-400 border border-emerald-500/30"
                    : hasLink
                    ? "bg-canvas-gold/15 text-canvas-gold border border-canvas-gold/30"
                    : "bg-canvas-bg text-slate-400 border border-canvas-border"
                }">
                  ${scoreVal !== "" ? `GRADED: ${scoreVal}/${task.maxScore}` : hasLink ? "SUBMITTED • READY" : "NOT SUBMITTED"}
                </span>
              </div>

              <!-- Prominent Google Drive Link Box -->
              <div class="bg-canvas-bg border border-canvas-border rounded-xl p-3 space-y-2">
                <div class="flex items-center justify-between gap-2">
                  <span class="text-[10px] font-mono uppercase text-canvas-blue font-bold">Google Drive Submission Link</span>
                  ${
                    hasLink
                      ? `<div class="flex items-center gap-1.5">
                          <a href="${linkVal}" target="_blank" rel="noopener noreferrer" class="px-2.5 py-1 rounded-lg bg-canvas-gold text-canvas-bg font-mono text-[11px] font-extrabold hover:bg-amber-400 transition">
                            🔗 Open Drive Link ↗
                          </a>
                          <button onclick="copyTextSafe('${linkVal.replace(/'/g, "\\'")}', 'Drive link copied')" class="px-2 py-1 rounded-lg bg-canvas-panel border border-canvas-border text-slate-300 font-mono text-[10px] hover:text-white">
                            Copy
                          </button>
                        </div>`
                      : `<span class="text-[10px] font-mono text-slate-500">No link uploaded yet</span>`
                  }
                </div>
                <input
                  id="bgrade-link-${m.code}"
                  type="url"
                  value="${linkVal.replace(/"/g, "&quot;")}"
                  placeholder="https://drive.google.com/..."
                  class="w-full bg-canvas-panel border border-canvas-border focus:border-canvas-blue rounded-lg px-2.5 py-1.5 text-xs font-mono text-slate-200 focus:outline-none"
                />
                ${
                  sub?.note
                    ? `<div class="text-[11px] text-slate-300 bg-canvas-panel/60 px-2.5 py-1.5 rounded border border-canvas-border">
                        <strong class="text-canvas-gold font-mono">Member Note:</strong> ${sub.note}
                      </div>`
                    : ""
                }
              </div>

              <!-- Criteria Scores & Feedback -->
              <div class="grid grid-cols-3 gap-2.5">
                ${TASK_CRITERIA.map((c) => {
                  const saved = sub?.criteria?.[c.id];
                  const hasSaved = saved !== null && saved !== undefined && saved !== "";
                  const suggest = c.id === "deadline" ? onTimeSuggestion(task, sub) : null;
                  const val = hasSaved ? saved : suggest !== null && !sub?.criteria ? suggest : "";
                  return `
                    <div>
                      <label class="block text-[10px] font-mono uppercase text-slate-400 mb-1">${c.title} (/${c.max})</label>
                      <input
                        id="bgrade-c-${c.id}-${m.code}"
                        type="number" min="0" max="${c.max}" step="0.5"
                        value="${val}"
                        placeholder="0 - ${c.max}"
                        class="w-full bg-canvas-bg border border-canvas-border focus:border-canvas-gold rounded-xl px-3 py-2 text-xs font-mono font-bold text-canvas-gold focus:outline-none"
                      />
                      ${c.id === "deadline" && suggest !== null && !hasSaved ? `<div class="text-[9px] font-mono text-slate-500 mt-0.5">${suggest === c.max ? "On time ✓ (suggested)" : "Late (suggested)"}</div>` : ""}
                    </div>`;
                }).join("")}
              </div>
              <div>
                <label class="block text-[10px] font-mono uppercase text-slate-400 mb-1">Board Feedback &nbsp;•&nbsp; Task total = ${TASK_MAX_TOTAL} points</label>
                <input
                  id="bgrade-fb-${m.code}"
                  type="text"
                  value="${fbVal.replace(/"/g, "&quot;")}"
                  placeholder="Write constructive feedback for ${m.name}..."
                  class="w-full bg-canvas-bg border border-canvas-border focus:border-canvas-gold rounded-xl px-3 py-2 text-xs text-white focus:outline-none"
                />
              </div>

              <div class="flex justify-end pt-1">
                <button onclick="saveBoardCardEvaluation('${task.id}', '${m.code}')" class="bg-canvas-gold hover:bg-amber-400 text-canvas-bg font-extrabold text-xs px-4 py-2 rounded-xl transition shadow">
                  💾 Save Grade &amp; Feedback
                </button>
              </div>
            </div>
          `;
        })
        .join("");

      if (summaryPill) {
        summaryPill.textContent = `Submitted: ${submittedCount} / ${members.length}`;
      }
    }

    // Suggest the Deadline score: full points if submitted on/before the deadline, otherwise 0
    function onTimeSuggestion(task, sub) {
      if (!sub || !sub.timestamp || !task?.deadlineISO) return null;
      const deadline = String(task.deadlineISO).slice(0, 16).replace("T", " ");
      return String(sub.timestamp) <= deadline ? TASK_CRITERIA[0].max : 0;
    }

    function criteriaTotal(crit) {
      const any = TASK_CRITERIA.some((c) => crit[c.id] !== null && crit[c.id] !== undefined);
      return any ? TASK_CRITERIA.reduce((sum, c) => sum + (Number(crit[c.id]) || 0), 0) : null;
    }

    function saveBoardCardEvaluation(taskId, memberCode) {
      const task = tasks.find((t) => t.id === taskId);
      const member = members.find((m) => m.code === memberCode);
      if (!task || !member) return;

      const fbVal = document.getElementById(`bgrade-fb-${memberCode}`)?.value.trim() || "";
      const linkVal = document.getElementById(`bgrade-link-${memberCode}`)?.value.trim() || "";
      const evaluator = document.getElementById("boardActiveEvaluator")?.value || "Omar Shalaby";

      const crit = {};
      for (const c of TASK_CRITERIA) {
        const raw = document.getElementById(`bgrade-c-${c.id}-${memberCode}`)?.value.trim() || "";
        if (raw !== "" && (isNaN(Number(raw)) || Number(raw) < 0 || Number(raw) > c.max)) {
          showToast(`${c.title} must be between 0 and ${c.max}`, "!");
          return;
        }
        crit[c.id] = raw === "" ? null : Number(raw);
      }
      const total = criteriaTotal(crit);

      let sub = submissions.find(
        (s) => s.taskId === taskId && (s.memberCode === memberCode || s.memberId === member.id)
      );

      if (!sub) {
        sub = {
          refCode: "REF-" + Math.floor(1000 + Math.random() * 9000),
          taskId,
          memberId: member.id,
          memberCode: member.code,
          driveLink: linkVal,
          note: "",
          timestamp: new Date().toISOString().slice(0, 16).replace("T", " "),
          score: total,
          criteria: crit,
          evaluator,
          feedback: fbVal
        };
        submissions.push(sub);
      } else {
        sub.driveLink = linkVal;
        sub.score = total;
        sub.criteria = crit;
        sub.evaluator = evaluator;
        sub.feedback = fbVal;
      }

      saveState();
      renderBoardAllPanels();
      showToast(`Saved evaluation for ${member.name}${total !== null ? ` (${total}/${TASK_MAX_TOTAL})` : ""}`);
    }

    // Inline edit from the Evaluation Matrix (Tasks pillar): one criterion at a time
    function updateInlineTaskCriterion(taskId, memberCode, critId, val) {
      const crit = TASK_CRITERIA.find((c) => c.id === critId);
      const clean = String(val).trim();
      if (!crit) return;
      if (clean !== "" && (isNaN(Number(clean)) || Number(clean) < 0 || Number(clean) > crit.max)) {
        showToast(`${crit.title} must be between 0 and ${crit.max}`, "!");
        renderEvaluationMatrix();
        return;
      }
      const member = members.find((m) => m.code === memberCode);
      if (!member) return;
      let sub = submissions.find((s) => s.taskId === taskId && (s.memberCode === memberCode || s.memberId === member.id));
      if (!sub) {
        sub = {
          refCode: "REF-" + Math.floor(1000 + Math.random() * 9000),
          taskId, memberId: member.id, memberCode, driveLink: "", note: "", timestamp: "",
          score: null, criteria: {}, evaluator: document.getElementById("boardActiveEvaluator")?.value || "Board", feedback: ""
        };
        submissions.push(sub);
      }
      sub.criteria = sub.criteria || {};
      sub.criteria[critId] = clean === "" ? null : Number(clean);
      sub.score = criteriaTotal(sub.criteria);
      saveState();
      renderEvaluationMatrix();
      renderBoardRankingsDashboard();
    }

    // -----------------------------------------------------------------
    // 8. BOARD SUB-TAB 5: TEAM ROSTER & PRIVATE SECRET CODES
    // -----------------------------------------------------------------
    function generateUniqueSecretCode() {
      let code = "";
      let tries = 0;
      do {
        const randNum = Math.floor(1000 + Math.random() * 9000);
        code = `VID-${randNum}`;
        tries++;
      } while (members.some((m) => m.code === code) && tries < 50);
      return code;
    }

    function fillRandomNewMemberCode() {
      const el = document.getElementById("rosterNewCode");
      if (el) el.value = generateUniqueSecretCode();
    }

    function renderRosterTable() {
      const tbody = document.getElementById("rosterTableBody");
      const badge = document.getElementById("rosterTotalBadge");
      if (!tbody) return;
      if (badge) badge.textContent = `${members.length} Members`;

      tbody.innerHTML = members
        .map((m, idx) => {
          return `
            <tr class="hover:bg-canvas-elevated/40 transition">
              <td class="py-2.5 px-3 font-mono text-slate-400">${idx + 1}</td>
              <td class="py-2.5 px-3 font-bold text-white">${m.name}</td>
              <td class="py-2.5 px-3">
                <span class="font-mono text-[11px] px-2 py-0.5 rounded bg-canvas-bg text-canvas-blue border border-canvas-border">Video</span>
              </td>
              <td class="py-2.5 px-3">
                <input
                  type="text"
                  value="${m.code}"
                  onchange="updateMemberSecretCodeInline('${m.id}', this.value)"
                  class="w-32 bg-canvas-bg border border-canvas-gold/40 focus:border-canvas-gold rounded-lg px-2.5 py-1 text-xs font-mono font-bold text-canvas-gold uppercase focus:outline-none"
                />
              </td>
              <td class="py-2.5 px-3 text-right space-x-1.5 whitespace-nowrap">
                <button
                  onclick="copySingleMemberCodeMessage('${m.id}')"
                  class="bg-canvas-gold/15 hover:bg-canvas-gold hover:text-canvas-bg text-canvas-gold border border-canvas-gold/30 font-mono text-[11px] font-bold px-2.5 py-1 rounded-lg transition"
                >
                  📋 Copy Code Msg
                </button>
                <button
                  onclick="removeMemberFromRoster('${m.id}')"
                  class="bg-red-500/15 hover:bg-red-500/30 text-red-400 font-mono text-[11px] px-2.5 py-1 rounded-lg transition"
                >
                  ✕
                </button>
              </td>
            </tr>
          `;
        })
        .join("");
    }

    function addBulkMembersToRoster() {
      const ta = document.getElementById("rosterBulkNames");
      const names = (ta?.value || "").split(/\r?\n/).map((n) => n.trim()).filter(Boolean);
      if (!names.length) { showToast("Paste at least one name", "!"); return; }
      names.forEach((name, i) => {
        members.push({ id: `MEM-${Date.now().toString().slice(-6)}${i}`, code: generateUniqueSecretCode(), name, team: "Video" });
      });
      ta.value = "";
      fillRandomNewMemberCode();
      saveState();
      refreshAllSelectorsAndViews();
      renderRosterTable();
      showToast(`Added ${names.length} members`);
    }

    function addNewMemberToRoster() {
      const nameIn = document.getElementById("rosterNewName");
      const codeIn = document.getElementById("rosterNewCode");
      const name = nameIn?.value.trim() || "";
      let code = codeIn?.value.trim().toUpperCase() || generateUniqueSecretCode();

      if (!name) {
        showToast("Please enter the member's full name", "!");
        return;
      }
      if (members.some((m) => m.code.toUpperCase() === code)) {
        code = generateUniqueSecretCode();
      }

      const newId = `MEM-${Date.now().toString().slice(-4)}`;
      members.push({ id: newId, code, name, team: "Video" });
      if (nameIn) nameIn.value = "";
      fillRandomNewMemberCode();

      saveState();
      refreshAllSelectorsAndViews();
      showToast(`Added ${name} (${code})`);
    }

    function copySingleMemberCodeMessage(memberId) {
      const m = members.find((item) => item.id === memberId);
      if (!m) return;
      const msg = `🎬 Enactus Menufiya University — Video Team\nHi ${m.name},\nYour private Secret Code to check your task evaluations on the portal is: ${m.code}\n(Keep this code private so only you can view your grades).`;
      copyTextSafe(msg, `Copied secret code message for ${m.name}`);
    }

    function copyAllMemberCodesList() {
      const lines = members.map((m, i) => `${i + 1}. ${m.name} — Private Code: ${m.code}`);
      copyTextSafe(lines.join("\n"), "Copied all member private codes");
    }

    function updateMemberSecretCodeInline(memberId, newCodeRaw) {
      const clean = (newCodeRaw || "").trim().toUpperCase();
      if (!clean) {
        renderRosterTable();
        return;
      }
      if (members.some((m) => m.id !== memberId && m.code.toUpperCase() === clean)) {
        showToast("This Secret Code is already assigned to another member", "!");
        renderRosterTable();
        return;
      }
      const member = members.find((m) => m.id === memberId);
      if (!member) return;
      const oldCode = member.code;
      member.code = clean;

      // Keep submissions & pillarScores linked to the updated code
      submissions.forEach((s) => {
        if (s.memberId === member.id || s.memberCode === oldCode) {
          s.memberId = member.id;
          s.memberCode = clean;
        }
      });
      if (pillarScores[oldCode] && oldCode !== clean) {
        pillarScores[clean] = pillarScores[oldCode];
        delete pillarScores[oldCode];
      }

      saveState();
      refreshAllSelectorsAndViews();
      showToast(`Updated private code for ${member.name} to ${clean}`);
    }

    function regenerateAllRandomSecretCodes() {
      members.forEach((m) => {
        const oldCode = m.code;
        const newCode = generateUniqueSecretCode();
        m.code = newCode;
        submissions.forEach((s) => {
          if (s.memberId === m.id || s.memberCode === oldCode) {
            s.memberId = m.id;
            s.memberCode = newCode;
          }
        });
        if (pillarScores[oldCode]) {
          pillarScores[newCode] = pillarScores[oldCode];
          delete pillarScores[oldCode];
        }
      });
      saveState();
      refreshAllSelectorsAndViews();
      showToast("Generated new random Secret Codes for all members", "🎲");
    }

    function removeMemberFromRoster(memberId) {
      const target = members.find((m) => m.id === memberId || m.code === memberId);
      members = members.filter((m) => m.id !== memberId && m.code !== memberId);
      saveState();
      refreshAllSelectorsAndViews();
      showToast(`Removed ${target ? target.name : memberId}`);
    }

    // -----------------------------------------------------------------
    // 9. BOARD SUB-TAB 2: 6-PILLARS MIND MAP & DRILL-DOWN EVALUATION MATRIX
    // -----------------------------------------------------------------
    function selectMindMapPillar(pillarId) {
      activeMindMapPillarId = pillarId;
      renderEvaluationMatrix();
    }

    function addSubColumnToPillar(pillarId) {
      const pillar = FIXED_PILLARS.find((p) => p.id === pillarId);
      if (!pillar) return;

      if (pillar.isTasksPillar) {
        // Add a new Task directly into the Tasks pillar (30%)
        const nextNum = tasks.length + 1;
        const newId = `TASK-${String(nextNum).padStart(2, "0")}-${Date.now().toString().slice(-2)}`;
        const newTitle = `Task ${String(nextNum).padStart(2, "0")}: New Task`;
        tasks.push({
          id: newId,
          title: newTitle,
          brief: "Upload your task files to Google Drive and submit the link.",
          maxScore: TASK_MAX_TOTAL,
          deadlineISO: "2026-12-20T23:59",
          forceLocked: false
        });
        saveState();
        refreshAllSelectorsAndViews();
        showToast(`Added "${newTitle}" under Tasks (30%)`);
        return;
      }

      if (!Array.isArray(pillarSubColumns[pillarId])) {
        pillarSubColumns[pillarId] = [];
      }
      const nextIdx = pillarSubColumns[pillarId].length + 1;
      const newSub = {
        id: `SUB-${pillarId.replace("PIL-", "")}-${Date.now().toString().slice(-4)}`,
        title: `${pillar.shortTitle} ${String(nextIdx).padStart(2, "0")}`,
        maxScore: 100
      };
      pillarSubColumns[pillarId].push(newSub);
      saveState();
      renderBoardAllPanels();
      showToast(`Added "${newSub.title}" inside ${pillar.title} (${pillar.weight100}%)`);
    }

    function updatePillarSubColTitle(pillarId, subColId, newTitle) {
      const list = pillarSubColumns[pillarId] || [];
      const col = list.find((c) => c.id === subColId);
      if (!col) return;
      col.title = newTitle.trim() || "Evaluation Item";
      saveState();
      renderEvaluationMatrix();
      showToast(`Renamed column to "${col.title}"`);
    }

    function updatePillarSubColMax(pillarId, subColId, newMax) {
      const list = pillarSubColumns[pillarId] || [];
      const col = list.find((c) => c.id === subColId);
      if (!col) return;
      const parsed = Number(newMax);
      if (isNaN(parsed) || parsed <= 0) {
        renderEvaluationMatrix();
        return;
      }
      col.maxScore = parsed;
      saveState();
      renderBoardAllPanels();
      showToast(`Updated max score for "${col.title}" to ${parsed}`);
    }

    function removePillarSubCol(pillarId, subColId) {
      if (!Array.isArray(pillarSubColumns[pillarId])) return;
      const target = pillarSubColumns[pillarId].find((c) => c.id === subColId);
      pillarSubColumns[pillarId] = pillarSubColumns[pillarId].filter((c) => c.id !== subColId);
      Object.keys(pillarScores).forEach((memberCode) => {
        if (pillarScores[memberCode] && subColId in pillarScores[memberCode]) {
          delete pillarScores[memberCode][subColId];
        }
      });
      saveState();
      renderBoardAllPanels();
      showToast(`Removed "${target ? target.title : subColId}"`);
    }

    function updateTaskHeaderTitleInline(taskId, newTitle) {
      const task = tasks.find((t) => t.id === taskId);
      if (!task) return;
      if (newTitle.trim()) {
        task.title = newTitle.trim();
        saveState();
        refreshAllSelectorsAndViews();
        showToast(`Renamed task to "${task.title}"`);
      }
    }

    function updateTaskHeaderMaxInline() {
      showToast("Task score is fixed: Deadline 10 + Creativity 10 + Quality 10 = 30", "i");
      renderEvaluationMatrix();
    }

    function updateInlineTaskScore(taskId, memberCode, val) {
      const task = tasks.find((t) => t.id === taskId);
      const maxPts = task ? task.maxScore : TASK_MAX_TOTAL;
      const clean = val.trim();

      if (clean !== "" && (isNaN(Number(clean)) || Number(clean) < 0 || Number(clean) > maxPts)) {
        showToast(`Max score for ${task ? task.title : taskId} is ${maxPts}`, "!");
        renderEvaluationMatrix();
        return;
      }

      const member = members.find((m) => m.code === memberCode);
      let sub = submissions.find((s) => s.taskId === taskId && s.memberCode === memberCode);
      if (!sub) {
        sub = {
          refCode: "REF-" + Math.floor(1000 + Math.random() * 9000),
          taskId,
          memberId: member ? member.id : "",
          memberCode,
          driveLink: "",
          note: "",
          timestamp: "",
          score: clean === "" ? null : Number(clean),
          evaluator: document.getElementById("boardActiveEvaluator")?.value || "Board",
          feedback: ""
        };
        submissions.push(sub);
      } else {
        sub.score = clean === "" ? null : Number(clean);
      }

      saveState();
      renderEvaluationMatrix();
      renderBoardRankingsDashboard();
    }

    function updateInlinePillarSubScore(pillarId, subColId, memberCode, val) {
      const list = pillarSubColumns[pillarId] || [];
      const col = list.find((c) => c.id === subColId);
      const maxPts = col ? col.maxScore : 100;
      const clean = val.trim();

      if (clean !== "" && (isNaN(Number(clean)) || Number(clean) < 0 || Number(clean) > maxPts)) {
        showToast(`Max score for ${col ? col.title : subColId} is ${maxPts}`, "!");
        renderEvaluationMatrix();
        return;
      }

      if (!pillarScores[memberCode]) pillarScores[memberCode] = {};
      pillarScores[memberCode][subColId] = clean === "" ? null : Number(clean);

      saveState();
      renderEvaluationMatrix();
      renderBoardRankingsDashboard();
    }

    // Calculate each member's Total Score per Pillar + Weighted 100% Video Score + 45% Official Video Share
    function calculateAllMemberStats() {
      const stats = members.map((m) => {
        const pillarTotals = {}; // { [pillarId]: { rawSum, maxSum, pct0to1, earnedOf100, weight100, earnedOf45, maxOf45 } }
        const taskBreakdown = {};

        let totalVideo100 = 0; // Sum of earnedOf100 across the 6 pillars (max 100%)
        let grandRawSum = 0;
        let grandMaxSum = 0;

        FIXED_PILLARS.forEach((p) => {
          const weight100 = Number(p.weight100);
          const maxOf45 = weight100; // weights are already "% of the 45%"

          if (p.isTasksPillar) {
            let rawSum = 0;
            let maxSum = 0;
            tasks.forEach((t) => {
              const sub = submissions.find((s) => s.taskId === t.id && s.memberCode === m.code);
              const hasScore = sub && sub.score !== null && sub.score !== "";
              const sc = hasScore ? Number(sub.score) : 0;
              const mSc = Number(t.maxScore || 100);
              rawSum += sc;
              maxSum += mSc;

              taskBreakdown[t.id] = {
                score: hasScore ? Number(sub.score) : null,
                numScore: sc,
                maxScore: mSc,
                driveLink: sub?.driveLink || "",
                feedback: sub?.feedback || "",
                criteria: sub?.criteria || null
              };
            });

            // Per-criterion totals across all tasks (Deadline / Creativity / Quality)
            const criteria = {};
            TASK_CRITERIA.forEach((c) => {
              let earned = 0;
              let graded = false;
              tasks.forEach((t) => {
                const sb = submissions.find((s) => s.taskId === t.id && s.memberCode === m.code);
                const v = sb?.criteria?.[c.id];
                if (v !== null && v !== undefined && v !== "") {
                  earned += Number(v);
                  graded = true;
                }
              });
              criteria[c.id] = { earned, max: tasks.length * c.max, graded };
            });

            const ratio = maxSum > 0 ? rawSum / maxSum : 0;
            const earnedOf100 = Number((ratio * weight100).toFixed(2));
            const earnedOf45 = earnedOf100;

            pillarTotals[p.id] = {
              rawSum,
              maxSum,
              pct100: Number((ratio * 100).toFixed(1)),
              weight100,
              earnedOf100,
              maxOf45,
              earnedOf45,
              criteria
            };

            totalVideo100 += earnedOf100;
            grandRawSum += rawSum;
            grandMaxSum += maxSum;
          } else {
            const subCols = pillarSubColumns[p.id] || [];
            let rawSum = 0;
            let maxSum = 0;
            const subBreakdown = {};

            subCols.forEach((scCol) => {
              const rawVal = pillarScores[m.code]?.[scCol.id];
              const hasScore = rawVal !== undefined && rawVal !== null && rawVal !== "";
              const sc = hasScore ? Number(rawVal) : 0;
              const mSc = Number(scCol.maxScore || 100);
              rawSum += sc;
              maxSum += mSc;
              subBreakdown[scCol.id] = hasScore ? Number(rawVal) : null;
            });

            const ratio = maxSum > 0 ? rawSum / maxSum : 0;
            const earnedOf100 = Number((ratio * weight100).toFixed(2));
            const earnedOf45 = earnedOf100;

            pillarTotals[p.id] = {
              rawSum,
              maxSum,
              pct100: Number((ratio * 100).toFixed(1)),
              weight100,
              earnedOf100,
              maxOf45,
              earnedOf45,
              subBreakdown
            };

            totalVideo100 += earnedOf100;
            grandRawSum += rawSum;
            grandMaxSum += maxSum;
          }
        });

        // totalVideo100 is the sum of pillar points out of 45 (30 tasks + 5 x 3)
        const videoShare45 = Number(totalVideo100.toFixed(2));
        const weightedTotalPct100 = Number(((totalVideo100 / VIDEO_OFFICIAL_SHARE) * 100).toFixed(2));

        return {
          code: m.code,
          name: m.name,
          team: "Video",
          pillarTotals,
          taskBreakdown,
          grandTotal: grandRawSum,
          maxGrandTotal: grandMaxSum,
          weightedTotalPct100,
          videoShare45,
          overallRank: 1,
          tasksRank: 1
        };
      });

      // Overall Rank based on Final Video Score
      const byOverall = [...stats].sort(
        (a, b) => b.weightedTotalPct100 - a.weightedTotalPct100 || b.grandTotal - a.grandTotal
      );
      byOverall.forEach((item, idx) => {
        const target = stats.find((s) => s.code === item.code);
        if (target) target.overallRank = idx + 1;
      });

      // Tasks Pillar Rank
      const byTasks = [...stats].sort(
        (a, b) =>
          (b.pillarTotals["PIL-TASKS"]?.earnedOf100 || 0) -
          (a.pillarTotals["PIL-TASKS"]?.earnedOf100 || 0)
      );
      byTasks.forEach((item, idx) => {
        const target = stats.find((s) => s.code === item.code);
        if (target) target.tasksRank = idx + 1;
      });

      return stats;
    }

    function renderEvaluationMatrix() {
      const gridEl = document.getElementById("mindMapPillarsGrid");
      const toolbarEl = document.getElementById("pillarDrilldownToolbar");
      const thead = document.getElementById("matrixTableHead");
      const tbody = document.getElementById("matrixTableBody");
      const masterBtn = document.getElementById("btnMasterOverview");
      if (!gridEl || !thead || !tbody) return;

      const stats = calculateAllMemberStats();

      if (masterBtn) {
        if (activeMindMapPillarId === "ALL") {
          masterBtn.className = "bg-canvas-gold text-canvas-bg font-extrabold text-xs px-3.5 py-2 rounded-xl transition shadow";
        } else {
          masterBtn.className = "bg-canvas-bg hover:bg-canvas-elevated text-slate-200 border border-canvas-gold/40 font-extrabold text-xs px-3.5 py-2 rounded-xl transition";
        }
      }

      // 1. Render the 6 Fixed Pillar Nodes in the Mind Map
      gridEl.innerHTML = FIXED_PILLARS.map((p) => {
        const isSelected = activeMindMapPillarId === p.id;
        const subCount = p.isTasksPillar ? tasks.length : (pillarSubColumns[p.id] || []).length;
        const share45 = Number(p.weight100);

        return `
          <div
            onclick="selectMindMapPillar('${p.id}')"
            class="cursor-pointer rounded-2xl p-3.5 border-2 transition flex flex-col justify-between gap-2 ${
              isSelected
                ? "bg-canvas-gold/15 border-canvas-gold shadow-lg scale-[1.02]"
                : "bg-canvas-panel/90 border-canvas-border hover:border-canvas-gold/50"
            }"
          >
            <div class="flex items-center justify-between gap-1">
              <span class="text-base">${p.icon}</span>
              <span class="font-mono text-xs font-black px-2 py-0.5 rounded ${
                isSelected ? "bg-canvas-gold text-canvas-bg" : "bg-canvas-bg text-canvas-gold border border-canvas-gold/30"
              }">
                ${p.weight100}%
              </span>
            </div>

            <div>
              <div class="text-xs font-extrabold text-white leading-snug">${p.title}</div>
              <div class="text-[10px] font-mono text-slate-400 mt-0.5">
                = ${share45}% of 45%
              </div>
            </div>

            <div class="flex items-center justify-between pt-1.5 border-t border-canvas-border/70 text-[10px] font-mono">
              <span class="text-canvas-blue font-bold">${subCount} ${p.isTasksPillar ? "Tasks" : "Columns"}</span>
              <span class="${isSelected ? "text-canvas-gold font-bold" : "text-slate-400"}">
                ${isSelected ? "▼ Opened" : "▶ Open Branch"}
              </span>
            </div>
          </div>
        `;
      }).join("");

      // 2. Render Either MASTER 6-PILLARS SUMMARY TABLE or SELECTED PILLAR BRANCH TABLE
      if (activeMindMapPillarId === "ALL") {
        if (toolbarEl) {
          toolbarEl.innerHTML = `
            <div class="flex flex-wrap items-center gap-2">
              <span class="font-mono text-xs font-black px-2.5 py-1 rounded bg-canvas-gold text-canvas-bg">
                🗺️ MASTER 6-PILLARS SUMMARY
              </span>
              <span class="text-xs text-slate-300">
                Showing each member's <strong>Total Score</strong> in each of the 6 fixed pillars and the final sum out of <strong>100%</strong> (and <strong>45%</strong> Enactus Video Share). Click any column header to open its sub-columns!
              </span>
            </div>
            <div class="text-[11px] font-mono text-slate-400 shrink-0">
              Video = <strong class="text-canvas-gold">45%</strong> • HR = <strong class="text-slate-200">55%</strong>
            </div>
          `;
        }

        let headHtml = `
          <tr class="bg-canvas-elevated text-slate-200 border-b border-canvas-border">
            <th class="py-3 px-3 font-mono whitespace-nowrap sticky left-0 bg-canvas-elevated z-10">Code</th>
            <th class="py-3 px-3 whitespace-nowrap">Member Name</th>
        `;

        FIXED_PILLARS.forEach((p) => {
          const sh45 = Number(p.weight100);
          const subCount = p.isTasksPillar ? tasks.length : (pillarSubColumns[p.id] || []).length;
          headHtml += `
            <th onclick="selectMindMapPillar('${p.id}')" class="cursor-pointer hover:bg-canvas-gold/10 transition py-3 px-3 min-w-[165px] border-l border-canvas-border align-top">
              <div class="flex items-center justify-between gap-1">
                <span class="text-xs font-extrabold text-white">${p.icon} ${p.shortTitle}</span>
                <span class="font-mono text-[10px] font-black px-1.5 py-0.5 rounded bg-canvas-gold/20 text-canvas-gold border border-canvas-gold/40">${p.weight100}%</span>
              </div>
              <div class="text-[10px] font-mono text-slate-400 mt-1 flex items-center justify-between">
                <span>(${sh45}% of 45%)</span>
                <span class="text-canvas-blue underline">Open ${subCount} Cols ↗</span>
              </div>
            </th>
          `;
        });

        headHtml += `
            <th class="py-3 px-4 whitespace-nowrap border-l border-canvas-border font-mono text-canvas-gold bg-canvas-gold/10 text-center">
              <div>Total Video (100%)</div>
              <div class="text-[10px] text-slate-400 font-normal">Sum of 6 Pillars</div>
            </th>
            <th class="py-3 px-4 whitespace-nowrap border-l border-canvas-border font-mono text-emerald-400 bg-emerald-500/10 text-center">
              <div>Official Share (45%)</div>
              <div class="text-[10px] text-slate-400 font-normal">+ 55% HR = 100%</div>
            </th>
            <th class="py-3 px-3 whitespace-nowrap border-l border-canvas-border font-mono text-center">Rank</th>
          </tr>
        `;
        thead.innerHTML = headHtml;

        tbody.innerHTML = stats
          .map((st) => {
            let row = `
              <tr class="hover:bg-canvas-elevated/40 transition">
                <td class="py-2.5 px-3 font-mono font-bold text-canvas-gold sticky left-0 bg-canvas-panel">${st.code}</td>
                <td class="py-2.5 px-3 font-bold text-white whitespace-nowrap">${st.name}</td>
            `;

            FIXED_PILLARS.forEach((p) => {
              const pt = st.pillarTotals[p.id];
              row += `
                <td class="py-2.5 px-3 border-l border-canvas-border font-mono">
                  <div class="font-extrabold text-xs text-white">${pt.rawSum} <span class="text-slate-500 font-normal">/ ${pt.maxSum}</span></div>
                  <div class="text-[10px] text-canvas-gold font-bold mt-0.5">${pt.earnedOf100}% <span class="text-slate-500 font-normal">/ ${p.weight100}%</span></div>
                </td>
              `;
            });

            row += `
                <td class="py-2.5 px-4 border-l border-canvas-border font-mono bg-canvas-gold/5 text-center whitespace-nowrap">
                  <div class="font-black text-sm text-canvas-gold">${st.weightedTotalPct100}% <span class="text-xs text-slate-400 font-normal">/ 100%</span></div>
                </td>
                <td class="py-2.5 px-4 border-l border-canvas-border font-mono bg-emerald-500/5 text-center whitespace-nowrap">
                  <div class="font-black text-sm text-emerald-400">${st.videoShare45}% <span class="text-xs text-slate-400 font-normal">/ 45%</span></div>
                </td>
                <td class="py-2.5 px-3 border-l border-canvas-border text-center">
                  <span class="font-mono font-black text-xs px-2.5 py-1 rounded-lg ${
                    st.overallRank === 1
                      ? "bg-canvas-gold text-canvas-bg"
                      : st.overallRank <= 3
                      ? "bg-canvas-blue/20 text-canvas-blue border border-canvas-blue/40"
                      : "bg-canvas-bg text-slate-300 border border-canvas-border"
                  }">
                    #${st.overallRank}
                  </span>
                </td>
              </tr>
            `;
            return row;
          })
          .join("");
        return;
      }

      // 3. Render Drill-Down Sub-Columns Table for the Active Pillar
      const activePillar = FIXED_PILLARS.find((p) => p.id === activeMindMapPillarId) || FIXED_PILLARS[0];
      const sh45 = Number(activePillar.weight100);

      if (toolbarEl) {
        toolbarEl.innerHTML = `
          <div class="flex flex-wrap items-center gap-3">
            <span class="font-mono text-xs font-black px-3 py-1 rounded-lg bg-canvas-gold text-canvas-bg">
              ${activePillar.icon} ${activePillar.title.toUpperCase()} — FIXED WEIGHT: ${activePillar.weight100}% (= ${sh45}% OF 45%)
            </span>
            <span class="text-xs text-slate-300">
              ${
                activePillar.isTasksPillar
                  ? "Each Task is a column below. Set max score in Row 1, enter member task scores, and the Total Score is calculated automatically."
                  : "Add or rename sub-columns inside this pillar. Row 1 sets the column name & max score, and the Total Score is calculated automatically."
              }
            </span>
          </div>
          <div class="flex items-center gap-2 shrink-0">
            <button onclick="addSubColumnToPillar('${activePillar.id}')" class="bg-canvas-gold hover:bg-amber-400 text-canvas-bg font-extrabold text-xs px-3.5 py-2 rounded-xl transition shadow">
              + Add ${activePillar.isTasksPillar ? "New Task Column" : "Sub-Column"}
            </button>
            <button onclick="selectMindMapPillar('ALL')" class="bg-canvas-panel hover:bg-canvas-elevated text-slate-300 border border-canvas-border font-mono text-xs px-3 py-2 rounded-xl transition">
              ← Back to 6-Pillars Summary
            </button>
          </div>
        `;
      }

      if (activePillar.isTasksPillar) {
        // Render Tasks Branch Table
        let headHtml = `
          <tr class="bg-canvas-elevated text-slate-200 border-b border-canvas-border">
            <th class="py-3 px-3 font-mono whitespace-nowrap sticky left-0 bg-canvas-elevated z-10">Code</th>
            <th class="py-3 px-3 whitespace-nowrap">Member Name</th>
        `;

        tasks.forEach((t) => {
          headHtml += `
            <th class="py-2.5 px-3 min-w-[260px] border-l border-canvas-border bg-canvas-gold/5 align-top space-y-1.5">
              <div class="flex items-center gap-1">
                <input
                  type="text"
                  value="${t.title.replace(/"/g, "&quot;")}"
                  onchange="updateTaskHeaderTitleInline('${t.id}', this.value)"
                  title="Click to edit Task title"
                  class="w-full bg-canvas-bg border border-canvas-gold/40 focus:border-canvas-gold rounded-lg px-2 py-1 text-xs font-bold text-canvas-gold focus:outline-none"
                />
                ${
                  tasks.length > 1
                    ? `<button onclick="deleteTaskItem('${t.id}')" title="Delete Task" class="w-6 h-6 rounded-lg bg-red-500/15 hover:bg-red-500/30 text-red-400 flex items-center justify-center text-xs font-mono shrink-0">✕</button>`
                    : ""
                }
              </div>
              <div class="flex items-center justify-between gap-1 text-[10px] font-mono bg-canvas-bg/80 px-2 py-1 rounded border border-canvas-border">
                <span class="text-slate-400">Score Out Of:</span>
                <input
                  type="number"
                  min="1"
                  max="1000"
                  value="${t.maxScore}" readonly title="Fixed: Deadline 10 + Creativity 10 + Quality 10"
                  onchange="updateTaskHeaderMaxInline()"
                  class="w-14 bg-canvas-panel border border-canvas-border focus:border-canvas-gold rounded px-1 py-0.5 text-[10px] font-mono font-bold text-white text-center focus:outline-none"
                />
              </div>
            </th>
          `;
        });

        headHtml += `
            <th class="py-2.5 px-2.5 border-l border-canvas-border bg-canvas-bg/50 align-middle text-center">
              <button onclick="addSubColumnToPillar('PIL-TASKS')" class="px-2.5 py-2 rounded-xl bg-canvas-gold/15 hover:bg-canvas-gold hover:text-canvas-bg text-canvas-gold border border-canvas-gold/30 font-mono text-[11px] font-bold transition whitespace-nowrap">
                + Add Task
              </button>
            </th>
            <th class="py-3 px-4 whitespace-nowrap border-l border-canvas-border font-mono text-canvas-gold bg-canvas-gold/10 text-center">
              <div>Tasks Total Score</div>
              <div class="text-[10px] text-slate-400 font-normal">Sum of All Tasks</div>
            </th>
            <th class="py-3 px-4 whitespace-nowrap border-l border-canvas-border font-mono text-emerald-400 bg-emerald-500/10 text-center">
              <div>Pillar Share (${activePillar.weight100}%)</div>
              <div class="text-[10px] text-slate-400 font-normal">(${sh45}% of 45%)</div>
            </th>
          </tr>
        `;
        thead.innerHTML = headHtml;

        tbody.innerHTML = stats
          .map((st) => {
            const pt = st.pillarTotals["PIL-TASKS"];
            let row = `
              <tr class="hover:bg-canvas-elevated/40 transition">
                <td class="py-2.5 px-3 font-mono font-bold text-canvas-gold sticky left-0 bg-canvas-panel">${st.code}</td>
                <td class="py-2.5 px-3 font-bold text-white whitespace-nowrap">${st.name}</td>
            `;

            tasks.forEach((t) => {
              const tb = st.taskBreakdown[t.id];
              const val = tb && tb.score !== null ? tb.score : "";
              const hasLink = !!(tb && tb.driveLink);

              row += `
                <td class="py-2.5 px-3 border-l border-canvas-border align-middle">
                  <div class="flex items-center gap-1">
                    ${TASK_CRITERIA.map((c) => {
                      const cv = tb && tb.criteria && tb.criteria[c.id] !== null && tb.criteria[c.id] !== undefined ? tb.criteria[c.id] : "";
                      return `<div class="text-center">
                        <div class="text-[9px] font-mono text-slate-500">${c.title.slice(0, 3).toUpperCase()}/${c.max}</div>
                        <input type="number" min="0" max="${c.max}" step="0.5" value="${cv}" placeholder="—"
                          title="${c.title} (out of ${c.max})"
                          onchange="updateInlineTaskCriterion('${t.id}', '${st.code}', '${c.id}', this.value)"
                          class="w-12 bg-canvas-bg border border-canvas-border focus:border-canvas-gold rounded-lg px-1 py-1 text-xs font-mono font-bold text-white text-center focus:outline-none" />
                      </div>`;
                    }).join("")}
                    <div class="text-center px-1">
                      <div class="text-[9px] font-mono text-slate-500">TOTAL</div>
                      <div class="text-xs font-mono font-black text-canvas-gold">${tb && tb.score !== null ? tb.score : "—"}<span class="text-slate-500 font-normal">/${t.maxScore}</span></div>
                    </div>
                    ${
                      hasLink
                        ? `<a href="${tb.driveLink}" target="_blank" rel="noopener noreferrer" title="Open Submitted Google Drive Link" class="px-2 py-1 rounded bg-canvas-blue/15 text-canvas-blue hover:bg-canvas-blue hover:text-canvas-bg font-mono text-[10px] font-bold transition whitespace-nowrap">🔗</a>`
                        : ""
                    }
                  </div>
                </td>
              `;
            });

            row += `
                <td class="py-2.5 px-2 border-l border-canvas-border text-center text-[10px] text-slate-600 font-mono">•</td>
                <td class="py-2.5 px-4 border-l border-canvas-border font-mono bg-canvas-gold/5 text-center whitespace-nowrap">
                  <span class="font-black text-sm text-canvas-gold">${pt.rawSum}</span>
                  <span class="text-xs text-slate-400"> / ${pt.maxSum}</span>
                </td>
                <td class="py-2.5 px-4 border-l border-canvas-border font-mono bg-emerald-500/5 text-center whitespace-nowrap">
                  <div class="font-black text-sm text-emerald-400">${pt.earnedOf100}% <span class="text-xs text-slate-400 font-normal">/ ${activePillar.weight100}%</span></div>
                  <div class="text-[10px] text-slate-400">(${pt.earnedOf45}% of 45%)</div>
                </td>
              </tr>
            `;
            return row;
          })
          .join("");
        return;
      }

      // Render Non-Task Pillar Sub-Columns Table (Problem Solving, Collaboration, Alignment, Growth Mindset, Leadership)
      const subCols = pillarSubColumns[activePillar.id] || [];
      let headHtml = `
        <tr class="bg-canvas-elevated text-slate-200 border-b border-canvas-border">
          <th class="py-3 px-3 font-mono whitespace-nowrap sticky left-0 bg-canvas-elevated z-10">Code</th>
          <th class="py-3 px-3 whitespace-nowrap">Member Name</th>
      `;

      subCols.forEach((scCol) => {
        headHtml += `
          <th class="py-2.5 px-3 min-w-[185px] border-l border-canvas-border bg-canvas-blue/10 align-top space-y-1.5">
            <div class="flex items-center gap-1">
              <input
                type="text"
                value="${scCol.title.replace(/"/g, "&quot;")}"
                placeholder="Write Column Name..."
                onchange="updatePillarSubColTitle('${activePillar.id}', '${scCol.id}', this.value)"
                class="w-full bg-canvas-bg border border-canvas-blue/40 focus:border-canvas-blue rounded-lg px-2 py-1 text-xs font-bold text-canvas-blue focus:outline-none"
              />
              <button onclick="removePillarSubCol('${activePillar.id}', '${scCol.id}')" title="Delete Sub-Column" class="w-6 h-6 rounded-lg bg-red-500/15 hover:bg-red-500/30 text-red-400 flex items-center justify-center text-xs font-mono shrink-0">✕</button>
            </div>
            <div class="flex items-center justify-between gap-1 text-[10px] font-mono bg-canvas-bg/80 px-2 py-1 rounded border border-canvas-border">
              <span class="text-slate-400">Score Out Of:</span>
              <input
                type="number"
                min="1"
                max="1000"
                value="${scCol.maxScore}"
                onchange="updatePillarSubColMax('${activePillar.id}', '${scCol.id}', this.value)"
                class="w-14 bg-canvas-panel border border-canvas-border focus:border-canvas-blue rounded px-1 py-0.5 text-[10px] font-mono font-bold text-white text-center focus:outline-none"
              />
            </div>
          </th>
        `;
      });

      headHtml += `
          <th class="py-2.5 px-2.5 border-l border-canvas-border bg-canvas-bg/50 align-middle text-center">
            <button onclick="addSubColumnToPillar('${activePillar.id}')" class="px-2.5 py-2 rounded-xl bg-canvas-blue/15 hover:bg-canvas-blue hover:text-canvas-bg text-canvas-blue border border-canvas-blue/30 font-mono text-[11px] font-bold transition whitespace-nowrap">
              + New Col
            </button>
          </th>
          <th class="py-3 px-4 whitespace-nowrap border-l border-canvas-border font-mono text-canvas-blue bg-canvas-blue/10 text-center">
            <div>${activePillar.shortTitle} Total Score</div>
            <div class="text-[10px] text-slate-400 font-normal">Sum of Sub-Columns</div>
          </th>
          <th class="py-3 px-4 whitespace-nowrap border-l border-canvas-border font-mono text-emerald-400 bg-emerald-500/10 text-center">
            <div>Pillar Share (${activePillar.weight100}%)</div>
            <div class="text-[10px] text-slate-400 font-normal">(${sh45}% of 45%)</div>
          </th>
        </tr>
      `;
      thead.innerHTML = headHtml;

      tbody.innerHTML = stats
        .map((st) => {
          const pt = st.pillarTotals[activePillar.id];
          let row = `
            <tr class="hover:bg-canvas-elevated/40 transition">
              <td class="py-2.5 px-3 font-mono font-bold text-canvas-gold sticky left-0 bg-canvas-panel">${st.code}</td>
              <td class="py-2.5 px-3 font-bold text-white whitespace-nowrap">${st.name}</td>
          `;

          subCols.forEach((scCol) => {
            const val =
              pt.subBreakdown && pt.subBreakdown[scCol.id] !== null && pt.subBreakdown[scCol.id] !== undefined
                ? pt.subBreakdown[scCol.id]
                : "";
            row += `
              <td class="py-2.5 px-3 border-l border-canvas-border align-middle">
                <input
                  type="number"
                  min="0"
                  max="${scCol.maxScore}"
                  value="${val}"
                  placeholder="—"
                  onchange="updateInlinePillarSubScore('${activePillar.id}', '${scCol.id}', '${st.code}', this.value)"
                  class="w-24 bg-canvas-bg border border-canvas-border focus:border-canvas-blue rounded-lg px-2.5 py-1.5 text-xs font-mono font-bold text-canvas-blue text-center focus:outline-none"
                />
              </td>
            `;
          });

          row += `
              <td class="py-2.5 px-2 border-l border-canvas-border text-center text-[10px] text-slate-600 font-mono">•</td>
              <td class="py-2.5 px-4 border-l border-canvas-border font-mono bg-canvas-blue/5 text-center whitespace-nowrap">
                <span class="font-black text-sm text-canvas-blue">${pt.rawSum}</span>
                <span class="text-xs text-slate-400"> / ${pt.maxSum}</span>
              </td>
              <td class="py-2.5 px-4 border-l border-canvas-border font-mono bg-emerald-500/5 text-center whitespace-nowrap">
                <div class="font-black text-sm text-emerald-400">${pt.earnedOf100}% <span class="text-xs text-slate-400 font-normal">/ ${activePillar.weight100}%</span></div>
                <div class="text-[10px] text-slate-400">(${pt.earnedOf45}% of 45%)</div>
              </td>
            </tr>
          `;
          return row;
        })
        .join("");
    }

    // -----------------------------------------------------------------
    // 10. BOARD SUB-TAB 3: LOCKED RANKINGS DASHBOARD (BY PILLAR OR OVERALL)
    // -----------------------------------------------------------------
    function populateRankingSortOptions() {
      const sel = document.getElementById("rankingSortSelector");
      if (!sel) return;
      const currentVal = sel.value || "OVERALL";

      let html = `
        <option value="OVERALL">🏆 Overall Total Ranking (100% Video = 45% Enactus Share)</option>
      `;

      FIXED_PILLARS.forEach((p) => {
        html += `<option value="PILLAR:${p.id}">${p.icon} ${p.title} (${p.weight100}%)</option>`;
      });

      tasks.forEach((t) => {
        html += `<option value="TASK:${t.id}">🎬 Task Ranking: ${t.title} (Max ${t.maxScore})</option>`;
      });

      sel.innerHTML = html;
      if ([...sel.options].some((o) => o.value === currentVal)) {
        sel.value = currentVal;
      }
    }

    function renderBoardRankingsDashboard() {
      const podium = document.getElementById("rankingsTop3Podium");
      const thead = document.getElementById("rankingsTableHead");
      const tbody = document.getElementById("rankingsTableBody");
      if (!podium || !thead || !tbody) return;

      const sortMode = document.getElementById("rankingSortSelector")?.value || "OVERALL";
      const stats = calculateAllMemberStats();

      stats.forEach((st) => {
        if (sortMode === "OVERALL") {
          st.activeMetricScore = st.weightedTotalPct100;
          st.activeMetricMax = 100;
          st.activeMetricLabel = "Total Video Score (100%)";
          st.activeMetricUnit = "%";
        } else if (sortMode.startsWith("PILLAR:")) {
          const pid = sortMode.replace("PILLAR:", "");
          const pObj = FIXED_PILLARS.find((p) => p.id === pid);
          const pt = st.pillarTotals[pid];
          st.activeMetricScore = pt ? pt.rawSum : 0;
          st.activeMetricMax = pt ? pt.maxSum : 100;
          st.activeMetricLabel = pObj ? `${pObj.title} (${pObj.weight100}%)` : pid;
          st.activeMetricUnit = "";
        } else if (sortMode.startsWith("TASK:")) {
          const tid = sortMode.replace("TASK:", "");
          const tObj = tasks.find((t) => t.id === tid);
          const tb = st.taskBreakdown[tid];
          st.activeMetricScore = tb?.numScore || 0;
          st.activeMetricMax = tObj ? tObj.maxScore : 100;
          st.activeMetricLabel = tObj ? tObj.title : tid;
          st.activeMetricUnit = "";
        }
      });

      const sorted = [...stats].sort(
        (a, b) =>
          b.activeMetricScore - a.activeMetricScore ||
          b.weightedTotalPct100 - a.weightedTotalPct100 ||
          b.grandTotal - a.grandTotal
      );

      // Render Top 3 Podium
      const top3 = sorted.slice(0, 3);
      const medals = ["🥇 RANK #1", "🥈 RANK #2", "🥉 RANK #3"];
      const borderColors = ["border-canvas-gold", "border-canvas-blue/60", "border-amber-600/60"];

      podium.innerHTML = top3
        .map((m, i) => {
          return `
            <div class="bg-canvas-panel border-2 ${borderColors[i]} rounded-2xl p-5 space-y-3 shadow-xl">
              <div class="flex items-center justify-between">
                <span class="font-mono text-xs font-black px-2.5 py-1 rounded bg-canvas-gold text-canvas-bg">${medals[i]}</span>
                <span class="font-mono text-xs text-canvas-gold">${m.code}</span>
              </div>
              <div>
                <div class="text-lg font-extrabold text-white">${m.name}</div>
                <div class="text-xs text-slate-400">Overall Rank: #${m.overallRank} • Official Share: ${m.videoShare45}% / 45%</div>
              </div>
              <div class="bg-canvas-bg border border-canvas-border rounded-xl p-3 flex items-center justify-between">
                <span class="text-xs text-slate-400 truncate max-w-[140px]">${m.activeMetricLabel}</span>
                <span class="font-mono text-sm font-black text-emerald-400">${m.activeMetricScore}${m.activeMetricUnit} / ${m.activeMetricMax}${m.activeMetricUnit}</span>
              </div>
            </div>
          `;
        })
        .join("");

      thead.innerHTML = `
        <tr>
          <th class="py-3 px-4 font-mono">Rank</th>
          <th class="py-3 px-4 font-mono">Code</th>
          <th class="py-3 px-4">Member Name</th>
          <th class="py-3 px-4 font-mono text-canvas-gold">Selected Metric</th>
          <th class="py-3 px-4 font-mono text-white">Total Video (100%)</th>
          <th class="py-3 px-4 font-mono text-emerald-400">Official Share (45%)</th>
        </tr>
      `;

      tbody.innerHTML = sorted
        .map((m, idx) => {
          const rankNum = idx + 1;
          return `
            <tr class="hover:bg-canvas-elevated/50 transition">
              <td class="py-3 px-4 font-mono font-black text-sm ${rankNum <= 3 ? "text-canvas-gold" : "text-slate-400"}">
                #${rankNum}
              </td>
              <td class="py-3 px-4 font-mono font-bold text-canvas-gold">${m.code}</td>
              <td class="py-3 px-4 font-bold text-white">${m.name}</td>
              <td class="py-3 px-4 font-mono font-extrabold text-canvas-gold">
                ${m.activeMetricScore}${m.activeMetricUnit} <span class="text-slate-500 font-normal">/ ${m.activeMetricMax}${m.activeMetricUnit}</span>
              </td>
              <td class="py-3 px-4 font-mono font-extrabold text-white">
                ${m.weightedTotalPct100}% <span class="text-slate-500 font-normal">/ 100%</span>
              </td>
              <td class="py-3 px-4 font-mono font-extrabold text-emerald-400">
                <span class="px-2 py-0.5 rounded bg-emerald-500/15 border border-emerald-500/30 text-emerald-400 mr-1.5">Overall #${m.overallRank}</span>
                ${m.videoShare45}% / 45%
              </td>
            </tr>
          `;
        })
        .join("");
    }

    // -----------------------------------------------------------------
    // 11. BOARD SUB-TAB 4: UNLIMITED TASKS MANAGER
    // -----------------------------------------------------------------
    function createNewTaskItem() {
      const titleIn = document.getElementById("newTaskTitleInput");
      const briefIn = document.getElementById("newTaskBriefInput");
      const maxIn = document.getElementById("newTaskMaxInput");
      const dlIn = document.getElementById("newTaskDeadlineInput");

      const title = titleIn.value.trim();
      const brief = briefIn.value.trim();
      const maxScore = TASK_MAX_TOTAL;
      const deadlineISO = dlIn.value || "2026-12-15T23:59";

      if (!title) {
        showToast("Please enter a task title", "!");
        return;
      }

      const nextNum = tasks.length + 1;
      const newId = `TASK-${String(nextNum).padStart(2, "0")}-${Date.now().toString().slice(-2)}`;

      tasks.push({
        id: newId,
        title,
        brief: brief || "Upload your task files to Google Drive and submit the link.",
        maxScore,
        deadlineISO,
        forceLocked: false
      });

      titleIn.value = "";
      briefIn.value = "";
      activeSubmitTaskId = newId;
      activeBoardGradeTaskId = newId;

      saveState();
      refreshAllSelectorsAndViews();
      showToast(`Created ${title} (Score Out Of: ${maxScore})`);
    }

    function renderUnlimitedTasksManager() {
      const listEl = document.getElementById("boardTasksManagerList");
      const badgeEl = document.getElementById("tasksTotalCountBadge");
      if (!listEl) return;

      if (badgeEl) badgeEl.textContent = `${tasks.length} Tasks`;

      listEl.innerHTML = tasks
        .map((t, index) => {
          return `
            <div class="bg-canvas-panel border border-canvas-border rounded-2xl p-4 sm:p-5 space-y-3">
              <div class="flex flex-wrap items-center justify-between gap-2">
                <div class="flex items-center gap-2">
                  <span class="font-mono text-xs font-bold px-2.5 py-1 rounded bg-canvas-gold text-canvas-bg">Task #${index + 1}</span>
                  <span class="font-mono text-xs text-slate-400">${t.id}</span>
                  <span class="font-mono text-xs font-bold px-2.5 py-0.5 rounded bg-emerald-500/15 text-emerald-400 border border-emerald-500/30">
                    Score Out Of: ${t.maxScore}
                  </span>
                </div>
                <div class="flex items-center gap-2">
                  <button onclick="toggleTaskForceLock('${t.id}')" class="text-xs font-mono font-bold px-3 py-1 rounded-lg border transition ${
                    t.forceLocked
                      ? "bg-red-500/20 text-red-300 border-red-500/40"
                      : "bg-emerald-500/15 text-emerald-400 border-emerald-500/30"
                  }">
                    ${t.forceLocked ? "🔒 Locked (Click to Reopen)" : "● Open (Click to Force Lock)"}
                  </button>
                  ${
                    tasks.length > 1
                      ? `<button onclick="deleteTaskItem('${t.id}')" class="text-xs font-mono text-red-400 hover:underline px-2 py-1">Delete Task</button>`
                      : ""
                  }
                </div>
              </div>

              <div class="grid grid-cols-1 md:grid-cols-12 gap-3">
                <div class="md:col-span-4">
                  <label class="block text-[10px] font-mono text-slate-400 mb-1">Title</label>
                  <input id="edit-title-${t.id}" type="text" value="${t.title.replace(/"/g, "&quot;")}" class="w-full bg-canvas-bg border border-canvas-border rounded-xl px-3 py-2 text-xs text-white focus:outline-none" />
                </div>
                <div class="md:col-span-4">
                  <label class="block text-[10px] font-mono text-slate-400 mb-1">Brief</label>
                  <input id="edit-brief-${t.id}" type="text" value="${(t.brief || "").replace(/"/g, "&quot;")}" class="w-full bg-canvas-bg border border-canvas-border rounded-xl px-3 py-2 text-xs text-white focus:outline-none" />
                </div>
                <div class="md:col-span-2">
                  <label class="block text-[10px] font-mono text-slate-400 mb-1">Score Out Of</label>
                  <input id="edit-max-${t.id}" type="number" value="${t.maxScore}" readonly title="Fixed: Deadline 10 + Creativity 10 + Quality 10" class="w-full bg-canvas-bg border border-canvas-border rounded-xl px-3 py-2 text-xs font-mono text-white focus:outline-none" />
                </div>
                <div class="md:col-span-2">
                  <label class="block text-[10px] font-mono text-slate-400 mb-1">Deadline</label>
                  <input id="edit-dl-${t.id}" type="datetime-local" value="${t.deadlineISO.slice(0, 16)}" class="w-full bg-canvas-bg border border-canvas-border rounded-xl px-2.5 py-2 text-xs font-mono text-white focus:outline-none" />
                </div>
              </div>

              <div class="flex justify-end">
                <button onclick="saveEditedTask('${t.id}')" class="bg-canvas-elevated hover:bg-canvas-gold hover:text-canvas-bg text-slate-200 font-bold text-xs px-4 py-1.5 rounded-xl border border-canvas-border transition">
                  Save Changes
                </button>
              </div>
            </div>
          `;
        })
        .join("");
    }

    function saveEditedTask(taskId) {
      const task = tasks.find((t) => t.id === taskId);
      if (!task) return;

      const title = document.getElementById(`edit-title-${taskId}`)?.value.trim();
      const brief = document.getElementById(`edit-brief-${taskId}`)?.value.trim();
      const maxScore = TASK_MAX_TOTAL;
      const deadlineISO = document.getElementById(`edit-dl-${taskId}`)?.value;

      if (title) task.title = title;
      task.brief = brief;
      task.maxScore = maxScore;
      if (deadlineISO) task.deadlineISO = deadlineISO;

      saveState();
      refreshAllSelectorsAndViews();
      showToast(`Task updated (Score Out Of: ${task.maxScore})`);
    }

    function toggleTaskForceLock(taskId) {
      const task = tasks.find((t) => t.id === taskId);
      if (!task) return;
      task.forceLocked = !task.forceLocked;
      saveState();
      refreshAllSelectorsAndViews();
      showToast(task.forceLocked ? "Task Force-Locked" : "Task Reopened");
    }

    function deleteTaskItem(taskId) {
      if (tasks.length <= 1) return;
      tasks = tasks.filter((t) => t.id !== taskId);
      if (activeSubmitTaskId === taskId) activeSubmitTaskId = tasks[0].id;
      if (activeBoardGradeTaskId === taskId) activeBoardGradeTaskId = tasks[0].id;
      saveState();
      refreshAllSelectorsAndViews();
      showToast("Task deleted");
    }

    // -----------------------------------------------------------------
    // 12. EXCEL / CSV EXPORTERS (SEPARATE & ALL-IN-ONE MASTER EXPORTS)
    // -----------------------------------------------------------------
    function downloadCSVFile(filename, headers, rows) {
      const escapeCell = (val) => {
        const str = val === null || val === undefined ? "" : String(val);
        return `"${str.replace(/"/g, '""')}"`;
      };
      const csvContent =
        "\uFEFF" +
        [headers.map(escapeCell).join(","), ...rows.map((r) => r.map(escapeCell).join(","))].join("\r\n");

      const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = filename;
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
    }

    // 1) Export Currently Selected Task Submissions, Drive Links, Scores & Feedback
    function exportSingleTaskSubmissionsExcel() {
      const task = tasks.find((t) => t.id === activeBoardGradeTaskId) || tasks[0];
      if (!task) return;

      const headers = [
        "Member Name",
        "Private Secret Code",
        "Team",
        "Task Title",
        "Submission Status",
        "Submitted Timestamp",
        "Google Drive Link",
        "Member Delivery Note",
        ...TASK_CRITERIA.map((c) => `${c.title} (Out of ${c.max})`),
        `Score (Out of ${task.maxScore})`,
        "Score %",
        "Reviewed By (Board)",
        "Board Feedback"
      ];

      const rows = members.map((m) => {
        const sub = submissions.find(
          (s) => s.taskId === task.id && (s.memberCode === m.code || s.memberId === m.id)
        );
        const hasLink = !!(sub && sub.driveLink);
        const hasScore = sub && sub.score !== null && sub.score !== "";
        return [
          m.name,
          m.code,
          "Video",
          task.title,
          hasScore ? "GRADED" : hasLink ? "SUBMITTED" : "NOT SUBMITTED",
          sub?.timestamp || "",
          sub?.driveLink || "",
          sub?.note || "",
          ...TASK_CRITERIA.map((c) => {
            const v = sub?.criteria?.[c.id];
            return v === null || v === undefined ? "" : v;
          }),
          hasScore ? sub.score : "",
          hasScore ? `${Number(((sub.score / task.maxScore) * 100).toFixed(2))}%` : "",
          sub?.evaluator || "",
          sub?.feedback || ""
        ];
      });

      const safeTitle = task.title.replace(/[^a-zA-Z0-9_-]/g, "_").slice(0, 30);
      downloadCSVFile(`Enactus_Video_${safeTitle}_Submissions.csv`, headers, rows);
      showToast(`Exported ${task.title} & Drive Links to Excel`, "📊");
    }

    // 2) Export All Tasks Submissions, Drive Links, Scores & Feedback
    function exportAllTasksSubmissionsExcel() {
      const headers = [
        "Member Name",
        "Private Secret Code",
        "Team"
      ];

      tasks.forEach((t) => {
        headers.push(
          `${t.title} - Drive Link`,
          `${t.title} - Timestamp`,
          `${t.title} - Member Note`,
          `${t.title} - Score (/${t.maxScore})`,
          `${t.title} - Evaluator`,
          `${t.title} - Feedback`
        );
      });

      headers.push("Total Tasks Raw Score", "TASK QUALITY & RELIABILITY (30%)");

      const stats = calculateAllMemberStats();
      const rows = stats.map((st) => {
        const row = [st.name, st.code, "Video"];
        tasks.forEach((t) => {
          const sub = submissions.find(
            (s) => s.taskId === t.id && s.memberCode === st.code
          );
          row.push(
            sub?.driveLink || "",
            sub?.timestamp || "",
            sub?.note || "",
            sub && sub.score !== null && sub.score !== "" ? sub.score : "",
            sub?.evaluator || "",
            sub?.feedback || ""
          );
        });
        const pt = st.pillarTotals["PIL-TASKS"];
        row.push(`${pt.rawSum} out of ${pt.maxSum}`, `${pt.earnedOf100}% / ${FIXED_PILLARS[0].weight100}%`);
        return row;
      });

      downloadCSVFile("Enactus_Video_All_Tasks_And_Drive_Links.csv", headers, rows);
      showToast("Exported All Tasks & Google Drive Links to Excel", "📊");
    }

    // ===== NEW: Evaluation export — one row per evaluation item =====
    // Columns: Name | Code | Evaluation Type | Criterion | Points earned | Points out of | % earned | Weight (% of 45) | Gained (% of 45) | extras...
    function exportEvaluationDetailedExcel(pillarId) {
      const r2 = (n) => Number(Number(n).toFixed(2));
      const pct = (n) => `${r2(n * 100)}%`;
      const stats = calculateAllMemberStats();
      const pillars = FIXED_PILLARS.filter((p) => !pillarId || p.id === pillarId);

      const headers = [
        "Member Name",
        "Private Secret Code",
        "Evaluation Type",
        "Criterion",
        "Points Earned",
        "Points Out Of",
        "Percentage Earned (of this item)",
        "Weight (% out of 45%)",
        "Score Gained (% of 45%)",
        "Team",
        "Overall Rank",
        "Total Video Score (100%)",
        "Official Video Share (45%)"
      ];

      const rows = [];
      stats.forEach((st) => {
        const base = [st.name, st.code];
        const tail = ["Video", `#${st.overallRank}`, `${st.weightedTotalPct100}%`, `${st.videoShare45}%`];

        pillars.forEach((p) => {
          const pt = st.pillarTotals[p.id];
          const items = [];

          if (p.isTasksPillar) {
            TASK_CRITERIA.forEach((c) => {
              const cr = pt.criteria && pt.criteria[c.id];
              items.push({ title: c.title, pts: cr && cr.graded ? cr.earned : null, max: cr ? cr.max : 0, weight: c.weight });
            });
          } else {
            (pillarSubColumns[p.id] || []).forEach((sc) => {
              const v = pt.subBreakdown ? pt.subBreakdown[sc.id] : null;
              const mx = Number(sc.maxScore || 100);
              items.push({
                title: sc.title,
                pts: v === null || v === undefined ? null : v,
                max: mx,
                weight: pt.maxSum > 0 ? (p.weight100 * mx) / pt.maxSum : 0
              });
            });
          }

          items.forEach((it) => {
            const ratio = it.pts !== null && it.max > 0 ? it.pts / it.max : null;
            rows.push([
              ...base, p.title, it.title,
              it.pts === null ? "" : it.pts,
              it.max,
              ratio === null ? "" : pct(ratio),
              `${r2(it.weight)}%`,
              `${ratio === null ? 0 : r2(ratio * it.weight)}%`,
              ...tail
            ]);
          });

          rows.push([
            ...base, p.title, "PILLAR TOTAL",
            pt.rawSum, pt.maxSum,
            pct(pt.maxSum > 0 ? pt.rawSum / pt.maxSum : 0),
            `${p.weight100}%`,
            `${pt.earnedOf100}%`,
            ...tail
          ]);
        });

        if (!pillarId) {
          rows.push([
            ...base, "ALL PILLARS", "GRAND TOTAL",
            st.videoShare45, VIDEO_OFFICIAL_SHARE,
            pct(st.videoShare45 / VIDEO_OFFICIAL_SHARE),
            `${VIDEO_OFFICIAL_SHARE}%`,
            `${st.videoShare45}%`,
            ...tail
          ]);
        }
      });

      const suffix = pillarId ? FIXED_PILLARS.find((p) => p.id === pillarId).shortTitle.replace(/[^a-zA-Z0-9_-]/g, "_") : "All";
      downloadCSVFile(`Enactus_Video_Evaluation_Points_And_Percent_${suffix}.csv`, headers, rows);
      showToast("Exported Evaluation (points & percentages) to Excel", "📊");
    }

    // 3) Export Currently Opened Mind Map Pillar (Alone)
    function exportActivePillarExcel() {
      if (activeMindMapPillarId === "ALL") {
        exportFullMatrixCSV();
        return;
      }

      const activePillar = FIXED_PILLARS.find((p) => p.id === activeMindMapPillarId);
      if (!activePillar) return;

      if (activePillar.isTasksPillar) {
        exportAllTasksSubmissionsExcel();
        return;
      }

      const subCols = pillarSubColumns[activePillar.id] || [];
      const sh45 = Number(((activePillar.weight100 / 100) * VIDEO_OFFICIAL_SHARE).toFixed(2));
      const headers = [
        "Member Name",
        "Private Secret Code",
        "Team",
        ...subCols.map((c) => `${c.title} (Out of ${c.maxScore})`),
        `${activePillar.title} Total Score`,
        `Pillar Share (Out of ${activePillar.weight100}%)`,
        `Official 45% Share (Out of ${sh45}%)`
      ];

      const stats = calculateAllMemberStats();
      const rows = stats.map((st) => {
        const pt = st.pillarTotals[activePillar.id];
        return [
          st.name,
          st.code,
          "Video",
          ...subCols.map((c) =>
            pt.subBreakdown && pt.subBreakdown[c.id] !== null && pt.subBreakdown[c.id] !== undefined
              ? pt.subBreakdown[c.id]
              : ""
          ),
          `${pt.rawSum} out of ${pt.maxSum}`,
          `${pt.earnedOf100}%`,
          `${pt.earnedOf45}%`
        ];
      });

      const safeName = activePillar.shortTitle.replace(/[^a-zA-Z0-9_-]/g, "_");
      downloadCSVFile(`Enactus_Video_${safeName}_Evaluation.csv`, headers, rows);
      showToast(`Exported ${activePillar.title} to Excel`, "📊");
    }

    // 4) Export 6-Pillars Summary Matrix
    function exportFullMatrixCSV() {
      const stats = calculateAllMemberStats();
      // Max points of each pillar are the same for every member, so they go in the header
      const pillarMax = (p) =>
        p.isTasksPillar
          ? tasks.reduce((sum, t) => sum + Number(t.maxScore || 0), 0)
          : (pillarSubColumns[p.id] || []).reduce((sum, c) => sum + Number(c.maxScore || 0), 0);

      const headers = [
        "Overall Rank",
        "Private Secret Code",
        "Member Name",
        "Team",
        ...FIXED_PILLARS.map((p) => `${p.title} Score (out of ${pillarMax(p)})`),
        "Total Video Score (100%)",
        "Official Video Share (45%)"
      ];

      // Scores are written as plain numbers (no "x / y" text) so Excel never turns them into dates
      const rows = stats.map((st) => [
        `#${st.overallRank}`,
        st.code,
        st.name,
        "Video",
        ...FIXED_PILLARS.map((p) => st.pillarTotals[p.id].rawSum),
        `${st.weightedTotalPct100}%`,
        `${st.videoShare45}%`
      ]);

      downloadCSVFile("Enactus_Video_6Pillars_Summary_45pct.csv", headers, rows);
      showToast("Exported 6-Pillars Summary to Excel", "📊");
    }

    // 5) Export Rankings Leaderboard
    function exportRankingsExcel() {
      const stats = calculateAllMemberStats();
      const sorted = [...stats].sort(
        (a, b) => b.weightedTotalPct100 - a.weightedTotalPct100 || b.grandTotal - a.grandTotal
      );

      const headers = [
        "Overall Rank",
        "Member Name",
        "Private Secret Code",
        "Total Video Score (100%)",
        "Official Video Share (45%)",
        ...FIXED_PILLARS.map((p) => `${p.title} (${p.weight100}%)`)
      ];

      const rows = sorted.map((st) => [
        `#${st.overallRank}`,
        st.name,
        st.code,
        `${st.weightedTotalPct100}%`,
        `${st.videoShare45}%`,
        ...FIXED_PILLARS.map((p) => `${st.pillarTotals[p.id].earnedOf100}% (${st.pillarTotals[p.id].rawSum}/${st.pillarTotals[p.id].maxSum})`)
      ]);

      downloadCSVFile("Enactus_Video_Rankings_Leaderboard.csv", headers, rows);
      showToast("Exported Rankings Leaderboard to Excel", "🏆");
    }

    // 6) Export Team Roster & Private Secret Codes
    function exportRosterCodesExcel() {
      const headers = ["#", "Member Full Name", "Team", "Private Secret Code"];
      const rows = members.map((m, idx) => [idx + 1, m.name, "Video", m.code]);
      downloadCSVFile("Enactus_Video_Team_Roster_Private_Codes.csv", headers, rows);
      showToast("Exported Team Roster & Secret Codes to Excel", "👥");
    }

    // 7) Export EVERYTHING All-in-One Master Excel (Roster + All Tasks & Drive Links + All Sub-Columns + 6 Pillars + Totals & Rank)
    function exportEverythingMasterExcel() {
      const stats = calculateAllMemberStats();
      const headers = [
        "Overall Rank",
        "Member Name",
        "Private Secret Code",
        "Team",
        "Total Video Score (100%)",
        "Official Video Share (45%)"
      ];

      // Add all Tasks (Score + Drive Link + Feedback + Timestamp)
      tasks.forEach((t) => {
        headers.push(
          `${t.title} - Score (/${t.maxScore})`,
          `${t.title} - Drive Link`,
          `${t.title} - Feedback`,
          `${t.title} - Timestamp`
        );
      });
      headers.push("TASK QUALITY AND RELIABILITY Total", "TASK QUALITY AND RELIABILITY (30%)");

      // Add all other 5 Pillars and their Sub-Columns
      FIXED_PILLARS.filter((p) => !p.isTasksPillar).forEach((p) => {
        const subCols = pillarSubColumns[p.id] || [];
        subCols.forEach((c) => {
          headers.push(`${p.shortTitle} -> ${c.title} (/${c.maxScore})`);
        });
        headers.push(`${p.title} Total Score`, `${p.title} (${p.weight100}%)`);
      });

      const rows = stats.map((st) => {
        const row = [
          `#${st.overallRank}`,
          st.name,
          st.code,
          "Video",
          `${st.weightedTotalPct100}%`,
          `${st.videoShare45}%`
        ];

        tasks.forEach((t) => {
          const sub = submissions.find((s) => s.taskId === t.id && s.memberCode === st.code);
          row.push(
            sub && sub.score !== null && sub.score !== "" ? sub.score : "",
            sub?.driveLink || "",
            sub?.feedback || "",
            sub?.timestamp || ""
          );
        });

        const taskPillar = st.pillarTotals["PIL-TASKS"];
        row.push(`${taskPillar.rawSum} out of ${taskPillar.maxSum}`, `${taskPillar.earnedOf100}%`);

        FIXED_PILLARS.filter((p) => !p.isTasksPillar).forEach((p) => {
          const pt = st.pillarTotals[p.id];
          const subCols = pillarSubColumns[p.id] || [];
          subCols.forEach((c) => {
            const val =
              pt.subBreakdown && pt.subBreakdown[c.id] !== null && pt.subBreakdown[c.id] !== undefined
                ? pt.subBreakdown[c.id]
                : "";
            row.push(val);
          });
          row.push(`${pt.rawSum} out of ${pt.maxSum}`, `${pt.earnedOf100}%`);
        });

        return row;
      });

      downloadCSVFile("Enactus_Video_Master_All_In_One_Data.csv", headers, rows);
      showToast("Exported Complete All-in-One Master Excel File!", "📊");
    }

    // -----------------------------------------------------------------
    // 13. VISIBLE INTERACTIVE CINEMA BACKGROUND & MOUSE PARALLAX
    // -----------------------------------------------------------------
    function initInteractiveCinemaBackground() {
      const canvas = document.getElementById("cinemaBgCanvas");
      if (!canvas) return;
      const ctx = canvas.getContext("2d");

      let width = (canvas.width = window.innerWidth);
      let height = (canvas.height = window.innerHeight);

      let mouseX = width / 2;
      let mouseY = height / 2;
      let targetX = width / 2;
      let targetY = height / 2;

      window.addEventListener("resize", () => {
        width = canvas.width = window.innerWidth;
        height = canvas.height = window.innerHeight;
      });

      // Track Mouse for Spotlight, Card Borders & Parallax
      window.addEventListener("mousemove", (e) => {
        targetX = e.clientX;
        targetY = e.clientY;

        document.documentElement.style.setProperty("--cursor-x", `${e.clientX}px`);
        document.documentElement.style.setProperty("--cursor-y", `${e.clientY}px`);

        // Update card glow coordinates
        document.querySelectorAll(".vf-frame").forEach((card) => {
          const rect = card.getBoundingClientRect();
          card.style.setProperty("--card-mouse-x", `${e.clientX - rect.left}px`);
          card.style.setProperty("--card-mouse-y", `${e.clientY - rect.top}px`);
        });

        // Subtle Parallax on Camera & Origami Watermarks
        const normX = (e.clientX / width - 0.5) * 2;
        const normY = (e.clientY / height - 0.5) * 2;
        const camRight = document.getElementById("parallaxCameraRight");
        const logoLeft = document.getElementById("parallaxLogoLeft");
        if (camRight) {
          camRight.style.transform = `translate3d(${normX * -18}px, ${normY * -18}px, 0)`;
        }
        if (logoLeft) {
          logoLeft.style.transform = `translate3d(${normX * 16}px, ${normY * 16}px, 0)`;
        }
      });

      // Create Floating Cinema Bokeh Particles (Enactus Gold & Cinema Blue)
      const particleCount = 34;
      const particles = Array.from({ length: particleCount }, (_, i) => ({
        x: Math.random() * width,
        y: Math.random() * height,
        r: Math.random() * 2.6 + 1.1,
        vx: (Math.random() - 0.5) * 0.35,
        vy: (Math.random() - 0.5) * 0.35,
        color: i % 3 === 0 ? "#38BDF8" : "#FFC222",
        alpha: Math.random() * 0.45 + 0.18
      }));

      function animateBg() {
        mouseX += (targetX - mouseX) * 0.08;
        mouseY += (targetY - mouseY) * 0.08;

        ctx.clearRect(0, 0, width, height);

        // 1. Ambient Cinema Studio Glows
        const gradGold = ctx.createRadialGradient(
          width * 0.18 + (mouseX - width / 2) * 0.05,
          height * 0.22 + (mouseY - height / 2) * 0.05,
          20,
          width * 0.18,
          height * 0.22,
          460
        );
        gradGold.addColorStop(0, "rgba(255, 194, 34, 0.11)");
        gradGold.addColorStop(1, "rgba(9, 11, 16, 0)");
        ctx.fillStyle = gradGold;
        ctx.fillRect(0, 0, width, height);

        const gradBlue = ctx.createRadialGradient(
          width * 0.82 - (mouseX - width / 2) * 0.05,
          height * 0.75 - (mouseY - height / 2) * 0.05,
          20,
          width * 0.82,
          height * 0.75,
          480
        );
        gradBlue.addColorStop(0, "rgba(56, 189, 248, 0.09)");
        gradBlue.addColorStop(1, "rgba(9, 11, 16, 0)");
        ctx.fillStyle = gradBlue;
        ctx.fillRect(0, 0, width, height);

        // 2. Parallax Perspective Grid
        const gridSize = 44;
        const offsetX = ((mouseX - width / 2) * -0.03) % gridSize;
        const offsetY = ((mouseY - height / 2) * -0.03) % gridSize;

        ctx.strokeStyle = "rgba(38, 47, 69, 0.38)";
        ctx.lineWidth = 1;
        ctx.beginPath();
        for (let x = offsetX; x < width; x += gridSize) {
          ctx.moveTo(x, 0);
          ctx.lineTo(x, height);
        }
        for (let y = offsetY; y < height; y += gridSize) {
          ctx.moveTo(0, y);
          ctx.lineTo(width, y);
        }
        ctx.stroke();

        // 3. Floating Bokeh Particles & Mouse Constellation Lines
        particles.forEach((p) => {
          p.x += p.vx;
          p.y += p.vy;

          if (p.x < 0) p.x = width;
          if (p.x > width) p.x = 0;
          if (p.y < 0) p.y = height;
          if (p.y > height) p.y = 0;

          const dx = mouseX - p.x;
          const dy = mouseY - p.y;
          const dist = Math.hypot(dx, dy);

          // Draw subtle connection line when near cursor
          if (dist < 170) {
            ctx.beginPath();
            ctx.moveTo(p.x, p.y);
            ctx.lineTo(mouseX, mouseY);
            ctx.strokeStyle =
              p.color === "#FFC222"
                ? `rgba(255, 194, 34, ${(1 - dist / 170) * 0.22})`
                : `rgba(56, 189, 248, ${(1 - dist / 170) * 0.22})`;
            ctx.lineWidth = 1;
            ctx.stroke();
          }

          ctx.beginPath();
          ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
          ctx.fillStyle = p.color;
          ctx.globalAlpha = p.alpha;
          ctx.fill();
          ctx.globalAlpha = 1;
        });

        requestAnimationFrame(animateBg);
      }

      animateBg();
    }

    window.onload = function () {
      initApp();
      initInteractiveCinemaBackground();
    };
  </script>
</body>
</html>
