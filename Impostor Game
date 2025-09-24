<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Juego del Impostor - Fútbol</title>
  <style>
    body { 
      font-family: Arial, sans-serif; 
      text-align: center; 
      background: url('https://i.ibb.co/5F7LRwV/grass-texture.jpg') no-repeat center center fixed;
      background-size: cover;
      color: #fff; 
    }
    h1 { 
      color: #ffcc00; 
      text-shadow: 2px 2px 4px #000;
    }
    .container { 
      max-width: 420px; 
      margin: auto; 
      background: rgba(0,0,0,0.6); 
      padding: 20px; 
      border-radius: 12px;
    }
    input, button, textarea { 
      margin: 5px; padding: 8px; width: 90%; border-radius: 6px;
    }
    button { 
      background: #28a745; 
      border: none; 
      cursor: pointer; 
      font-weight: bold; 
      color: white;
    }
    button:hover { background: #218838; }
    .hidden { display: none; }
    ul { list-style: none; padding: 0; }
    #wordBankSection { margin-bottom: 15px; }
  </style>
</head>
<body>
  <div class="container">
    <h1>⚽ El Juego del Impostor ⚽</h1>

    <!-- Banco de palabras -->
    <div id="wordBankSection">
      <button onclick="toggleWordBank()">Mostrar / Ocultar Banco</button>
      <div id="wordBankContainer" class="hidden">
        <h3>Banco de palabras</h3>
        <textarea id="wordBank" rows="4" placeholder="Escribe palabras, una por línea"></textarea><br>
        <button onclick="saveWordBank()">Guardar banco</button>
      </div>
    </div>

    <!-- Nombres de jugadores -->
    <h3>Jugadores</h3>
    <input type="text" id="playerName" placeholder="Nombre del jugador">
    <button onclick="addPlayer()">Agregar jugador</button>
    <ul id="playerList"></ul>
    <button onclick="clearPlayers()">Borrar jugadores</button>

    <br><br>
    <button onclick="startGame()">Jugar</button>

    <!-- Mostrar palabras -->
    <div id="gameSection" class="hidden">
      <h2 id="currentPlayer"></h2>
      <button id="showWordBtn" onclick="showWord()">Mostrar palabra</button>
      <p id="wordDisplay" class="hidden"></p>
      <button id="nextBtn" class="hidden" onclick="nextPlayer()">Siguiente</button>
    </div>

    <!-- Botón para nuevo juego -->
    <div id="newGameSection" class="hidden">
      <button onclick="newGame()">Juego Nuevo</button>
    </div>
  </div>

  <script>
    let players = [];
    let assignments = {};
    let currentIndex = 0;

    // === CARGAR DATOS DE LOCALSTORAGE ===
    window.onload = function() {
      const savedPlayers = JSON.parse(localStorage.getItem("players"));
      const savedWordBank = localStorage.getItem("wordBank");
      if (savedPlayers) {
        players = savedPlayers;
        renderPlayerList();
      }
      if (savedWordBank) {
        document.getElementById("wordBank").value = savedWordBank;
      }
    };

    // === BANCO DE PALABRAS ===
    function toggleWordBank() {
      const container = document.getElementById("wordBankContainer");
      container.classList.toggle("hidden");
    }

    function saveWordBank() {
      const wordBank = document.getElementById("wordBank").value.trim();
      localStorage.setItem("wordBank", wordBank);
      alert("Banco de palabras guardado ✅");
    }

    // === MANEJO DE JUGADORES ===
    function addPlayer() {
      const name = document.getElementById("playerName").value.trim();
      if (name && !players.includes(name)) {
        players.push(name);
        localStorage.setItem("players", JSON.stringify(players));
        renderPlayerList();
        document.getElementById("playerName").value = "";
      }
    }

    function renderPlayerList() {
      const list = document.getElementById("playerList");
      list.innerHTML = "";
      players.forEach(p => {
        const li = document.createElement("li");
        li.textContent = p;
        list.appendChild(li);
      });
    }

    function clearPlayers() {
      if (confirm("¿Seguro que quieres borrar todos los jugadores?")) {
        players = [];
        localStorage.removeItem("players");
        renderPlayerList();
      }
    }

    // === INICIAR EL JUEGO ===
    function startGame() {
      const wordBank = document.getElementById("wordBank").value.trim().split("\n").filter(w => w);
      if (players.length < 3) {
        alert("Necesitas al menos 3 jugadores.");
        return;
      }
      if (wordBank.length < 1) {
        alert("Agrega al menos una palabra al banco.");
        return;
      }

      // Elegir palabra y asignar impostor
      const chosenWord = wordBank[Math.floor(Math.random() * wordBank.length)];
      const impostorIndex = Math.floor(Math.random() * players.length);

      assignments = {};
      players.forEach((p, i) => {
        assignments[p] = (i === impostorIndex) ? "IMPOSTOR" : chosenWord;
      });

      currentIndex = 0;
      document.getElementById("gameSection").classList.remove("hidden");
      document.getElementById("newGameSection").classList.add("hidden");
      updatePlayer();
    }

    // === MOSTRAR TURNO Y PALABRAS ===
    function updatePlayer() {
      const player = players[currentIndex];
      document.getElementById("currentPlayer").textContent = "Turno de: " + player;
      document.getElementById("wordDisplay").classList.add("hidden");
      document.getElementById("nextBtn").classList.add("hidden");
      document.getElementById("showWordBtn").classList.remove("hidden");
      document.getElementById("wordDisplay").textContent = "";
    }

    function showWord() {
      const player = players[currentIndex];
      document.getElementById("wordDisplay").textContent = assignments[player];
      document.getElementById("wordDisplay").classList.remove("hidden");
      document.getElementById("showWordBtn").classList.add("hidden");
      document.getElementById("nextBtn").classList.remove("hidden");
    }

    function nextPlayer() {
      currentIndex++;
      if (currentIndex < players.length) {
        updatePlayer();
      } else {
        alert("Todos vieron su palabra. ¡Que comience el partido!");
        document.getElementById("gameSection").classList.add("hidden");
        document.getElementById("newGameSection").classList.remove("hidden");
      }
    }

    // === NUEVO JUEGO ===
    function newGame() {
      startGame();
    }
  </script>
</body>
</html>
