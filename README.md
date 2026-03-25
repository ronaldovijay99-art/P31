<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Love Letter</title>
    <style>
        :root {
            /* Palette: Soft Pinks & White */
            --bg-color: #fce4ec; /* Very pale pink */
            --env-color: #ff8a80; /* Soft coral/pink */
            --env-dark: #e57373; /* Darker coral for the flap */
            --card-bg: #ffffff;
            --heart-rain-color: rgba(255, 138, 128, 0.4); /* Faded coral for hearts */
        }

        * {
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent; /* Mobile optimization */
        }

        body {
            margin: 0;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: var(--bg-color);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            overflow: hidden; /* Crucial: stops the page from scrolling */
        }

        /* --- HEART RAIN CANVAS --- */
        #heartCanvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none; /* Let clicks pass through to the envelope */
            z-index: 0;
        }

        /* --- ENVELOPE CONTAINER --- */
        .container {
            position: relative;
            width: 280px;
            height: 180px;
            perspective: 1000px;
            z-index: 10; /* Sits above the rain */
        }

        /* Responsive Scaling for larger phones */
        @media (min-width: 375px) {
            .container { width: 320px; height: 210px; }
        }

        .envelope {
            position: relative;
            width: 100%;
            height: 100%;
            background: var(--env-color);
            border-radius: 0 0 8px 8px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            cursor: pointer;
            transition: transform 0.3s ease;
        }

        /* The Top Flap */
        .flap {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 0;
            border-left: 140px solid transparent;
            border-right: 140px solid transparent;
            border-top: 100px solid var(--env-dark);
            transform-origin: top;
            transition: transform 0.4s 0.2s ease, z-index 0.1s 0.4s;
            z-index: 3;
        }

        @media (min-width: 375px) {
            .flap { border-left-width: 160px; border-right-width: 160px; border-top-width: 115px; }
        }

        /* The Pocket/Front */
        .envelope::after {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--env-color);
            clip-path: polygon(0 0, 50% 50%, 100% 0, 100% 100%, 0 100%);
            z-index: 2;
            border-radius: 0 0 8px 8px;
        }

        /* The Card (The Letter) */
        .card {
            position: absolute;
            bottom: 5px;
            left: 5%;
            width: 90%;
            height: 90%;
            background: var(--card-bg);
            border-radius: 5px;
            padding: 20px;
            text-align: center;
            transition: transform 0.5s 0.4s ease, z-index 0.1s 0.4s;
            z-index: 1;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        .card h2 { margin: 0; color: var(--env-dark); font-size: 1.2rem; }
        .card p { font-size: 0.9rem; color: #555; line-height: 1.4; }

        /* --- ANIMATION STATES ('OPEN') --- */
        .container.open .flap {
            transform: rotateX(180deg);
            z-index: 0;
        }

        .container.open .card {
            transform: translateY(-90px); /* Adjust based on mobile screen comfort */
            z-index: 4;
        }

        /* Tap Instruction Hint */
        .hint {
            position: absolute;
            bottom: -50px;
            width: 100%;
            text-align: center;
            color: var(--env-dark);
            font-weight: bold;
            font-size: 0.8rem;
            text-transform: uppercase;
            letter-spacing: 1px;
            animation: bounce 2s infinite;
            z-index: 5;
        }

        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% {transform: translateY(0);}
            40% {transform: translateY(-5px);}
        }
    </style>
</head>
<body>

    <canvas id="heartCanvas"></canvas>

    <div class="container" onclick="this.classList.toggle('open')">
        <div class="envelope">
            <div class="flap"></div>
            <div class="card">
                <h2>A Special Message</h2>
                <p>Hello there!<br>Sending some love and warmth your way today.<br><br>✨❤️✨</p>
            </div>
        </div>
        <div class="hint">Tap to Open</div>
    </div>

    <script>
        const canvas = document.getElementById('heartCanvas');
        const ctx = canvas.getContext('2d');

        // Set canvas size
        let width, height;
        function setCanvasSize() {
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;
        }
        setCanvasSize();
        window.addEventListener('resize', setCanvasSize);

        const hearts = [];
        // Increase/decrease this number for more or fewer hearts (mobile friendly)
        const heartCount = 50; 

        class Heart {
            constructor() {
                this.reset();
            }

            reset() {
                this.x = Math.random() * width;
                this.y = Math.random() * height - height; // Start above the screen
                this.size = Math.random() * 8 + 4; // Randomized size
                this.speedY = Math.random() * 2 + 1; // Falling speed
                this.speedX = Math.random() * 1 - 0.5; // Slight sideways flow
                this.opacity = Math.random() * 0.5 + 0.1; // Soft randomized opacity
                // Choose a random rotation
                this.rotation = Math.random() * Math.PI * 2;
                this.rotationSpeed = (Math.random() - 0.5) * 0.02;
            }

            update() {
                this.y += this.speedY;
                this.x += this.speedX;
                this.rotation += this.rotationSpeed;

                // Reset heart if it falls off the bottom
                if (this.y > height + this.size) {
                    this.reset();
                    this.y = -this.size; // Place just above top
                }
            }

            draw() {
                ctx.save();
                ctx.translate(this.x, this.y);
                ctx.rotate(this.rotation);
                ctx.beginPath();
                ctx.moveTo(0, 0);
                
                // Drawing the heart shape
                const topCurveHeight = this.size * 0.3;
                ctx.bezierCurveTo(0, topCurveHeight, -this.size / 2, topCurveHeight, -this.size / 2, 0);
                ctx.bezierCurveTo(-this.size / 2, -topCurveHeight, 0, -topCurveHeight * 1.7, 0, -this.size);
                ctx.bezierCurveTo(0, -topCurveHeight * 1.7, this.size / 2, -topCurveHeight, this.size / 2, 0);
                ctx.bezierCurveTo(this.size / 2, topCurveHeight, 0, topCurveHeight, 0, 0);
                
                // Color and transparency (using CSS variable value)
                ctx.fillStyle = `rgba(229, 115, 115, ${this.opacity})`; // #e57373
                ctx.fill();
                ctx.restore();
            }
        }

        // Initialize hearts
        for (let i = 0; i < heartCount; i++) {
            hearts.push(new Heart());
            // Randomly pre-fill the screen
            hearts[i].y = Math.random() * height; 
        }

        function animate() {
            ctx.clearRect(0, 0, width, height); // Clear canvas
            for (let heart of hearts) {
                heart.update();
                heart.draw();
            }
            requestAnimationFrame(animate); // Repeat
        }

        animate();
    </script>

</body>
</html>
