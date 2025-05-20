# Tic-Tac-Toe
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Tic Tac Toe - Dark X & O</title>
  <style>
    * {
      box-sizing: border-box;
      font-family: 'Segoe UI', sans-serif;
      margin: 0;
      padding: 0;
    }

    body {
      height: 100vh;
      background: linear-gradient(135deg, #6a1b9a, #8e24aa, #ab47bc);
      background-size: 400% 400%;
      animation: gradientAnimation 5s ease infinite;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    @keyframes gradientAnimation {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    .game-container {
      background: rgba(255, 255, 255, 0.1);
      backdrop-filter: blur(12px);
      border-radius: 25px;
      padding: 35px;
      text-align: center;
      box-shadow: 0 15px 40px rgba(0, 0, 0, 0.6);
    }

    h1 {
      color: #fff;
      margin-bottom: 20px;
      text-shadow: 2px 2px 6px rgba(0, 0, 0, 0.5);
    }

    .status {
      color: #eee;
      margin: 15px 0;
      font-size: 1.3rem;
    }

    .board {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      grid-gap: 10px;
      margin: 20px auto;
    }

    .cell {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 15px;
      width: 100px;
      height: 100px;
      font-size: 2.5rem;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      transition: transform 0.2s ease, background 0.3s ease;
      box-shadow: inset 0 0 15px rgba(255, 255, 255, 0.3);
    }

    .cell:hover {
      background: rgba(255, 255, 255, 0.2);
      transform: scale(1.05);
    }

    .cell.X {
      color: #003366; /* Dark Blue */
    }

    .cell.O {
      color: #8B0000; /* Dark Red */
    }

    .cell.win {
      background-color: #00e676 !important;
      color: #000;
      font-weight: bold;
    }

    button {
      margin-top: 20px;
      padding: 12px 30px;
      font-size: 1rem;
      border: none;
      border-radius: 8px;
      background: linear-gradient(45deg, #ff77ff, #ff4081);
      color: #fff;
      font-weight: bold;
      cursor: pointer;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
      transition: transform 0.3s, background 0.3s;
    }

    button:hover {
      transform: scale(1.05);
      background: linear-gradient(45deg, #ff4081, #ff77ff);
    }
  </style>
</head>
<body>
  <div class="game-container">
    <h1>Tic Tac Toe</h1>
    <div class="status" id="statusText">Player X's Turn</div>
    <div class="board" id="board"></div>
    <button onclick="resetGame()">Restart</button>
  </div>

  <script>
    const board = document.getElementById("board");
    const statusText = document.getElementById("statusText");
    let currentPlayer = "X";
    let gameActive = true;
    let gameState = Array(9).fill("");

    const winPatterns = [
      [0,1,2], [3,4,5], [6,7,8],
      [0,3,6], [1,4,7], [2,5,8],
      [0,4,8], [2,4,6]
    ];

    function createBoard() {
      board.innerHTML = "";
      for (let i = 0; i < 9; i++) {
        const cell = document.createElement("div");
        cell.classList.add("cell");
        cell.dataset.index = i;
        cell.addEventListener("click", handleClick);
        board.appendChild(cell);
      }
    }

    function handleClick(e) {
      const index = e.target.dataset.index;
      if (gameState[index] || !gameActive) return;

      gameState[index] = currentPlayer;
      e.target.textContent = currentPlayer;
      e.target.classList.add(currentPlayer);

      if (checkWinner()) {
        statusText.textContent = `🎉 Player ${currentPlayer} Wins!`;
        gameActive = false;
        return;
      }

      if (!gameState.includes("")) {
        statusText.textContent = "🤝 It's a Draw!";
        gameActive = false;
        return;
      }

      currentPlayer = currentPlayer === "X" ? "O" : "X";
      statusText.textContent = `Player ${currentPlayer}'s Turn`;
    }

    function checkWinner() {
      for (const pattern of winPatterns) {
        const [a, b, c] = pattern;
        if (
          gameState[a] &&
          gameState[a] === gameState[b] &&
          gameState[a] === gameState[c]
        ) {
          document.querySelectorAll(".cell")[a].classList.add("win");
          document.querySelectorAll(".cell")[b].classList.add("win");
          document.querySelectorAll(".cell")[c].classList.add("win");
          return true;
        }
      }
      return false;
    }

    function resetGame() {
      currentPlayer = "X";
      gameActive = true;
      gameState = Array(9).fill("");
      statusText.textContent = `Player ${currentPlayer}'s Turn`;
      createBoard();
    }

    createBoard();
  </script>
</body>
</html>

