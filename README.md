<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>لعبة إكس أو - نيون</title>
    <style>
        * {
            box-sizing: border-box;
            user-select: none;
        }
        body {
            margin: 0;
            background-color: #0b0f19;
            color: #fff;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
        }
        h1 {
            color: #00f3ff;
            text-shadow: 0 0 10px rgba(0, 243, 255, 0.7);
            margin-bottom: 10px;
        }
        #status {
            font-size: 1.2rem;
            margin-bottom: 20px;
            color: #ff007f;
            text-shadow: 0 0 8px rgba(255, 0, 127, 0.7);
        }
        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 10px;
            background: #111a2e;
            padding: 10px;
            border-radius: 15px;
            box-shadow: 0 0 20px rgba(0, 243, 255, 0.2);
        }
        .cell {
            background-color: #1a233a;
            border: 2px solid #00f3ff;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: inset 0 0 10px rgba(0, 243, 255, 0.1);
        }
        .cell:hover {
            background-color: #243050;
            box-shadow: 0 0 15px rgba(0, 243, 255, 0.5);
        }
        .cell.x {
            color: #00f3ff;
            text-shadow: 0 0 10px #00f3ff;
        }
        .cell.o {
            color: #ff007f;
            text-shadow: 0 0 10px #ff007f;
        }
        #reset-btn {
            margin-top: 25px;
            background: transparent;
            border: 2px solid #00f3ff;
            color: #00f3ff;
            padding: 10px 25px;
            font-size: 1rem;
            font-weight: bold;
            border-radius: 8px;
            cursor: pointer;
            box-shadow: 0 0 10px rgba(0, 243, 255, 0.3);
            transition: 0.2s;
        }
        #reset-btn:hover {
            background: #00f3ff;
            color: #0b0f19;
            box-shadow: 0 0 20px #00f3ff;
        }
    </style>
</head>
<body>

    <h1>لعبة X-O النيونية</h1>
    <div id="status">دور اللاعب: X</div>

    <div class="board" id="board">
        <div class="cell" data-index="0"></div>
        <div class="cell" data-index="1"></div>
        <div class="cell" data-index="2"></div>
        <div class="cell" data-index="3"></div>
        <div class="cell" data-index="4"></div>
        <div class="cell" data-index="5"></div>
        <div class="cell" data-index="6"></div>
        <div class="cell" data-index="7"></div>
        <div class="cell" data-index="8"></div>
    </div>

    <button id="reset-btn" onclick="resetGame()">إعادة اللعبة</button>

    <script>
        const board = document.getElementById('board');
        const statusDisplay = document.getElementById('status');
        let cells = ['', '', '', '', '', '', '', '', ''];
        let currentPlayer = 'X';
        let gameActive = true;

        const winningConditions = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8], // صفوف
            [0, 3, 6], [1, 4, 7], [2, 5, 8], // أعمدة
            [0, 4, 8], [2, 4, 6]            // أقطار
        ];

        function handleCellClick(e) {
            const clickedCell = e.target;
            const clickedCellIndex = parseInt(clickedCell.getAttribute('data-index'));

            if (cells[clickedCellIndex] !== '' || !gameActive) return;

            cells[clickedCellIndex] = currentPlayer;
            clickedCell.innerText = currentPlayer;
            clickedCell.classList.add(currentPlayer.toLowerCase());

            checkResultValidation();
        }

        function checkResultValidation() {
            let roundWon = false;
            for (let i = 0; i < winningConditions.length; i++) {
                const [a, b, c] = winningConditions[i];
                if (cells[a] && cells[a] === cells[b] && cells[a] === cells[c]) {
                    roundWon = true;
                    break;
                }
            }

            if (roundWon) {
                statusDisplay.innerText = `اللاعب (${currentPlayer}) فاز! 🎉`;
                gameActive = false;
                return;
            }

            if (!cells.includes('')) {
                statusDisplay.innerText = `تعادل! 🤝`;
                gameActive = false;
                return;
            }

            currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
            statusDisplay.innerText = `دور اللاعب: ${currentPlayer}`;
        }

        function resetGame() {
            cells = ['', '', '', '', '', '', '', '', ''];
            gameActive = true;
            currentPlayer = 'X';
            statusDisplay.innerText = `دور اللاعب: ${currentPlayer}`;
            document.querySelectorAll('.cell').forEach(cell => {
                cell.innerText = '';
                cell.classList.remove('x', 'o');
            });
        }

        document.querySelectorAll('.cell').forEach(cell => {
            cell.addEventListener('click', handleCellClick);
        });
    </script>
</body>
</html>
