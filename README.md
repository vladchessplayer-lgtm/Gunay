<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Подарок для Гюнай</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
            touch-action: manipulation;
        }
        body {
            background-color: black;
            color: white;
            font-family: 'Arial', sans-serif;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            text-align: center;
            position: relative;
            overflow-x: hidden;
            padding: 10px;
        }
        /* Звёзды - уменьшено количество и упрощена анимация */
        .star {
            position: absolute;
            background-color: white;
            border-radius: 50%;
            opacity: 0.8;
            width: 2px;
            height: 2px;
            animation: twinkle 4s infinite alternate;
        }
        @keyframes twinkle {
            0% { opacity: 0.4; transform: scale(1);}
            100% { opacity: 1; transform: scale(1.5);}
        }
        .sun-moon {
            position: absolute;
            bottom: 20px;
            left: 20px;
            font-size: 40px;
            opacity: 0.6;
            animation: float 6s infinite ease-in-out;
            pointer-events: none;
        }
        .moon-star {
            position: absolute;
            top: 20px;
            right: 20px;
            font-size: 40px;
            opacity: 0.6;
            animation: float 8s infinite reverse;
            pointer-events: none;
        }
        @keyframes float {
            0% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
            100% { transform: translateY(0px); }
        }
        .container {
            background: rgba(0,0,0,0.85);
            backdrop-filter: blur(8px);
            border-radius: 40px;
            border: 1px solid rgba(255,215,0,0.3);
            padding: 20px;
            width: 95%;
            max-width: 800px;
            z-index: 10;
            box-shadow: 0 0 40px rgba(0,0,0,0.8);
        }
        .container h1 {
            white-space: nowrap;
            font-size: 28px;
        }
        input, button {
            padding: 12px 20px;
            font-size: 18px;
            margin: 10px;
            border: none;
            border-radius: 40px;
            outline: none;
        }
        input {
            background-color: #222;
            color: white;
            width: 200px;
            text-align: center;
        }
        button {
            background-color: #ff9800;
            color: black;
            cursor: pointer;
            transition: 0.3s;
            font-weight: bold;
        }
        button:hover {
            background-color: #ffc107;
            transform: scale(1.02);
        }
        .hidden {
            display: none;
        }
        #hint {
            margin-top: 20px;
            color: #ffaa55;
            font-size: 14px;
        }
        canvas {
            background: #111;
            border-radius: 20px;
            display: block;
            margin: 10px auto;
            box-shadow: 0 0 20px rgba(255,255,255,0.2);
            width: 100%;
            height: auto;
            touch-action: none;
        }
        .game-info {
            display: flex;
            justify-content: space-between;
            margin-top: 10px;
            font-size: 16px;
            flex-wrap: wrap;
            gap: 10px;
        }
        .controls {
            display: flex;
            justify-content: center;
            gap: 30px;
            margin-top: 15px;
        }
        .ctrl-btn {
            background: #333;
            color: white;
            font-size: 40px;
            font-weight: bold;
            padding: 15px 30px;
            border-radius: 60px;
            cursor: pointer;
            transition: 0.2s;
            touch-action: manipulation;
            min-width: 100px;
        }
        .ctrl-btn:active {
            background: #ff9800;
            transform: scale(0.95);
        }
        .reset-game {
            background-color: #444;
            margin-top: 15px;
            padding: 12px 24px;
            font-size: 18px;
        }
        .current-word {
            font-size: 28px;
            margin-bottom: 10px;
            background: rgba(0,0,0,0.6);
            display: inline-block;
            padding: 10px 20px;
            border-radius: 60px;
            white-space: nowrap;
        }
        .final-screen {
            background: rgba(0,0,0,0.95);
            border-radius: 60px;
            padding: 40px;
            text-align: center;
            border: 2px solid #ffaa33;
            box-shadow: 0 0 50px rgba(255,170,51,0.5);
            margin: 20px;
        }
        .final-screen h2 {
            font-size: 36px;
            margin-bottom: 20px;
            color: #ffcc66;
        }
        .final-screen p {
            font-size: 28px;
            margin: 20px 0;
            font-weight: bold;
        }
        .final-screen .hearts {
            font-size: 48px;
            letter-spacing: 10px;
        }
        .final-screen.lose {
            border-color: #ff6666;
            box-shadow: 0 0 50px rgba(255,102,102,0.5);
        }
        .final-screen.lose h2 {
            color: #ff8888;
        }
        @media (max-width: 600px) {
            .container h1 {
                font-size: 20px;
                white-space: nowrap;
            }
            .ctrl-btn { font-size: 36px; padding: 12px 20px; min-width: 80px; }
            .current-word { font-size: 20px; white-space: nowrap; }
            .game-info { font-size: 14px; }
            .container { padding: 15px; }
            .final-screen h2 { font-size: 28px; }
            .final-screen p { font-size: 22px; }
        }
    </style>
