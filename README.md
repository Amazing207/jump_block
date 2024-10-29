<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jump Block</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js"></script>
</head>
<body>
    <script>
        let cube, obstacle;
        let jumpSpeed = 0;
        let gravity = 0.5;
        let isJumping = false;
        let floorLevel = 300;
        let score = 0;

        function setup() {
            createCanvas(800, 600);
            cube = createVector(100, floorLevel);
            obstacle = createVector(600, floorLevel);
        }

        function draw() {
            background(255);
            fill(0);
            textSize(32);
            text(`Score: ${score}`, 10, 30);

            // Draw cube
            fill(255, 0, 0);
            rect(cube.x, cube.y - 50, 50, 50);

            // Draw obstacle
            fill(0, 0, 255);
            rect(obstacle.x, obstacle.y - 50, 50, 50);

            // Move obstacle
            obstacle.x -= 5;
            if (obstacle.x < -50) {
                obstacle.x = width + random(100, 300);
                score++;
            }

            // Jump logic
            if (isJumping) {
                cube.y -= jumpSpeed;
                jumpSpeed -= gravity;
                if (cube.y >= floorLevel) {
                    cube.y = floorLevel;
                    isJumping = false;
                    jumpSpeed = 0;
                }
            }

            // Check for collision
            if (obstacle.x < cube.x + 50 && obstacle.x + 50 > cube.x && cube.y === floorLevel) {
                noLoop();
                textSize(64);
                fill(255, 0, 0);
                text("Game Over", width / 2 - 150, height / 2);
            }
        }

        function keyPressed() {
            if (key === ' ') {
                if (!isJumping) {
                    isJumping = true;
                    jumpSpeed = 10;
                }
            }
        }
    </script>
</body>
</html>
