
<!DOCTYPE html>
<html lang="am">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Keno80-Fast - Activate Empire</title>
    <style>
        body { font-family: 'Segoe UI', sans-serif; text-align: center; background-color: #1a1a2e; color: white; margin: 0; padding: 20px; }
        .container { max-width: 600px; margin: auto; background: #16213e; padding: 20px; border-radius: 15px; box-shadow: 0 0 20px rgba(0,0,0,0.5); }
        h1 { color: #e94560; margin-bottom: 10px; }
        .grid { display: grid; grid-template-columns: repeat(10, 1fr); gap: 5px; margin: 20px 0; }
        .number { width: 40px; height: 40px; line-height: 40px; background: #0f3460; border-radius: 5px; cursor: pointer; font-weight: bold; transition: 0.3s; }
        .number:hover { background: #533483; }
        .selected { background: #e94560 !important; color: white; }
        .won { background: #4caf50 !important; color: white; animation: flash 0.5s infinite; }
        button { padding: 15px 30px; font-size: 18px; cursor: pointer; border: none; border-radius: 8px; background: #e94560; color: white; font-weight: bold; margin: 10px; }
        button:disabled { background: #555; }
        #result { font-size: 20px; font-weight: bold; margin-top: 20px; color: #4caf50; }
        @keyframes flash { 50% { opacity: 0.5; } }
    </style>
</head>
<body>

    <div class="container">
        <h1>Keno80-Fast</h1>
        <p>10 ቁጥሮችን ይምረጡ</p>
        
        <div id="grid" class="grid"></div>

        <button id="drawBtn" onclick="startDraw()" disabled>ጨዋታውን ጀምር</button>
        <button onclick="resetGame()" style="background:#555;">ድገም</button>

        <div id="result"></div>
        <div id="wallet" style="margin-top: 20px; font-size: 18px; color: #f8b400;">ሂሳብ፡ <span id="balance">0</span> ብር</div>
    </div>

    <script>
        const grid = document.getElementById('grid');
        const drawBtn = document.getElementById('drawBtn');
        const resultDiv = document.getElementById('result');
        const balanceSpan = document.getElementById('balance');
        let selectedNumbers = [];
        let balance = parseFloat(localStorage.getItem('kenoBalance')) || 0;

        balanceSpan.innerText = balance;

        // 80 ቁጥሮችን መፍጠር
        for (let i = 1; i <= 80; i++) {
            let div = document.createElement('div');
            div.className = 'number';
            div.innerText = i;
            div.onclick = () => selectNumber(i, div);
            grid.appendChild(div);
        }

        function selectNumber(num, el) {
            if (selectedNumbers.includes(num)) {
                selectedNumbers = selectedNumbers.filter(n => n !== num);
                el.classList.remove('selected');
            } else if (selectedNumbers.length < 10) {
                selectedNumbers.push(num);
                el.classList.add('selected');
            }
            drawBtn.disabled = selectedNumbers.length !== 10;
        }

        function startDraw() {
            resultDiv.innerText = "እጣ እየወጣ ነው...";
            drawBtn.disabled = true;
            
            let winningNumbers = [];
            while(winningNumbers.length < 20) {
                let r = Math.floor(Math.random() * 80) + 1;
                if(!winningNumbers.includes(r)) winningNumbers.push(r);
            }

            let matches = selectedNumbers.filter(n => winningNumbers.includes(n));
            
            // ውጤቱን አሳይ
            setTimeout(() => {
                winningNumbers.forEach(num => {
                    document.querySelectorAll('.number')[num-1].classList.add('won');
                });

                if (matches.length >= 5) {
                    let prize = matches.length * 10;
                    balance += prize;
                    resultDiv.innerText = `እንኳን ደስ አለዎት! ${matches.length} ቁጥሮች ገጥመዋል። ${prize} ብር አሸንፈዋል!`;
                } else {
                    resultDiv.innerText = `ይቅርታ! ${matches.length} ቁጥሮች ብቻ ናቸው የገጠሙት። እንደገና ይሞክሩ።`;
                }

                localStorage.setItem('kenoBalance', balance);
                balanceSpan.innerText = balance;
            }, 1000);
        }

        function resetGame() {
            location.reload();
        }
    </script>
</body>
</html>
