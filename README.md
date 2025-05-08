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
      overflow: hidden;
    }

    header {
      background-color: rgba(0, 0, 0, 0.7);
      color: #fff;
      padding: 1rem;
      text-align: center;
      font-size: 2.5rem;
    }

    nav {
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      background-color: rgba(255, 255, 255, 0.8);
      padding: 1rem;
      position: sticky;
      top: 0;
      z-index: 1000;
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
      height: calc(100vh - 200px);
    }

    .character {
      position: absolute;
      bottom: 10%;
      transition: all 0.3s ease;
    }

    #squidward {
      left: 15%;
      width: 250px;
    }

    #spongebob {
      right: 15%;
      width: 250px;
    }

    .guest {
      position: absolute;
      bottom: 10%;
      left: 40%;
      width: 180px;
      transition: all 1s ease-in-out;
    }

    .coin-display {
      position: absolute;
      top: 20px;
      right: 20px;
      font-size: 1.5rem;
      color: white;
      background-color: rgba(0,0,0,0.5);
      padding: 10px 20px;
      border-radius: 10px;
      z-index: 10;
    }
  </style>
</head>
<body onclick="handleGuestClick()">
  <header>집게리아</header>

  <nav>
    <button onclick="openGame('burger')">버거 미니게임</button>
    <button onclick="openGame('plankton')">플랑크톤 침략</button>
    <button onclick="openGame('enhancement')">강화</button>
    <button onclick="openGame('attendance')">출석 보상</button>
    <button onclick="openGame('gag')">개그 코너</button>
    <button onclick="openGame('skill')">캐릭터 스킬</button>
  </nav>

  <div id="mainScene">
    <img id="squidward" class="character" src="squidward.gif" alt="징징이">
    <img id="spongebob" class="character" src="spongebob.gif" alt="스폰지밥">
    <img id="guest" class="guest" src="guest.png" style="display:none" alt="손님">
    <div class="coin-display">코인: <span id="coinCount">0</span></div>
  </div>

  <audio id="bgm" src="bgm.mp3" autoplay loop></audio>
  <audio id="effect" src="effect.mp3"></audio>

  <script>
    let coins = 0;
    let guestVisible = false;

    function openGame(type) {
      alert(type + ' 모드 진입! (기능 구현 예정)');
    }

    function handleGuestClick() {
      if (guestVisible) {
        document.getElementById('guest').style.display = 'none';
        document.getElementById('effect').play();
        coins++;
        document.getElementById('coinCount').textContent = coins;
        guestVisible = false;
        spawnGuestWithDelay();
      }
    }

    function spawnGuest() {
      const guest = document.getElementById('guest');
      guest.style.display = 'block';
      guestVisible = true;
    }

    function spawnGuestWithDelay() {
      setTimeout(spawnGuest, 3000);
    }

    spawnGuestWithDelay();
  </script>
</body>
</html>
