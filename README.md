<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>집게리아 게임</title>
  <style>
    body {
      margin: 0;
      font-family: 'Arial', sans-serif;
      background: url('krustykrab_bg.jpg') no-repeat center center fixed;
      background-size: cover;
      overflow-x: hidden;
    }
    header {
      background-color: rgba(0, 0, 0, 0.7);
      color: #fff;
      padding: 1rem;
      text-align: center;
      font-size: 2rem;
    }
    nav {
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      background-color: rgba(255, 255, 255, 0.8);
      padding: 1rem;
    }
    nav button {
      margin: 0.5rem;
      padding: 0.75rem 1.5rem;
      font-size: 1rem;
      border: none;
      border-radius: 10px;
      background-color: #ffcc00;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }
    nav button:hover {
      background-color: #ffaa00;
    }
    #mainScene {
      position: relative;
      width: 100vw;
      height: 70vh;
    }
    .character {
      position: absolute;
      transition: all 0.3s ease;
    }
    #squidward {
      left: 20%;
      bottom: 0;
      width: 200px;
    }
    #spongebob {
      right: 20%;
      bottom: 0;
      width: 200px;
    }
    .guest {
      position: absolute;
      bottom: 0;
      left: 5%;
      width: 150px;
      transition: all 1s ease-in-out;
    }
    .coin-display {
      position: absolute;
      top: 10px;
      right: 20px;
      font-size: 1.5rem;
      color: white;
      background-color: rgba(0,0,0,0.5);
      padding: 10px 20px;
      border-radius: 10px;
    }
    #gameModal {
      position: fixed;
      top: 10%;
      left: 50%;
      transform: translateX(-50%);
      background: white;
      border-radius: 12px;
      padding: 20px;
      width: 80%;
      max-width: 600px;
      box-shadow: 0 0 20px rgba(0,0,0,0.3);
      display: none;
      z-index: 1000;
    }
    #gameModal h2 {
      margin-top: 0;
    }
    #closeModal {
      position: absolute;
      top: 10px;
      right: 20px;
      cursor: pointer;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <header>집게리아</header>
  <nav>
    <button onclick="openGame('burger')">버거 미니게임</button>
    <button onclick="openGame('plankton')">플랑크톤 침략</button>
    <button onclick="openGame('enhancement')">강화</button>
    <button onclick="openGame('attendance')">출석 보상</button>
    <button onclick="openGame('gag')">개그 코너</button>
    <button onclick="openGame('skill')">캐릭터 스킬</button>
  </nav>
  <div id="mainScene" onclick="handleGuestClick()">
    <img id="squidward" class="character" src="squidward.png" alt="징징이" />
    <img id="spongebob" class="character" src="spongebob.png" alt="스폰지밥" />
    <div class="coin-display">코인: <span id="coinCount">0</span></div>
    <img id="guest" class="guest" src="guest.png" style="display:none" alt="손님" />
  </div>
  <div id="gameModal">
    <span id="closeModal" onclick="closeGame()">X</span>
    <div id="modalContent"></div>
  </div>
  <audio id="bgm" src="bgm.mp3" autoplay loop></audio>
  <audio id="effect" src="effect.mp3"></audio>
  <script>
    let coins = 0;
    let guestActive = false;

    function updateCoins(amount) {
      coins += amount;
      document.getElementById('coinCount').innerText = coins;
    }

    function openGame(type) {
      const modal = document.getElementById('gameModal');
      const content = document.getElementById('modalContent');
      modal.style.display = 'block';
      if (type === 'burger') {
        content.innerHTML = '<h2>버거 미니게임</h2><p>재료를 조합해서 버거를 완성하세요!</p>';
      } else if (type === 'plankton') {
        content.innerHTML = '<h2>플랑크톤 침략</h2><p>침략자를 막아 코인을 벌자!</p>';
      } else if (type === 'enhancement') {
        content.innerHTML = '<h2>강화 시스템</h2><p>캐릭터 능력치를 강화해보세요!</p>';
      } else if (type === 'attendance') {
        content.innerHTML = '<h2>출석 보상</h2><p>매일 보상을 받아보세요!</p>';
        updateCoins(3);
      } else if (type === 'gag') {
        content.innerHTML = '<h2>개그 코너</h2><p>집게사장: 이 가게의 비밀 레시피는 절대 못 뺏긴다!</p>';
      } else if (type === 'skill') {
        content.innerHTML = '<h2>캐릭터 스킬</h2><p>10강마다 새로운 능력이 해금됩니다.</p>';
      }
    }

    function closeGame() {
      document.getElementById('gameModal').style.display = 'none';
    }

    function handleGuestClick() {
      if (!guestActive) return;
      const chance = Math.random();
      if (chance < 0.1) {
        alert('뚱이가 나타났다! 코인 +3');
        updateCoins(3);
      } else if (chance < 0.2) {
        alert('만수르 손님 등장! 코인 +3');
        updateCoins(3);
      } else {
        updateCoins(1);
      }
      document.getElementById('guest').style.display = 'none';
      guestActive = false;
    }

    setInterval(() => {
      if (Math.random() < 0.05 && !guestActive) {
        document.getElementById('guest').style.display = 'block';
        guestActive = true;
      }
    }, 5000);
  </script>
</body>
</html>
