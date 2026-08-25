<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>REV Engine — Gen 1 | Mod Engine untuk Minecraft</title>
<link rel="icon" type="image/png" href="favicon.png">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Oswald:wght@500;600;700&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#08080a; --bg-alt:#101014; --panel:#131316; --line:#232329;
    --text:#f2f0eb; --text-dim:#8b8b93;
    --idle:#6fffb0; --mid:#ffb020; --hot:#ff5a1f; --hot-2:#ff2e2e;
  }
  *{box-sizing:border-box; margin:0; padding:0;}
  html{scroll-behavior:smooth;}
  body{background:var(--bg); color:var(--text); font-family:'Inter',sans-serif; line-height:1.55; overflow-x:hidden;}
  ::selection{background:var(--hot); color:#0a0a0a;}
  a{color:inherit;}
  .mono{font-family:'JetBrains Mono',monospace;}
  .display{font-family:'Oswald',sans-serif; text-transform:uppercase;}
  .wrap{max-width:1180px; margin:0 auto; padding:0 24px;}
  @media(prefers-reduced-motion: reduce){ *{animation-duration:0.01ms !important; animation-iteration-count:1 !important; transition-duration:0.01ms !important;} }

  header{position:sticky; top:0; z-index:50; background:rgba(8,8,10,0.85); backdrop-filter:blur(10px); border-bottom:1px solid var(--line);}
  nav{display:flex; align-items:center; justify-content:space-between; padding:14px 24px; max-width:1180px; margin:0 auto;}
  .brand{display:flex; align-items:center; gap:10px;}
  .brand img{height:34px; width:auto; display:block;}
  .brand span{font-family:'Oswald',sans-serif; font-weight:600; letter-spacing:0.06em; font-size:14px; color:var(--text-dim);}
  .navlinks{display:flex; gap:28px; font-size:14px; color:var(--text-dim);}
  .navlinks a{text-decoration:none; transition:color .2s;}
  .navlinks a:hover{color:var(--text);}
  a:focus-visible, button:focus-visible, input:focus-visible, select:focus-visible{outline:2px solid var(--hot); outline-offset:2px;}
  .cta-nav{background:linear-gradient(90deg,var(--hot),var(--hot-2)); color:#0a0a0a; font-weight:700; font-size:13px; padding:9px 18px; border-radius:3px; text-decoration:none;}
  @media(max-width:760px){ .navlinks{display:none;} }

  .hero{position:relative; padding:88px 24px 64px; text-align:center; overflow:hidden; border-bottom:1px solid var(--line);}
  .hero::before{content:''; position:absolute; inset:0; background:radial-gradient(ellipse 700px 400px at 50% -10%, rgba(255,90,31,0.18), transparent 60%), repeating-linear-gradient(90deg, rgba(255,255,255,0.025) 0 1px, transparent 1px 64px); pointer-events:none;}
  .eyebrow{display:inline-flex; align-items:center; gap:8px; font-family:'JetBrains Mono',monospace; font-size:12px; letter-spacing:0.12em; color:var(--mid); border:1px solid var(--line); background:var(--bg-alt); padding:6px 14px; border-radius:999px; margin-bottom:26px;}
  .eyebrow .dot{width:6px; height:6px; border-radius:50%; background:var(--idle); box-shadow:0 0 8px var(--idle); animation:pulse 1.8s ease-in-out infinite;}
  @keyframes pulse{0%,100%{opacity:1;}50%{opacity:.35;}}
  .hero h1{font-size:clamp(44px, 9vw, 96px); font-weight:700; line-height:0.92; letter-spacing:-0.01em; position:relative; z-index:1;}
  .hero h1 .engine{display:block; background:linear-gradient(90deg,#fff, var(--mid) 55%, var(--hot)); -webkit-background-clip:text; background-clip:text; color:transparent;}
  .gen-tag{display:inline-block; margin-top:16px; font-family:'JetBrains Mono',monospace; font-size:13px; color:var(--text-dim); border:1px solid var(--line); padding:4px 12px; border-radius:3px;}
  .hero p.lead{max-width:560px; margin:22px auto 0; color:var(--text-dim); font-size:16px;}
  .hero-cta{display:flex; gap:14px; justify-content:center; margin-top:32px; flex-wrap:wrap;}
  .btn{font-family:'Inter',sans-serif; font-weight:600; font-size:15px; padding:13px 26px; border-radius:3px; text-decoration:none; cursor:pointer; border:1px solid transparent; transition:transform .15s ease;}
  .btn:hover{transform:translateY(-2px);}
  .btn-hot{background:linear-gradient(90deg,var(--hot),var(--hot-2)); color:#0a0a0a; box-shadow:0 8px 30px -8px rgba(255,90,31,0.6);}
  .btn-ghost{background:transparent; border-color:var(--line); color:var(--text);}
  .btn-ghost:hover{border-color:var(--text-dim);}

  .tach-wrap{margin:48px auto 0; max-width:460px; position:relative; z-index:1;}
  svg#tach{width:100%; height:auto; display:block;}
  .tach-needle{transform-origin:250px 250px; transition:transform 1.2s cubic-bezier(.2,.9,.2,1);}
  .tach-label{font-family:'JetBrains Mono',monospace; font-size:11px; letter-spacing:.08em; fill:var(--text-dim);}
  .tach-readout{font-family:'Oswald',sans-serif; font-weight:600; fill:var(--text); font-size:14px;}

  section{padding:80px 24px;}
  .section-head{max-width:640px; margin:0 auto 44px; text-align:center;}
  .section-head .kicker{font-family:'JetBrains Mono',monospace; color:var(--mid); font-size:12px; letter-spacing:.12em; display:block; margin-bottom:10px;}
  .section-head h2{font-size:clamp(26px,4.5vw,40px); font-weight:600; line-height:1.1;}
  .section-head p{color:var(--text-dim); margin-top:12px; font-size:15.5px;}

  /* STUDIO */
  .studio{background:var(--bg-alt); border-top:1px solid var(--line); border-bottom:1px solid var(--line);}
  .studio-shell{max-width:1080px; margin:0 auto; border:1px solid var(--line); border-radius:8px; overflow:hidden; background:var(--panel);}
  .studio-tabs{display:flex; border-bottom:1px solid var(--line);}
  .studio-tab{flex:1; padding:16px; text-align:center; background:transparent; border:none; color:var(--text-dim); font-family:'Oswald',sans-serif; text-transform:uppercase; font-size:14px; letter-spacing:.04em; cursor:pointer; border-bottom:2px solid transparent;}
  .studio-tab.active{color:var(--text); border-bottom-color:var(--hot); background:rgba(255,90,31,0.06);}
  .studio-body{display:grid; grid-template-columns:1fr 300px;}
  @media(max-width:820px){.studio-body{grid-template-columns:1fr;}}

  .canvas-col{padding:20px; border-right:1px solid var(--line);}
  @media(max-width:820px){.canvas-col{border-right:none; border-bottom:1px solid var(--line);}}
  #editorCanvas{width:100%; aspect-ratio:4/3; border-radius:6px; background:#050506; display:block; cursor:crosshair; border:1px solid var(--line);}
  .canvas-hint{font-family:'JetBrains Mono',monospace; font-size:11px; color:var(--text-dim); margin-top:10px;}
  .toolbar{display:flex; gap:8px; flex-wrap:wrap; margin-top:14px;}
  .tool-btn{background:var(--bg); border:1px solid var(--line); color:var(--text); padding:8px 14px; border-radius:5px; font-size:13px; cursor:pointer;}
  .tool-btn.active{border-color:var(--mid); color:var(--mid); background:rgba(255,176,32,0.08);}
  .swatches{display:flex; gap:8px; margin-top:14px;}
  .swatch{width:28px; height:28px; border-radius:5px; cursor:pointer; border:2px solid transparent;}
  .swatch.active{border-color:#fff; transform:scale(1.1);}
  .layer-row{display:flex; align-items:center; gap:10px; margin-top:14px; font-family:'JetBrains Mono',monospace; font-size:12.5px; color:var(--text-dim);}
  .layer-row button{background:var(--bg); border:1px solid var(--line); color:var(--text); width:28px; height:28px; border-radius:4px; cursor:pointer;}
  .save-row{display:flex; gap:8px; margin-top:16px;}
  .save-row input{flex:1; background:var(--bg); border:1px solid var(--line); color:var(--text); padding:10px 12px; border-radius:5px; font-size:13px;}

  .side-col{padding:20px; display:flex; flex-direction:column; gap:18px;}
  .side-title{font-family:'JetBrains Mono',monospace; font-size:11.5px; color:var(--text-dim); letter-spacing:.08em;}
  .asset-list{display:flex; flex-direction:column; gap:8px; max-height:180px; overflow-y:auto;}
  .asset-item{border:1px solid var(--line); border-radius:5px; padding:10px 12px; font-size:13px; display:flex; justify-content:space-between; align-items:center; background:var(--bg);}
  .asset-item .del{color:var(--hot); cursor:pointer; font-size:12px; font-family:'JetBrains Mono',monospace; background:none; border:none;}
  .empty-note{color:var(--text-dim); font-size:12.5px; font-family:'JetBrains Mono',monospace;}

  /* FUNCTIONS TAB */
  .fn-panel{padding:24px;}
  .fn-select-row{display:flex; gap:10px; margin-bottom:20px; flex-wrap:wrap;}
  .fn-select-row select{flex:1; min-width:180px; background:var(--bg); border:1px solid var(--line); color:var(--text); padding:10px 12px; border-radius:5px; font-size:13.5px;}
  .fn-form{display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-bottom:16px;}
  .fn-form select, .fn-form input{background:var(--bg); border:1px solid var(--line); color:var(--text); padding:10px 12px; border-radius:5px; font-size:13.5px;}
  .fn-form input{grid-column:1/3;}
  .fn-list{display:flex; flex-direction:column; gap:8px;}
  .fn-row{border:1px solid var(--line); border-radius:5px; padding:12px 14px; font-size:13px; background:var(--bg); display:flex; justify-content:space-between; align-items:center; gap:10px;}
  .fn-row b{color:var(--mid); font-weight:600;}
  .fn-row .del{color:var(--hot); cursor:pointer; font-size:12px; font-family:'JetBrains Mono',monospace; background:none; border:none;}

  /* ROADMAP */
  .roadmap-grid{display:grid; grid-template-columns:repeat(3,1fr); gap:20px; max-width:1080px; margin:0 auto;}
  .gen-card{border:1px solid var(--line); border-radius:6px; padding:28px 24px; background:var(--panel); position:relative; overflow:hidden;}
  .gen-card::before{content:''; position:absolute; top:0; left:0; right:0; height:3px;}
  .gen-card.g1::before{background:var(--idle);} .gen-card.g2::before{background:var(--mid);} .gen-card.g3::before{background:linear-gradient(90deg,var(--hot),var(--hot-2));}
  .gen-card .zone{font-family:'JetBrains Mono',monospace; font-size:11px; letter-spacing:.1em; margin-bottom:12px;}
  .gen-card.g1 .zone{color:var(--idle);} .gen-card.g2 .zone{color:var(--mid);} .gen-card.g3 .zone{color:var(--hot);}
  .gen-card h3{font-family:'Oswald',sans-serif; font-size:24px; text-transform:uppercase; margin-bottom:10px;}
  .gen-card p{color:var(--text-dim); font-size:14px; margin-bottom:16px;}
  .gen-card .status{font-family:'JetBrains Mono',monospace; font-size:11.5px; padding:4px 10px; border-radius:999px; display:inline-block; border:1px solid var(--line);}
  .gen-card.g1 .status{color:var(--idle); border-color:rgba(111,255,176,0.3);}
  @media(max-width:820px){.roadmap-grid{grid-template-columns:1fr;}}

  /* EXPORT WIZARD */
  .demo{border-bottom:1px solid var(--line);}
  .wizard{max-width:620px; margin:0 auto; background:var(--panel); border:1px solid var(--line); border-radius:8px; overflow:hidden;}
  .wizard-head{padding:20px 24px; border-bottom:1px solid var(--line); display:flex; align-items:center; justify-content:space-between;}
  .wizard-head .fname{font-family:'JetBrains Mono',monospace; font-size:13px; color:var(--text-dim);}
  .rpm-bar{display:flex; gap:4px; padding:0 24px; margin-top:16px;}
  .rpm-seg{flex:1; height:5px; border-radius:3px; background:var(--line);}
  .rpm-seg.on:nth-child(1){background:var(--idle);} .rpm-seg.on:nth-child(2){background:var(--idle);}
  .rpm-seg.on:nth-child(3){background:var(--mid);} .rpm-seg.on:nth-child(4){background:var(--hot);}
  .step-meta{padding:10px 24px 0; font-family:'JetBrains Mono',monospace; font-size:11.5px; color:var(--text-dim);}
  .wizard-body{padding:28px 24px 8px; min-height:200px;}
  .step-title{font-family:'Oswald',sans-serif; font-size:21px; text-transform:uppercase; margin-bottom:6px;}
  .step-sub{color:var(--text-dim); font-size:13.5px; margin-bottom:22px;}
  .choice-row{display:grid; grid-template-columns:1fr 1fr; gap:12px;}
  .choice{border:1px solid var(--line); background:var(--bg); border-radius:5px; padding:16px; cursor:pointer; text-align:left; color:var(--text); font-family:'Inter',sans-serif;}
  .choice b{display:block; font-family:'Oswald',sans-serif; font-size:14px; text-transform:uppercase; margin-bottom:4px;}
  .choice span{color:var(--text-dim); font-size:12px;}
  .choice:hover{border-color:var(--text-dim);}
  .choice.sel{border-color:var(--mid); background:rgba(255,176,32,0.08); box-shadow:0 0 0 1px var(--mid) inset;}
  .field-block{margin-bottom:16px;}
  .field-block label{display:block; font-size:12.5px; color:var(--text-dim); margin-bottom:8px; font-family:'JetBrains Mono',monospace;}
  select.wselect, .file-drop{width:100%; background:var(--bg); border:1px solid var(--line); color:var(--text); padding:12px 14px; border-radius:5px; font-size:14px;}
  .checkbox-row{display:flex; align-items:center; gap:10px; margin-top:12px; font-size:13px; color:var(--text-dim);}
  .file-drop{display:flex; align-items:center; justify-content:space-between; cursor:pointer; gap:10px;}
  .file-drop input{display:none;}
  .preview-chip{display:flex; align-items:center; gap:10px; margin-top:14px; font-size:13px; color:var(--text-dim);}
  .preview-chip img{width:36px; height:36px; border-radius:5px; object-fit:cover; border:1px solid var(--line);}
  .summary-list{display:flex; flex-direction:column; border:1px solid var(--line); border-radius:5px; overflow:hidden;}
  .summary-row{display:flex; justify-content:space-between; padding:12px 16px; font-size:13px; border-bottom:1px solid var(--line);}
  .summary-row:last-child{border-bottom:none;}
  .summary-row span:first-child{color:var(--text-dim); font-family:'JetBrains Mono',monospace; font-size:11.5px;}
  .wizard-foot{display:flex; justify-content:space-between; padding:20px 24px 26px; gap:12px;}
  .btn-sm{padding:11px 20px; font-size:14px;}
  .btn-disabled{opacity:.35; pointer-events:none;}
  .download-note{font-size:12px; color:var(--text-dim); text-align:center; padding:0 24px 22px; font-family:'JetBrains Mono',monospace;}

  footer{padding:44px 24px; text-align:center;}
  footer .brand{justify-content:center; margin-bottom:12px;}
  footer p{color:var(--text-dim); font-size:13px;}
</style>
</head>
<body>

<header>
  <nav>
    <div class="brand"><img src="favicon.png" alt="REV Engine"><span>REV ENGINE</span></div>
    <div class="navlinks">
      <a href="#studio">Studio</a>
      <a href="#roadmap">Roadmap</a>
      <a href="#wizard">Ekspor</a>
    </div>
    <a class="cta-nav" href="#studio">Buka Studio</a>
  </nav>
</header>

<section class="hero">
  <div class="eyebrow"><span class="dot"></span> MOD ENGINE UNTUK MINECRAFT</div>
  <h1><span>REV</span><span class="engine">ENGINE</span></h1>
  <div class="gen-tag mono">GEN 1 — DESAIN 3D + FUNGSI MOD → EKSPOR</div>
  <p class="lead">Bangun aset 3D untuk Minecraft, tambahkan fungsi mod-nya langsung di sini, baru ekspor jadi file mod ke perangkatmu.</p>
  <div class="hero-cta">
    <a href="#studio" class="btn btn-hot">Buka Studio 3D</a>
    <a href="#roadmap" class="btn btn-ghost">Lihat Roadmap Gen</a>
  </div>
  <div class="tach-wrap">
    <svg id="tach" viewBox="0 0 500 300">
      <path d="M 60 260 A 190 190 0 0 1 145 95" fill="none" stroke="#6fffb0" stroke-width="14" stroke-linecap="round" opacity="0.85"/>
      <path d="M 155 87 A 190 190 0 0 1 345 87" fill="none" stroke="#ffb020" stroke-width="14" stroke-linecap="round" opacity="0.85"/>
      <path d="M 355 95 A 190 190 0 0 1 440 260" fill="none" stroke="#ff5a1f" stroke-width="14" stroke-linecap="round" opacity="0.9"/>
      <text x="95" y="290" class="tach-label">GEN 1</text>
      <text x="235" y="60" class="tach-label">GEN 2</text>
      <text x="392" y="290" class="tach-label">GEN 3 · AI</text>
      <g class="tach-needle" id="tachNeedle" style="transform:rotate(-58deg);">
        <line x1="250" y1="250" x2="250" y2="105" stroke="#f2f0eb" stroke-width="4" stroke-linecap="round"/>
        <circle cx="250" cy="250" r="10" fill="#f2f0eb"/>
      </g>
      <text x="250" y="240" text-anchor="middle" class="tach-readout">GEN 1 · AKTIF</text>
    </svg>
  </div>
</section>

<section class="studio" id="studio">
  <div class="section-head">
    <span class="kicker">STUDIO</span>
    <h2>Desain 3D & fungsi mod</h2>
    <p>Bentuk asetmu di kanvas, simpan sebagai aset, lalu pasangi fungsi — baru lanjut ke ekspor.</p>
  </div>

  <div class="studio-shell">
    <div class="studio-tabs">
      <button class="studio-tab active" data-tab="editor">Desain 3D</button>
      <button class="studio-tab" data-tab="functions">Fungsi Mod</button>
    </div>

    <!-- EDITOR TAB -->
    <div class="studio-body" data-panel="editor">
      <div class="canvas-col">
        <canvas id="editorCanvas"></canvas>
        <div class="canvas-hint mono">KLIK KIRI: taruh/hapus blok · KLIK KANAN + GESER: putar kamera · SCROLL: zoom</div>
        <div class="toolbar">
          <button class="tool-btn active" id="toolAdd">Tambah Blok</button>
          <button class="tool-btn" id="toolRemove">Hapus Blok</button>
        </div>
        <div class="swatches" id="swatches"></div>
        <div class="layer-row">
          <span>LAPISAN:</span>
          <button id="layerDown">–</button>
          <span id="layerVal">0</span>
          <button id="layerUp">+</button>
          <button id="clearCanvas" style="margin-left:auto; background:none; border:none; color:var(--hot); font-family:'JetBrains Mono',monospace; font-size:12px; cursor:pointer;">Bersihkan</button>
        </div>
        <div class="save-row">
          <input type="text" id="assetName" placeholder="Nama aset, mis. Blok Kristal">
          <button class="tool-btn" id="saveAsset">Simpan Aset</button>
        </div>
      </div>
      <div class="side-col">
        <div>
          <div class="side-title">ASET TERSIMPAN</div>
          <div class="asset-list" id="assetList"><div class="empty-note">Belum ada aset. Bentuk blok lalu simpan.</div></div>
        </div>
        <div>
          <div class="side-title">RINGKASAN PROYEK</div>
          <div class="empty-note" id="projectSummary" style="margin-top:8px;">0 aset · 0 fungsi</div>
        </div>
      </div>
    </div>

    <!-- FUNCTIONS TAB -->
    <div class="studio-body" data-panel="functions" style="display:none;">
      <div class="fn-panel" style="grid-column:1/3; width:100%;">
        <div class="fn-select-row">
          <select id="fnAssetSelect"><option value="">Pilih aset…</option></select>
        </div>
        <div id="fnEmpty" class="empty-note">Simpan aset dulu di tab Desain 3D untuk menambahkan fungsi.</div>
        <div id="fnEditor" style="display:none;">
          <div class="fn-form">
            <select id="fnTrigger">
              <option>Saat Dipecahkan</option>
              <option>Saat Diklik Kanan</option>
              <option>Saat Ditempatkan</option>
              <option>Saat Diinjak</option>
            </select>
            <select id="fnAction">
              <option>Jatuhkan Item</option>
              <option>Berikan Efek</option>
              <option>Mainkan Suara</option>
              <option>Ganti Jadi Blok Lain</option>
              <option>Beri XP</option>
            </select>
            <input type="text" id="fnValue" placeholder="Detail, mis. Berlian x2 / Speed II / ping.ogg">
          </div>
          <button class="tool-btn" id="fnAdd" style="margin-bottom:18px;">+ Tambah Fungsi</button>
          <div class="side-title" style="margin-bottom:10px;">DAFTAR FUNGSI</div>
          <div class="fn-list" id="fnList"><div class="empty-note">Belum ada fungsi untuk aset ini.</div></div>
        </div>
      </div>
    </div>
  </div>
</section>

<section id="roadmap">
  <div class="section-head">
    <span class="kicker">ROADMAP</span>
    <h2>Tiga generasi, satu mesin</h2>
    <p>Setiap generasi menaikkan putaran mesin — makin tinggi, makin otomatis.</p>
  </div>
  <div class="roadmap-grid">
    <div class="gen-card g1">
      <div class="zone mono">ZONA IDLE</div>
      <h3>Gen 1</h3>
      <p>Studio desain 3D manual + penyusun fungsi mod, lalu ekspor ke Bedrock atau Java lewat wizard.</p>
      <span class="status mono">● AKTIF SEKARANG</span>
    </div>
    <div class="gen-card g2">
      <div class="zone mono">ZONA REV</div>
      <h3>Gen 2</h3>
      <p>Mesin membantu men-generate dan meningkatkan kualitas desain otomatis — hasil lebih rapi dan lebih keren.</p>
      <span class="status mono">○ TAHAP BERIKUTNYA</span>
    </div>
    <div class="gen-card g3">
      <div class="zone mono">ZONA REDLINE</div>
      <h3>Gen 3</h3>
      <p>AI 3D Desain built-in. Ketik prompt, dan mesin langsung merakitnya jadi mod Minecraft yang siap diunduh.</p>
      <span class="status mono">○ MASA DEPAN</span>
    </div>
  </div>
</section>

<section class="demo" id="wizard">
  <div class="section-head">
    <span class="kicker">EKSPOR</span>
    <h2>Wizard ekspor mod</h2>
    <p>Proyek dari Studio-mu dirakit di sini — pilih platform, versi, favicon, lalu unduh.</p>
  </div>

  <div class="wizard">
    <div class="wizard-head">
      <div class="fname mono" id="wizardTitle">TAHAP 1 · PLATFORM</div>
      <div class="fname mono" id="wizardCount">1 / 4</div>
    </div>
    <div class="rpm-bar">
      <div class="rpm-seg on" data-seg="1"></div>
      <div class="rpm-seg" data-seg="2"></div>
      <div class="rpm-seg" data-seg="3"></div>
      <div class="rpm-seg" data-seg="4"></div>
    </div>

    <div class="wizard-body">
      <div class="step" data-step="1">
        <div class="step-title">Pilih platform</div>
        <div class="step-sub">Mod akan dirakit sesuai format platform yang dipilih.</div>
        <div class="choice-row">
          <button class="choice" data-field="platform" data-value="Bedrock"><b>Bedrock</b><span>Android, iOS, Windows, konsol</span></button>
          <button class="choice" data-field="platform" data-value="Java"><b>Java</b><span>PC (Java Edition)</span></button>
        </div>
      </div>

      <div class="step" data-step="2" style="display:none;">
        <div class="step-title">Pilih versi</div>
        <div class="step-sub">Sesuaikan mod dengan versi Minecraft target.</div>
        <div class="field-block">
          <label class="mono">VERSI MINECRAFT</label>
          <select class="wselect" id="versionSelect">
            <option value="1.21.x">1.21.x</option>
            <option value="1.20.x">1.20.x</option>
            <option value="1.19.x">1.19.x</option>
            <option value="1.18.x">1.18.x</option>
          </select>
        </div>
        <label class="checkbox-row"><input type="checkbox" id="allVersions"> Terapkan ke semua versi (All Version)</label>
      </div>

      <div class="step" data-step="3" style="display:none;">
        <div class="step-title">Favicon mod</div>
        <div class="step-sub">Unggah favicon.png sebagai ikon mod.</div>
        <label class="file-drop">
          <span id="fileLabel">Pilih favicon.png…</span>
          <span class="mono" style="color:var(--mid);">PILIH FILE</span>
          <input type="file" id="faviconInput" accept="image/png">
        </label>
        <div class="preview-chip" id="previewChip" style="display:none;">
          <img id="previewImg" src="" alt="Preview favicon">
          <span id="previewName"></span>
        </div>
      </div>

      <div class="step" data-step="4" style="display:none;">
        <div class="step-title">Konfirmasi</div>
        <div class="step-sub">Periksa kembali sebelum mengunduh ke perangkat.</div>
        <div class="summary-list">
          <div class="summary-row"><span>ASET DARI STUDIO</span><span id="sumAssets">—</span></div>
          <div class="summary-row"><span>TOTAL FUNGSI</span><span id="sumFunctions">—</span></div>
          <div class="summary-row"><span>PLATFORM</span><span id="sumPlatform">—</span></div>
          <div class="summary-row"><span>VERSI</span><span id="sumVersion">—</span></div>
          <div class="summary-row"><span>FAVICON</span><span id="sumFavicon">—</span></div>
          <div class="summary-row"><span>OUTPUT</span><span class="mono">.mcaddon</span></div>
        </div>
      </div>
    </div>

    <div class="step-meta mono" id="stepHint">Pilih salah satu untuk lanjut.</div>
    <div class="wizard-foot">
      <button class="btn btn-ghost btn-sm" id="btnBack">Kembali</button>
      <button class="btn btn-hot btn-sm btn-disabled" id="btnNext">Lanjut</button>
    </div>
    <div class="download-note" id="downloadNote" style="display:none;">File akan diunduh sebagai *.mcaddon berisi aset & fungsi dari Studio-mu.</div>
  </div>
</section>

<footer>
  <div class="brand"><img src="favicon.png" alt="REV Engine" style="height:28px;"></div>
  <p>REV Engine Gen 1 — Mod engine untuk Minecraft. Gen 2 &amp; Gen 3 dalam pengembangan.</p>
</footer>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
/* ---------- Tachometer intro ---------- */
window.addEventListener('load', () => {
  const needle = document.getElementById('tachNeedle');
  requestAnimationFrame(() => { needle.style.transform = 'rotate(-58deg)'; });
});

/* ---------- STUDIO STATE ---------- */
const COLORS = ['#6fffb0','#ffb020','#ff5a1f','#4da3ff','#c084fc','#f2f0eb'];
let selectedColor = COLORS[0];
let tool = 'add';
let currentLayer = 0;
let blocks = []; // {x,y,z,color}
let assets = []; // {id,name,blocks,functions:[]}
let assetCounter = 0;

const swatchWrap = document.getElementById('swatches');
COLORS.forEach((c,i)=>{
  const s = document.createElement('div');
  s.className = 'swatch' + (i===0?' active':'');
  s.style.background = c;
  s.addEventListener('click', ()=>{
    document.querySelectorAll('.swatch').forEach(el=>el.classList.remove('active'));
    s.classList.add('active');
    selectedColor = c;
  });
  swatchWrap.appendChild(s);
});

document.getElementById('toolAdd').addEventListener('click', (e)=>{
  tool='add';
  document.getElementById('toolAdd').classList.add('active');
  document.getElementById('toolRemove').classList.remove('active');
});
document.getElementById('toolRemove').addEventListener('click', (e)=>{
  tool='remove';
  document.getElementById('toolRemove').classList.add('active');
  document.getElementById('toolAdd').classList.remove('active');
});
document.getElementById('layerUp').addEventListener('click', ()=>{ currentLayer=Math.min(5,currentLayer+1); document.getElementById('layerVal').textContent=currentLayer; });
document.getElementById('layerDown').addEventListener('click', ()=>{ currentLayer=Math.max(0,currentLayer-1); document.getElementById('layerVal').textContent=currentLayer; });
document.getElementById('clearCanvas').addEventListener('click', ()=>{
  blocks.forEach(b=>scene.remove(b.mesh));
  blocks = [];
});

/* ---------- THREE.JS EDITOR ---------- */
const canvas = document.getElementById('editorCanvas');
const renderer = new THREE.WebGLRenderer({canvas, antialias:true});
const scene = new THREE.Scene();
scene.background = new THREE.Color(0x050506);

function sizeCanvas(){
  const w = canvas.clientWidth || 500;
  const h = canvas.clientHeight || 375;
  renderer.setSize(w, h, false);
  camera.aspect = w/h;
  camera.updateProjectionMatrix();
}

const camera = new THREE.PerspectiveCamera(45, 4/3, 0.1, 100);
let spherical = { theta: 0.9, phi: 1.0, radius: 16 };
function updateCamera(){
  camera.position.x = spherical.radius * Math.sin(spherical.phi) * Math.sin(spherical.theta);
  camera.position.y = spherical.radius * Math.cos(spherical.phi);
  camera.position.z = spherical.radius * Math.sin(spherical.phi) * Math.cos(spherical.theta);
  camera.lookAt(5,1,5);
}
updateCamera();

scene.add(new THREE.AmbientLight(0xffffff, 0.55));
const dirLight = new THREE.DirectionalLight(0xffffff, 0.9);
dirLight.position.set(10,20,10);
scene.add(dirLight);

const grid = new THREE.GridHelper(10, 10, 0x3a3a42, 0x1c1c22);
grid.position.set(5,0,5);
scene.add(grid);

const raycaster = new THREE.Raycaster();
const mouse = new THREE.Vector2();
const cubeGeo = new THREE.BoxGeometry(1,1,1);

function cellFromEvent(evt){
  const rect = canvas.getBoundingClientRect();
  mouse.x = ((evt.clientX-rect.left)/rect.width)*2-1;
  mouse.y = -((evt.clientY-rect.top)/rect.height)*2+1;
  raycaster.setFromCamera(mouse, camera);
  const planeY = currentLayer;
  const plane = new THREE.Plane(new THREE.Vector3(0,1,0), -planeY);
  const pt = new THREE.Vector3();
  const hit = raycaster.ray.intersectPlane(plane, pt);
  if(!hit) return null;
  const x = Math.floor(pt.x);
  const z = Math.floor(pt.z);
  if(x<0||x>9||z<0||z>9) return null;
  return {x,z};
}

function findBlockAt(x,y,z){
  return blocks.find(b=>b.x===x && b.y===y && b.z===z);
}

function addBlockMesh(x,y,z,color){
  const mat = new THREE.MeshStandardMaterial({color});
  const mesh = new THREE.Mesh(cubeGeo, mat);
  mesh.position.set(x+0.5, y+0.5, z+0.5);
  scene.add(mesh);
  const entry = {x,y,z,color,mesh};
  blocks.push(entry);
  return entry;
}

function removeBlockNear(evt){
  const rect = canvas.getBoundingClientRect();
  mouse.x = ((evt.clientX-rect.left)/rect.width)*2-1;
  mouse.y = -((evt.clientY-rect.top)/rect.height)*2+1;
  raycaster.setFromCamera(mouse, camera);
  const meshes = blocks.map(b=>b.mesh);
  const hits = raycaster.intersectObjects(meshes);
  if(hits.length){
    const mesh = hits[0].object;
    const idx = blocks.findIndex(b=>b.mesh===mesh);
    if(idx>=0){
      scene.remove(mesh);
      blocks.splice(idx,1);
    }
  }
}

let dragging = false, lastX=0, lastY=0, didDrag=false;
canvas.addEventListener('contextmenu', e=>e.preventDefault());
canvas.addEventListener('mousedown', (e)=>{
  if(e.button===2){ dragging=true; lastX=e.clientX; lastY=e.clientY; didDrag=false; }
});
window.addEventListener('mousemove', (e)=>{
  if(dragging){
    const dx = e.clientX-lastX, dy = e.clientY-lastY;
    if(Math.abs(dx)+Math.abs(dy) > 2) didDrag = true;
    spherical.theta -= dx*0.008;
    spherical.phi = Math.min(Math.max(spherical.phi - dy*0.008, 0.2), 1.5);
    lastX=e.clientX; lastY=e.clientY;
    updateCamera();
  }
});
window.addEventListener('mouseup', ()=>{ dragging=false; });
canvas.addEventListener('wheel', (e)=>{
  e.preventDefault();
  spherical.radius = Math.min(Math.max(spherical.radius + e.deltaY*0.01, 6), 28);
  updateCamera();
}, {passive:false});

canvas.addEventListener('click', (e)=>{
  if(didDrag){ didDrag=false; return; }
  if(tool==='add'){
    const cell = cellFromEvent(e);
    if(!cell) return;
    if(findBlockAt(cell.x, currentLayer, cell.z)) return;
    addBlockMesh(cell.x, currentLayer, cell.z, selectedColor);
  } else {
    removeBlockNear(e);
  }
});

function animate(){
  requestAnimationFrame(animate);
  sizeCanvas();
  renderer.render(scene, camera);
}
animate();

/* ---------- ASSETS ---------- */
function renderAssetList(){
  const list = document.getElementById('assetList');
  if(assets.length===0){
    list.innerHTML = '<div class="empty-note">Belum ada aset. Bentuk blok lalu simpan.</div>';
  } else {
    list.innerHTML = '';
    assets.forEach(a=>{
      const row = document.createElement('div');
      row.className = 'asset-item';
      row.innerHTML = '<span>'+a.name+' · '+a.blocks.length+' blok</span>';
      const del = document.createElement('button');
      del.className = 'del'; del.textContent = 'Hapus';
      del.addEventListener('click', ()=>{
        assets = assets.filter(x=>x.id!==a.id);
        renderAssetList(); renderFnSelect(); renderSummary();
      });
      row.appendChild(del);
      list.appendChild(row);
    });
  }
  renderSummary();
}

function renderSummary(){
  const totalFns = assets.reduce((s,a)=>s+a.functions.length,0);
  document.getElementById('projectSummary').textContent = assets.length+' aset · '+totalFns+' fungsi';
}

document.getElementById('saveAsset').addEventListener('click', ()=>{
  if(blocks.length===0){ alert('Bentuk minimal satu blok dulu.'); return; }
  const nameInput = document.getElementById('assetName');
  const name = nameInput.value.trim() || ('Aset '+(assetCounter+1));
  assetCounter++;
  assets.push({
    id: 'asset_'+Date.now(),
    name,
    blocks: blocks.map(b=>({x:b.x,y:b.y,z:b.z,color:b.color})),
    functions: []
  });
  blocks.forEach(b=>scene.remove(b.mesh));
  blocks = [];
  nameInput.value='';
  renderAssetList();
  renderFnSelect();
});

/* ---------- TABS ---------- */
document.querySelectorAll('.studio-tab').forEach(tab=>{
  tab.addEventListener('click', ()=>{
    document.querySelectorAll('.studio-tab').forEach(t=>t.classList.remove('active'));
    tab.classList.add('active');
    const target = tab.dataset.tab;
    document.querySelectorAll('.studio-body').forEach(p=>{
      p.style.display = (p.dataset.panel===target) ? 'grid' : 'none';
    });
  });
});

/* ---------- FUNCTIONS TAB ---------- */
const fnAssetSelect = document.getElementById('fnAssetSelect');
function renderFnSelect(){
  fnAssetSelect.innerHTML = '<option value="">Pilih aset…</option>';
  assets.forEach(a=>{
    const opt = document.createElement('option');
    opt.value = a.id; opt.textContent = a.name;
    fnAssetSelect.appendChild(opt);
  });
  document.getElementById('fnEmpty').style.display = assets.length ? 'none' : 'block';
}

fnAssetSelect.addEventListener('change', ()=>{
  const id = fnAssetSelect.value;
  document.getElementById('fnEditor').style.display = id ? 'block' : 'none';
  renderFnList();
});

function currentFnAsset(){
  return assets.find(a=>a.id===fnAssetSelect.value);
}

function renderFnList(){
  const asset = currentFnAsset();
  const list = document.getElementById('fnList');
  if(!asset || asset.functions.length===0){
    list.innerHTML = '<div class="empty-note">Belum ada fungsi untuk aset ini.</div>';
    return;
  }
  list.innerHTML = '';
  asset.functions.forEach((f,i)=>{
    const row = document.createElement('div');
    row.className = 'fn-row';
    row.innerHTML = '<span><b>'+f.trigger+'</b> → '+f.action+(f.value?' ('+f.value+')':'')+'</span>';
    const del = document.createElement('button');
    del.className='del'; del.textContent='Hapus';
    del.addEventListener('click', ()=>{
      asset.functions.splice(i,1);
      renderFnList(); renderSummary();
    });
    row.appendChild(del);
    list.appendChild(row);
  });
}

document.getElementById('fnAdd').addEventListener('click', ()=>{
  const asset = currentFnAsset();
  if(!asset) return;
  const trigger = document.getElementById('fnTrigger').value;
  const action = document.getElementById('fnAction').value;
  const value = document.getElementById('fnValue').value.trim();
  asset.functions.push({trigger, action, value});
  document.getElementById('fnValue').value = '';
  renderFnList();
  renderAssetList();
});

renderAssetList();
renderFnSelect();

/* ---------- EXPORT WIZARD ---------- */
const wstate = { platform:null, version:'1.21.x', allVersions:false, faviconName:null };
let step = 1;
const totalSteps = 4;
const steps = document.querySelectorAll('.wizard .step');
const btnNext = document.getElementById('btnNext');
const btnBack = document.getElementById('btnBack');
const wizardTitle = document.getElementById('wizardTitle');
const wizardCount = document.getElementById('wizardCount');
const stepHint = document.getElementById('stepHint');
const downloadNote = document.getElementById('downloadNote');
const titles = {1:'TAHAP 1 · PLATFORM', 2:'TAHAP 2 · VERSI', 3:'TAHAP 3 · FAVICON', 4:'TAHAP 4 · KONFIRMASI'};
const hints = {1:'Pilih salah satu untuk lanjut.',2:'Pilih versi, atau centang semua versi.',3:'Favicon opsional, tapi disarankan.',4:'Klik unduh untuk membuat file .mcaddon.'};

function renderSegments(){
  document.querySelectorAll('.rpm-seg').forEach(seg=>{
    seg.classList.toggle('on', Number(seg.dataset.seg) <= step);
  });
}
function updateNextState(){
  let ok = true;
  if(step===1) ok = !!wstate.platform;
  btnNext.classList.toggle('btn-disabled', !ok);
}
function goTo(n){
  step = Math.max(1, Math.min(totalSteps, n));
  steps.forEach(s => s.style.display = (Number(s.dataset.step)===step) ? 'block' : 'none');
  wizardTitle.textContent = titles[step];
  wizardCount.textContent = step+' / '+totalSteps;
  stepHint.textContent = hints[step];
  renderSegments();
  updateNextState();
  btnBack.classList.toggle('btn-disabled', step===1);
  if(step===totalSteps){
    const totalFns = assets.reduce((s,a)=>s+a.functions.length,0);
    document.getElementById('sumAssets').textContent = assets.length ? assets.map(a=>a.name).join(', ') : 'Tidak ada';
    document.getElementById('sumFunctions').textContent = totalFns;
    document.getElementById('sumPlatform').textContent = wstate.platform || '—';
    document.getElementById('sumVersion').textContent = wstate.allVersions ? 'Semua versi' : wstate.version;
    document.getElementById('sumFavicon').textContent = wstate.faviconName || 'Tidak diunggah';
    btnNext.textContent = 'Unduh ke Perangkat';
    downloadNote.style.display = 'block';
  } else {
    btnNext.textContent = 'Lanjut';
    downloadNote.style.display = 'none';
  }
}
document.querySelectorAll('.choice[data-field="platform"]').forEach(btn=>{
  btn.addEventListener('click', ()=>{
    document.querySelectorAll('.choice[data-field="platform"]').forEach(b=>b.classList.remove('sel'));
    btn.classList.add('sel');
    wstate.platform = btn.dataset.value;
    updateNextState();
  });
});
document.getElementById('versionSelect').addEventListener('change', e=>{ wstate.version = e.target.value; });
document.getElementById('allVersions').addEventListener('change', e=>{
  wstate.allVersions = e.target.checked;
  document.getElementById('versionSelect').disabled = e.target.checked;
});
document.getElementById('faviconInput').addEventListener('change', e=>{
  const file = e.target.files[0];
  if(!file) return;
  wstate.faviconName = file.name;
  document.getElementById('fileLabel').textContent = file.name;
  const reader = new FileReader();
  reader.onload = ev=>{
    document.getElementById('previewImg').src = ev.target.result;
    document.getElementById('previewName').textContent = file.name;
    document.getElementById('previewChip').style.display = 'flex';
  };
  reader.readAsDataURL(file);
});
btnBack.addEventListener('click', ()=>{ if(step>1) goTo(step-1); });
btnNext.addEventListener('click', ()=>{
  if(btnNext.classList.contains('btn-disabled')) return;
  if(step<totalSteps){
    goTo(step+1);
  } else {
    const payload = {
      engine: 'REV Engine Gen 1',
      platform: wstate.platform,
      version: wstate.allVersions ? 'all' : wstate.version,
      favicon: wstate.faviconName || 'none',
      assets: assets.map(a=>({name:a.name, blocks:a.blocks.length, functions:a.functions})),
      generated: new Date().toISOString()
    };
    const blob = new Blob([JSON.stringify(payload, null, 2)], {type:'application/octet-stream'});
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'rev-engine-mod-'+(wstate.platform||'output').toLowerCase()+'.mcaddon';
    document.body.appendChild(a); a.click(); a.remove();
    URL.revokeObjectURL(url);
    btnNext.textContent = 'Terunduh ✓';
  }
});
goTo(1);
</script>
</body>
</html>
