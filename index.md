<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #333;
            color: white;
            font-family: Arial, sans-serif;
        }
        canvas {
            border: 2px solid white;
        }
        button {
            margin-bottom: 10px;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <button id="startButton">Start Game</button>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const startButton = document.getElementById('startButton');
        const gridSize = 20;
        let snake = [{ x: 200, y: 200 }];
        let direction = { x: gridSize, y: 0 };
        let food = { x: gridSize * 5, y: gridSize * 5 };
        let score = 0;
        let gameInterval;

        function update() {
            const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };

            if (head.x < 0 || head.y < 0 || head.x >= canvas.width || head.y >= canvas.height || snake.some(segment => segment.x === head.x && segment.y === head.y)) {
                clearInterval(gameInterval);
                alert(`Game Over! Your score: ${score}`);
                document.location.reload();
            }

            snake.unshift(head);

            if (head.x === food.x && head.y === food.y) {
                score++;
                food.x = Math.floor(Math.random() * (canvas.width / gridSize)) * gridSize;
                food.y = Math.floor(Math.random() * (canvas.height / gridSize)) * gridSize;
            } else {
                snake.pop();
            }
        }

        function draw() {
            ctx.fillStyle = '#333';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            ctx.fillStyle = 'lime';
            snake.forEach(segment => ctx.fillRect(segment.x, segment.y, gridSize, gridSize));

            ctx.fillStyle = 'red';
            ctx.fillRect(food.x, food.y, gridSize, gridSize);

            ctx.fillStyle = 'white';
            ctx.fillText(`Score: ${score}`, 10, canvas.height - 10);
        }

        function loop() {
            update();
            draw();
        }

        function startGame() {
            if (!gameInterval) {
                gameInterval = setInterval(loop, 100);
            }
        }

        startButton.addEventListener('click', startGame);

        window.addEventListener('keydown', e => {
            const keyMap = {
                ArrowUp: { x: 0, y: -gridSize },
                ArrowDown: { x: 0, y: gridSize },
                ArrowLeft: { x: -gridSize, y: 0 },
                ArrowRight: { x: gridSize, y: 0 },
                KeyW: { x: 0, y: -gridSize },
                KeyS: { x: 0, y: gridSize },
                KeyA: { x: -gridSize, y: 0 },
                KeyD: { x: gridSize, y: 0 }
            };
            if (e.code === 'Space') {
                startGame();
            }
            if (keyMap[e.code] && !(keyMap[e.code].x === -direction.x && keyMap[e.code].y === -direction.y)) {
                direction = keyMap[e.code];
            }
        });
    </script>
</body>
</html>
