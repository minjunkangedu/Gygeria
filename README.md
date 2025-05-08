<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
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
    <img id="squidward" class="character" src="squidward.png" alt="징징이">
    <img id="spongebob" class="character" src="spongebob.png" alt="스폰지밥">
    <div class="coin-display">코인: <span id="coinCount">0</span></div>
    <img id="guest" class="guest" src="guest.png" style="display:none" alt="손님">
  </div>

  <audio id="bgm" src="bgm.mp3" autoplay loop></audio>
  <audio id="effect" src="effect.mp3"></audio>

  <script>
    let coins = 0;
    let guestVisible = false;
    let burgerCount = 0;

    function openGame(type) {
      if (type === 'burger') {
        makeBurger();
      } else {
        alert(type + ' 모드 시작! (추후 구현됨)');
      }
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

    // 버거 생산 시스템
    function makeBurger() {
      if (burgerCount >= 10) {
        alert('버거가 꽉 찼습니다! 징징이가 판매해야 합니다.');
        return;
      }
      burgerCount++;
      alert('스폰지밥이 버거를 만들었습니다! 현재 버거 수: ' + burgerCount);
      autoSell();
    }

    function autoSell() {
      if (burgerCount >= 5) {
        let sold = burgerCount;
        burgerCount = 0;
        coins += sold;
        alert('징징이가 버거 ' + sold + '개를 팔았습니다! 코인 +' + sold);
        document.getElementById('coinCount').textContent = coins;
      }
    }

    // 초기 손님 스폰
    spawnGuestWithDelay();
  </script>
</body>
</html>
