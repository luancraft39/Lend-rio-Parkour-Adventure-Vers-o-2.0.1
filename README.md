<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>Lendário Parkour Adventure</title>

<style>
body{
margin:0;
overflow:hidden;
font-family:Arial;
}

canvas{
display:block;
background:#87CEEB;
}

#loading, #updateInfo, #menu, #death{
position:absolute;
width:100%;
height:100%;
color:white;
display:flex;
align-items:center;
justify-content:center;
flex-direction:column;
text-align:center;
}

#loading{
background:black;
font-size:40px;
z-index:3;
}

#updateInfo{
background:#111;
font-size:25px;
z-index:3;
display:none;
}

#menu{
background:#222;
display:none;
z-index:2;
}

#death{
background:rgba(0,0,0,0.9);
display:none;
z-index:4;
font-size:40px;
}

button{
font-size:20px;
padding:10px 20px;
margin:10px;
cursor:pointer;
}

#info{
position:absolute;
top:10px;
left:10px;
color:black;
font-weight:bold;
}

/* CONTROLES MOBILE */
#mobileControls{
position:absolute;
bottom:20px;
width:100%;
display:none;
justify-content:center;
gap:20px;
z-index:5;
}
#mobileControls button{
font-size:30px;
padding:15px 25px;
border:none;
border-radius:10px;
background:#333;
color:white;
}
</style>
</head>

<body>

<audio id="music" autoplay loop>
<source src="https://media.vocaroo.com/mp3/1iOle3gRKpTl" type="audio/mpeg">
</audio>

<div id="loading">
LENDÁRIO PARKOUR ADVENTURE
<br>
<small>Carregando...</small>
</div>

<!-- Tela de novidades da atualização -->
<div id="updateInfo">
<h1>Atualização 2.0.1</h1>
<p><strong>Desempenho:</strong> Movimentos mais fluidos, menos travamentos e otimização geral.</p>
<p><strong>Compatibilidade:</strong> Agora funciona em mais dispositivos e navegadores, com controles mobile melhorados.</p>
<button onclick="showMenu()">Continuar</button>
</div>

<div id="menu">
<h1>Lendário Parkour Adventure</h1>
<p>
COMO JOGAR
<br><br>
PC: A / D = andar<br>
ESPAÇO = pular<br><br>
CELULAR: Use os botões na tela
</p>
<button onclick="startGame('boy')">Menino</button>
<button onclick="startGame('girl')">Menina</button>
</div>

<div id="death">
VOCÊ MORREU
<button onclick="continueGame()">Continuar</button>
<button onclick="goMenu()">Tela inicial</button>
</div>

<div id="info">
PC: A/D = andar | ESPAÇO = pular
</div>

<canvas id="game"></canvas>

<!-- CONTROLES MOBILE -->
<div id="mobileControls">
<button id="left">A</button>
<button id="jump">PULAR</button>
<button id="right">D</button>
</div>

<script>
const canvas = document.getElementById("game")
const ctx = canvas.getContext("2d")
canvas.width = window.innerWidth
canvas.height = window.innerHeight

const music = document.getElementById("music")
document.addEventListener("click",()=>{music.play()})

let gravity = 0.6
let gameRunning=false
let gameStarted=false

let player={x:100,y:300,w:40,h:60,vx:0,vy:0,speed:5,jump:14,onGround:false,type:"boy"}
let checkpoint={x:100,y:300}
let keys={}

document.addEventListener("keydown",e=>keys[e.key.toLowerCase()]=true)
document.addEventListener("keyup",e=>keys[e.key.toLowerCase()]=false)

function startGame(type){
player.type=type
document.getElementById("menu").style.display="none"
gameRunning=true
if(!gameStarted){gameStarted=true;gameLoop()}
}

function continueGame(){
document.getElementById("death").style.display="none"
player.x=checkpoint.x;player.y=checkpoint.y;player.vx=0;player.vy=0
gameRunning=true
}

