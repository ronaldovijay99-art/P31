<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Mobile Envelope</title>
    <style>
        :root {
            --bg-color: #fce4ec;
            --env-color: #ff8a80;
            --env-dark: #e57373;
            --card-bg: #ffffff;
        }

        * {
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent; /* Removes blue flash on tap */
        }

        body {
            margin: 0;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: var(--bg-color);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            overflow: hidden; /* Prevents scrolling on mobile */
        }

        /* Responsive Wrapper */
        .container {
            position: relative;
            width: 280px; /* Smaller default for narrow phones */
            height: 180px;
            perspective: 1000px;
        }

        /* Scaling for slightly larger phones */
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

        /* The Card */
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

        /* The Pocket */
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

        /* "Open" Animations */
        .container.open .flap {
            transform: rotateX(180deg);
            z-index: 0;
        }

        .container.open .card {
            transform: translateY(-80px); /* Moves up just enough for mobile screens */
            z-index: 4;
        }

        /* Instructions Text */
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
        }

        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% {transform: translateY(0);}
            40% {transform: translateY(-5px);}
        }
    </style>
</head>
<body>

    <div class="container" onclick="this.classList.toggle('open')">
        <div class="envelope">
            <div class="flap"></div>
            <div class="card">
                <h2>Tap to Close</h2>
                <p>Designed for your phone screen!<br>✨ 📱 ✨</p>
            </div>
        </div>
        <div class="hint">Tap to Open</div>
    </div>

</body>
</html>
