<html>
<head>
    <style>
        body {
            background-color: #000000;
        }
        #container {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            color: #FFFFFF;
            font-family: Arial, sans-serif;
            font-size: 36px;
        }
        #confetti {
            position: relative;
            width: 200px;
            height: 200px;
        }
        .confetti-piece {
            position: absolute;
            width: 10px;
            height: 10px;
            background-color: #F9A602;
            transform: rotate(45deg);
        }
    </style>
</head>
<body>
    <div id="container">
        <div id="confetti">
            <div class="confetti-piece"></div>
            <div class="confetti-piece"></div>
            <div class="confetti-piece"></div>
            <div class="confetti-piece"></div>
            <div class="confetti-piece"></div>
            <div class="confetti-piece"></div>
            <div class="confetti-piece"></div>
            <div class="confetti-piece"></div>
            <div class="confetti-piece"></div>
            <div class="confetti-piece"></div>
        </div>
        <p>Congratulations! You won the game!</p>
    </div>
    <script>
        function createConfetti() {
            const confettiContainer = document.getElementById('confetti');
            for (let i = 0; i < 100; i++) {
                const confettiPiece = document.createElement('div');
                confettiPiece.className = 'confetti-piece';
                confettiPiece.style.left = Math.random() * 100 + '%';
                confettiPiece.style.animationDuration = Math.random() * 3 + 2 + 's';
                confettiContainer.appendChild(confettiPiece);
            }
        }
        createConfetti();
    </script>
</body>
</html>
