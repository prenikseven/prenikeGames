<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
<title>Oyun Merkezi - Ultra Premium 3D Style</title>
<style>
  * { margin:0; padding:0; box-sizing:border-box; }
  body {
    background: linear-gradient(135deg, #0a0018, #001133);
    color: #fff;
    font-family: 'Segoe UI', system-ui, sans-serif;
    overflow: hidden;
    touch-action: none;
  }
  #menu {
    position: absolute;
    inset: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    z-index: 100;
    background: rgba(8,0,35,0.94);
  }
  .title {
    font-size: 62px;
    font-weight: 900;
    margin-bottom: 70px;
    background: linear-gradient(90deg, #00ffff, #ff00cc, #00ffff);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    text-shadow: 0 0 50px rgba(0,255,255,0.7);
    letter-spacing: -2px;
  }
  .games-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 35px;
    max-width: 1300px;
    padding: 20px;
  }
  .game-btn {
    background: linear-gradient(145deg, rgba(255,255,255,0.1), rgba(0,255,255,0.15));
    border: 6px solid #00ffff;
    color: #fff;
    padding: 35px 25px;
    font-size: 31px;
    border-radius: 28px;
    cursor: pointer;
    transition: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
    text-align: center;
    box-shadow: 0 15px 40px rgba(0,255,255,0.3);
    position: relative;
    overflow: hidden;
  }
  .game-btn::before {
    content: '';
    position: absolute;
    top: -50%;
    left: -50%;
    width: 40%;
    height: 300%;
    background: linear-gradient(120deg, transparent, rgba(255,255,255,0.4), transparent);
    transform: skewX(-25deg);
    animation: shine 4s linear infinite;
  }
  @keyframes shine { 100% { transform: translateX(300%); } }
  .game-btn:hover {
    transform: translateY(-18px) scale(1.08);
    box-shadow: 0 30px 70px rgba(0,255,255,0.6);
  }
  .container {
    width: 100vw;
    height: 100vh;
    position: absolute;
    top: 0; left: 0;
    display: none;
    background: #000;
  }
  canvas { 
    display: block; 
    image-rendering: crisp-edges;
    box-shadow: 0 0 80px rgba(0,255,255,0.5);
  }
  .back-btn {
    position: absolute;
    top: 25px;
    left: 25px;
    z-index: 999;
    padding: 16px 36px;
    background: linear-gradient(#ff2266, #aa0022);
    color: white;
    border: none;
    border-radius: 14px;
    font-size: 21px;
    cursor: pointer;
    transition: all 0.3s;
    box-shadow: 0 8px 25px rgba(255,34,102,0.4);
  }
  .back-btn:hover { transform: scale(1.12); }
  .score {
    position: absolute;
    top: 30px;
    right: 30px;
    font-size: 36px;
    z-index: 900;
    background: rgba(0,0,0,0.7);
    padding: 14px 34px;
    border-radius: 18px;
    border: 3px solid #00ffff;
    box-shadow: 0 0 30px rgba(0,255,255,0.5);
    font-weight: bold;
  }
</style>
</head>
<body>
<!-- ANA MENÜ -->
<div id="menu">
    <h1 class="title">🎮 ULTRA OYUN MERKEZİ</h1>
    <div class="games-grid">
        <div class="game-btn" onclick="startGame('gd')">Geometry Dash Lite</div>
        <div class="game-btn" onclick="startGame('snake')">🐍 Yılan</div>
        <div class="game-btn" onclick="startGame('mine')">💣 Mayın Tarlası</div>
        <div class="game-btn" onclick="startGame('pong')">🏓 Ping Pong 3D</div>
        <div class="game-btn" onclick="startGame('minecraft')">🧱 Minecraft</div> <!-- Yeni -->
    </div>
</div>

<!-- GEOMETRY DASH LITE -->
<div id="gdContainer" class="container">
    <button class="back-btn" onclick="backToMenu()">← Menüye Dön</button>
    <div id="gdLoader" style="position:absolute;inset:0;background:rgba(10,0,40,0.96);display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:10;color:#0ff;">
        <div style="width:85px;height:85px;border:12px solid rgba(255,255,255,0.2);border-top-color:#00ffff;border-radius:50%;animation:spin 0.85s linear infinite;margin-bottom:25px;"></div>
        <p style="font-size:24px;letter-spacing:1px;">Geometry Dash Lite Yükleniyor...</p>
    </div>
    <div id="gameContainer" style="width:100%;height:100%;"></div>
</div>

<!-- YILAN -->
<div id="snakeContainer" class="container">
    <button class="back-btn" onclick="backToMenu()">← Menüye Dön</button>
    <canvas id="snakeCanvas" width="720" height="720"></canvas>
    <div class="score" id="snakeScore">Skor: 0</div>
</div>

<!-- MAYIN TARLASI -->
<div id="mineContainer" class="container" style="display:flex;align-items:center;justify-content:center;flex-direction:column;background:#000c22;">
    <button class="back-btn" onclick="backToMenu()">← Menüye Dön</button>
    <h2 style="margin-bottom:25px;font-size:30px;text-shadow:0 0 20px #0ff;">Mayın Tarlası (9×9)</h2>
    <div id="mineBoard" style="display:grid;grid-template-columns:repeat(9,48px);gap:5px;background:#112;padding:25px;border-radius:20px;box-shadow:0 0 60px rgba(0,255,255,0.3);"></div>
    <p id="mineInfo" style="margin-top:30px;font-size:26px;"></p>
</div>

<!-- PING PONG -->
<div id="pongContainer" class="container">
    <button class="back-btn" onclick="backToMenu()">← Menüye Dön</button>
    <canvas id="pongCanvas" width="1000" height="560"></canvas>
    <div class="score" id="pongScore">0 : 0</div>
</div>

<!-- YENİ: MINECRAFT -->
<div id="minecraftContainer" class="container" style="display:none;">
    <button class="back-btn" onclick="backToMenu()">← Menüye Dön</button>
    <!-- Minecraft gömme iframe veya oyun bağlantısı -->
    <iframe src="https://classic.minecraft.net/" style="width:100%; height:100%; border:none;"></iframe>
</div>

<script src="https://rawcdn.githack.com/genizy/google-class/main/gdlite/Build/UnityLoader.js"></script>
<script>
let currentGame = null;
let gameInstance = null;
let pongAnim;

function startGame(game) {
    document.getElementById('menu').style.display = 'none';
    currentGame = game;
    document.querySelectorAll('.container').forEach(c => c.style.display = 'none');

    if (game === 'gd') {
        document.getElementById('gdContainer').style.display = 'block';
        if (!gameInstance) loadGD();
    } else if (game === 'snake') {
        document.getElementById('snakeContainer').style.display = 'block';
        initSnake();
    } else if (game === 'mine') {
        document.getElementById('mineContainer').style.display = 'flex';
        initMinesweeper();
    } else if (game === 'pong') {
        document.getElementById('pongContainer').style.display = 'block';
        initPong();
    } else if (game === 'minecraft') {
        document.getElementById('minecraftContainer').style.display = 'block';
    }
}

function backToMenu() {
    document.querySelectorAll('.container').forEach(c => c.style.display = 'none');
    document.getElementById('menu').style.display = 'falex';
    currentGame = null;

    // Minecraft durumu sıfırlama veya durdurma işlemleri buraya eklenebilir
}

// ... diğer oyun fonksiyonları (loadGD, initSnake, initMinesweeper, initPong) devam eder ...

// Bu örnekte, Minecraft iframe doğrudan gösteriliyor. Gelişmiş iframe veya gömme yöntemi kullanabilirsiniz.

</script>
</body>
</html>
