<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>Lendário Parkour - MULTIPLAYER MUNDIAL</title>

<script src="https://cdn.socket.io/4.7.2/socket.io.min.js"></script>

<style>
body { margin: 0; overflow: hidden; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
canvas { display: block; background: linear-gradient(to bottom, #87CEEB, #E0F6FF); }

#loading, #menu, #death {
    position: absolute; width: 100%; height: 100%; display: flex;
    align-items: center; justify-content: center; flex-direction: column; z-index: 10;
}

#loading { background: black; color: white; font-size: 40px; text-align: center;}
#menu { background: #222; color: white; display: none; text-align: center; }
#death { background: rgba(0,0,0,0.9); color: white; display: none; font-size: 40px; text-align: center;}

button {
    font-size: 18px; padding: 12px 24px; margin: 10px; cursor: pointer;
    background: #00f0ff; border: none; border-radius: 8px; font-weight: bold; transition: 0.2s;
}

button:hover { background: #ff007f; color: white; transform: scale(1.05);}

#info { position: absolute; top: 10px; left: 10px; color: black; font-weight: bold; background: rgba(255,255,255,0.7); padding: 5px 10px; border-radius: 5px; z-index: 5;}

#chat-container {
    position: absolute; bottom: 20px; left: 20px; width: 320px; height: 250px;
    background: rgba(0, 0, 0, 0.6); backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2); border-radius: 12px;
    display: flex; flex-direction: column; z-index: 20; overflow: hidden;
}

#chat-messages { flex: 1; padding: 10px; overflow-y: auto; color: white; font-size: 14px; display: flex; flex-direction: column; gap: 5px; }
.message { background: rgba(255, 255, 255, 0.1); padding: 6px 10px; border-radius: 8px; word-wrap: break-word;}
.message .sender { font-weight: bold; color: #00f0ff; }

#chat-input-area { display: flex; padding: 10px; background: rgba(0, 0, 0, 0.3); gap: 5px; }
#chat-input { flex: 1; background: rgba(255, 255, 255, 0.2); border: 1px solid rgba(255, 255, 255, 0.3); border-radius: 6px; padding: 8px; color: white; outline: none; }
#chat-send { background: #ff007f; color: white; padding: 8px 15px; margin: 0; font-size: 14px; border-radius: 6px; }

#mobileControls { position: absolute; bottom: 20px; right: 20px; display: none; gap: 15px; z-index: 5; }
#mobileControls button { font-size: 24px; padding: 20px; background: rgba(0, 0, 0, 0.7); color: white; border: 1px solid #00f0ff; border-radius: 50%; width: 70px; height: 70px; display: flex; align-items: center; justify-content: center;}
</style>
</head>

<body>

<audio id="music" loop>
    <source src="https://media.vocaroo.com/mp3/1iOle3gRKpTl" type="audio/mpeg">
</audio>

<div id="loading">
    LENDÁRIO PARKOUR ADVENTURE
    <br><small style="font-size: 20px; color: #aaa;">Conectando ao mundo online...</small>
</div>

<div id="menu">
    <h1>Lendário Parkour Adventure</h1>
    <p>Escolha seu personagem para entrar no mundo Aberto!</p>
    <div>
        <button onclick="startGame('boy')">Menino (Azul)</button>
        <button onclick="startGame('girl')">Menina (Rosa)</button>
    </div>
</div>

<div id="death">
    ☠️ VOCÊ CAIU!
    <div>
        <button onclick="continueGame()">Renascer</button>
        <button onclick="goMenu()">Menu Principal</button>
    </div>
</div>

<div id="info">PC: A/D = andar | ESPAÇO = pular</div>

<canvas id="game"></canvas>

<div id="chat-container">
    <div id="chat-messages"></div>
    <div id="chat-input-area">
        <input type="text" id="chat-input" placeholder="Diga olá para o mundo..." autocomplete="off">
        <button id="chat-send" onclick="sendChatMessage()">Enviar</button>
    </div>
</div>

<div id="mobileControls">
    <button id="left">⬅️</button>
    <button id="jump">🚀</button>
    <button id="right">➡️</button>
</div>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

const music = document.getElementById("music");
document.addEventListener("click", () => { music.play().catch(()=>{}); });

// --- 🌐 MULTIPLAYER (LENDÁRIO SERVER) ---
// SEU LINK DO RENDER JÁ ESTÁ APLICADO AQUI ABAIXO:
const socket = io('https://lend-rio-parkour-adventure-vers-o-2-0-1.onrender.com'); 

let otherPlayers = {}; 

let gravity = 0.6;
let gameRunning = false;
let gameStarted = false;

let player = {
    x: 100, y: 300, w: 40, h: 60,
    vx: 0, vy: 0, speed: 5, jump: 14,
    onGround: false, type: "boy"
};

let checkpoint = { x: 100, y: 300 };
let keys = {};

document.addEventListener("keydown", e => {
    if (document.activeElement === document.getElementById("chat-input")) return;
    keys[e.key.toLowerCase()] = true;
});

document.addEventListener("keyup", e => {
    keys[e.key.toLowerCase()] = false;
});

// --- RECEBER DADOS DOS OUTROS PLAYERS DO SERVIDOR ---
socket.on('currentPlayers', (serverPlayers) => {
    Object.keys(serverPlayers).forEach((id) => {
        if (id !== socket.id) {
            otherPlayers[id] = serverPlayers[id];
        }
    });
    document.getElementById("loading").style.display = "none";
    document.getElementById("menu").style.display = "flex";
});

socket.on('newPlayer', (playerData) => {
    otherPlayers[playerData.id] = playerData;
    addSystemMessage("Alguém entrou no jogo!");
});

socket.on('playerMoved', (playerData) => {
    otherPlayers[playerData.id] = playerData;
});

socket.on('playerDisconnected', (id) => {
    delete otherPlayers[id];
    addSystemMessage("Alguém saiu do jogo.");
});

socket.on('newMessage', (msg) => {
    addMessage(msg.sender, msg.text, msg.senderId === socket.id);
});


// --- SISTEMA DO JOGO ---
function startGame(type) {
    player.type = type;
    document.getElementById("menu").style.display = "none";
    gameRunning = true;

    socket.emit('playerMovement', { x: player.x, y: player.y, type: player.type });

    if (!gameStarted) {
        gameStarted = true;
        gameLoop();
    }
}

function continueGame() {
    document.getElementById("death").style.display = "none";
    player.x = checkpoint.x;
    player.y = checkpoint.y;
    player.vx = 0; player.vy = 0;
    gameRunning = true;
}

function goMenu() {
    document.getElementById("death").style.display = "none";
    document.getElementById("menu").style.display = "flex";
    gameRunning = false;
}

let platforms = [];
platforms.push({ x: -500, y: 550, w: 2000, h: 50, type: "grass" });
let lastPlatformX = 300;

function generatePlatform() {
    let gap = 150 + Math.random() * 200;
    let height = 200 + Math.random() * 250;
    let type = "dirt";

    if (Math.random() < 0.15) type = "checkpoint";

    platforms.push({ x: lastPlatformX, y: height, w: 150, h: 20, type: type });
    lastPlatformX += gap;
}

for (let i = 0; i < 20; i++) generatePlatform();

let cameraX = 0;

function update() {
    if (!gameRunning) return;

    let moved = false;

    if (keys["a"]) { player.vx = -player.speed; moved = true; }
    else if (keys["d"]) { player.vx = player.speed; moved = true; }
    else { player.vx = 0; }

    if (keys[" "] && player.onGround) {
        player.vy = -player.jump;
        player.onGround = false;
        moved = true;
    }

    player.vy += gravity;
    player.x += player.vx;
    player.y += player.vy;
    player.onGround = false;

    for (let p of platforms) {
        if (player.x < p.x + p.w && player.x + player.w > p.x && player.y < p.y + p.h && player.y + player.h > p.y) {
            if (player.vy > 0) {
                player.y = p.y - player.height; player.vy = 0; player.onGround = true;
                if (p.type == "checkpoint") { checkpoint.x = p.x; checkpoint.y = p.y - 60; }
            }
        }
    }

    if (player.x + canvas.width > lastPlatformX - 500) generatePlatform();

    if (player.y > canvas.height + 200) {
        gameRunning = false;
        document.getElementById("death").style.display = "flex";
    }

    cameraX = player.x - canvas.width / 3;

    if (moved || player.vx !== 0 || player.vy !== 0) {
        socket.emit('playerMovement', { x: player.x, y: player.y, type: player.type });
    }
}

function drawCharacter(pObj, label, isMe) {
    let px = pObj.x;
    let py = pObj.y;

    ctx.fillStyle = pObj.type == "boy" ? "#0066ff" : "#ff3399";
    ctx.fillRect(px, py + 20, 40, 40);
    ctx.fillStyle = "#ffe0bd";
    ctx.fillRect(px + 5, py, 30, 20);
    ctx.fillStyle = "black";
    ctx.fillRect(px + 10, py + 5, 5, 5); ctx.fillRect(px + 25, py + 5, 5, 5);
    ctx.fillStyle = "brown";
    
    if (pObj.type == "boy") ctx.fillRect(px + 5, py - 5, 30, 10);
    else { ctx.fillRect(px, py - 5, 40, 15); ctx.fillRect(px - 5, py + 5, 10, 25); ctx.fillRect(px + 35, py + 5, 10, 25); }

    ctx.fillStyle = "#ffe0bd"; ctx.fillRect(px - 5, py + 25, 10, 10); ctx.fillRect(px + 35, py + 25, 10, 10);

    ctx.fillStyle = isMe ? "#00f0ff" : "white";
    ctx.font = "bold 14px Arial";
    ctx.textAlign = "center";
    ctx.fillText(label, px + 20, py - 15);
}

function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.save();
    ctx.translate(-cameraX, 0);

    for (let p of platforms) {
        if (p.type == "grass") ctx.fillStyle = "#3cb043";
        else if (p.type == "dirt") ctx.fillStyle = "#8B4513";
        else if (p.type == "checkpoint") ctx.fillStyle = "gold";
        ctx.fillRect(p.x, p.y, p.w, p.h);
    }

    Object.keys(otherPlayers).forEach((id) => {
        drawCharacter(otherPlayers[id], "Player", false);
    });

    drawCharacter(player, "Você", true);

    ctx.restore();
}

function gameLoop() {
    update(); draw();
    requestAnimationFrame(gameLoop);
}

const chatInput = document.getElementById("chat-input");
const chatMessages = document.getElementById("chat-messages");

function sendChatMessage() {
    const text = chatInput.value.trim();
    if (text === "") return;

    socket.emit('sendMessage', { text: text });
    chatInput.value = "";
    chatInput.blur();
}

function addMessage(sender, text, isMe) {
    const msgDiv = document.createElement("div");
    msgDiv.className = "message";
    msgDiv.innerHTML = `<span class="sender" style="color:${isMe ? '#ff007f' : '#00f0ff'}">${isMe ? "Você" : "Mundo"}:</span> ${text}`;
    chatMessages.appendChild(msgDiv);
    chatMessages.scrollTop = chatMessages.scrollHeight;
}

function addSystemMessage(text) {
    const msgDiv = document.createElement("div");
    msgDiv.style.color = "#ffff00";
    msgDiv.innerText = `[Aviso] ${text}`;
    chatMessages.appendChild(msgDiv);
}

chatInput.addEventListener("keypress", (e) => { if (e.key === "Enter") sendChatMessage(); });

let isMobile = /Android|iPhone|iPad|iPod/i.test(navigator.userAgent);
if (isMobile) {
    document.getElementById("mobileControls").style.display = "flex";
}

const leftBtn = document.getElementById("left");
const rightBtn = document.getElementById("right");
const jumpBtn = document.getElementById("jump");

leftBtn.ontouchstart = (e) => { e.preventDefault(); keys["a"] = true; }
leftBtn.ontouchend = () => { keys["a"] = false; }

rightBtn.ontouchstart = (e) => { e.preventDefault(); keys["d"] = true; }
rightBtn.ontouchend = () => { keys["d"] = false; }

jumpBtn.ontouchstart = (e) => { e.preventDefault(); keys[" "] = true; }
jumpBtn.ontouchend = () => { keys[" "] = false; }
</script>

</body>
</html>
