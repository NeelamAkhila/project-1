# project-1

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Tic Tac Toe</title>
  <link rel="stylesheet" href="pro1.css" />
</head>
<body>
  <h1>Tic Tac Toe</h1>
  <div class="board" id="board"></div>
  <p id="status">Player X's turn</p>
  <button onclick="resetGame()">Restart</button>

  <script src="pro.js"></script>
</body>
</html>

css (pro1.css)

body {
  font-family: Arial, sans-serif;
  text-align: center;
  padding: 30px;
  background: yellow;
}

h1 {
  margin-bottom: 20px;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-gap: 5px;
  justify-content: center;
  margin: 0 auto 20px;
}

.cell {
  width: 100px;
  height: 100px;
  background: white;
  border: 2px solid #333;
  font-size: 2rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.cell:hover {
  background-color: red;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

java script (pro.js)

const board = document.getElementById('board');
const statusText = document.getElementById('status');
let currentPlayer = 'X';
let gameActive = true;
let cells = Array(9).fill("");

const winConditions = [
  [0,1,2], [3,4,5], [6,7,8], // rows
  [0,3,6], [1,4,7], [2,5,8], // columns
  [0,4,8], [2,4,6]           // diagonals
];

function createBoard() {
  board.innerHTML = '';
  cells.forEach((cell, index) => {
    const div = document.createElement('div');
    div.classList.add('cell');
    div.setAttribute('data-index', index);
    div.addEventListener('click', handleClick);
    div.textContent = cell;
    board.appendChild(div);
  });
}

function handleClick(e) {
  const index = e.target.getAttribute('data-index');
  if (!gameActive || cells[index] !== '') return;

  cells[index] = currentPlayer;
  e.target.textContent = currentPlayer;

  if (checkWin()) {
    statusText.textContent = `Player ${currentPlayer} wins!`;
    gameActive = false;
  } else if (cells.every(cell => cell !== '')) {
    statusText.textContent = "It's a draw!";
    gameActive = false;
  } else {
    currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
    statusText.textContent = `Player ${currentPlayer}'s turn`;
  }
}

function checkWin() {
  return winConditions.some(condition => {
    const [a, b, c] = condition;
    return cells[a] && cells[a] === cells[b] && cells[a] === cells[c];
  });
}

function resetGame() {
  cells = Array(9).fill("");
  currentPlayer = 'X';
  gameActive = true;
  statusText.textContent = "Player X's turn";
  createBoard();
}

createBoard();

