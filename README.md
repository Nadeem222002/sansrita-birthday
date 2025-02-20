<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Happy Birthday Sansrita!</title>
    <style>
        body {
            background: url('birthday-celebration.jpg') no-repeat center center/cover; background-size: cover; height: 100vh; width: 100vw; no-repeat center center/cover;
            text-align: center;
            overflow: hidden;
        }
        .message, .final-message {
            position: absolute;
            top: 40%;
            width: 100%;
            font-size: 3em;
            color: white;
            text-shadow: 2px 2px 10px black;
            opacity: 0;
            animation: fadeIn 3s forwards 5s;
        }
        .final-message {
            top: 60%;
            font-size: 2em;
            animation-delay: 10s;
        }
        @keyframes fadeIn {
            to { opacity: 1; }
        }
        .balloon {
            position: absolute;
            bottom: -100px;
            font-size: 2em;
            animation: floatUp 5s linear forwards;
        }
        @keyframes floatUp {
            to { transform: translateY(-800px); }
        }
        .fireworks {
            position: absolute;
            width: 5px;
            height: 5px;
            background: gold;
            border-radius: 50%;
            animation: explode 1.5s ease-out forwards;
        }
        @keyframes explode {
            0% { transform: scale(1); opacity: 1; }
            100% { transform: scale(50); opacity: 0; }
        }
        .cake {
            position: absolute;
            bottom: 50px;
            left: 50%;
            transform: translateX(-50%);
            font-size: 3em;
        }
    </style>
</head>
<body>
    <audio id="bgMusic" src="happy-birthday.mp3" loop autoplay></audio>
    <div class="cake">🎂<span id="candles">🕯️ 🕯️ 🕯️</span></div>
    <div class="message">Happy Birthday Sansrita!</div>
    <div class="final-message">Wishing you a day as magical as you are! May your dreams float higher than these balloons and your happiness shine brighter than these candles! 🎊</div>
    <script>
        document.addEventListener("DOMContentLoaded", function() {
            document.getElementById("bgMusic").play().catch(() => {
                console.log("Autoplay blocked");
            });
            navigator.mediaDevices.getUserMedia({ audio: true }).then(stream => {
                const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
                recognition.start();
                recognition.onresult = function(event) {
                    let transcript = event.results[0][0].transcript.toLowerCase();
                    if (transcript.includes("blow")) {
                        document.getElementById("candles").innerHTML = "💨";
                        triggerFireworks();
                        launchBalloons();
                    }
                };
            }).catch(error => {
                console.log("Microphone permission denied", error);
            });
        });

        function triggerFireworks() {
            for (let i = 0; i < 10; i++) {
                let fw = document.createElement("div");
                fw.className = "fireworks";
                fw.style.left = Math.random() * 100 + "vw";
                fw.style.top = Math.random() * 100 + "vh";
                document.body.appendChild(fw);
                setTimeout(() => fw.remove(), 1500);
            }
        }

        function launchBalloons() {
    const text = "Happy Birthday Sansrita".split(" ");
    text.forEach((letter, i) => {
        setTimeout(() => {
            let balloon = document.createElement("div");
            balloon.className = "balloon";
            balloon.innerText = letter;
            balloon.style.left = Math.random() * 90 + "vw";
            balloon.style.animationDelay = i * 0.2 + "s";
            balloon.style.position = "absolute";
            balloon.style.bottom = "0";
            balloon.style.color = "red";
            document.body.appendChild(balloon);
            setTimeout(() => balloon.remove(), 5000);
        }, i * 500);
    });
}, i * 500);
    });
}, i * 500);
            });
        }
    </script>
</body>
</html>