function goMenu(){
document.getElementById("death").style.display="none"
document.getElementById("menu").style.display="flex"
gameRunning=false
}

function showMenu(){
document.getElementById("updateInfo").style.display="none"
document.getElementById("menu").style.display="flex"
}

setTimeout(()=>{
document.getElementById("loading").style.display="none"
document.getElementById("updateInfo").style.display="flex"
},2000)

let platforms=[]
platforms.push({x:-500,y:550,w:2000,h:50,type:"grass"})
let lastPlatformX=300

function generatePlatform(){
let gap = 150 + Math.random()*200
let height = 200 + Math.random()*250
let type="dirt"
if(Math.random()<0.15){type="checkpoint"}
platforms.push({x:lastPlatformX,y:height,w:150,h:20,type:type})
lastPlatformX+=gap
}
for(let i=0;i<20;i++){generatePlatform()}

let cameraX=0

function update(){
if(!gameRunning)return
if(keys["a"]) player.vx=-player.speed
else if(keys["d"]) player.vx=player.speed
else player.vx=0
if(keys[" "] && player.onGround){player.vy=-player.jump;player.onGround=false}
player.vy+=gravity
player.x+=player.vx;player.y+=player.vy
player.onGround=false
for(let p of platforms){
if(player.x < p.x+p.w && player.x+player.w > p.x && player.y < p.y+p.h && player.y+player.h > p.y){
if(player.vy>0){
player.y=p.y-player.h;player.vy=0;player.onGround=true
if(p.type=="checkpoint"){checkpoint.x=p.x;checkpoint.y=p.y-60}
}}}
if(player.x + canvas.width > lastPlatformX - 500){generatePlatform()}
if(player.y>canvas.height+200){gameRunning=false;document.getElementById("death").style.display="flex"}
cameraX = player.x - canvas.width/3
}

function drawPlayer(){
let px=player.x;let py=player.y
ctx.fillStyle = player.type=="boy" ? "blue" : "pink"
ctx.fillRect(px,py+20,40,40)
ctx.fillStyle="#ffe0bd";ctx.fillRect(px+5,py,30,20)
ctx.fillStyle="black";ctx.fillRect(px+10,py+5,5,5);ctx.fillRect(px+25,py+5,5,5)
ctx.fillStyle="brown"
if(player.type=="boy"){ctx.fillRect(px+5,py-5,30,10)}
else{ctx.fillRect(px,py-5,40,15);ctx.fillRect(px-5,py+5,10,25);ctx.fillRect(px+35,py+5,10,25)}
ctx.fillStyle="#ffe0bd";ctx.fillRect(px-5,py+25,10,10);ctx.fillRect(px+35,py+25,10,10)
}

function draw(){
ctx.clearRect(0,0,canvas.width,canvas.height)
ctx.save();ctx.translate(-cameraX,0)
for(let p of platforms){
if(p.type=="grass") ctx.fillStyle="#3cb043"
else if(p.type=="dirt") ctx.fillStyle="#8B4513"
else if(p.type=="checkpoint") ctx.fillStyle="gold"
ctx.fillRect(p.x,p.y,p.w,p.h)
}
drawPlayer()
ctx.restore()
}

function gameLoop(){update();draw();requestAnimationFrame(gameLoop)}

/* DETECTAR CELULAR */
let isMobile = /Android|iPhone|iPad|iPod/i.test(navigator.userAgent)
if(isMobile){document.getElementById("mobileControls").style.display="flex"}

/* CONTROLES MOBILE */
const leftBtn = document.getElementById("left")
const rightBtn = document.getElementById("right")
const jumpBtn = document.getElementById("jump")
leftBtn.ontouchstart = ()=> keys["a"]=true
leftBtn.ontouchend = ()=> keys["a"]=false
rightBtn.ontouchstart = ()=> keys["d"]=true
rightBtn.ontouchend = ()=> keys["d"]=false
jumpBtn.ontouchstart = ()=> keys[" "]=true
jumpBtn.ontouchend = ()=> keys[" "]=false
</script>

</body>
</html>
