<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Krusty Krab Web Game</title>
  <style>
    body {
      background: linear-gradient(to bottom, #87ceeb 0%, #fefbd8 100%);
      background-attachment: fixed;
      font-family: 'Comic Sans MS', cursive, sans-serif;
      margin: 0;
      padding: 0;
    }
    header {
      text-align: center;
      padding: 20px;
      background-color: rgba(255, 255, 255, 0.8);
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    h1 {
      color: #e07b00;
    }
    .container {
      padding: 20px;
    }
    .button {
      background-color: #ffcc00;
      border: none;
      padding: 10px 20px;
      margin: 10px;
      font-size: 16px;
      cursor: pointer;
      border-radius: 10px;
      transition: 0.3s;
    }
    .button:hover {
      background-color: #ff9900;
    }
    .game-section {
      margin-top: 30px;
      background-color: rgba(255,255,255,0.9);
      padding: 20px;
      border-radius: 15px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.2);
    }
  </style>
</head>
<body>
  <header>
    <h1>Welcome to the Krusty Krab Game!</h1>
  </header>
  <div class="container">
    <button class="button" onclick="startBurgerGame()">Start Burger Mini-Game</button>
    <button class="button" onclick="startPlanktonGame()">Start Plankton Defense</button>
  
    <div class="game-section" id="gameArea">
      <!-- Game content will load here -->
    </div>
  </div>
  
  <audio id="bgm" src="bgm.mp3" autoplay loop></audio>
  <audio id="effect" src="effect.mp3"></audio>

  <script>
    function playEffect() {
      document.getElementById('effect').play();
    }

    function startBurgerGame() {
      playEffect();
      document.getElementById('gameArea').innerHTML = '<h2>Burger Game Started!</h2>';
      // 여기에 버거 게임 로직 삽입
    }

    function startPlanktonGame() {
      playEffect();
      document.getElementById('gameArea').innerHTML = '<h2>Defend Against Plankton!</h2>';
      // 여기에 플랑크톤 침략 게임 로직 삽입
    }
  </script>
</body>
</html>
