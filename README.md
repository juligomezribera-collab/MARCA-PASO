# MARCA-PASO
Calculadora web interactiva para tiempos de running, precisa y útil. Ideal para atletas y corredores. ¡Consigue tu mejor marca con solo un clic!
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MarcaPaso — Calculadora de ritmo para runners</title>
<meta name="description" content="Calcula el tiempo que tardarás en completar cualquier distancia según tu ritmo. Hecho por y para runners.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Work+Sans:wght@400;500;600;700&family=JetBrains+Mono:wght@500;700&display=swap" rel="stylesheet">
<style>
  :root{
    --track-black:#14171c;
    --track-black-2:#1b1f26;
    --dusk-blue:#1c2e4a;
    --lane-red:#e4572e;
    --lane-red-dim:#b5431f;
    --lime-flash:#c9ff3e;
    --chalk-white:#f5f5f0;
    --smoke:#9aa1ad;
    --radius:14px;
  }
  *{box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{
    margin:0;
    background:var(--track-black);
    color:var(--chalk-white);
    font-family:'Work Sans',sans-serif;
    overflow-x:hidden;
  }
  h1,h2,.display{
    font-family:'Bebas Neue',sans-serif;
    letter-spacing:0.03em;
    line-height:0.95;
  }
  .mono{
    font-family:'JetBrains Mono',monospace;
  }

  /* ---------- HERO ---------- */
  .hero{
    position:relative;
    height:56vh;
    min-height:380px;
    width:100%;
    overflow:hidden;
    display:flex;
    align-items:center;
    justify-content:center;
  }
  .hero-slide{
    position:absolute;
    inset:0;
    background-size:cover;
    background-position:center;
    opacity:0;
    transform:scale(1.06);
    transition:opacity 1.4s ease, transform 8s ease;
  }
  .hero-slide.active{
    opacity:1;
    transform:scale(1);
  }
  .hero::after{
    content:"";
    position:absolute;
    inset:0;
    background:linear-gradient(180deg, rgba(20,23,28,0.55) 0%, rgba(20,23,28,0.35) 40%, rgba(20,23,28,0.85) 100%);
    z-index:2;
  }
  .brand{
    position:absolute;
    top:22px;
    left:26px;
    z-index:4;
    font-family:'Bebas Neue',sans-serif;
    font-size:1.4rem;
    letter-spacing:0.12em;
    color:var(--chalk-white);
  }
  .brand span{color:var(--lime-flash);}
  .hero-content{
    position:relative;
    z-index:3;
    text-align:center;
    padding:0 20px;
    max-width:900px;
  }
  .eyebrow{
    font-family:'JetBrains Mono',monospace;
    font-size:0.72rem;
    letter-spacing:0.28em;
    text-transform:uppercase;
    color:var(--lime-flash);
    margin-bottom:14px;
    display:block;
  }
  .slogan{
    font-size:clamp(2.1rem, 6.4vw, 4.6rem);
    color:var(--chalk-white);
    text-shadow:0 4px 24px rgba(0,0,0,0.45);
    margin:0;
  }
  .slogan em{
    font-style:normal;
    color:var(--lane-red);
  }

  /* ---------- ARROW / INVITE SECTION ---------- */
  .invite{
    min-height:44vh;
    display:flex;
    flex-direction:column;
    align-items:center;
    justify-content:center;
    gap:22px;
    padding:40px 20px;
    background:
      radial-gradient(ellipse at 50% 0%, rgba(228,87,46,0.10), transparent 60%),
      var(--track-black);
    text-align:center;
  }
  .invite p{
    color:var(--smoke);
    max-width:520px;
    font-size:0.98rem;
    margin:0;
  }
  .arrow-btn{
    appearance:none;
    border:none;
    cursor:pointer;
    background:var(--lane-red);
    color:var(--track-black);
    display:flex;
    flex-direction:column;
    align-items:center;
    gap:6px;
    padding:22px 34px;
    border-radius:100px;
    font-family:'Bebas Neue',sans-serif;
    font-size:1.05rem;
    letter-spacing:0.1em;
    box-shadow:0 12px 34px rgba(228,87,46,0.35);
    transition:transform 0.2s ease, box-shadow 0.2s ease, background 0.2s ease;
    animation:bob 2.2s ease-in-out infinite;
  }
  .arrow-btn:hover{
    background:var(--lime-flash);
    transform:translateY(4px);
    box-shadow:0 8px 22px rgba(201,255,62,0.35);
  }
  .arrow-btn svg{width:30px;height:30px;}
  @keyframes bob{
    0%,100%{transform:translateY(0);}
    50%{transform:translateY(10px);}
  }

  /* ---------- CALCULATOR ---------- */
  .calc-section{
    background:var(--dusk-blue);
    padding:64px 20px 90px;
    display:flex;
    justify-content:center;
  }
  .calc-card{
    width:100%;
    max-width:760px;
    background:var(--track-black-2);
    border-radius:22px;
    padding:34px clamp(18px, 4vw, 44px) 42px;
    border:1px solid rgba(255,255,255,0.06);
    box-shadow:0 30px 60px rgba(0,0,0,0.35);
  }
  .calc-title{
    font-size:2.2rem;
    color:var(--chalk-white);
    margin:0 0 4px;
    text-align:center;
  }
  .calc-sub{
    text-align:center;
    color:var(--smoke);
    font-size:0.92rem;
    margin:0 0 30px;
  }
  .lane-row{
    padding:24px 0;
    border-top:2px dashed rgba(245,245,240,0.12);
  }
  .lane-row:first-of-type{border-top:none;}
  .lane-label{
    display:flex;
    align-items:baseline;
    gap:10px;
    margin-bottom:16px;
  }
  .lane-num{
    font-family:'JetBrains Mono',monospace;
    color:var(--lane-red);
    font-size:0.85rem;
  }
  .lane-title{
    font-family:'Bebas Neue',sans-serif;
    font-size:1.3rem;
    letter-spacing:0.08em;
    color:var(--chalk-white);
  }

  /* distance chips */
  .chip-grid{
    display:flex;
    flex-wrap:wrap;
    gap:10px;
  }
  .chip{
    appearance:none;
    cursor:pointer;
    border:1px solid rgba(245,245,240,0.18);
    background:transparent;
    color:var(--chalk-white);
    font-family:'Work Sans',sans-serif;
    font-size:0.86rem;
    font-weight:500;
    padding:9px 16px;
    border-radius:100px;
    transition:all 0.15s ease;
  }
  .chip:hover{border-color:var(--lime-flash);}
  .chip.selected{
    background:var(--lime-flash);
    border-color:var(--lime-flash);
    color:var(--track-black);
    font-weight:700;
  }

  /* pace slider */
  .pace-readout{
    font-family:'JetBrains Mono',monospace;
    font-size:2.4rem;
    color:var(--lime-flash);
    margin-bottom:14px;
  }
  .pace-readout span{
    font-size:1rem;
    color:var(--smoke);
    font-family:'Work Sans',sans-serif;
  }
  input[type="range"]{
    -webkit-appearance:none;
    width:100%;
    height:8px;
    border-radius:6px;
    background:linear-gradient(90deg, var(--lane-red), var(--lime-flash));
    outline:none;
  }
  input[type="range"]::-webkit-slider-thumb{
    -webkit-appearance:none;
    width:26px;
    height:26px;
    border-radius:50%;
    background:var(--chalk-white);
    border:4px solid var(--track-black-2);
    box-shadow:0 0 0 2px var(--lime-flash);
    cursor:pointer;
  }
  input[type="range"]::-moz-range-thumb{
    width:26px;
    height:26px;
    border-radius:50%;
    background:var(--chalk-white);
    border:4px solid var(--track-black-2);
    box-shadow:0 0 0 2px var(--lime-flash);
    cursor:pointer;
  }
  .pace-scale{
    display:flex;
    justify-content:space-between;
    color:var(--smoke);
    font-family:'JetBrains Mono',monospace;
    font-size:0.7rem;
    margin-top:8px;
  }

  /* result scoreboard */
  .scoreboard{
    background:var(--track-black);
    border-radius:16px;
    padding:26px 20px;
    text-align:center;
    border:1px solid rgba(201,255,62,0.18);
  }
  .scoreboard-label{
    font-family:'JetBrains Mono',monospace;
    font-size:0.72rem;
    letter-spacing:0.24em;
    text-transform:uppercase;
    color:var(--smoke);
    margin-bottom:10px;
    display:block;
  }
  .scoreboard-time{
    font-family:'JetBrains Mono',monospace;
    font-weight:700;
    font-size:clamp(2.6rem, 9vw, 4rem);
    color:var(--lime-flash);
    text-shadow:0 0 18px rgba(201,255,62,0.35);
    letter-spacing:0.04em;
  }
  .scoreboard-detail{
    margin-top:10px;
    color:var(--smoke);
    font-size:0.88rem;
  }
  .scoreboard-detail b{color:var(--chalk-white);}

  footer{
    text-align:center;
    padding:26px 20px 34px;
    color:var(--smoke);
    font-size:0.78rem;
    background:var(--dusk-blue);
  }
  footer a{color:var(--lime-flash);text-decoration:none;}

  @media (max-width:520px){
    .calc-card{padding:28px 16px 34px;border-radius:18px;}
    .lane-row{padding:20px 0;}
  }
</style>
</head>
<body>

  <!-- ================= HERO ================= -->
  <section class="hero" id="hero">
    <div class="hero-slide active" style="background-image:url('https://images.unsplash.com/photo-1516398810565-0cb4310bb8ea?auto=format&fit=crop&w=1600&q=80')"></div>
    <div class="hero-slide" style="background-image:url('https://images.unsplash.com/photo-1514489024785-d5ba8dfb2198?auto=format&fit=crop&w=1600&q=80')"></div>
    <div class="hero-slide" style="background-image:url('https://images.unsplash.com/photo-1766970096346-937852c7d350?auto=format&fit=crop&w=1600&q=80')"></div>
    <div class="hero-slide" style="background-image:url('https://images.unsplash.com/photo-1615230106436-fd04a9cbaef6?auto=format&fit=crop&w=1600&q=80')"></div>

    <div class="brand">MARCA<span>PASO</span></div>

    <div class="hero-content">
      <span class="eyebrow">Calculadora de ritmo · Para runners de todo el mundo</span>
      <h1 class="slogan">Tu distancia, tu ritmo:<br><em>tu tiempo.</em></h1>
    </div>
  </section>

  <!-- ================= INVITE / ARROW ================= -->
  <section class="invite">
    <p>Elige tu distancia y tu ritmo objetivo, y descubre al instante el tiempo que necesitas para cruzar la meta.</p>
    <button class="arrow-btn" id="scrollBtn" aria-label="Bajar a la calculadora">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round">
        <polyline points="6 9 12 15 18 9"></polyline>
      </svg>
      BAJA PARA CALCULARLO
    </button>
  </section>

  <!-- ================= CALCULATOR ================= -->
  <section class="calc-section" id="calculadora">
    <div class="calc-card">
      <h2 class="calc-title">Calculadora de ritmo</h2>
      <p class="calc-sub">Tres pasos. Un resultado. Sin excusas.</p>

      <!-- FILA 1: DISTANCIA -->
      <div class="lane-row">
        <div class="lane-label">
          <span class="lane-num">01</span>
          <span class="lane-title">DISTANCIA</span>
        </div>
        <div class="chip-grid" id="distanceGrid"></div>
      </div>

      <!-- FILA 2: RITMO -->
      <div class="lane-row">
        <div class="lane-label">
          <span class="lane-num">02</span>
          <span class="lane-title">RITMO</span>
        </div>
        <div class="pace-readout"><span id="paceValue">5:00</span> <span>min/km</span></div>
        <input type="range" id="paceSlider" min="120" max="420" step="15" value="300">
        <div class="pace-scale">
          <span>2:00 (rápido)</span>
          <span>7:00 (suave)</span>
        </div>
      </div>

      <!-- FILA 3: RESULTADO -->
      <div class="lane-row">
        <div class="lane-label">
          <span class="lane-num">03</span>
          <span class="lane-title">RESULTADO</span>
        </div>
        <div class="scoreboard">
          <span class="scoreboard-label">Tiempo estimado</span>
          <div class="scoreboard-time" id="resultTime">00:00</div>
          <div class="scoreboard-detail">
            <b id="resultDistance">5000 m</b> a <b id="resultPace">5:00 min/km</b>
          </div>
        </div>
      </div>
    </div>
  </section>

  <footer>
    Hecho con 🧡 para runners de todo el mundo · MarcaPaso
  </footer>

<script>
  // ---------- HERO CAROUSEL ----------
  const slides = document.querySelectorAll('.hero-slide');
  let current = 0;
  setInterval(() => {
    slides[current].classList.remove('active');
    current = (current + 1) % slides.length;
    slides[current].classList.add('active');
  }, 4000);

  // ---------- SCROLL BUTTON ----------
  document.getElementById('scrollBtn').addEventListener('click', () => {
    document.getElementById('calculadora').scrollIntoView({ behavior: 'smooth' });
  });

  // ---------- DISTANCES ----------
  const distances = [
    { meters: 400,   label: '400 m' },
    { meters: 800,   label: '800 m' },
    { meters: 1000,  label: '1000 m' },
    { meters: 1500,  label: '1500 m' },
    { meters: 1609,  label: '1 milla' },
    { meters: 2000,  label: '2000 m' },
    { meters: 3000,  label: '3000 m' },
    { meters: 3218,  label: '2 millas' },
    { meters: 5000,  label: '5 km' },
    { meters: 10000, label: '10 km' },
    { meters: 21097, label: 'Media maratón' },
    { meters: 42195, label: 'Maratón' }
  ];

  const grid = document.getElementById('distanceGrid');
  let selectedDistance = 5000;

  distances.forEach(d => {
    const btn = document.createElement('button');
    btn.className = 'chip' + (d.meters === selectedDistance ? ' selected' : '');
    btn.textContent = d.label;
    btn.dataset.meters = d.meters;
    btn.addEventListener('click', () => {
      selectedDistance = d.meters;
      document.querySelectorAll('.chip').forEach(c => c.classList.remove('selected'));
      btn.classList.add('selected');
      calculate();
    });
    grid.appendChild(btn);
  });

  // ---------- PACE ----------
  const paceSlider = document.getElementById('paceSlider');
  const paceValueEl = document.getElementById('paceValue');

  function formatPace(totalSeconds) {
    const m = Math.floor(totalSeconds / 60);
    const s = totalSeconds % 60;
    return `${m}:${s.toString().padStart(2, '0')}`;
  }

  paceSlider.addEventListener('input', calculate);

  // ---------- CALCULATE ----------
  function calculate() {
    const paceSeconds = parseInt(paceSlider.value, 10);
    paceValueEl.textContent = formatPace(paceSeconds);

    const totalSeconds = Math.round((selectedDistance / 1000) * paceSeconds);
    const h = Math.floor(totalSeconds / 3600);
    const m = Math.floor((totalSeconds % 3600) / 60);
    const s = totalSeconds % 60;

    let display;
    if (h > 0) {
      display = `${h}:${m.toString().padStart(2,'0')}:${s.toString().padStart(2,'0')}`;
    } else {
      display = `${m.toString().padStart(2,'0')}:${s.toString().padStart(2,'0')}`;
    }

    document.getElementById('resultTime').textContent = display;

    const distObj = distances.find(d => d.meters === selectedDistance);
    document.getElementById('resultDistance').textContent = distObj ? distObj.label : `${selectedDistance} m`;
    document.getElementById('resultPace').textContent = `${formatPace(paceSeconds)} min/km`;
  }

  calculate();
</script>

</body>
</html>
