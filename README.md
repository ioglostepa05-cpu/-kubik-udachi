# -kubik-udachi
<!doctype html>
<html lang="ru">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>Rabbit Runner</title>

<style>
* {
  box-sizing: border-box;
  -webkit-tap-highlight-color: transparent;
}

html, body {
  margin: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  background: #9edcff;
  font-family: Arial, sans-serif;
}

#game {
  position: relative;
  width: 100%;
  height: 100%;
  max-width: 900px;
  margin: auto;
  overflow: hidden;
  background: linear-gradient(
    #9edcff 0%,
    #eaf9ff 72%,
    #9bd56d 72%,
    #78b84e 100%
  );
}

.sun {
  position: absolute;
  right: 10%;
  top: 8%;
  width: 70px;
  height: 70px;
  background: #ffe66d;
  border-radius: 50%;
  box-shadow: 0 0 40px #fff3;
}

.score {
  position: absolute;
  right: 18px;
  top: 16px;
  font-weight: 900;
  font-size: 20px;
  color: #253044;
  z-index: 10;
}

.best {
  position: absolute;
  left: 18px;
  top: 18px;
  font-size: 14px;
  color: #34445a;
  z-index: 10;
}

.rabbit {
  position: absolute;
  width: 58px;
  height: 70px;
  left: 14%;
  bottom: 28%;
  z-index: 5;
}

.body {
  position: absolute;
  width: 43px;
  height: 45px;
  left: 7px;
  bottom: 0;
  background: #fff;
  border-radius: 50% 50% 42% 42%;
  box-shadow: inset -5px -5px #ddd;
}

.head {
  position: absolute;
  width: 43px;
  height: 40px;
  left: 15px;
  top: 5px;
  background: #fff;
  border-radius: 50%;
  box-shadow: inset -4px -4px #ddd;
}

.ear {
  position: absolute;
  width: 13px;
  height: 35px;
  top: -22px;
  background: #fff;
  border-radius: 50%;
  transform: rotate(-9deg);
  box-shadow: inset -3px 0 #f4b9c8;
}

.ear.two {
  left: 28px;
  transform: rotate(9deg);
}

.eye {
  position: absolute;
  width: 5px;
  height: 5px;
  background: #222;
  border-radius: 50%;
  right: 8px;
  top: 15px;
}

.nose {
  position: absolute;
  width: 6px;
  height: 5px;
  background: #f18b9d;
  border-radius: 50%;
  right: 1px;
  top: 22px;
}

.leg {
  position: absolute;
  width: 10px;
  height: 17px;
  background: #eee;
  border-radius: 50%;
  bottom: -5px;
}

.leg.a {
  left: 12px;
}

.leg.b {
  left: 31px;
}

.cloud {
  position: absolute;
  width: 100px;
  height: 28px;
  background: #fff;
  border-radius: 30px;
  top: 16%;
  left: 12%;
  opacity: .8;
}

.cloud:before,
.cloud:after {
  content: "";
  position: absolute;
  background: #fff;
  border-radius: 50%;
}

.cloud:before {
  width: 45px;
  height: 45px;
  left: 20px;
  bottom: 0;
}

.cloud:after {
  width: 35px;
  height: 35px;
  left: 52px;
  bottom: 0;
}

.ground {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 27.5%;
  height: 5px;
  background: #4d8737;
}

.obstacle {
  position: absolute;
  bottom: 28%;
  width: 30px;
  height: 55px;
  background: #4d9b42;
  border-radius: 15px 15px 4px 4px;
  z-index: 4;
}

.obstacle:before {
  content: "";
  position: absolute;
  width: 22px;
  height: 35px;
  left: -12px;
  bottom: 0;
  background: #4d9b42;
  border-radius: 15px 15px 0 0;
  transform: rotate(-25deg);
}

#msg {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 20;
  pointer-events: none;
}

.card {
  background: #ffffffdd;
  border-radius: 22px;
  padding: 22px 28px;
  text-align: center;
  box-shadow: 0 12px 40px #0003;
}

.card h1 {
  margin: 0 0 8px;
}

.card p {
  margin: 0 0 12px;
  color: #555;
}

button {
  border: 0;
  border-radius: 14px;
  padding: 12px 22px;
  background: #ffcf3f;
  font-weight: 900;
  font-size: 17px;
}

.hidden {
  display: none !important;
}

#hint {
  position: absolute;
  bottom: 8%;
  left: 0;
  right: 0;
  text-align: center;
  color: #35502e;
  font-weight: 700;
}
</style>
</head>

<body>

