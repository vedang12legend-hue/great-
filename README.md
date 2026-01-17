<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Dodge The Blocks</title>
<style>
    body {
        background: #111;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        margin: 0;
        font-family: Arial, sans-serif;
        color: white;
    }

    #game {
        position: relative;
        width: 300px;
        height: 500px;
        background: #222;
        overflow: hidden;
        border: 2px solid white;
    }

    #player {
        position: absolute;
        width: 50px;
        height: 50px;
        background: lime;
        bottom: 10px;
        left: 125px;
    }

    .block {
        position: absolute;
        width: 50px;
        height: 50px;
        background: red;
        top: -60px;
    }

    #score {
        position: absolute;
        top: 10px;
        left: 10px;
        font-size: 18px;
    }
</style>
</head>
<body>

<div id="game">
    <div id="score">Score: 0</div>
    <div id="player"></div>
</div>

<script>
    const game = document.getElementById("game");
    const player = document.getElementById("player");
    const scoreText = document.getElementById("score");

    let score = 0;
    let playerX = 125;
    let speed = 4;
    let gameOver = false;

    document.addEventListener("keydown", (e) => {
        if (e.key === "ArrowLeft" && playerX > 0) {
            playerX -= 20;
        }
        if (e.key === "ArrowRight" && playerX < 250) {
            playerX += 20;
        }
        player.style.left = playerX + "px";
    });

    function createBlock() {
        const block = document.createElement("div");
        block.classList.add("block");
        block.style.left = Math.floor(Math.random() * 6) * 50 + "px";
        game.appendChild(block);

        let blockY = -50;

        const fall = setInterval(() => {
            if (gameOver) {
                clearInterval(fall);
                return;
            }

            blockY += speed;
            block.style.top = blockY + "px";

            // Collision detection
            if (
                blockY > 420 &&
                parseInt(block.style.left) === playerX
            ) {
                alert("💀 Game Over! Your Score: " + score);
                gameOver = true;
                location.reload();
            }

            if (blockY > 500) {
                clearInterval(fall);
                block.remove();
                score++;
                scoreText.textContent = "Score: " + score;
            }
        }, 20);
    }

    setInterval(() => {
        if (!gameOver) createBlock();
    }, 1000);
</script>

</body>
</html>
