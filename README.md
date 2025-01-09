<!DOCTYPE html>
<html>
<head>
    <title>Typing Test</title>
    <style>
        body {
            font-family: sans-serif;
            text-align: center;
        }

        #timer {
            font-size: 20px;
            margin-bottom: 10px;
        }

        #text {
            font-size: 18px;
            margin-bottom: 15px;
        }

        #input-text {
            width: 500px;
            padding: 10px;
            font-size: 16px;
        }

        #stats {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1>Typing Test</h1>
    <div id="timer">Time: 0</div>
    <div id="text">
        <span id="current-char"></span>
        <span id="remaining-text"></span>
    </div>
    <input type="text" id="input-text" autofocus>
    <div id="stats">
        <p>WPM: <span id="wpm">0</span></p>
        <p>Accuracy: <span id="accuracy">100%</span></p>
    </div>
    <button id="start-btn">Start Test</button>

    <script>
        const textArea = document.getElementById('text');
        const inputField = document.getElementById('input-text');
        const timerDisplay = document.getElementById('timer');
        const wpmDisplay = document.getElementById('wpm');
        const accuracyDisplay = document.getElementById('accuracy');
        const startButton = document.getElementById('start-btn');

        let text = "";
        let startTime, endTime;
        let charactersTyped = 0;
        let errors = 0;
        let timerInterval;

        // Fetch sample text (replace with your preferred source)
        fetch('https://api.quotable.io/random')
            .then(response => response.json())
            .then(data => {
                text = data.content;
                textArea.textContent = text;
            });

        startButton.addEventListener('click', () => {
            startTime = new Date();
            timerInterval = setInterval(() => {
                const elapsedTime = Math.floor((new Date() - startTime) / 1000);
                timerDisplay.textContent = `Time: ${elapsedTime}`;
            }, 1000);

            inputField.disabled = false;
            inputField.focus();
        });

        inputField.addEventListener('input', () => {
            const currentChar = text[charactersTyped];
            const inputChar = inputField.value[charactersTyped];

            if (inputChar === currentChar) {
                charactersTyped++;
            } else {
                errors++;
            }

            if (charactersTyped === text.length) {
                clearInterval(timerInterval);
                endTime = new Date();

                const elapsedTimeInSeconds = (endTime - startTime) / 1000;
                const wordsTyped = charactersTyped / 5; // Assuming 5 characters per word

                const wpm = Math.round(wordsTyped / (elapsedTimeInSeconds / 60));
                const accuracy = Math.round((charactersTyped - errors) / charactersTyped * 100);

                wpmDisplay.textContent = wpm;
                accuracyDisplay.textContent = `${accuracy}%`;
            }

            // Update UI
            textArea.innerHTML = 
                `<span id="current-char">${text.slice(0, charactersTyped)}</span>` + 
                `<span>${text.slice(charactersTyped)}</span>`;
        });
    </script>
</body>
</html>
