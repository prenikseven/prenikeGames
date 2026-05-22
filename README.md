<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
<title>Prenik Games - Ultra Premium 3D Style</title>
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
    width: 100%;
    justify-content: center;
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
  .canvas-container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
    flex-direction: column;
  }
  canvas { 
    display: block; 
    image-rendering: crisp-edges;
    box-shadow: 0 0 80px rgba(0,255,255,0.5);
    border: 4px solid #00ffff;
    border-radius: 10px;
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
  
  /* Difficulty Selection Overlay HUD */
  .diff-selector {
    display: flex;
    gap: 15px;
    margin-bottom: 20px;
    z-index: 910;
  }
  .diff-btn {
    padding: 10px 24px;
    border: 2px solid #00ffff;
    background: rgba(0, 255, 255, 0.1);
    color: #fff;
    font-size: 18px;
    font-weight: bold;
    border-radius: 10px;
    cursor: pointer;
    transition: all 0.2s;
  }
  .diff-btn:hover, .diff-btn.active {
    background: #00ffff;
    color: #000;
    box-shadow: 0 0 15px #00ffff;
  }
  
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
  @keyframes spin { 100% { transform: rotate(360deg); } }

  /* Minesweeper Grid Layout Modifications */
  .mine-cell {
    width: 48px;
    height: 48px;
    background: #1a1a3a;
    border: 2px solid #00ffff;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 20px;
    font-weight: bold;
    cursor: pointer;
    user-select: none;
    transition: all 0.2s;
  }
  .mine-cell.revealed {
    background: #0a0a20;
    border-color: #555;
  }
  .mine-cell.mine {
    background: #ff2266;
  }
</style>
</head>
<body>

<!-- MAIN MENU -->
<div id="menu">
    <h1 class="title">🎮 PRENIK GAMES</h1>
    <div class="games-grid">
        <div class="game-btn" onclick="startGame('gd')">Geometry Dash Lite</div>
        <div class="game-btn" onclick="startGame('snake')">🐍 Snake</div>
        <div class="game-btn" onclick="startGame('mine')">💣 Minesweeper</div>
        <div class="game-btn" onclick="startGame('pong')">🏓 Ping Pong 3D</div>
        <div class="game-btn" onclick="startGame('minecraft')">🧱 Minecraft</div>
    </div>
</div>

<!-- GEOMETRY DASH LITE -->
<div id="gdContainer" class="container">
    <button class="back-btn" onclick="backToMenu()">← Back to Menu</button>
    <div id="gdLoader" style="position:absolute;inset:0;background:rgba(10,0,40,0.96);display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:10;color:#0ff;">
        <div style="width:85px;height:85px;border:12px solid rgba(255,255,255,0.2);border-top-color:#00ffff;border-radius:50%;animation:spin 0.85s linear infinite;margin-bottom:25px;"></div>
        <p style="font-size:24px;letter-spacing:1px;">Loading Geometry Dash Lite...</p>
    </div>
    <div id="gameContainer" style="width:100%;height:100%;"></div>
</div>

<!-- SNAKE -->
<div id="snakeContainer" class="container">
    <button class="back-btn" onclick="backToMenu()">← Back to Menu</button>
    <div class="canvas-container">
        <div class="diff-selector">
            <button class="diff-btn" id="snake-easy" onclick="setSnakeDifficulty(150, 'easy')">Easy</button>
            <button class="diff-btn active" id="snake-normal" onclick="setSnakeDifficulty(100, 'normal')">Normal</button>
            <button class="diff-btn" id="snake-hard" onclick="setSnakeDifficulty(50, 'hard')">Hard</button>
        </div>
        <canvas id="snakeCanvas" width="600" height="600"></canvas>
    </div>
    <div class="score" id="snakeScore">Score: 0</div>
</div>

<!-- MINESWEEPER -->
<div id="mineContainer" class="container" style="display:flex;align-items:center;justify-content:center;flex-direction:column;background:#000c22;">
    <button class="back-btn" onclick="backToMenu()">← Back to Menu</button>
    <h2 style="margin-bottom:15px;font-size:30px;text-shadow:0 0 20px #0ff;">Minesweeper</h2>
    <div class="diff-selector">
        <button class="diff-btn active" id="mine-easy" onclick="setMineDifficulty(9, 10, 'easy')">Easy (9x9)</button>
        <button class="diff-btn" id="mine-normal" onclick="setMineDifficulty(12, 22, 'normal')">Normal (12x12)</button>
        <button class="diff-btn" id="mine-hard" onclick="setMineDifficulty(15, 40, 'hard')">Hard (15x15)</button>
    </div>
    <div id="mineBoard" style="display:grid;gap:5px;background:#112;padding:25px;border-radius:20px;box-shadow:0 0 60px rgba(0,255,255,0.3);"></div>
    <p id="mineInfo" style="margin-top:30px;font-size:26px;font-weight:bold;min-height:35px;"></p>
</div>

<!-- PING PONG -->
<div id="pongContainer" class="container">
    <button class="back-btn" onclick="backToMenu()">← Back to Menu</button>
    <div class="canvas-container">
        <div class="diff-selector">
            <button class="diff-btn" id="pong-easy" onclick="setPongDifficulty(3, 'easy')">Easy AI</button>
            <button class="diff-btn active" id="pong-normal" onclick="setPongDifficulty(4.5, 'normal')">Normal AI</button>
            <button class="diff-btn" id="pong-hard" onclick="setPongDifficulty(6.5, 'hard')">God Mode AI</button>
        </div>
        <canvas id="pongCanvas" width="1000" height="560"></canvas>
    </div>
    <div class="score" id="pongScore">0 : 0</div>
</div>

<!-- MINECRAFT -->
<div id="minecraftContainer" class="container">
    <button class="back-btn" onclick="backToMenu()">← Back to Menu</button>
    <iframe src="https://classic.minecraft.net/" style="width:100%; height:100%; border:none;"></iframe>
</div>

<script src="https://rawcdn.githack.com/genizy/google-class/main/gdlite/Build/UnityLoader.js"></script>
<script>
let currentGame = null;
let gameInstance = null;
let gdLoaded = false;

// Game Loops & Intervals
let snakeInterval = null;
let pongAnim = null;

// Difficulty Parameters
let snakeSpeed = 100;
let aiSpeed = 4.5;
let mineSize = 9;
let mineCount = 10;

function startGame(game) {
    document.getElementById('menu').style.display = 'none';
    currentGame = game;
    document.querySelectorAll('.container').forEach(c => c.style.display = 'none');

    if (game === 'gd') {
        document.getElementById('gdContainer').style.display = 'block';
        if (!gdLoaded) loadGD();
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
    if (snakeInterval) clearInterval(snakeInterval);
    if (pongAnim) cancelAnimationFrame(pongAnim);
    
    document.querySelectorAll('.container').forEach(c => c.style.display = 'none');
    document.getElementById('menu').style.display = 'flex';
    currentGame = null;
}

// ==========================================
// GEOMETRY DASH LITE ENGINE
// ==========================================
function loadGD() {
    try {
        gameInstance = UnityLoader.instantiate("gameContainer", "https://rawcdn.githack.com/genizy/google-class/main/gdlite/Build/gdlite.json", {
            onProgress: function(instance, progress) {
                if (progress === 1) {
                    document.getElementById('gdLoader').style.display = 'none';
                    gdLoaded = true;
                }
            }
        });
    } catch(e) {
        console.error("GD Lite asset loading failed.", e);
        document.getElementById('gdLoader').innerText = "Failed to load Geometry Dash.";
    }
}

// ==========================================
// SNAKE GAME ENGINE (WITH DIFFICULTY CONTROL)
// ==========================================
let snake, components, food, dx, dy, snakeScore;
const snakeCanvas = document.getElementById('snakeCanvas');
const snakeCtx = snakeCanvas.getContext('2d');

function setSnakeDifficulty(ms, level) {
    snakeSpeed = ms;
    document.querySelectorAll('[id^="snake-"]').forEach(b => b.classList.remove('active'));
    document.getElementById(`snake-${level}`).classList.add('active');
    initSnake();
}

function initSnake() {
    snake = [{x: 10, y: 10}];
    components = 20;
    dx = 1; dy = 0;
    snakeScore = 0;
    document.getElementById('snakeScore').innerText = "Score: " + snakeScore;
    generateFood();
    
    if (snakeInterval) clearInterval(snakeInterval);
    snakeInterval = setInterval(updateSnake, snakeSpeed);
}

function generateFood() {
    food = {
        x: Math.floor(Math.random() * components),
        y: Math.floor(Math.random() * components)
    };
}

window.addEventListener('keydown', e => {
    if (currentGame !== 'snake') return;
    if (e.key === 'ArrowUp' && dy === 0) { dx = 0; dy = -1; }
    if (e.key === 'ArrowDown' && dy === 0) { dx = 0; dy = 1; }
    if (e.key === 'ArrowLeft' && dx === 0) { dx = -1; dy = 0; }
    if (e.key === 'ArrowRight' && dx === 0) { dx = 1; dy = 0; }
});

function updateSnake() {
    const head = {x: snake[0].x + dx, y: snake[0].y + dy};
    
    if (head.x < 0 || head.x >= components || head.y < 0 || head.y >= components || snake.some(seg => seg.x === head.x && seg.y === head.y)) {
        clearInterval(snakeInterval);
        alert("Game Over! Score: " + snakeScore);
        initSnake();
        return;
    }

    snake.unshift(head);

    if (head.x === food.x && head.y === food.y) {
        snakeScore += 10;
        document.getElementById('snakeScore').innerText = "Score: " + snakeScore;
        generateFood();
    } else {
        snake.pop();
    }

    snakeCtx.fillStyle = '#050015';
    snakeCtx.fillRect(0, 0, snakeCanvas.width, snakeCanvas.height);

    const size = snakeCanvas.width / components;
    
    snakeCtx.fillStyle = '#ff00cc';
    snakeCtx.shadowBlur = 15;
    snakeCtx.shadowColor = '#ff00cc';
    snakeCtx.fillRect(food.x * size + 2, food.y * size + 2, size - 4, size - 4);

    snakeCtx.fillStyle = '#00ffff';
    snakeCtx.shadowColor = '#00ffff';
    snake.forEach(seg => {
        snakeCtx.fillRect(seg.x * size + 1, seg.y * size + 1, size - 2, size - 2);
    });
    snakeCtx.shadowBlur = 0;
}

// ==========================================
// MINESWEEPER ENGINE (WITH GRID SELECTION)
// ==========================================
let mineBoard = [];

function setMineDifficulty(size, count, level) {
    mineSize = size;
    mineCount = count;
    document.querySelectorAll('[id^="mine-"]').forEach(b => b.classList.remove('active'));
    document.getElementById(`mine-${level}`).classList.add('active');
    initMinesweeper();
}

function initMinesweeper() {
    const boardElement = document.getElementById('mineBoard');
    const infoElement = document.getElementById('mineInfo');
    
    // Dynamically re-adjust CSS template columns based on the difficulty size selected
    boardElement.style.gridTemplateColumns = `repeat(${mineSize}, 48px)`;
    boardElement.innerHTML = '';
    infoElement.innerText = '';
    infoElement.style.color = '#fff';
    mineBoard = [];

    for(let i=0; i<mineSize*mineSize; i++) {
        mineBoard.push({
            id: i,
            isMine: false,
            revealed: false,
            count: 0
        });
    }

    let placed = 0;
    while(placed < mineCount) {
        let rand = Math.floor(Math.random() * mineBoard.length);
        if(!mineBoard[rand].isMine) {
            mineBoard[rand].isMine = true;
            placed++;
        }
    }

    for(let i=0; i<mineBoard.length; i++) {
        if(mineBoard[i].isMine) continue;
        let neighbors = getNeighbors(i);
        let minesNear = neighbors.filter(n => mineBoard[n].isMine).length;
        mineBoard[i].count = minesNear;
    }

    mineBoard.forEach(cell => {
        const div = document.createElement('div');
        div.classList.add('mine-cell');
        div.dataset.id = cell.id;
        div.addEventListener('click', () => revealCell(cell.id));
        boardElement.appendChild(div);
    });
}

function getNeighbors(index) {
    let list = [];
    let r = Math.floor(index / mineSize);
    let c = index % mineSize;

    for(let i=-1; i<=1; i++) {
        for(let j=-1; j<=1; j++) {
            if(i===0 && j===0) continue;
            let nr = r + i;
            let nc = c + j;
            if(nr >= 0 && nr < mineSize && nc >= 0 && nc < mineSize) {
                list.push(nr * mineSize + nc);
            }
        }
    }
    return list;
}

function revealCell(id) {
    let cell = mineBoard[id];
    if(cell.revealed) return;
    
    const boardElement = document.getElementById('mineBoard');
    const cellDiv = boardElement.children[id];
    cell.revealed = true;
    cellDiv.classList.add('revealed');

    if(cell.isMine) {
        cellDiv.classList.add('mine');
        cellDiv.innerText = "💣";
        document.getElementById('mineInfo').innerText = "BOOM! Game Over!";
        document.getElementById('mineInfo').style.color = "#ff2266";
        mineBoard.forEach((c, idx) => {
            if(c.isMine) {
                boardElement.children[idx].classList.add('mine');
                boardElement.children[idx].innerText = "💣";
            }
        });
    } else {
        if(cell.count > 0) {
            cellDiv.innerText = cell.count;
            cellDiv.style.color = ['#00ffff','#00ff00','#ff00ff','#ffcc00'][cell.count-1] || '#ff0000';
        } else {
            let neighbors = getNeighbors(id);
            neighbors.forEach(n => revealCell(n));
        }
        checkMineWin();
    }
}

function checkMineWin() {
    let unrevealedSafeCells = mineBoard.filter(c => !c.isMine && !c.revealed).length;
    if(unrevealedSafeCells === 0) {
        document.getElementById('mineInfo').innerText = "Congratulations! You Won! 🎉";
        document.getElementById('mineInfo').style.color = "#00ffcc";
    }
}

// ==========================================
// PING PONG 3D NEON ENGINE (WITH AI ENGINE CONTROL)
// ==========================================
const pongCanvas = document.getElementById('pongCanvas');
const pongCtx = pongCanvas.getContext('2d');
let ball, p1, p2;

function setPongDifficulty(speed, level) {
    aiSpeed = speed;
    document.querySelectorAll('[id^="pong-"]').forEach(b => b.classList.remove('active'));
    document.getElementById(`pong-${level}`).classList.add('active');
    initPong();
}

function initPong() {
    ball = { x: 500, y: 280, vx: 5, vy: 3, radius: 12 };
    p1 = { x: 20, y: 220, w: 15, h: 110, score: 0 };
    p2 = { x: 965, y: 220, w: 15, h: 110, score: 0 };
    document.getElementById('pongScore').innerText = "0 : 0";
    
    if (pongAnim) cancelAnimationFrame(pongAnim);
    updatePong();
}

pongCanvas.addEventListener('mousemove', e => {
    let rect = pongCanvas.getBoundingClientRect();
    p1.y = e.clientY - rect.top - p1.h/2;
});

function updatePong() {
    ball.x += ball.vx;
    ball.y += ball.vy;

    // AI Follow Vector tracking utilizing chosen engine velocity parameter
    if(p2.y + p2.h/2 < ball.y - 10) p2.y += aiSpeed;
    else if(p2.y + p2.h/2 > ball.y + 10) p2.y -= aiSpeed;

    if(ball.y - ball.radius < 0 || ball.y + ball.radius > pongCanvas.height) ball.vy = -ball.vy;

    if(ball.x - ball.radius < p1.x + p1.w && ball.y > p1.y && ball.y < p1.y + p1.h) {
        ball.vx = -ball.vx;
        ball.vx *= 1.05; 
    }
    if(ball.x + ball.radius > p2.x && ball.y > p2.y && ball.y < p2.y + p2.h) {
        ball.vx = -ball.vx;
        ball.vx *= 1.05;
    }

    if(ball.x < 0) {
        p2.score++;
        resetBall();
    } else if(ball.x > pongCanvas.width) {
        p1.score++;
        resetBall();
    }

    document.getElementById('pongScore').innerText = `${p1.score} : ${p2.score}`;

    pongCtx.fillStyle = '#030010';
    pongCtx.fillRect(0, 0, pongCanvas.width, pongCanvas.height);

    pongCtx.strokeStyle = 'rgba(0,255,255,0.15)';
    pongCtx.lineWidth = 4;
    pongCtx.setLineDash([10, 10]);
    pongCtx.beginPath();
    pongCtx.moveTo(pongCanvas.width/2, 0);
    pongCtx.lineTo(pongCanvas.width/2, pongCanvas.height);
    pongCtx.stroke();
    pongCtx.setLineDash([]);

    pongCtx.shadowBlur = 20;
    
    pongCtx.fillStyle = '#ff00cc';
    pongCtx.shadowColor = '#ff00cc';
    pongCtx.fillRect(p1.x, p1.y, p1.w, p1.h);

    pongCtx.fillStyle = '#00ffff';
    pongCtx.shadowColor = '#00ffff';
    pongCtx.fillRect(p2.x, p2.y, p2.w, p2.h);

    pongCtx.beginPath();
    pongCtx.arc(ball.x, ball.y, ball.radius, 0, Math.PI*2);
    pongCtx.fillStyle = '#fff';
    pongCtx.shadowColor = '#fff';
    pongCtx.fill();

    pongCtx.shadowBlur = 0;

    pongAnim = requestAnimationFrame(updatePong);
}

function resetBall() {
    ball.x = 500;
    ball.y = 280;
    ball.vx = (Math.random() > 0.5 ? 5 : -5);
    ball.vy = (Math.random() * 6) - 3;
}
</script>
</body>
</html>
