# cobrinha-angolana
Jogo da Cobrinha Angolana 🇦🇴 feito em HTML, CSS e JavaScript. Inspirado nas cores da bandeira de Angola.

  <!DOCTYPE html>
<html lang="pt">
<head>
  <meta charset="UTF-8">
  <title>Cobrinha Angolana 🐍🇦🇴</title>
  <style>
    body {
      text-align: center;
      font-family: Arial, sans-serif;
      background: linear-gradient(180deg, #d60000 50%, #000000 50%);
      color: #ffd700;
    }
    canvas {
      display: block;
      margin: 20px auto;
      background: #111;
      border: 4px solid #ffd700;
    }
  </style>
</head>
<body>
  <h1>🐍 Cobrinha Angolana 🇦🇴</h1>
  <p>Use as setas do teclado para mover</p>
  <canvas id="game" width="400" height="400"></canvas>

  <script>
    const canvas = document.getElementById("game");
    const ctx = canvas.getContext("2d");

    let box = 20;
    let snake = [{ x: 9 * box, y: 10 * box }];
    let direction = null;
    let score = 0;

    let food = {
      x: Math.floor(Math.random() * 19) * box,
      y: Math.floor(Math.random() * 19) * box
    };

    document.addEventListener("keydown", e => {
      if (e.key === "ArrowLeft" && direction !== "RIGHT") direction = "LEFT";
      if (e.key === "ArrowUp" && direction !== "DOWN") direction = "UP";
      if (e.key === "ArrowRight" && direction !== "LEFT") direction = "RIGHT";
      if (e.key === "ArrowDown" && direction !== "UP") direction = "DOWN";
    });

    function draw() {
      ctx.clearRect(0, 0, 400, 400);

      // comida (amarelo da bandeira)
      ctx.fillStyle = "#ffd700";
      ctx.fillRect(food.x, food.y, box, box);

      // cobra (primeiro vermelho, depois preto)
      for (let i = 0; i < snake.length; i++) {
        ctx.fillStyle = i === 0 ? "#d60000" : "#000000";
        ctx.fillRect(snake[i].x, snake[i].y, box, box);
        ctx.strokeStyle = "#ffd700";
        ctx.strokeRect(snake[i].x, snake[i].y, box, box);
      }

      // posição da cabeça
      let snakeX = snake[0].x;
      let snakeY = snake[0].y;

      if (direction === "LEFT") snakeX -= box;
      if (direction === "UP") snakeY -= box;
      if (direction === "RIGHT") snakeX += box;
      if (direction === "DOWN") snakeY += box;

      // se comer comida
      if (snakeX === food.x && snakeY === food.y) {
        score++;
        food = {
          x: Math.floor(Math.random() * 19) * box,
          y: Math.floor(Math.random() * 19) * box
        };
      } else {
        snake.pop();
      }

      let newHead = { x: snakeX, y: snakeY };

      // colisão = game over
      if (
        snakeX < 0 || snakeY < 0 ||
        snakeX >= 400 || snakeY >= 400 ||
        snake.some(s => s.x === newHead.x && s.y === newHead.y)
      ) {
        clearInterval(game);
        alert("Game Over! Pontuação final: " + score);
      }

      snake.unshift(newHead);

      // pontuação
      ctx.fillStyle = "#ffd700";
      ctx.font = "18px Arial";
      ctx.fillText("Pontuação: " + score, 10, 390);
    }

    let game = setInterval(draw, 100);
  </script>
</body>
</html>
