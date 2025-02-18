<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Happy Birthday Sansrita!</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            text-align: center;
            color: white;
            font-family: 'Comic Sans MS', cursive, sans-serif;
            background: url('sansrita-picture.jpg') center/cover no-repeat;
            position: relative;
        }
        .candles {
            display: flex;
            justify-content: center;
            position: absolute;
            bottom: 20px;
            width: 100%;
        }
        .candle {
            width: 30px;
            height: 100px;
            background: orange;
            margin: 0 15px;
            border-radius: 5px;
            position: relative;
        }
        .flame {
            width: 20px;
            height: 30px;
            background: gold;
            border-radius: 50%;
            position: absolute;
            top: -30px;
            left: 5px;
            animation: flicker 1s infinite;
        }
        @keyframes flicker {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }
        .message {
            position: absolute;
            top: 10%;
            width: 100%;
            font-size: 2em;
            text-shadow: 2px 2px black;
            opacity: 0;
            transition: opacity 2s;
        }
        .balloon {
            position: absolute;
            bottom: -100px;
            width: 60px;
            height: 80px;
            background: red;
            border-radius: 50% 50% 45% 45%;
            animation: float 5s ease-out infinite;
        }
        @keyframes float {
            0% { transform: translateY(0); }
            100% { transform: translateY(-100vh); }
        }
    </style>
</head>
<body>
    <div class="message" id="birthdayMessage">Wishing you a day as magical as you are! May your dreams float higher than these balloons and your happiness shine brighter than these candles! 🎊 Happy Birthday, Sansrita! 🎉🎂🎈</div>
    <div class="candles">
        <div class="candle"><div class="flame"></div></div>
        <div class="candle"><div class="flame"></div></div>
        <div class="candle"><div class="flame"></div></div>
    </div>
    <audio id="birthdaySong" src="happy_birthday_song.mp3"></audio>
    <script>
        async function getMicrophonePermission() {
            try {
                const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
                const audioContext = new (window.AudioContext || window.webkitAudioContext)();
                const source = audioContext.createMediaStreamSource(stream);
                const analyser = audioContext.createAnalyser();
                source.connect(analyser);
                analyser.fftSize = 256;
                const bufferLength = analyser.frequencyBinCount;
                const dataArray = new Uint8Array(bufferLength);

                function checkBlow() {
                    analyser.getByteTimeDomainData(dataArray);
                    let loud = dataArray.some(amplitude => Math.abs(amplitude - 128) > 50);
                    if (loud) {
                        document.querySelectorAll('.flame').forEach(flame => flame.style.display = 'none');
                        document.getElementById('birthdaySong').play();
                        releaseBalloons();
                        document.getElementById('birthdayMessage').style.opacity = 1;
                    } else {
                        requestAnimationFrame(checkBlow);
                    }
                }
                checkBlow();
            } catch (err) {
                alert('Microphone permission is required to blow out the candles.');
            }
        }

        function releaseBalloons() {
            setInterval(() => {
                const balloon = document.createElement('div');
                balloon.className = 'balloon';
                balloon.style.background = `hsl(${Math.random() * 360}, 70%, 60%)`;
                balloon.style.left = `${Math.random() * window.innerWidth}px`;
                document.body.appendChild(balloon);
                setTimeout(() => balloon.remove(), 6000);
            }, 300);
        }

        getMicrophonePermission();
    </script>
</body>
</html>
