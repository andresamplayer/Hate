# Hate
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Retribution.exe - Ira</title>
  <style>
    body { margin: 0; background: black; overflow: hidden; }
    canvas { display: block; margin: auto; background: #111; }
    #rageBar {
      position: absolute;
      top: 10px;
      left: 50%;
      transform: translateX(-50%);
      width: 60%;
      height: 20px;
      background: #333;
      border: 1px solid red;
    }
    #rageLevel {
      height: 100%;
      background: red;
      width: 0%;
    }
    #msg {
      position: absolute;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      color: white;
      font-family: monospace;
      font-size: 18px;
      text-align: center;
      white-space: pre-wrap;
    }
  </style>
</head>
<body>
  <div id="rageBar"><div id="rageLevel"></div></div>
  <div id="msg"></div>
  <canvas id="game" width="512" height="384"></canvas>

  <script>
    const canvas = document.getElementById("game");
    const ctx = canvas.getContext("2d");
    const rageBar = document.getElementById("rageLevel");
    const msg = document.getElementById("msg");

    const playerImg = new Image();
    playerImg.src = "sprites/player.png";

    const player = {
      x: 240,
      y: 180,
      speed: 2
    };

    const keys = {};
    const messages = [
      "...Why won't they listen?",
      "Everything burns eventually.",
      "This isn't who I was...",
      "Don't push me anymore.",
      "Stop smiling at me.",
      "I said STOP!"
    ];

    let rage = 0;
    let msgIndex = 0;

    document.addEventListener("keydown", e => keys[e.key] = true);
    document.addEventListener("keyup", e => keys[e.key] = false);

    function drawPlayer() {
      ctx.drawImage(playerImg, player.x, player.y, 32, 32);
    }

    function update() {
      if (keys["ArrowUp"] || keys["w"]) player.y -= player.speed;
      if (keys["ArrowDown"] || keys["s"]) player.y += player.speed;
      if (keys["ArrowLeft"] || keys["a"]) player.x -= player.speed;
      if (keys["ArrowRight"] || keys["d"]) player.x += player.speed;

      rage += 0.05;
      rageBar.style.width = `${Math.min(rage, 100)}%`;

      if (rage > (msgIndex + 1) * 15 && msgIndex < messages.length) {
        msg.textContent = messages[msgIndex];
        msgIndex++;
      }

      if (rage >= 100) {
        msg.textContent = "GAME OVER - YOU LET THE RAGE CONTROL YOU";
      }
    }

    function draw() {
      ctx.fillStyle = "#111";
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      drawPlayer();
    }

    function loop() {
      update();
      draw();
      requestAnimationFrame(loop);
    }

    playerImg.onload = () => {
      loop();
    };
  </script>
</body>
</html>
