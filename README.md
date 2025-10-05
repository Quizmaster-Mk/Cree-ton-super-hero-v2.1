<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Deviens un Super-Héros</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Bangers&display=swap');

  body {
    font-family: 'Bangers', cursive;
    background: radial-gradient(circle at center, #000 50%, #111);
    color: white;
    text-align: center;
    margin: 0;
    padding: 0;
    overflow-x: hidden;
  }

  .intro, .creator, .mission, .result {
    animation: fadeIn 1.2s;
  }

  h1 {
    color: #ffeb3b;
    text-shadow: 3px 3px red;
    font-size: clamp(30px, 7vw, 48px);
  }

  .start-btn {
    background: #ff4444;
    border: none;
    padding: 15px 30px;
    border-radius: 12px;
    font-size: 22px;
    color: #fff;
    cursor: pointer;
    margin-top: 20px;
    transition: 0.3s;
  }

  .start-btn:hover { transform: scale(1.05); background: #ff6666; }

  .creator, .mission, .result {
    display: none;
    background: #111;
    padding: 25px;
    border-top: 3px solid yellow;
  }

  select, input {
    font-family: 'Bangers', cursive;
    width: 80%;
    margin: 8px 0;
    padding: 10px;
    font-size: 18px;
    text-align: center;
    border-radius: 8px;
    border: none;
  }

  button {
    background: #ff4444;
    color: white;
    border: none;
    padding: 12px 20px;
    border-radius: 10px;
    font-size: 20px;
    cursor: pointer;
    transition: 0.3s;
    width: 85%;
    margin-top: 10px;
  }

  button:hover { background: #ff6666; transform: scale(1.05); }

  .hero-card {
    display: none;
    margin-top: 20px;
    padding: 20px;
    border-radius: 10px;
    background: rgba(0,0,0,0.8);
    border: 3px solid #ffeb3b;
    box-shadow: 0 0 15px #ffeb3b;
    animation: fadeIn 1s;
  }

  .hero-emoji { font-size: 60px; margin-bottom: 10px; }
  .hero-name { font-size: 30px; color: #ffeb3b; text-shadow: 2px 2px red; }
  .hero-line { margin: 8px 0; font-size: 20px; }
  .devise { color: #00e5ff; font-style: italic; }

  .choice {
    background: rgba(255,255,255,0.1);
    margin: 12px auto;
    padding: 10px;
    border-radius: 10px;
    max-width: 400px;
    cursor: pointer;
    transition: 0.3s;
  }
  .choice:hover { background: rgba(255,255,255,0.2); transform: scale(1.05); }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }

  canvas#bg {
    position: fixed;
    top: 0; left: 0;
    z-index: -1;
    width: 100%; height: 100%;
  }
</style>
</head>
<body>

<canvas id="bg"></canvas>

<!-- INTRO -->
<div class="intro" id="intro">
  <h1>💥 Ton destin t’appelle ! 💥</h1>
  <p>Une explosion cosmique bouleverse ton ADN. Tes pouvoirs s’éveillent… 🌀<br>
  Qui deviendras-tu ? Héros ou menace ? Le monde attend ton nom.</p>
  <button class="start-btn" onclick="startCreation()">Commencer ma transformation</button>
</div>

<!-- CREATOR -->
<div class="creator" id="creator">
  <input type="text" id="heroName" placeholder="Nom de ton héros...">
  <select id="heroPower">
    <option value="">-- Pouvoir principal --</option>
    <option>Contrôle du feu 🔥</option>
    <option>Super vitesse ⚡</option>
    <option>Télépathie 🧠</option>
    <option>Force surhumaine 💪</option>
    <option>Invisibilité 👻</option>
    <option>Technologie avancée 🤖</option>
    <option>Énergie cosmique 🌌</option>
  </select>
  <select id="heroOrigin">
    <option value="">-- Origine --</option>
    <option>New York 🇺🇸</option>
    <option>Wakanda 🌍</option>
    <option>Asgard 🌩️</option>
    <option>Gotham City 🌃</option>
    <option>Tokyo 🇯🇵</option>
    <option>Krypton 💫</option>
  </select>
  <select id="heroEmoji">
    <option value="">-- Avatar --</option>
    <option>🦸</option><option>🦹</option><option>🤖</option><option>🕷️</option>
    <option>🐆</option><option>⚡</option><option>🔥</option><option>💀</option>
  </select>
  <input type="text" id="heroMotto" placeholder="Devise héroïque...">
  <button onclick="generateHero()">✨ Révéler mon identité</button>
</div>

<!-- HERO CARD -->
<div class="hero-card" id="heroCard">
  <div class="hero-emoji" id="displayEmoji">🦸</div>
  <div class="hero-name" id="displayName"></div>
  <div class="hero-line" id="displayPower"></div>
  <div class="hero-line" id="displayOrigin"></div>
  <div class="hero-line devise" id="displayMotto"></div>
  <p>🌌 Le monde a changé... et tu en es désormais le gardien.</p>
  <button onclick="startMission()">💥 Première mission</button>
</div>

<!-- MISSION -->
<div class="mission" id="mission">
  <h2>🚨 Alerte à la ville !</h2>
  <p>Une explosion secoue ton quartier ! Des civils sont piégés dans un immeuble en feu.  
  Tu ressens l’appel du devoir...</p>

  <div class="choice" onclick="missionResult('sauver')">🦸 Sauver les civils</div>
  <div class="choice" onclick="missionResult('combattre')">💥 Affronter le criminel responsable</div>
  <div class="choice" onclick="missionResult('analyser')">🧠 Analyser la situation depuis un toit</div>
</div>

<!-- RESULT -->
<div class="result" id="result">
  <h2 id="resultTitle"></h2>
  <p id="resultText"></p>
  <button onclick="restart()">🔁 Rejouer</button>
</div>

<script>
// --- Étape 1 : Création du héros ---
function startCreation() {
  document.getElementById('intro').style.display = 'none';
  document.getElementById('creator').style.display = 'block';
  playSound();
}

function generateHero() {
  const name = document.getElementById('heroName').value.trim() || "Héros mystère";
  const power = document.getElementById('heroPower').value || "Pouvoir inconnu";
  const origin = document.getElementById('heroOrigin').value || "Origine secrète";
  const emoji = document.getElementById('heroEmoji').value || "🦸";
  const motto = document.getElementById('heroMotto').value.trim() || "La justice triomphe toujours !";

  document.getElementById('displayEmoji').innerText = emoji;
  document.getElementById('displayName').innerText = name;
  document.getElementById('displayPower').innerText = "Pouvoir : " + power;
  document.getElementById('displayOrigin').innerText = "Origine : " + origin;
  document.getElementById('displayMotto').innerText = `"${motto}"`;

  document.getElementById('heroCard').style.display = 'block';
  changeBackground(power);
}

// --- Étape 2 : Mission interactive ---
function startMission() {
  document.getElementById('heroCard').style.display = 'none';
  document.getElementById('mission').style.display = 'block';
}

function missionResult(choice) {
  document.getElementById('mission').style.display = 'none';
  document.getElementById('result').style.display = 'block';

  const name = document.getElementById('displayName').innerText;
  let title = "";
  let text = "";

  if (choice === "sauver") {
    title = "👏 Héros du peuple !";
    text = `${name} s’élance dans les flammes et sauve des dizaines de vies.  
    Les habitants scandent ton nom. Un symbole est né.`;
    changeBackground("feu");
  } else if (choice === "combattre") {
    title = "💥 Combat épique !";
    text = `${name} affronte le criminel dans un duel titanesque.  
    Les éclairs illuminent la ville… et ta légende commence.`;
    changeBackground("énergie");
  } else {
    title = "🧠 Maître stratège !";
    text = `${name} observe, analyse, et découvre un complot bien plus vaste.  
    L’ombre t’appelle vers ta prochaine mission...`;
    changeBackground("technologie");
  }

  document.getElementById('resultTitle').innerText = title;
  document.getElementById('resultText').innerText = text;
}

function restart() {
  window.scrollTo(0, 0);
  location.reload();
}

// --- Animation de fond ---
const canvas = document.getElementById('bg');
const ctx = canvas.getContext('2d');
let particles = [];
resizeCanvas();
window.addEventListener('resize', resizeCanvas);

function resizeCanvas() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
}

function createParticles(color) {
  particles = [];
  for (let i = 0; i < 60; i++) {
    particles.push({
      x: Math.random() * canvas.width,
      y: Math.random() * canvas.height,
      r: Math.random() * 3 + 1,
      dx: (Math.random() - 0.5) * 1.5,
      dy: (Math.random() - 0.5) * 1.5,
      color
    });
  }
}

function animate() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  particles.forEach(p => {
    ctx.beginPath();
    ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
    ctx.fillStyle = p.color;
    ctx.fill();
    p.x += p.dx;
    p.y += p.dy;
    if (p.x < 0 || p.x > canvas.width) p.dx *= -1;
    if (p.y < 0 || p.y > canvas.height) p.dy *= -1;
  });
  requestAnimationFrame(animate);
}
animate();

function changeBackground(power) {
  let color = '#fff';
  if (power.includes('feu')) color = 'rgba(255,80,0,0.7)';
  else if (power.includes('vitesse')) color = 'rgba(255,255,0,0.7)';
  else if (power.includes('Technologie') || power.includes('technologie')) color = 'rgba(0,200,255,0.7)';
  else if (power.includes('Énergie') || power.includes('énergie')) color = 'rgba(150,0,255,0.7)';
  else color = 'rgba(255,255,255,0.5)';
  createParticles(color);
}

// --- Son ---
function playSound() {
  const audio = new Audio('https://cdn.pixabay.com/download/audio/2022/03/15/audio_c17f61c367.mp3?filename=cinematic-whoosh-transition-1-14687.mp3');
  audio.volume = 0.2;
  audio.play();
}
</script>

</body>
</html>
