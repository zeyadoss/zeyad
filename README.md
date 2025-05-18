<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Simple Shooter</title>
<style>
body { margin: 0; overflow: hidden; background-color: #222; display: flex; justify-content: center; align-items: center; min-height: 100vh; }
.game-container { position: relative; width: 400px; height: 300px; background-color: #333; }
.player { position: absolute; bottom: 10px; left: 50%; transform: translateX(-50%); width: 20px; height: 30px; background-color: lime; }
.enemy { position: absolute; top: 10px; background-color: red; width: 15px; height: 15px; border-radius: 50%; }
.bullet { position: absolute; background-color: yellow; width: 5px; height: 10px; }
.score { position: absolute; top: 5px; left: 5px; color: white; font-size: 16px; }
</style>
</head>
<body>
<div class="game-container">
<div class="player"></div>
<div class="score">Score: 0</div>
</div>
<script>
const gameContainer = document.querySelector('.game-container');
const player = document.querySelector('.player');
const scoreDisplay = document.querySelector('.score');
let score = 0;
let playerX = 0;
const playerSpeed = 5;
const enemies = [];
const bullets = [];
const enemyInterval = 1000;

function createEnemy() {
const enemy = document.createElement('div');
enemy.classList.add('enemy');
enemy.style.left = Math.random() * (gameContainer.offsetWidth - 15) + 'px';
gameContainer.appendChild(enemy);
enemies.push(enemy);
enemy.movementInterval = setInterval(() => {
enemy.style.top = (enemy.offsetTop + 2) + 'px';
if (enemy.offsetTop > gameContainer.offsetHeight) {
enemy.remove();
enemies.splice(enemies.indexOf(enemy), 1);
}
checkCollision(enemy);
}, 30);
}

function shoot() {
const bullet = document.createElement('div');
bullet.classList.add('bullet');
bullet.style.left = player.offsetLeft + player.offsetWidth / 2 - 2.5 + 'px';
bullet.style.top = player.offsetTop - 10 + 'px';
gameContainer.appendChild(bullet);
bullets.push(bullet);
bullet.movementInterval = setInterval(() => {
bullet.style.top = (bullet.offsetTop - 5) + 'px';
if (bullet.offsetTop < 0) {
bullet.remove();
bullets.splice(bullets.indexOf(bullet), 1);
}
checkBulletCollision(bullet);
}, 20);
}

function checkCollision(enemy) {
const playerRect = player.getBoundingClientRect();
const enemyRect = enemy.getBoundingClientRect();
if (playerRect.left < enemyRect.right && playerRect.right > enemyRect.left && playerRect.top < enemyRect.bottom && playerRect.bottom > enemyRect.top) {
gameOver();
}
}

function checkBulletCollision(bullet) {
const bulletRect = bullet.getBoundingClientRect();
enemies.forEach(enemy => {
const enemyRect = enemy.getBoundingClientRect();
if (bulletRect.left < enemyRect.right && bulletRect.right > enemyRect.left && bulletRect.top < enemyRect.bottom && bulletRect.bottom > enemyRect.top) {
score++;
scoreDisplay.textContent = `Score: ${score}`;
enemy.remove();
enemies.splice(enemies.indexOf(enemy), 1);
bullet.remove();
bullets.splice(bullets.indexOf(bullet), 1);
}
});
}

function gameOver() {
alert(`Game Over! Your score: ${score}`);
score = 0;
scoreDisplay.textContent = 'Score: 0';
enemies.forEach(enemy => { clearInterval(enemy.movementInterval); enemy.remove(); });
enemies.length = 0;
bullets.forEach(bullet => { clearInterval(bullet.movementInterval); bullet.remove(); });
bullets.length = 0;
clearInterval(enemySpawnInterval);
enemySpawnInterval = setInterval(createEnemy, enemyInterval);
}

document.addEventListener('keydown', (e) => {
if (e.key === 'ArrowLeft') playerX -= playerSpeed;
if (e.key === 'ArrowRight') playerX += playerSpeed;
player.style.left = `calc(50% + ${playerX}px)`;
});

document.addEventListener('keyup', (e) => {
if (e.key === ' ') shoot();
});

let enemySpawnInterval = setInterval(createEnemy, enemyInterval);
</script>
</body>
</html
