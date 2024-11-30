# Roland-Garros
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Roland Garros Quiz</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header>
    <h1>Roland Garros Quiz</h1>
  </header>
  <main>
    <!-- Name Input -->
    <div id="start-container">
      <h2>Enter Your Name to Start the Quiz</h2>
      <input type="text" id="player-name" placeholder="Your Name">
      <button id="start-btn">Start Quiz</button>
    </div>

    <!-- Quiz -->
    <div id="quiz-container" style="display: none;">
      <div id="question"></div>
      <div id="options"></div>
      <button id="next-btn">Next</button>
    </div>

    <!-- Results -->
    <div id="result-container" style="display: none;">
      <h2>Quiz Completed!</h2>
      <p id="score"></p>
      <button id="restart-btn">Restart Quiz</button>
      <h3>Leaderboard</h3>
      <ul id="leaderboard"></ul>
    </div>
  </main>
  <script src="script.js"></script>
</body>
</html>
body {
  font-family: Arial, sans-serif;
  text-align: center;
  background-color: #f7f7f7;
}

header {
  background-color: #d35400;
  color: white;
  padding: 1rem;
}

#quiz-container, #result-container, #start-container {
  margin: 2rem auto;
  padding: 1rem;
  max-width: 600px;
  background: white;
  border-radius: 5px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

button {
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  background-color: #d35400;
  color: white;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

button:hover {
  background-color: #e67e22;
}

ul {
  list-style: none;
  padding: 0;
}
const quizData = [
  { question: "Who has the most Roland Garros titles in men's singles?", options: ["Roger Federer", "Rafael Nadal", "Novak Djokovic", "Björn Borg"], answer: "Rafael Nadal" },
  { question: "In which year was Roland Garros first held?", options: ["1891", "1901", "1925", "1930"], answer: "1891" },
  { question: "Which surface is Roland Garros played on?", options: ["Grass", "Clay", "Hardcourt", "Carpet"], answer: "Clay" },
  { question: "Who won the women's singles title in 2023?", options: ["Iga Swiatek", "Ons Jabeur", "Coco Gauff", "Aryna Sabalenka"], answer: "Iga Swiatek" },
  { question: "Which country hosts Roland Garros?", options: ["France", "Spain", "USA", "Australia"], answer: "France" },
  { question: "Who is the youngest winner of Roland Garros?", options: ["Michael Chang", "Björn Borg", "Rafael Nadal", "Mats Wilander"], answer: "Michael Chang" },
  { question: "What is the nickname for Roland Garros?", options: ["The Clay Masters", "French Open", "Paris Slam", "Red Open"], answer: "French Open" },
  { question: "How many Grand Slam titles did Steffi Graf win?", options: ["22", "18", "24", "20"], answer: "22" },
  { question: "Who has the most titles in women's singles at Roland Garros?", options: ["Chris Evert", "Serena Williams", "Steffi Graf", "Martina Navratilova"], answer: "Chris Evert" },
  { question: "Which player has the fastest serve recorded at Roland Garros?", options: ["Nick Kyrgios", "Andy Roddick", "Ivo Karlovic", "John Isner"], answer: "John Isner" },
  { question: "Which court is the main stadium at Roland Garros?", options: ["Court Philippe-Chatrier", "Court Suzanne-Lenglen", "Court 1", "Court Central"], answer: "Court Philippe-Chatrier" },
  { question: "What is the capacity of Court Philippe-Chatrier?", options: ["15,000", "10,000", "12,000", "14,000"], answer: "15,000" },
  { question: "Which year did Rafael Nadal win his first Roland Garros?", options: ["2005", "2006", "2004", "2007"], answer: "2005" },
  { question: "Who is the tournament named after?", options: ["A pilot", "A tennis player", "A politician", "A scientist"], answer: "A pilot" },
  { question: "Which player has won the most consecutive matches at Roland Garros?", options: ["Rafael Nadal", "Björn Borg", "Novak Djokovic", "Ivan Lendl"], answer: "Rafael Nadal" }
];

let currentQuestion = 0;
let score = 0;
let playerName = "";

const startContainer = document.getElementById("start-container");
const quizContainer = document.getElementById("quiz-container");
const resultContainer = document.getElementById("result-container");

const playerNameInput = document.getElementById("player-name");
const startBtn = document.getElementById("start-btn");

const questionEl = document.getElementById("question");
const optionsEl = document.getElementById("options");
const nextBtn = document.getElementById("next-btn");

const scoreEl = document.getElementById("score");
const restartBtn = document.getElementById("restart-btn");
const leaderboardEl = document.getElementById("leaderboard");

startBtn.onclick = () => {
  playerName = playerNameInput.value.trim();
  if (!playerName) {
    alert("Please enter your name!");
    return;
  }
  startContainer.style.display = "none";
  quizContainer.style.display = "block";
  loadQuestion();
};

function loadQuestion() {
  const question = quizData[currentQuestion];
  questionEl.textContent = question.question;
  optionsEl.innerHTML = "";
  question.options.forEach((option) => {
    const button = document.createElement("button");
    button.textContent = option;
    button.onclick = () => checkAnswer(option);
    optionsEl.appendChild(button);
  });
}

function checkAnswer(selectedOption) {
  const correctAnswer = quizData[currentQuestion].answer;
  if (selectedOption === correctAnswer) {
    score++;
  }
  currentQuestion++;
  if (currentQuestion < quizData.length) {
    loadQuestion();
  } else {
    showResults();
  }
}

function showResults() {
  quizContainer.style.display = "none";
  resultContainer.style.display = "block";
  scoreEl.textContent = `Your score: ${score}/${quizData.length}`;
  updateLeaderboard();
  displayLeaderboard();
}

function updateLeaderboard() {
  const leaderboard = JSON.parse(localStorage.getItem("leaderboard")) || [];
  leaderboard.push({ name: playerName, score });
  leaderboard.sort((a, b) => b.score - a.score);
  localStorage.setItem("leaderboard", JSON.stringify(leaderboard.slice(0, 5))); // Top 5 scores
}

function displayLeaderboard() {
  const leaderboard = JSON.parse(localStorage.getItem("leaderboard")) || [];
  leaderboardEl.innerHTML = leaderboard
    .map((entry) => `<li>${entry.name}: ${entry.score}</li>`)
    .join("");
}

restartBtn.onclick = () => {
  currentQuestion = 0;
  score = 0;
  resultContainer.style.display = "none";
  startContainer.style.display = "block";
  playerNameInput.value = "";
};
