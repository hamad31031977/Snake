<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>لعبة السنيك</title>
    <style>
        body { text-align: center; font-family: Arial, sans-serif; background-color: #222; color: white; }
        canvas { background-color: black; display: block; margin: auto; }
    </style>
</head>
<body>

<h1>لعبة السنيك</h1>
<canvas id="gameCanvas" width="400" height="400"></canvas>
<p>استخدم الأسهم للتحكم بالثعبان</p>

<script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");

    const gridSize = 20;
    let snake = [{ x: 200, y: 200 }];
    let direction = "RIGHT";
    let food = generateFood();
    let gameRunning = true;

    document.addEventListener("keydown", changeDirection);

    function gameLoop() {
        if (!gameRunning) return;

        setTimeout(() => {
            clearCanvas();
            moveSnake();
            drawFood();
            drawSnake();
            checkGameOver();
            gameLoop();
        }, 100);
    }

    function clearCanvas() {
        ctx.fillStyle = "black";
        ctx.fillRect(0, 0, canvas.width, canvas.height);
    }

    function drawSnake() {
        ctx.fillStyle = "green";
        snake.forEach(segment => {
            ctx.fillRect(segment.x, segment.y, gridSize, gridSize);
        });
    }

    function moveSnake() {
        let head = { ...snake[0] };
        if (direction === "UP") head.y -= gridSize;
        if (direction === "DOWN") head.y += gridSize;
        if (direction === "LEFT") head.x -= gridSize;
        if (direction === "RIGHT") head.x += gridSize;

        snake.unshift(head);

        if (head.x === food.x && head.y === food.y) {
            food = generateFood();
        } else {
            snake.pop();
        }
    }

    function changeDirection(event) {
        const key = event.key;
        if (key === "ArrowUp" && direction !== "DOWN") direction = "UP";
        if (key === "ArrowDown" && direction !== "UP") direction = "DOWN";
        if (key === "ArrowLeft" && direction !== "RIGHT") direction = "LEFT";
        if (key === "ArrowRight" && direction !== "LEFT") direction = "RIGHT";
    }

    function drawFood() {
        ctx.fillStyle = "red";
        ctx.fillRect(food.x, food.y, gridSize, gridSize);
    }

    function generateFood() {
        return {
            x: Math.floor(Math.random() * (canvas.width / gridSize)) * gridSize,
            y: Math.floor(Math.random() * (canvas.height / gridSize)) * gridSize
        };
    }

    function checkGameOver() {
        const head = snake[0];

        if (head.x < 0 || head.y < 0 || head.x >= canvas.width || head.y >= canvas.height) {
            gameRunning = false;
            alert("انتهت اللعبة! اضغط F5 لإعادة المحاولة.");
        }

        for (let i = 1; i < snake.length; i++) {
            if (head.x === snake[i].x && head.y === snake[i].y) {
                gameRunning = false;
                alert("انتهت اللعبة! اضغط F5 لإعادة المحاولة.");
            }
        }
    }

    gameLoop();
</script>

</body>
</html>