</head>
<body>
<div id="starsContainer"></div>
<div class="sun-moon">🌞🌙</div>
<div class="moon-star">🌙✨</div>

<div id="passwordPage" class="container">
    <h1>🌞 Введи пароль 🌙</h1>
    <input type="text" id="passwordInput" placeholder="Пароль" autocomplete="off">
    <button onclick="checkPassword()">Войти</button>
    <div id="hint"></div>
    <footer>Подсказка через 10 секунд</footer>
</div>

<div id="gamePage" class="container hidden">
    <h1>✨ Собери слово ✨</h1>
    <div id="gameplayArea">
        <div class="current-word" id="currentWordDisplay">Ты ...</div>
        <canvas id="gameCanvas" style="width:100%; height:auto; background:#111; border-radius:20px;"></canvas>
        <div class="game-info">
            <span>🔤 Собрано: <span id="collectedCount">0</span> / <span id="totalCount">0</span></span>
            <span>📖 Слово <span id="wordIndex">1</span> / <span id="totalWords">5</span></span>
        </div>
        <div class="controls">
            <div class="ctrl-btn" id="leftBtn">←</div>
            <div class="ctrl-btn" id="rightBtn">→</div>
        </div>
        <button id="resetGameBtn" class="reset-game">⟳ Заново</button>
        <div id="messageArea" class="win-message"></div>
    </div>
    <div id="finalScreen" style="display: none;"></div>
</div>

