<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Flappy Bird 2 - Gamers Station</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(to bottom, #ffecd2, #fcb69f);
      margin: 0;
      padding: 0;
      text-align: center;
    }

    header {
      background-color: #222;
      color: white;
      padding: 20px 0;
      font-size: 2em;
    }

    section {
      padding: 20px;
    }

    iframe {
      width: 90%;
      height: 500px;
      border: 2px solid #000;
      border-radius: 10px;
    }

    .download-btn {
      display: inline-block;
      margin-top: 20px;
      padding: 12px 24px;
      font-size: 16px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 8px;
      text-decoration: none;
    }

    .download-btn:hover {
      background-color: #0056b3;
    }

    footer {
      background-color: #222;
      color: white;
      padding: 10px;
      margin-top: 40px;
    }
  </style>
</head>
<body>

  <header>
    Flappy Bird 2 - Gamers Station
  </header>

  <section>
    <h2>Fly Again!</h2>
    <p>Enjoy a second take on the classic Flappy Bird experience.</p>
    <iframe src="adityasingh.html" title="Flappy Bird Game"></iframe>

    <br />
    <a href="adityasingh.html" download class="download-btn">Download Game</a>
  </section>

  <footer>
    &copy; 2025 Gamers Station. Built for fun.
  </footer>                                                                                                                                                                                                                                                                 
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Flappy Bird Easy</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <style>
    body {
      margin: 0;
      overflow: hidden;
      display: flex;
      flex-direction: column;
      align-items: center;
      background-color: #70c5ce;
      touch-action: manipulation;
    }
    canvas {
      border: 2px solid #000;
      background-color: #fff;
    }
    #controls {
      margin: 10px;
    }
    button {
      padding: 10px 20px;
      font-size: 16px;
      margin: 5px;
      border: none;
      background: #4CAF50;
      color: white;
      border-radius: 5px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div id="controls">
    <button id="startBtn">Start Game</button>
    <button id="restartBtn" style="display: none;">Restart</button>
  </div>
  <canvas id="gameCanvas" width="400" height="600"></canvas>

  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");

    const birdImg = new Image();
    birdImg.src = "https://iili.io/3YwWhP4.png";

    const pipeUpImg = new Image();
    pipeUpImg.src = "https://iili.io/3YOqnpe.png";

    const pipeDownImg = new Image();
    pipeDownImg.src = "https://iili.io/3YO0JwJ.png";

    const bgImg = new Image();
    bgImg.src = "https://iili.io/3YjtDQa.png";

    const startBtn = document.getElementById("startBtn");
    const restartBtn = document.getElementById("restartBtn");

    let bird, pipes, score, gameOver, frame;
    let pipeSpeed = 1.0;

    function initGame() {
      bird = {
        x: 50,
        y: 150,
        width: 70,
        height: 60,
        gravity: 0.25,
        lift: -5.5,
        velocity: 0
      };
      pipes = [];
      score = 0;
      gameOver = false;
      frame = 0;
    }

    function drawBird() {
      ctx.drawImage(birdImg, bird.x, bird.y, bird.width, bird.height);
    }

    function updateBird() {
      bird.velocity += bird.gravity;
      bird.y += bird.velocity;

      if (bird.y + bird.height > canvas.height || bird.y < 0) {
        gameOver = true;
      }
    }

    function drawPipes() {
      pipes.forEach(pipe => {
        ctx.drawImage(pipeUpImg, pipe.x, 0, pipe.width, pipe.topHeight);
        ctx.drawImage(pipeDownImg, pipe.x, canvas.height - pipe.bottomHeight, pipe.width, pipe.bottomHeight);
      });
    }

    function updatePipes() {
      pipes.forEach(pipe => {
        pipe.x -= pipeSpeed;

        const birdBox = {
          x: bird.x + 10,
          y: bird.y + 10,
          width: bird.width - 20,
          height: bird.height - 20
        };

        if (
          birdBox.x < pipe.x + pipe.width &&
          birdBox.x + birdBox.width > pipe.x &&
          (birdBox.y < pipe.topHeight || birdBox.y + birdBox.height > canvas.height - pipe.bottomHeight)
        ) {
          gameOver = true;
        }

        if (!pipe.passed && pipe.x + pipe.width < bird.x) {
          score++;
          pipe.passed = true;
        }
      });

      pipes = pipes.filter(pipe => pipe.x + pipe.width > 0);

      if (frame % 100 === 0) {
        const gap = 240; // ✅ Wide gap for easier play
        const topHeight = Math.floor(Math.random() * (canvas.height / 2 - 50)) + 50;
        const bottomHeight = canvas.height - topHeight - gap;

        pipes.push({
          x: canvas.width,
          width: 52,
          topHeight,
          bottomHeight,
          passed: false
        });
      }
    }

    function drawBackground() {
      ctx.drawImage(bgImg, 0, 0, canvas.width, canvas.height);
    }

    function drawScore() {
      ctx.fillStyle = "#000";
      ctx.font = "24px Arial";
      ctx.fillText(`Score: ${score}`, 10, 30);
    }

    function drawGameOver() {
      ctx.fillStyle = "red";
      ctx.font = "36px Arial";
      ctx.fillText("Game Over", canvas.width / 2 - 90, canvas.height / 2);
    }

    function loop() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      drawBackground();
      drawBird();
      drawPipes();
      drawScore();

      if (gameOver) {
        drawGameOver();
        restartBtn.style.display = 'inline-block';
        return;
      }

      updateBird();
      updatePipes();

      frame++;
      requestAnimationFrame(loop);
    }

    function jump() {
      bird.velocity = bird.lift;
    }

    document.addEventListener("keydown", e => {
      if (e.code === "Space" || e.code === "ArrowUp") jump();
    });

    canvas.addEventListener("touchstart", jump);

    startBtn.onclick = () => {
      initGame();
      startBtn.style.display = 'none';
      restartBtn.style.display = 'none';
      loop();
    };

    restartBtn.onclick = () => {
      initGame();
      restartBtn.style.display = 'none';
      loop();
    };
  </script>
</body>
</html>
