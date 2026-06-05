<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cozy Rainy Dice</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Comfortaa:wght@400;700&family=Quicksand:wght@400;600&display=swap');

        body {
            margin: 0;
            padding: 0;
            background: linear-gradient(to bottom, #161722, #0b0c13);
            color: #e2e8f0;
            font-family: 'Quicksand', sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            overflow: hidden;
            position: relative;
        }

        /* Animated CSS Rain Drops */
        .rain {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            z-index: 1;
            pointer-events: none;
            background-image: radial-gradient(rgba(255, 255, 255, 0.1) 1px, transparent 1px);
            background-size: 20px 30px;
            animation: rainAnimation 0.8s linear infinite;
            opacity: 0.4;
        }

        @keyframes rainAnimation {
            0% { background-position: 0px 0px; }
            100% { background-position: 20px 300px; }
        }

        .container {
            position: relative;
            z-index: 2;
            text-align: center;
            max-width: 550px;
            width: 90%;
            padding: 2.5rem 2rem;
            background: rgba(255, 255, 255, 0.03);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border-radius: 24px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.08);
        }

        h1 {
            font-family: 'Comfortaa', cursive;
            color: #ffb4a2;
            font-size: 2.4rem;
            margin: 0 0 8px 0;
            letter-spacing: -0.5px;
        }

        p.subtitle {
            font-size: 0.95rem;
            color: #94a3b8;
            margin: 0 0 2.5rem 0;
            font-style: italic;
        }

        .dice-container {
            display: flex;
            justify-content: center;
            gap: 12px;
            flex-wrap: wrap;
            margin-bottom: 2.5rem;
        }

        .dice-btn {
            background: rgba(255, 255, 255, 0.07);
            color: #f1f5f9;
            border: 1px solid rgba(255, 255, 255, 0.15);
            padding: 14px 22px;
            font-size: 0.95rem;
            font-weight: 600;
            border-radius: 14px;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            font-family: 'Quicksand', sans-serif;
        }

        .dice-btn:hover {
            background: #ffb4a2;
            color: #161722;
            transform: translateY(-4px);
            box-shadow: 0 8px 20px rgba(255, 180, 162, 0.3);
            border-color: #ffb4a2;
        }

        .result-box {
            min-height: 140px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
            border-radius: 16px;
            background: rgba(0, 0, 0, 0.25);
            border: 1px solid rgba(255, 255, 255, 0.03);
            opacity: 0;
            transform: scale(0.95);
            transition: all 0.4s ease;
        }

        .result-box.show {
            opacity: 1;
            transform: scale(1);
        }

        .dice-value {
            font-size: 3.5rem;
            margin-bottom: 12px;
            color: #ffb4a2;
            line-height: 1;
        }

        .dice-value.rolling {
            animation: spin 0.1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .output-text {
            font-size: 1.15rem;
            line-height: 1.6;
            color: #e2e8f0;
            margin-bottom: 10px;
            font-weight: 500;
        }

        .bg-music {
            font-size: 0.85rem;
            color: #a2d2ff;
            font-style: italic;
            background: rgba(162, 210, 255, 0.1);
            padding: 4px 12px;
            border-radius: 20px;
            display: inline-block;
        }

        .audio-player {
            margin-top: 2.5rem;
            opacity: 0.4;
            transition: opacity 0.3s;
        }
        .audio-player:hover { opacity: 0.9; }
    </style>
</head>
<body>

    <div class="rain"></div>

    <div class="container">
        <h1>Cozy Corner</h1>
        <p class="subtitle">Listen to the rain, clear your mind, and roll a dice.</p>

        <div class="dice-container">
            <button class="dice-btn" onclick="rollDice('rizz')">✨ Rizz Dice</button>
            <button class="dice-btn" onclick="rollDice('whatif')">🌧️ What If Dice</button>
            <button class="dice-btn" onclick="rollDice('realtalk')">💔 Real Talk Dice</button>
        </div>

        <div class="result-box" id="resultBox">
            <div class="dice-value" id="diceRoll">⚀</div>
            <div class="output-text" id="outputText"></div>
            <div class="bg-music" id="bgMusic" style="display: none;"></div>
        </div>

        <div class="audio-player">
            <p style="font-size: 0.75rem; margin-bottom: 8px; color:#94a3b8; letter-spacing: 1px;">AMBIENT RAIN</p>
            <audio controls loop autoplay src="https://assets.mixkit.co/active_storage/sfx/2433/2433-84.wav" style="height: 30px; width: 180px;"></audio>
        </div>
    </div>

    <script>
        const rizzQuotes = {
            1: "Are you a keyboard? Because you're just my type.",
            2: "Do you have a map? I keep getting lost in your eyes.",
            3: "Are you a magician? Whenever I look at you, everyone else disappears.",
            4: "If beauty were time, you'd be an eternity.",
            5: "I’m not a photographer, but I can easily picture us together.",
            6: "Is it raining outside, or is it just the vibe you're bringing that's making me fall?"
        };

        const whatIfs = {
            1: { text: "What if we never crossed paths that day?", song: "Tahanan by Adie" },
            2: { text: "What if you stayed just a little bit longer?", song: "Pasilyo by SunKissed Lola" },
            3: { text: "What if I told you how I really felt back then?", song: "14 by Silent Sanctuary" },
            4: { text: "What if we were just right for each other, but at the wrong time?", song: "Kathang Isip by Ben&Ben" },
            5: { text: "What if you're already happier with someone else?", song: "Multo by Janine Berdin" },
            6: { text: "What if we were meant to be, but we both just gave up too soon?", song: "Huling Sandali by I Belong to the Zoo" }
        };

        const realTalks = {
            1: "You miss the memories, not the person.",
            2: "Stop reopening your wounds just to see if they are still bleeding.",
            3: "They aren't busy. You are just not a priority.",
            4: "You cannot continue to love someone into loving you back.",
            5: "They walked away because it was easy for them. Let that sink in.",
            6: "You are holding onto an idea of them that no longer exists. They moved on a long time ago."
        };

        const diceFaces = { 1: '⚀', 2: '⚁', 3: '⚂', 4: '⚃', 5: '⚄', 6: '⚅' };

        function rollDice(type) {
            const resultBox = document.getElementById('resultBox');
            const diceRoll = document.getElementById('diceRoll');
            const outputText = document.getElementById('outputText');
            const bgMusic = document.getElementById('bgMusic');

            // Reset states and trigger animation
            resultBox.classList.add('show');
            diceRoll.classList.add('rolling');
            outputText.innerText = "Letting the dice fall...";
            bgMusic.style.display = 'none';

            let counter = 0;
            const interval = setInterval(() => {
                // Change faces rapidly during spin animation
                const randomFace = Math.floor(Math.random() * 6) + 1;
                diceRoll.innerText = diceFaces[randomFace];
                counter++;

                if (counter > 10) {
                    clearInterval(interval);
                    diceRoll.classList.remove('rolling');

                    // Final roll outcome
                    const finalRoll = Math.floor(Math.random() * 6) + 1;
                    diceRoll.innerText = diceFaces[finalRoll];

                    if (type === 'rizz') {
                        outputText.innerText = rizzQuotes[finalRoll];
                    } else if (type === 'whatif') {
                        outputText.innerText = `Level ${finalRoll}: "${whatIfs[finalRoll].text}"`;
                        bgMusic.innerText = `🎵 Reco track: ${whatIfs[finalRoll].song}`;
                        bgMusic.style.display = 'inline-block';
                    } else if (type === 'realtalk') {
                        outputText.innerText = `Level ${finalRoll}: "${realTalks[finalRoll]}"`;
                    }
                }
            }, 70);
        }
    </script>
</body>
</html>
