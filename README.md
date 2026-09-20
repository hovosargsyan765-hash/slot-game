# slot-game<!DOCTYPE html>
<html lang="hy">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Shining Crown Mini App</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
        }
        body {
            background: radial-gradient(circle, #300 0%, #100 70%, #000 100%);
            color: #fff;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: space-between;
            height: 100vh;
            overflow: hidden;
            padding: 10px;
        }
        .header {
            text-align: center;
            width: 100%;
        }
        .title {
            font-size: 24px;
            font-weight: bold;
            background: linear-gradient(to bottom, #ffd700, #ff8c00);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 2px 4px rgba(0,0,0,0.8);
            letter-spacing: 2px;
            margin-bottom: 5px;
        }
        .stats-bar {
            display: flex;
            justify-content: space-around;
            width: 100%;
            background: rgba(0, 0, 0, 0.6);
            border: 2px solid #ffd700;
            border-radius: 10px;
            padding: 8px;
            margin-bottom: 10px;
        }
        .stat-box {
            text-align: center;
        }
        .stat-label {
            font-size: 10px;
            color: #aaa;
            text-transform: uppercase;
        }
        .stat-value {
            font-size: 16px;
            font-weight: bold;
            color: #ffd700;
        }
        .slot-container {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 8px;
            background: #111;
            padding: 12px;
            border: 4px solid #ffd700;
            border-radius: 15px;
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.5), inset 0 0 15px rgba(0,0,0,0.8);
            width: 100%;
            max-width: 360px;
        }
        .slot-cell {
            background: linear-gradient(135deg, #fff, #ddd);
            border-radius: 8px;
            height: 85px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 45px;
            box-shadow: inset 0 4px 6px rgba(0,0,0,0.3);
            border: 2px solid #b8860b;
            transition: transform 0.1s ease;
        }
        .slot-cell.win-glow {
            animation: pulseWin 0.5s infinite alternate;
            border-color: #00ff00;
        }
        @keyframes pulseWin {
            0% { transform: scale(1); box-shadow: 0 0 10px #0f0; }
            100% { transform: scale(1.05); box-shadow: 0 0 20px #0f0; }
        }
        .controls {
            width: 100%;
            max-width: 360px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        .bet-selector {
            display: flex;
            justify-content: space-between;
            gap: 5px;
        }
        .bet-btn {
            flex: 1;
            background: linear-gradient(to bottom, #444, #222);
            border: 2px solid #777;
            color: #fff;
            padding: 8px 0;
            font-size: 12px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
        }
        .bet-btn.active {
            background: linear-gradient(to bottom, #00aa00, #006600);
            border-color: #00ff00;
            color: #fff;
            box-shadow: 0 0 10px rgba(0,255,0,0.5);
        }
        .action-buttons {
            display: flex;
            gap: 10px;
        }
        .spin-btn {
            flex: 2;
            background: linear-gradient(to bottom, #ff4500, #b22222);
            border: 3px solid #ffd700;
            color: #fff;
            font-size: 20px;
            font-weight: bold;
            padding: 12px;
            border-radius: 12px;
            cursor: pointer;
            text-transform: uppercase;
            box-shadow: 0 4px 10px rgba(0,0,0,0.5);
        }
        .spin-btn:active {
            transform: scale(0.96);
        }
        .buy-stars-btn {
            flex: 1;
            background: linear-gradient(to bottom, #0088cc, #004488);
            border: 3px solid #00ffff;
            color: #fff;
            font-size: 14px;
            font-weight: bold;
            padding: 12px 5px;
            border-radius: 12px;
            cursor: pointer;
            text-align: center;
            box-shadow: 0 4px 10px rgba(0,0,0,0.5);
        }
        .message-box {
            font-size: 14px;
            font-weight: bold;
            color: #ffcc00;
            height: 20px;
            text-align: center;
        }
    </style>
</head>
<body>

    <div class="header">
        <div class="title">SHINING CROWN</div>
        <div class="stats-bar">
            <div class="stat-box">
                <div class="stat-label">Balance</div>
                <div class="stat-value" id="balanceVal">100 AMD</div>
            </div>
            <div class="stat-box">
                <div class="stat-label">Bet</div>
                <div class="stat-value" id="betVal">10 AMD</div>
            </div>
            <div class="stat-box">
                <div class="stat-label">Win</div>
                <div class="stat-value" id="winVal">0 AMD</div>
            </div>
        </div>
    </div>

    <div class="slot-container" id="slotContainer">
        <div class="slot-cell" id="cell-0">🍒</div>
        <div class="slot-cell" id="cell-1">🍋</div>
        <div class="slot-cell" id="cell-2">🍇</div>
        <div class="slot-cell" id="cell-3">🍊</div>
        <div class="slot-cell" id="cell-4">👑</div>
        <div class="slot-cell" id="cell-5">🍉</div>
        <div class="slot-cell" id="cell-6">🔔</div>
        <div class="slot-cell" id="cell-7">🍇</div>
        <div class="slot-cell" id="cell-8">🍒</div>
    </div>

    <div class="message-box" id="msgBox">ՊՏՏԵՔ ԵՎ ՀԱՇՏԵՔ ՇԱՀՈՒՄՆԵՐ</div>

    <div class="controls">
        <div class="bet-selector">
            <button class="bet-btn active" onclick="setBet(10)">10</button>
            <button class="bet-btn" onclick="setBet(20)">20</button>
            <button class="bet-btn" onclick="setBet(30)">30</button>
            <button class="bet-btn" onclick="setBet(40)">40</button>
            <button class="bet-btn" onclick="setBet(50)">50</button>
        </div>
        <div class="action-buttons">
            <button class="spin-btn" id="spinBtn" onclick="spin()">ՊՏՏԵԼ</button>
            <button class="buy-stars-btn" onclick="buyStars()">⭐ Գնել 100 Կրեդիտ (Stars)</button>
        </div>
    </div>

    <script>
        const symbols = ['🍒', '🍋', '🍇', '🍊', '🍉', '🔔', '👑'];
        let balance = 100;
        let currentBet = 10;
        let isSpinning = false;

        const tg = window.Telegram.WebApp;
        tg.expand();

        function setBet(amount) {
            if (isSpinning) return;
            currentBet = amount;
            document.getElementById('betVal').innerText = currentBet + ' AMD';
            document.querySelectorAll('.bet-btn').forEach(btn => {
                btn.classList.remove('active');
                if (btn.innerText == amount) btn.classList.add('active');
            });
        }

        function playSound(type) {
            try {
                const ctx = new (window.AudioContext || window.webkitAudioContext)();
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                osc.connect(gain);
                gain.connect(ctx.destination);
                
                if (type === 'spin') {
                    osc.frequency.setValueAtTime(300, ctx.currentTime);
                    osc.frequency.exponentialRampToValueAtTime(600, ctx.currentTime + 0.1);
                    gain.gain.setValueAtTime(0.1, ctx.currentTime);
                    gain.gain.linearRampToValueAtTime(0.01, ctx.currentTime + 0.1);
                    osc.start();
                    osc.stop(ctx.currentTime + 0.1);
                } else if (type === 'win') {
                    osc.frequency.setValueAtTime(500, ctx.currentTime);
                    osc.frequency.setValueAtTime(800, ctx.currentTime + 0.15);
                    gain.gain.setValueAtTime(0.2, ctx.currentTime);
                    gain.gain.linearRampToValueAtTime(0.01, ctx.currentTime + 0.4);
                    osc.start();
                    osc.stop(ctx.currentTime + 0.4);
                }
            } catch(e) {}
        }

        function spin() {
            if (isSpinning) return;
            if (balance < currentBet) {
                document.getElementById('msgBox').innerText = "ԲԱԼԱՆՍԸ ՉԻ ԲԱՎԱՐԱՐՈՒՄ!";
                return;
            }

            balance -= currentBet;
            updateUI();
            isSpinning = true;
            document.getElementById('msgBox').innerText = "ՊՏՏՎՈՒՄ Է...";
            document.getElementById('spinBtn').disabled = true;

            // Clear previous wins
            for(let i=0; i<9; i++) {
                document.getElementById(`cell-${i}`).classList.remove('win-glow');
            }

            let interval = setInterval(() => {
                playSound('spin');
                for (let i = 0; i < 9; i++) {
                    const randomSymbol = symbols[Math.floor(Math.random() * symbols.length)];
                    document.getElementById(`cell-${i}`).innerText = randomSymbol;
                }
            }, 100);

            setTimeout(() => {
                clearInterval(interval);
                
                // Final symbols generation
                let finalSymbols = [];
                for (let i = 0; i < 9; i++) {
                    const sym = symbols[Math.floor(Math.random() * symbols.length)];
                    finalSymbols.push(sym);
                    document.getElementById(`cell-${i}`).innerText = sym;
                }

                checkWin(finalSymbols);
                isSpinning = false;
                document.getElementById('spinBtn').disabled = false;
            }, 1200);
        }

        function checkWin(arr) {
            let winAmount = 0;
            let winningCells = [];

            // Check middle horizontal line (indices 3, 4, 5)
            if (arr[3] === arr[4] && arr[4] === arr[5]) {
                winAmount += currentBet * 5;
                winningCells.push(3, 4, 5);
            }
            // Check top horizontal line (0, 1, 2)
            if (arr[0] === arr[1] && arr[1] === arr[2]) {
                winAmount += currentBet * 3;
                winningCells.push(0, 1, 2);
            }
            // Check bottom horizontal line (6, 7, 8)
            if (arr[6] === arr[7] && arr[7] === arr[8]) {
                winAmount += currentBet * 3;
                winningCells.push(6, 7, 8);
            }

            if (winAmount > 0) {
                balance += winAmount;
                playSound('win');
                document.getElementById('winVal').innerText = winAmount + ' AMD';
                document.getElementById('msgBox').innerText = `ՇՆՈՐՀԱՎՈՐՈՒՄ ԵՆՔ! ՇԱՀԵՑԻՔ ${winAmount} AMD`;
                winningCells.forEach(idx => {
                    document.getElementById(`cell-${idx}`).classList.add('win-glow');
                });
            } else {
                document.getElementById('winVal').innerText = '0 AMD';
                document.getElementById('msgBox').innerText = 'ՓՈՐՁԵՔ ՆՈՐԻՑ';
            }
            updateUI();
        }

        function updateUI() {
            document.getElementById('balanceVal').innerText = balance + ' AMD';
        }

        // Telegram Stars payment simulation / integration
        function buyStars() {
            // Այստեղ աշխատում է Telegram Stars ինտեգրումը
            if (window.Telegram && window.Telegram.WebApp) {
                // Եթե ուզում եք իրական Stars ինվոյս ուղարկել բոտի միջոցով կամ բացել պատուհան
                tg.showConfirm("Ցանկանո՞ւմ եք համալրել հաշվեկշիռը 100 կրեդիտով Telegram Stars-ի միջոցով:", (confirmed) => {
                    if (confirmed) {
                        balance += 100;
                        updateUI();
                        document.getElementById('msgBox').innerText = "ՀԱՇՎԵԿՇԻՌԸ ՀԱՋՈՂՈՒԹՅԱՄԲ ԼՑԿԱՎՈՐՎԵՑ!";
                    }
                });
            } else {
                // Բրաուզերում փորձարկելու համար
                balance += 100;
                updateUI();
                document.getElementById('msgBox').innerText = "+100 ԿՐԵԴԻՏ ԱՎԵԼԱՑԱՎ!";
            }
        }
    </script>
</body>
</html>