<div id="game">

  <div class="sun"></div>
  <div class="cloud"></div>

  <div class="best">
    РЕКОРД: <span id="best">0</span>
  </div>

  <div class="score">
    СЧЁТ: <span id="score">0</span>
  </div>

  <div class="ground"></div>

  <div id="rabbit" class="rabbit">

    <div class="ear"></div>
    <div class="ear two"></div>

    <div class="head">
      <div class="eye"></div>
      <div class="nose"></div>
    </div>

    <div class="body"></div>

    <div class="leg a"></div>
    <div class="leg b"></div>

  </div>

  <div id="msg">
    <div class="card">
      <h1>🐰 Rabbit Runner</h1>
      <p>Нажми, чтобы кролик прыгнул!</p>
      <button id="start">НАЧАТЬ</button>
    </div>
  </div>

  <div id="hint">
    Тапай по экрану, чтобы прыгать
  </div>

</div>

<script>

const game = document.getElementById("game");
const rabbit = document.getElementById("rabbit");
const msg = document.getElementById("msg");

const scoreEl = document.getElementById("score");
const bestEl = document.getElementById("best");
const start = document.getElementById("start");

let running = false;
let jumping = false;

let velocityY = 0;
let score = 0;
let speed = 5;

let obstacles = [];
let lastTime = 0;
let spawnTimer = 0;

let best = Number(localStorage.rabbitBest || 0);

bestEl.textContent = best;


function jump() {

  if (!running) return;

  if (!jumping) {

    jumping = true;
    velocityY = 15;

  }

}


function startGame() {

  obstacles.forEach(o => o.remove());
  obstacles = [];

  running = true;
  jumping = false;

  velocityY = 0;
  score = 0;
  speed = 5;
  spawnTimer = 0;

  lastTime = performance.now();

  scoreEl.textContent = "0";

  msg.classList.add("hidden");

  requestAnimationFrame(gameLoop);

}


function addObstacle() {

  const obstacle = document.createElement("div");

  obstacle.className = "obstacle";

  obstacle.style.left = "100%";

  game.appendChild(obstacle);

  obstacles.push(obstacle);

}


function gameOver() {

  running = false;

  const finalScore = Math.floor(score);

  if (finalScore > best) {

    best = finalScore;

    localStorage.rabbitBest = best;

    bestEl.textContent = best;

  }

  msg.querySelector("h1").textContent = "🐰 Игра окончена!";

  msg.querySelector("p").textContent =
    "Счёт: " + finalScore;

  start.textContent = "ИГРАТЬ СНОВА";

  msg.classList.remove("hidden");

}


function gameLoop(time) {

  if (!running) return;

  const delta =
    Math.min(32, time - lastTime);

  lastTime = time;


  /* Прыжок */

  const currentBottom =
    parseFloat(
      getComputedStyle(rabbit).bottom
    );


  if (jumping) {

    velocityY -=
      0.75 * (delta / 16.67);

    rabbit.style.bottom =
      (currentBottom +
       velocityY * (delta / 16.67)) + "px";


    if (
      currentBottom <=
      game.clientHeight * 0.275
    ) {

      rabbit.style.bottom = "27.5%";

      jumping = false;
      velocityY = 0;

    }

  }


  /* Создание препятствий */

  spawnTimer += delta;

  if (
    spawnTimer >
    900 - Math.min(350, score * 1.8)
  ) {

    addObstacle();

    spawnTimer = 0;

  }


  /* Движение препятствий */

  const rabbitRect =
    rabbit.getBoundingClientRect();


  obstacles.forEach((obstacle, index) => {

    const x =
      parseFloat(obstacle.style.left)
      || game.clientWidth;

    obstacle.style.left =
      (x -
       speed * (delta / 16.67))
      + "px";


    const rect =
      obstacle.getBoundingClientRect();


    if (rect.right < 0) {

      obstacle.remove();

      obstacles.splice(index, 1);

      return;

    }


    /* Проверка столкновения */

    if (

      rabbitRect.right - 10 > rect.left &&

      rabbitRect.left + 10 < rect.right &&

      rabbitRect.bottom - 8 > rect.top &&

      rabbitRect.top + 10 < rect.bottom

    ) {

      gameOver();

    }

  });


  /* Счёт */

  score += delta * 0.012;

  scoreEl.textContent =
    Math.floor(score);


  /* Постепенно ускоряем игру */

  speed += delta * 0.00003;


  requestAnimationFrame(gameLoop);

}


/* Управление */

game.addEventListener(
  "pointerdown",
  event => {

    if (event.target !== start) {
      jump();
    }

  }
);


start.addEventListener(
  "click",
  startGame
);


document.addEventListener(
  "keydown",
  event => {

    if (
      event.code === "Space" ||
      event.code === "ArrowUp"
    ) {

      jump();

    }

  }
);

</script>

</body>
</html>