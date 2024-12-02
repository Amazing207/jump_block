<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jump Block</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js"></script>
    <style>
        body, html {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
            overflow: hidden;
        }
        .pause-button {
            position: absolute;
            top: 10px;
            right: 10px;
            padding: 10px;
            font-size: 16px;
            cursor: pointer;
            background-color: transparent;
            border: none;
        }
    </style>
</head>
<body>
    <button class="pause-button" onclick="togglePause()">
        <img src="stop.jpg" alt="Pause" width="40" height="40" id="pauseIcon">
    </button>
    <script>
        let cube;
        let obstacles = [];
        let jumpSpeed = 0;
        let gravity = 0.5;
        let isJumping = false;
        let floorLevel = 300;
        let score = 0;
        let isPaused = false;

        function setup() {
            createCanvas(800, 600).parent(document.body);
            cube = createVector(100, floorLevel);

            // Додаємо перешкоди
            obstacles.push({
                position: createVector(600, floorLevel),
                angle: 0,
                speed: 6
            });
            obstacles.push({
                position: createVector(900, floorLevel),
                angle: 0,
                speed: 6
            });
        }

        function draw() {
            background(255);

            // Лінія для перешкод
            stroke(0);
            strokeWeight(2);
            line(0, floorLevel, width, floorLevel);

            // Score display
            fill(0);
            textSize(32);
            textAlign(LEFT);
            text(`Score: ${score}`, 10, 40);

            // Cube
            fill(255, 0, 0);
            rect(cube.x, cube.y - 50, 50, 50);

            // Малюємо та оновлюємо перешкоди
            for (let i = 0; i < obstacles.length; i++) {
                let obstacle = obstacles[i];

                // Малюємо перешкоду
                push();
                translate(obstacle.position.x + 25, obstacle.position.y - 25);
                rotate(obstacle.angle);
                fill(0, 0, 255);
                ellipse(0, 0, 50, 50);
                pop();

                if (!isPaused) {
                    // Обертання і рух
                    obstacle.angle += radians(5);
                    obstacle.position.x -= obstacle.speed;

                    // Перезапуск перешкоди
                    if (obstacle.position.x < -50) {
                        let maxGap = 400;
                        let minGap = 200;
                        let previousX = obstacles[(i - 1 + obstacles.length) % obstacles.length].position.x;

                        obstacle.position.x = max(previousX + random(minGap, maxGap), width + random(minGap, maxGap));
                        score += 10;
                    }

                    // Перевірка зіткнення
                    if (
                        obstacle.position.x < cube.x + 50 &&
                        obstacle.position.x + 50 > cube.x &&
                        cube.y === floorLevel
                    ) {
                        noLoop();
                        textSize(64);
                        fill(255, 0, 0);
                        textAlign(CENTER);
                        text("Game Over", width / 2, height / 2);
                    }
                }
            }

            // Jump logic
            if (!isPaused && isJumping) {
                cube.y -= jumpSpeed;
                jumpSpeed -= gravity;
                if (cube.y >= floorLevel) {
                    cube.y = floorLevel;
                    isJumping = false;
                    jumpSpeed = 0;
                }
            }
        }

        function keyPressed() {
            if (document.activeElement.tagName !== 'BUTTON' && key === ' ' && !isJumping) {
                isJumping = true;
                jumpSpeed = 10;
            }
        }

        function togglePause() {
            isPaused = !isPaused;

            const pauseIcon = document.getElementById('pauseIcon');
            pauseIcon.src = isPaused ? 'play.jpg' : 'stop.jpg';
            pauseIcon.alt = isPaused ? 'Play' : 'Pause';

            document.activeElement.blur();
        }
    </script>
</body>
</html>

