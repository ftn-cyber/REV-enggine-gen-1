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
    --bg:#08080a;
    --bg-alt:#101014;
    --panel:#131316;
    --line:#232329;
    --text:#f2f0eb;
    --text-dim:#8b8b93;
    --idle:#6fffb0;
    --mid:#ffb020;
    --hot:#ff5a1f;
    --hot-2:#ff2e2e;
  }
  *{box-sizing:border-box; margin:0; padding:0;}
  html{scroll-behavior:smooth;}
  body{
    background:var(--bg);
    color:var(--text);
    font-family:'Inter',sans-serif;
    line-height:1.55;
    overflow-x:hidden;
  }
  ::selection{background:var(--hot); color:#0a0a0a;}
  a{color:inherit;}
  .mono{font-family:'JetBrains Mono',monospace;}
  .display{font-family:'Oswald',sans-serif; text-transform:uppercase; letter-spacing:0.01em;}
  .wrap{max-width:1180px; margin:0 auto; padding:0 24px;}
  @media(prefers-reduced-motion: reduce){ *{animation-duration:0.01ms !important; animation-iteration-count:1 !important; transition-duration:0.01ms !important;} }

  /* NAV */
  header{
    position:sticky; top:0; z-index:50;
    background:rgba(8,8,10,0.82);
    backdrop-filter:blur(10px);
    border-bottom:1px solid var(--line);
  }
  nav{display:flex; align-items:center; justify-content:space-between; padding:14px 24px; max-width:1180px; margin:0 auto;}
  .brand{display:flex; align-items:center; gap:10px;}
  .brand img{height:34px; width:auto; display:block;}
  .brand span{font-family:'Oswald',sans-serif; font-weight:600; letter-spacing:0.06em; font-size:14px; color:var(--text-dim);}
  .navlinks{display:flex; gap:28px; font-size:14px; color:var(--text-dim);}
  .navlinks a{text-decoration:none; transition:color .2s;}
  .navlinks a:hover{color:var(--text);}
  .navlinks a:focus-visible, button:focus-visible, input:focus-visible, select:focus-visible{outline:2px solid var(--hot); outline-offset:2px;}
  .cta-nav{
    background:linear-gradient(90deg,var(--hot),var(--hot-2));
    color:#0a0a0a; font-weight:700; font-size:13px;
    padding:9px 18px; border-radius:3px; text-decoration:none;
    letter-spacing:0.03em;
  }
  @media(max-width:760px){ .navlinks{display:none;} }

  /* HERO */
  .hero{
    position:relative;
    padding:96px 24px 80px;
    text-align:center;
    overflow:hidden;
    border-bottom:1px solid var(--line);
  }
  .hero::before{
    content:'';
    position:absolute; inset:0;
    background:
      radial-gradient(ellipse 700px 400px at 50% -10%, rgba(255,90,31,0.18), transparent 60%),
      repeating-linear-gradient(90deg, rgba(255,255,255,0.025) 0 1px, transparent 1px 64px);
    pointer-events:none;
  }
  .eyebrow{
    display:inline-flex; align-items:center; gap:8px;
    font-family:'JetBrains Mono',monospace; font-size:12px; letter-spacing:0.12em;
    color:var(--mid); border:1px solid var(--line); background:var(--bg-alt);
    padding:6px 14px; border-radius:999px; margin-bottom:28px;
  }
  .eyebrow .dot{width:6px; height:6px; border-radius:50%; background:var(--idle); box-shadow:0 0 8px var(--idle); animation:pulse 1.8s ease-in-out infinite;}
  @keyframes pulse{0%,100%{opacity:1;}50%{opacity:.35;}}

  .hero h1{
    font-size:clamp(48px, 10vw, 108px);
    font-weight:700; line-height:0.92;
    letter-spacing:-0.01em;
    position:relative; z-index:1;
  }
  .hero h1 .rev{color:var(--text);}
  .hero h1 .engine{
    display:block;
    background:linear-gradient(90deg,#fff, var(--mid) 55%, var(--hot));
    -webkit-background-clip:text; background-clip:text; color:transparent;
  }
  .gen-tag{
    display:inline-block; margin-top:18px;
    font-family:'JetBrains Mono',monospace; font-size:14px; color:var(--text-dim);
    border:1px solid var(--line); padding:4px 12px; border-radius:3px;
  }
  .hero p.lead{
    max-width:560px; margin:26px auto 0; color:var(--text-dim); font-size:17px;
  }
  .hero-cta{display:flex; gap:14px; justify-content:center; margin-top:36px; flex-wrap:wrap;}
  .btn{
    font-family:'Inter',sans-serif; font-weight:600; font-size:15px;
    padding:13px 26px; border-radius:3px; text-decoration:none; cursor:pointer; border:1px solid transparent;
    transition:transform .15s ease, box-shadow .15s ease;
  }
  .btn:hover{transform:translateY(-2px);}
  .btn-hot{background:linear-gradient(90deg,var(--hot),var(--hot-2)); color:#0a0a0a; box-shadow:0 8px 30px -8px rgba(255,90,31,0.6);}
  .btn-ghost{background:transparent; border-color:var(--line); color:var(--text);}
  .btn-ghost:hover{border-color:var(--text-dim);}

  /* TACHOMETER SIGNATURE */
  .tach-wrap{margin:64px auto 0; max-width:560px; position:relative; z-index:1;}
  svg#tach{width:100%; height:auto; display:block;}
  .tach-needle{transform-origin:250px 250px; transition:transform 1.4s cubic-bezier(.2,.9,.2,1);}
  .tach-label{font-family:'JetBrains Mono',monospace; font-size:11px; letter-spacing:.08em; fill:var(--text-dim);}
  .tach-readout{font-family:'Oswald',sans-serif; font-weight:600; fill:var(--text); font-size:15px;}

  /* SECTION GENERIC */
  section{padding:88px 24px;}
  .section-head{max-width:640px; margin:0 auto 56px; text-align:center;}
  .section-head .kicker{font-family:'JetBrains Mono',monospace; color:var(--mid); font-size:12px; letter-spacing:.12em; display:block; margin-bottom:10px;}
  .section-head h2{font-size:clamp(28px,4.5vw,42px); font-weight:600; line-height:1.1;}
  .section-head p{color:var(--text-dim); margin-top:14px; font-size:16px;}

  /* HOW IT WORKS */
  .how{border-bottom:1px solid var(--line); background:var(--bg-alt);}
  .how-grid{display:grid; grid-template-columns:repeat(3,1fr); gap:1px; background:var(--line); border:1px solid var(--line); max-width:1000px; margin:0 auto; border-radius:4px; overflow:hidden;}
  .how-card{background:var(--bg); padding:36px 28px;}
  .how-card .tick{display:flex; align-items:center; gap:8px; margin-bottom:20px;}
  .how-card .tick span{width:10px; height:10px; border-radius:2px;}
  .how-card:nth-child(1) .tick span{background:var(--idle);}
  .how-card:nth-child(2) .tick span{background:var(--mid);}
  .how-card:nth-child(3) .tick span{background:var(--hot);}
  .how-card .tick b{font-family:'JetBrains Mono',monospace; font-size:12px; letter-spacing:.1em; color:var(--text-dim); font-weight:500;}
  .how-card h3{font-family:'Oswald',sans-serif; font-size:22px; margin-bottom:10px; text-transform:uppercase;}
  .how-card p{color:var(--text-dim); font-size:14.5px;}
  @media(max-width:760px){.how-grid{grid-template-columns:1fr;}}

  /* ROADMAP */
  .roadmap-grid{display:grid; grid-template-columns:repeat(3,1fr); gap:20px; max-width:1080px; margin:0 auto;}
  .gen-card{
    border:1px solid var(--line); border-radius:6px; padding:30px 26px; background:var(--panel);
    position:relative; overflow:hidden;
  }
  .gen-card::before{content:''; position:absolute; top:0; left:0; right:0; height:3px;}
  .gen-card.g1::before{background:var(--idle);}
  .gen-card.g2::before{background:var(--mid);}
  .gen-card.g3::before{background:linear-gradient(90deg,var(--hot),var(--hot-2));}
  .gen-card .zone{font-family:'JetBrains Mono',monospace; font-size:11px; letter-spacing:.1em; color:var(--text-dim); margin-bottom:14px;}
  .gen-card.g1 .zone{color:var(--idle);}
  .gen-card.g2 .zone{color:var(--mid);}
  .gen-card.g3 .zone{color:var(--hot);}
  .gen-card h3{font-family:'Oswald',sans-serif; font-size:26px; text-transform:uppercase; margin-bottom:12px;}
  .gen-card p{color:var(--text-dim); font-size:14.5px; margin-bottom:18px;}
  .gen-card .status{font-family:'JetBrains Mono',monospace; font-size:11.5px; padding:4px 10px; border-radius:999px; display:inline-block; border:1px solid var(--line);}
  .gen-card.g1 .status{color:var(--idle); border-color:rgba(111,255,176,0.3);}
  .gen-card.g2 .status, .gen-card.g3 .status{color:var(--text-dim);}
  @media(max-width:820px){.roadmap-grid{grid-template-columns:1fr;}}

  /* WIZARD DEMO */
  .demo{background:var(--bg-alt); border-top:1px solid var(--line); border-bottom:1px solid var(--line);}
  .wizard{
    max-width:640px; margin:0 auto; background:var(--panel); border:1px solid var(--line); border-radius:8px; overflow:hidden;
  }
  .wizard-head{padding:22px 26px; border-bottom:1px solid var(--line); display:flex; align-items:center; justify-content:space-between;}
  .wizard-head .fname{font-family:'JetBrains Mono',monospace; font-size:13px; color:var(--text-dim);}
  .rpm-bar{display:flex; gap:4px; padding:0 26px; margin-top:18px;}
  .rpm-seg{flex:1; height:5px; border-radius:3px; background:var(--line); transition:background .3s;}
  .rpm-seg.on:nth-child(1){background:var(--idle);}
  .rpm-seg.on:nth-child(2){background:var(--idle);}
  .rpm-seg.on:nth-child(3){background:var(--mid);}
  .rpm-seg.on:nth-child(4){background:var(--hot);}
  .step-meta{padding:10px 26px 0; font-family:'JetBrains Mono',monospace; font-size:11.5px; color:var(--text-dim); letter-spacing:.05em;}
  .wizard-body{padding:30px 26px 10px; min-height:220px;}
  .step-title{font-family:'Oswald',sans-serif; font-size:22px; text-transform:uppercase; margin-bottom:6px;}
  .step-sub{color:var(--text-dim); font-size:14px; margin-bottom:24px;}
  .choice-row{display:grid; grid-template-columns:1fr 1fr; gap:12px;}
  .choice{
    border:1px solid var(--line); background:var(--bg); border-radius:5px; padding:18px; cursor:pointer;
    text-align:left; color:var(--text); font-family:'Inter',sans-serif; transition:.15s;
  }
  .choice b{display:block; font-family:'Oswald',sans-serif; font-size:15px; text-transform:uppercase; margin-bottom:4px;}
  .choice span{color:var(--text-dim); font-size:12.5px;}
  .choice:hover{border-color:var(--text-dim);}
  .choice.sel{border-color:var(--mid); background:rgba(255,176,32,0.08); box-shadow:0 0 0 1px var(--mid) inset;}
  .field-block{margin-bottom:18px;}
  .field-block label{display:block; font-size:13px; color:var(--text-dim); margin-bottom:8px; font-family:'JetBrains Mono',monospace;}
  select, .file-drop{
    width:100%; background:var(--bg); border:1px solid var(--line); color:var(--text);
    padding:12px 14px; border-radius:5px; font-family:'Inter',sans-serif; font-size:14px;
  }
  .checkbox-row{display:flex; align-items:center; gap:10px; margin-top:12px; font-size:13.5px; color:var(--text-dim);}
  .file-drop{
    display:flex; align-items:center; justify-content:space-between; cursor:pointer; gap:10px;
  }
  .file-drop input{display:none;}
  .preview-chip{display:flex; align-items:center; gap:10px; margin-top:14px; font-size:13px; color:var(--text-dim);}
  .preview-chip img{width:36px; height:36px; border-radius:5px; object-fit:cover; border:1px solid var(--line);}
  .summary-list{display:flex; flex-direction:column; gap:0; border:1px solid var(--line); border-radius:5px; overflow:hidden;}
  .summary-row{display:flex; justify-content:space-between; padding:13px 16px; font-size:13.5px; border-bottom:1px solid var(--line);}
  .summary-row:last-child{border-bottom:none;}
  .summary-row span:first-child{color:var(--text-dim); font-family:'JetBrains Mono',monospace; font-size:12px;}
  .wizard-foot{display:flex; justify-content:space-between; padding:22px 26px 28px; gap:12px;}
  .btn-sm{padding:11px 20px; font-size:14px;}
  .btn-disabled{opacity:.35; pointer-events:none;}
  .download-note{font-size:12px; color:var(--text-dim); text-align:center; padding:0 26px 24px; font-family:'JetBrains Mono',monospace;}

  footer{padding:48px 24px; text-align:center;}
  footer .brand{justify-content:center; margin-bottom:14px;}
  footer p{color:var(--text-dim); font-size:13px;}
</style>
</head>
<body>

<header>
  <nav>
    <div class="brand"><img src="favicon.png" alt="REV Engine"><span>REV ENGINE</span></div>
    <div class="navlinks">
      <a href="#cara-kerja">Cara Kerja</a>
      <a href="#roadmap">Roadmap</a>
      <a href="#wizard">Ekspor Mod</a>
    </div>
    <a class="cta-nav" href="#wizard">Coba Wizard</a>
  </nav>
</header>

<section class="hero">
  <div class="eyebrow"><span class="dot"></span> MOD ENGINE UNTUK MINECRAFT</div>
  <h1>
    <span class="rev">REV</span>
    <span class="engine">ENGINE</span>
  </h1>
  <div class="gen-tag mono">GEN 1 — DESAIN 3D → MOD MINECRAFT</div>
  <p class="lead">Desain aset 3D dengan mudah, lalu rakit langsung jadi mod Minecraft — dari satu mesin yang sama, tanpa pindah alat.</p>
  <div class="hero-cta">
    <a href="#wizard" class="btn btn-hot">Coba Wizard Ekspor</a>
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
      <text x="250" y="240" text-anchor="middle" class="tach-readout" id="tachReadout">GEN 1 · AKTIF</text>
    </svg>
  </div>
</section>

<section class="how" id="cara-kerja">
  <div class="section-head">
    <span class="kicker">ALUR KERJA</span>
    <h2>Dari desain sampai jadi mod</h2>
    <p>Tiga tahap dalam satu mesin — tidak perlu berpindah aplikasi.</p>
  </div>
  <div class="how-grid">
    <div class="how-card">
      <div class="tick"><span></span><b class="mono">TAHAP · IDLE</b></div>
      <h3>Desain 3D</h3>
      <p>Bangun aset 3D untuk Minecraft langsung di editor REV Engine — blok, entitas, item, sampai struktur custom.</p>
    </div>
    <div class="how-card">
      <div class="tick"><span></span><b class="mono">TAHAP · REV</b></div>
      <h3>Rakit Mod</h3>
      <p>Aset yang sudah dibuat langsung dirakit jadi paket mod — behavior, tekstur, dan model tersusun otomatis.</p>
    </div>
    <div class="how-card">
      <div class="tick"><span></span><b class="mono">TAHAP · REDLINE</b></div>
      <h3>Ekspor</h3>
      <p>Pilih Bedrock atau Java, pilih versi, dan unduh langsung ke perangkat sebagai file <span class="mono">.mcaddon</span>.</p>
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
      <p>Editor desain 3D manual untuk Minecraft, lalu ekspor mod ke Bedrock atau Java lewat wizard.</p>
      <span class="status mono">● AKTIF SEKARANG</span>
    </div>
    <div class="gen-card g2">
      <div class="zone mono">ZONA REV</div>
      <h3>Gen 2</h3>
      <p>Mesin bisa membantu men-generate dan meningkatkan kualitas desain otomatis — hasil lebih rapi dan lebih keren.</p>
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
    <span class="kicker">DEMO INTERAKTIF</span>
    <h2>Wizard ekspor mod</h2>
    <p>Empat tahap singkat dari pilihan platform sampai file terunduh ke perangkatmu.</p>
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

      <!-- STEP 1 -->
      <div class="step" data-step="1">
        <div class="step-title">Pilih platform</div>
        <div class="step-sub">Mod akan dirakit sesuai format platform yang dipilih.</div>
        <div class="choice-row">
          <button class="choice" data-field="platform" data-value="Bedrock">
            <b>Bedrock</b>
            <span>Android, iOS, Windows, konsol</span>
          </button>
          <button class="choice" data-field="platform" data-value="Java">
            <b>Java</b>
            <span>PC (Java Edition)</span>
          </button>
        </div>
      </div>

      <!-- STEP 2 -->
      <div class="step" data-step="2" style="display:none;">
        <div class="step-title">Pilih versi</div>
        <div class="step-sub">Sesuaikan mod dengan versi Minecraft target.</div>
        <div class="field-block">
          <label class="mono">VERSI MINECRAFT</label>
          <select id="versionSelect">
            <option value="1.21.x">1.21.x</option>
            <option value="1.20.x">1.20.x</option>
            <option value="1.19.x">1.19.x</option>
            <option value="1.18.x">1.18.x</option>
          </select>
        </div>
        <label class="checkbox-row">
          <input type="checkbox" id="allVersions"> Terapkan ke semua versi (All Version)
        </label>
      </div>

      <!-- STEP 3 -->
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

      <!-- STEP 4 -->
      <div class="step" data-step="4" style="display:none;">
        <div class="step-title">Konfirmasi</div>
        <div class="step-sub">Periksa kembali sebelum mengunduh ke perangkat.</div>
        <div class="summary-list">
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
    <div class="download-note" id="downloadNote" style="display:none;">File akan diunduh sebagai *.mcaddon ke perangkatmu.</div>
  </div>
</section>

<footer>
  <div class="brand"><img src="favicon.png" alt="REV Engine" style="height:28px;"></div>
  <p>REV Engine Gen 1 — Mod engine untuk Minecraft. Gen 2 &amp; Gen 3 dalam pengembangan.</p>
</footer>

<script>
// Tachometer intro sweep
window.addEventListener('load', () => {
  const needle = document.getElementById('tachNeedle');
  requestAnimationFrame(() => { needle.style.transform = 'rotate(-58deg)'; });
});

// Wizard state
const state = { platform:null, version:'1.21.x', allVersions:false, faviconName:null };
let step = 1;
const totalSteps = 4;

const steps = document.querySelectorAll('.step');
const btnNext = document.getElementById('btnNext');
const btnBack = document.getElementById('btnBack');
const wizardTitle = document.getElementById('wizardTitle');
const wizardCount = document.getElementById('wizardCount');
const stepHint = document.getElementById('stepHint');
const downloadNote = document.getElementById('downloadNote');
const titles = {1:'TAHAP 1 · PLATFORM', 2:'TAHAP 2 · VERSI', 3:'TAHAP 3 · FAVICON', 4:'TAHAP 4 · KONFIRMASI'};
const hints = {
  1:'Pilih salah satu untuk lanjut.',
  2:'Pilih versi, atau centang semua versi.',
  3:'Favicon opsional, tapi disarankan.',
  4:'Klik unduh untuk membuat file .mcaddon.'
};

function renderSegments(){
  document.querySelectorAll('.rpm-seg').forEach(seg=>{
    seg.classList.toggle('on', Number(seg.dataset.seg) <= step);
  });
}

function updateNextState(){
  let ok = true;
  if(step === 1) ok = !!state.platform;
  btnNext.classList.toggle('btn-disabled', !ok);
}

function goTo(n){
  step = Math.max(1, Math.min(totalSteps, n));
  steps.forEach(s => s.style.display = (Number(s.dataset.step) === step) ? 'block' : 'none');
  wizardTitle.textContent = titles[step];
  wizardCount.textContent = step + ' / ' + totalSteps;
  stepHint.textContent = hints[step];
  renderSegments();
  updateNextState();
  btnBack.classList.toggle('btn-disabled', step === 1);

  if(step === totalSteps){
    document.getElementById('sumPlatform').textContent = state.platform || '—';
    document.getElementById('sumVersion').textContent = state.allVersions ? 'Semua versi' : state.version;
    document.getElementById('sumFavicon').textContent = state.faviconName || 'Tidak diunggah';
    btnNext.textContent = 'Unduh ke Perangkat';
    downloadNote.style.display = 'block';
  } else {
    btnNext.textContent = 'Lanjut';
    downloadNote.style.display = 'none';
  }
}

document.querySelectorAll('.choice[data-field="platform"]').forEach(btn=>{
  btn.addEventListener('click', () => {
    document.querySelectorAll('.choice[data-field="platform"]').forEach(b=>b.classList.remove('sel'));
    btn.classList.add('sel');
    state.platform = btn.dataset.value;
    updateNextState();
  });
});

document.getElementById('versionSelect').addEventListener('change', e => {
  state.version = e.target.value;
});
document.getElementById('allVersions').addEventListener('change', e => {
  state.allVersions = e.target.checked;
  document.getElementById('versionSelect').disabled = e.target.checked;
});

document.getElementById('faviconInput').addEventListener('change', e => {
  const file = e.target.files[0];
  if(!file) return;
  state.faviconName = file.name;
  document.getElementById('fileLabel').textContent = file.name;
  const reader = new FileReader();
  reader.onload = ev => {
    document.getElementById('previewImg').src = ev.target.result;
    document.getElementById('previewName').textContent = file.name;
    document.getElementById('previewChip').style.display = 'flex';
  };
  reader.readAsDataURL(file);
});

btnBack.addEventListener('click', () => { if(step > 1) goTo(step - 1); });

btnNext.addEventListener('click', () => {
  if(btnNext.classList.contains('btn-disabled')) return;
  if(step < totalSteps){
    goTo(step + 1);
  } else {
    // Build a mock .mcaddon file and trigger download
    const meta = {
      engine: 'REV Engine Gen 1',
      platform: state.platform,
      version: state.allVersions ? 'all' : state.version,
      favicon: state.faviconName || 'none',
      generated: new Date().toISOString()
    };
    const blob = new Blob([JSON.stringify(meta, null, 2)], {type:'application/octet-stream'});
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    const safeName = 'rev-engine-mod-' + (state.platform || 'output').toLowerCase();
    a.href = url;
    a.download = safeName + '.mcaddon';
    document.body.appendChild(a);
    a.click();
    a.remove();
    URL.revokeObjectURL(url);
    btnNext.textContent = 'Terunduh ✓';
  }
});

goTo(1);
</script>

</body>
</html>

