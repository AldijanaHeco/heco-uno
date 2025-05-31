<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>HECO UNO</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="container">
    <img src="logo.png" alt="HECO UNO logo" class="logo" />
    <h1>🎮 HECO UNO</h1>
    <div id="game">
      <p>Broj igrača:</p>
      <select id="playerCount">
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4" selected>4</option>
      </select>
      <button onclick="startGame()">Započni igru</button>
    </div>
    <div id="output"></div>
    <div class="rules">
      <h2>📜 Pravila igre</h2>
      <p>👉 Igrači se izmjenjuju i igraju kartu iste boje ili broja kao gornja.</p>
      <p>👉 Ako nemaju odgovarajuću kartu – preskaču red.</p>
      <p>🎯 Pobjednik je onaj ko prvi ostane bez karata!</p>
    </div>
  </div>
  <script src="script.js"></script>
</body>
</html>
body {
  font-family: 'Segoe UI', sans-serif;
  background-color: #111;
  color: #fff;
  text-align: center;
  margin: 0;
  padding: 0;
}

.container {
  max-width: 700px;
  margin: auto;
  padding: 20px;
}

.logo {
  width: 100px;
  margin: 20px auto;
  display: block;
}

button {
  padding: 10px 20px;
  background-color: gold;
  color: black;
  font-weight: bold;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  margin-top: 10px;
}

select {
  padding: 10px;
  border-radius: 6px;
  margin-bottom: 10px;
}

.rules {
  margin-top: 40px;
  background: #222;
  padding: 15px;
  border-radius: 8px;
}
const colors = ['Red', 'Green', 'Blue', 'Yellow'];
const values = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'];

function generateDeck() {
  let deck = [];
  colors.forEach(color => {
    values.forEach(value => {
      deck.push(`${color} ${value}`);
    });
  });
  return deck.sort(() => Math.random() - 0.5);
}

function getPlayableCard(cards, topCard) {
  for (let card of cards) {
    if (card.split(' ')[0] === topCard.split(' ')[0] || card.split(' ')[1] === topCard.split(' ')[1]) {
      return card;
    }
  }
  return null;
}

function startGame() {
  const playerCount = parseInt(document.getElementById("playerCount").value);
  let output = "";
  let deck = generateDeck();
  let players = [];

  for (let i = 0; i < playerCount; i++) {
    players.push(deck.splice(0, 5));
  }

  let topCard = deck.pop();
  output += `<p>🃏 Gornja karta: <strong>${topCard}</strong></p>`;

  let winner = null;
  let round = 1;

  while (!winner && round < 100) {
    output += `<h3>🔁 Krug ${round}</h3>`;
    for (let i = 0; i < playerCount; i++) {
      let name = i === 0 ? "TI" : `AI ${i}`;
      let card = getPlayableCard(players[i], topCard);
      output += `<p><strong>${name}</strong>: `;

      if (card) {
        players[i].splice(players[i].indexOf(card), 1);
        topCard = card;
        output += `✅ igra: ${card}</p>`;
      } else {
        output += `⛔ preskače.</p>`;
      }

      if (players[i].length === 0) {
        winner = name;
        break;
      }
    }
    round++;
  }

  output += `<h2>🏆 ${winner} POBJEĐUJE!</h2>`;
  document.getElementById("output").innerHTML = output;
}