<script>
    // ЗВЁЗДЫ - уменьшено количество до 80, для производительности
    function createStars() {
        const container = document.getElementById('starsContainer');
        for (let i = 0; i < 80; i++) {
            let star = document.createElement('div');
            star.classList.add('star');
            let size = Math.random() * 3 + 1;
            star.style.width = size + 'px';
            star.style.height = size + 'px';
            star.style.left = Math.random() * 100 + '%';
            star.style.top = Math.random() * 100 + '%';
            star.style.animationDelay = Math.random() * 5 + 's';
            container.appendChild(star);
        }
    }
    createStars();

    // ПАРОЛЬ
    let hintTimer;
    const correctPassword = "Гюнай";

    function startHintTimer() {
        hintTimer = setTimeout(() => {
            document.getElementById('hint').innerHTML = "🌙 Подсказка: Солнце и Луна вместе — это... 🌞";
        }, 10000);
    }

    function checkPassword() {
        const input = document.getElementById('passwordInput').value.trim();
        if (input === correctPassword) {
            clearTimeout(hintTimer);
            document.getElementById('passwordPage').classList.add('hidden');
            document.getElementById('gamePage').classList.remove('hidden');
            startGame();
        } else {
            alert("Неверный пароль, попробуй ещё раз");
        }
    }
    startHintTimer();

    // ИГРА
    const adjectives = ["Добрая", "Щедрая", "Честная", "Красивая", "Заботливая"];
    let currentWordIndex = 0;
    let currentWord = adjectives[currentWordIndex];
    let currentLetters = currentWord.split('');
    let currentTargetIndex = 0;
    let collectedLetters = [];
    let gameRunning = true;
    let animationId = null;
    let fallingLetters = [];
    let frame = 0;
    const SPAWN_DELAY = 40; // чуть меньше для динамики

    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');

    let canvasWidth = 0, canvasHeight = 0;
    let heroX = 0, heroWidth = 0, heroHeight = 0, heroY = 0, heroSpeed = 0;
    let ballRadius = 0, letterFontSize = 0, infoFontSize = 0;
    let leftPressed = false, rightPressed = false;

    const collectedSpan = document.getElementById('collectedCount');
    const totalSpan = document.getElementById('totalCount');
    const wordIndexSpan = document.getElementById('wordIndex');
    const totalWordsSpan = document.getElementById('totalWords');
    const currentWordDisplay = document.getElementById('currentWordDisplay');
    const messageArea = document.getElementById('messageArea');
    totalWordsSpan.innerText = adjectives.length;

    function updateUI() {
        collectedSpan.innerText = collectedLetters.length;
        totalSpan.innerText = currentLetters.length;
        wordIndexSpan.innerText = currentWordIndex + 1;
        currentWordDisplay.innerHTML = `Ты ... ${currentWord}`;
    }

    function resizeCanvas() {
        const maxWidth = Math.min(window.innerWidth - 40, 800);
        canvas.width = maxWidth;
        canvas.height = maxWidth * 0.6;
        canvasWidth = canvas.width;
        canvasHeight = canvas.height;

        heroWidth = canvasWidth * 0.08;
        heroHeight = canvasHeight * 0.1;
        heroY = canvasHeight - heroHeight - 10;
        heroSpeed = canvasWidth * 0.015;
        if (heroX === 0) heroX = canvasWidth / 2 - heroWidth/2;
        if (heroX < 10) heroX = 10;
        if (heroX + heroWidth > canvasWidth - 10) heroX = canvasWidth - heroWidth - 10;

        ballRadius = canvasWidth * 0.045;
        letterFontSize = ballRadius * 0.9;
        infoFontSize = canvasWidth * 0.04;
    }

    function showLoseScreen() {
        document.getElementById('gameplayArea').style.display = 'none';
        const finalDiv = document.getElementById('finalScreen');
        finalDiv.style.display = 'block';
        finalDiv.innerHTML = `
            <div class="final-screen lose">
                <h2>😔 Ошибка 😔</h2>
                <p>Ты поймала не ту букву...</p>
                <p>Попробуй ещё раз!</p>
                <button id="restartFromLoseBtn" class="reset-game">⟳ Пройти заново</button>
            </div>
        `;
        document.getElementById('restartFromLoseBtn').addEventListener('click', () => {
            finalDiv.style.display = 'none';
            document.getElementById('gameplayArea').style.display = 'block';
            resetGame();
        });
    }

    function showWinScreen() {
        document.getElementById('gameplayArea').style.display = 'none';
        const finalDiv = document.getElementById('finalScreen');
        finalDiv.style.display = 'block';
        finalDiv.innerHTML = `
            <div class="final-screen">
                <h2>✨ Поздравляю! ✨</h2>
                <p>Ты лучше всех, Гюнай!</p>
                <div class="hearts">❤️ 🌞 🌙 ❤️</div>
                <button id="restartFromWinBtn" class="reset-game">⟳ Пройти заново</button>
            </div>
        `;
        document.getElementById('restartFromWinBtn').addEventListener('click', () => {
            finalDiv.style.display = 'none';
            document.getElementById('gameplayArea').style.display = 'block';
            resetGame();
        });
    }

    function resetGame() {
        gameRunning = true;
        currentWordIndex = 0;
        currentWord = adjectives[currentWordIndex];
        currentLetters = currentWord.split('');
        currentTargetIndex = 0;
        collectedLetters = [];
        fallingLetters = [];
        frame = 0;
        heroX = canvasWidth / 2 - heroWidth/2;
        leftPressed = false;
        rightPressed = false;
        updateUI();
        messageArea.innerHTML = '';
        if (animationId) cancelAnimationFrame(animationId);
        animationId = requestAnimationFrame(gameLoop);
    }

    function nextWord() {
        currentWordIndex++;
        if (currentWordIndex < adjectives.length) {
            currentWord = adjectives[currentWordIndex];
            currentLetters = currentWord.split('');
            currentTargetIndex = 0;
            collectedLetters = [];
            fallingLetters = [];
            updateUI();
        } else {
            gameRunning = false;
            cancelAnimationFrame(animationId);
            showWinScreen();
        }
    }

    function spawnLetter() {
        if (!gameRunning) return;
        if (currentTargetIndex >= currentLetters.length) return;
        let remaining = currentLetters.slice(currentTargetIndex);
        if (remaining.length === 0) return;
        let randomLetter = remaining[Math.floor(Math.random() * remaining.length)];
        let x = Math.random() * (canvasWidth - 2 * ballRadius) + ballRadius;
        fallingLetters.push({ letter: randomLetter, x: x, y: 0, radius: ballRadius });
    }

    function gameLoop() {
        if (!gameRunning) return;
        frame++;
        if (frame % SPAWN_DELAY === 0) spawnLetter();

        if (leftPressed && heroX > 10) heroX -= heroSpeed;
        if (rightPressed && heroX < canvasWidth - heroWidth - 10) heroX += heroSpeed;

        for (let i = 0; i < fallingLetters.length; i++) {
            let l = fallingLetters[i];
            l.y += canvasHeight * 0.008;
            if (l.y + l.radius >= heroY && l.y - l.radius <= heroY + heroHeight &&
                l.x + l.radius >= heroX && l.x - l.radius <= heroX + heroWidth) {
                if (l.letter === currentLetters[currentTargetIndex]) {
                    collectedLetters.push(l.letter);
                    currentTargetIndex++;
                    updateUI();
                    fallingLetters.splice(i,1);
                    i--;
                    if (currentTargetIndex === currentLetters.length) {
                        if (currentWordIndex + 1 < adjectives.length) nextWord();
                        else {
                            gameRunning = false;
                            cancelAnimationFrame(animationId);
                            showWinScreen();
                            return;
                        }
                    }
                } else {
                    gameRunning = false;
                    cancelAnimationFrame(animationId);
                    showLoseScreen();
                    return;
                }
            } else if (l.y + l.radius > canvasHeight) {
                fallingLetters.splice(i,1);
                i--;
            }
        }
        draw();
        animationId = requestAnimationFrame(gameLoop);
    }

    function draw() {
        if (!ctx) return;
        ctx.clearRect(0, 0, canvasWidth, canvasHeight);
        ctx.fillStyle = '#111';
        ctx.fillRect(0, 0, canvasWidth, canvasHeight);
        ctx.fillStyle = '#ffaa33';
        ctx.fillRect(heroX, heroY, heroWidth, heroHeight);
        ctx.fillStyle = 'black';
        ctx.font = `${heroHeight * 0.6}px Arial`;
        ctx.fillText('😊', heroX + heroWidth*0.25, heroY + heroHeight*0.7);
        for (let l of fallingLetters) {
            ctx.fillStyle = '#ffcc66';
            ctx.beginPath();
            ctx.arc(l.x, l.y, l.radius, 0, 2*Math.PI);
            ctx.fill();
            ctx.fillStyle = 'black';
            ctx.font = `bold ${letterFontSize}px monospace`;
            ctx.fillText(l.letter, l.x - l.radius*0.45, l.y + l.radius*0.35);
        }
        ctx.fillStyle = 'white';
        ctx.font = `${infoFontSize}px monospace`;
        ctx.fillText('Собрано: ' + collectedLetters.join(''), 10, infoFontSize + 5);
        ctx.fillStyle = '#aaa';
        ctx.fillText('Нужно: ' + currentWord, 10, infoFontSize*2 + 5);
        ctx.fillStyle = '#888';
        ctx.font = `${infoFontSize*0.8}px Arial`;
        ctx.fillText('← →  или кнопки', canvasWidth - infoFontSize*8, canvasHeight - 10);
    }

    window.addEventListener('resize', () => { if (gameRunning) { resizeCanvas(); heroX = Math.min(Math.max(heroX, 10), canvasWidth - heroWidth - 10); } });
    document.addEventListener('keydown', (e) => { if (e.key === 'ArrowLeft') leftPressed = true; if (e.key === 'ArrowRight') rightPressed = true; });
    document.addEventListener('keyup', (e) => { if (e.key === 'ArrowLeft') leftPressed = false; if (e.key === 'ArrowRight') rightPressed = false; });
    const leftBtn = document.getElementById('leftBtn'), rightBtn = document.getElementById('rightBtn');
    leftBtn.addEventListener('touchstart', (e) => { e.preventDefault(); leftPressed = true; });
    leftBtn.addEventListener('touchend', (e) => { e.preventDefault(); leftPressed = false; });
    rightBtn.addEventListener('touchstart', (e) => { e.preventDefault(); rightPressed = true; });
    rightBtn.addEventListener('touchend', (e) => { e.preventDefault(); rightPressed = false; });
    leftBtn.addEventListener('mousedown', () => leftPressed = true);
    leftBtn.addEventListener('mouseup', () => leftPressed = false);
    rightBtn.addEventListener('mousedown', () => rightPressed = true);
    rightBtn.addEventListener('mouseup', () => rightPressed = false);
    document.getElementById('resetGameBtn').addEventListener('click', () => {
        if (animationId) cancelAnimationFrame(animationId);
        document.getElementById('finalScreen').style.display = 'none';
        document.getElementById('gameplayArea').style.display = 'block';
        resetGame();
    });
    function startGame() { resizeCanvas(); resetGame(); }
</script>
</body>
</html>
