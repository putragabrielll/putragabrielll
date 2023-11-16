
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flappy Bird</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            background-color: #0095ff;
        }

        #bird {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 32px;
            height: 32px;
            background-image: url('bird.png');
        }

        #pipes {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }

        .pipe {
            position: absolute;
            width: 50px;
            height: 100px;
            background-color: #222;
        }

        .pipe-top {
            top: 0;
            transform: translateY(-100%);
        }

        .pipe-bottom {
            bottom: 0;
        }
    </style>
</head>
<body>
    <div id="bird"></div>
    <div id="pipes"></div>

    <script>
        const bird = document.getElementById('bird');
        const pipes = document.getElementById('pipes');

        let birdY = 0;
        let pipeWidth = 50;
        let pipeGap = 100;
        let pipeSpeed = 1;
        let score = 0;

        function createPipes() {
            const pipeTop = document.createElement('div');
            pipeTop.classList.add('pipe', 'pipe-top');

            const pipeBottom = document.createElement('div');
            pipeBottom.classList.add('pipe', 'pipe-bottom');

            const pipeHeight = Math.random() * (window.innerHeight - pipeGap);
            pipeTop.style.height = pipeHeight + 'px';
            pipeBottom.style.height = window.innerHeight - pipeHeight - pipeGap + 'px';

            pipes.appendChild(pipeTop);
            pipes.appendChild(pipeBottom);

            const pipeLeft = window.innerWidth + pipeWidth;
            pipeTop.style.left = pipeLeft + 'px';
            pipeBottom.style.left = pipeLeft + 'px';

            pipeTop.addEventListener('collision', handleCollision);
            pipeBottom.addEventListener('collision', handleCollision);
        }

        function handleCollision() {
            alert('Game over!');
        }

        function update() {
            birdY += 2;
            bird.style.top = birdY + 'px';

            const pipesArray = pipes.querySelectorAll('.pipe');
            for (let i = 0; i < pipesArray.length; i++) {
                const pipe = pipesArray[i];
                const pipeLeft = parseInt(pipe.style.left);

                pipe.style.left = pipeLeft - pipeSpeed + 'px';

                if (pipeLeft <= -pipeWidth) {
                    pipe.remove();
                    score++;

                    if (score % 5 === 0) {
                        pipeSpeed++;
                    }
                }
            }

            if (birdY < 0 || birdY > window.innerHeight) {
                handleCollision();
            }
        }

        createPipes();
        setInterval(update, 10);

        document.addEventListener('keydown', (event) => {
            if (event.key === ' ') {
                birdY -= 10;
            }
        });
    </script>
</body>
</html>
