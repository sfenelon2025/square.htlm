ideas

1)Butterfly Game(choas) 
)Doom Game
)Ski
4) Wordle
5) Hoppy Frog
6) Stragy
7)Card Games
8) Monopoly
9) Jepardy
10) Calendar
11) Study Guide
12) Steps Counter
13) Health Tracker
14) Workout Maker
15) Job Finder
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Hoppy Frog Game</title>
  <style>
    body {
      margin: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background: linear-gradient(#87CEEB, #e0f7fa);
      font-family: Arial, sans-serif;
    }
    #game {
      position: relative;
      width: 400px;
      height: 500px;
      background: #cceeff;
      overflow: hidden;
      border: 4px solid #333;
      border-radius: 10px;
    }
    #frog {
      position: absolute;
      left: 50px;
      top: 200px;
      font-size: 40px;
    }
    .pipe {
      position: absolute;
      width: 60px;
      background: green;
    }
    #score {
      position: absolute;
      top: 10px;
      left: 10px;
      font-weight: bold;
      font-size: 18px;
    }
  </style>
</head>
<body>
  <div id="game">
    <div id="score">Score: 0</div>
    <div id="frog">🐸</div>
  </div>

  <script>
    const game = document.getElementById("game");
    const frog = document.getElementById("frog");
    const scoreEl = document.getElementById("score");

    let frogY = 200;
    let velocity = 0;
    const gravity = 0.5;
    const jump = -8;

    let pipes = [];
    let score = 0;
    let gameOver = false;

    function createPipe() {
      const gap = 120;
      const pipeTopHeight = Math.random() * 250 + 50;
      const pipeBottomHeight = 500 - pipeTopHeight - gap;

      const pipeTop = document.createElement("div");
      pipeTop.className = "pipe";
      pipeTop.style.height = pipeTopHeight + "px";
      pipeTop.style.left = "400px";
      pipeTop.style.top = "0px";

      const pipeBottom = document.createElement("div");
      pipeBottom.className = "pipe";
      pipeBottom.style.height = pipeBottomHeight + "px";
      pipeBottom.style.left = "400px";
      pipeBottom.style.bottom = "0px";

      game.appendChild(pipeTop);
      game.appendChild(pipeBottom);

      pipes.push({ top: pipeTop, bottom: pipeBottom, x: 400, passed: false });
    }

    function update() {
      if (gameOver) return;

      // Frog physics
      velocity += gravity;
      frogY += velocity;
      if (frogY < 0) frogY = 0;
      if (frogY > 460) {
        frogY = 460;
        endGame();
      }
      frog.style.top = frogY + "px";

      // Move pipes
      pipes.forEach(pipe => {
        pipe.x -= 2;
        pipe.top.style.left = pipe.x + "px";
        pipe.bottom.style.left = pipe.x + "px";

        if (!pipe.passed && pipe.x + 60 < 50) {
          score++;
          scoreEl.textContent = "Score: " + score;
          pipe.passed = true;
        }

        // Collision detection
        const frogRect = frog.getBoundingClientRect();
        const topRect = pipe.top.getBoundingClientRect();
        const bottomRect = pipe.bottom.getBoundingClientRect();
        if (
          (frogRect.left < topRect.right &&
           frogRect.right > topRect.left &&
           frogRect.top < topRect.bottom) ||
          (frogRect.left < bottomRect.right &&
           frogRect.right > bottomRect.left &&
           frogRect.bottom > bottomRect.top)
        ) {
          endGame();
        }
      });

      // Remove old pipes
      pipes = pipes.filter(pipe => pipe.x > -60);

      requestAnimationFrame(update);
    }

    function endGame() {
      gameOver = true;
      alert("Game Over! Score: " + score);
      window.location.reload();
    }

    // Controls
    window.addEventListener("keydown", e => {
      if (e.code === "Space") velocity = jump;
    });
    game.addEventListener("click", () => velocity = jump);

    // Start game
    setInterval(createPipe, 2000);
    update();
  </script>
</body>
</html>
