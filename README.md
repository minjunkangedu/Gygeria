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
      font-size: 1.2rem;
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
      display: flex;
      justify-content: space-around;
      align-items: flex-end;
      position: relative;
      width: 100vw;
      height: 70vh;
      padding-bottom: 2rem;
    }
    .character {
      transition: all 0.3s ease;
      margin: 0 3rem;
    }
    #squidward {
      width: 300px;
    }
    #spongebob {
      width: 300px;
      animation: float 2s infinite ease-in-out;
    }
    @keyframes float {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-10px); }
    }
    .guest {
      position: absolute;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%);
      width: 220px;
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
    .popup {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: white;
      padding: 2rem;
      border-radius: 15px;
      box-shadow: 0 0 20px rgba(0,0,0,0.5);
      display: none;
      z-index: 1000;
    }
    .popup h2 {
      margin-top: 0;
    }
    .popup button {
      margin-top: 1rem;
      padding: 0.5rem 1rem;
      font-size: 1rem;
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
    <img id="guest" class="guest" src="guest.png" style="display:none" alt="손님">
    <div class="coin-display">코인: <span id="coinCount">0</span></div>
  </div>

  <div id="popup" class="popup">
    <h2 id="popupTitle">모드 이름</h2>
    <p id="popupContent">여기에 해당 모드 설명이 들어갑니다.</p>
    <button onclick="closePopup()">닫기</button>
  </div>

  <audio id="bgm" src="bgm.mp3" autoplay loop></audio>
  <audio id="effect" src="effect.mp3"></audio>

  <script>
    let coins = 0;
    let guestVisible = false;

    function openGame(type) {
      const popup = document.getElementById('popup');
      const title = document.getElementById('popupTitle');
      const content = document.getElementById('popupContent');

      switch(type) {
        case 'burger':
          title.textContent = '버거 미니게임';
          content.textContent = '시간 내에 정확한 순서로 재료를 클릭해 햄버거를 완성하세요!';
          break;
        case 'plankton':
          title.textContent = '플랑크톤 침략';
          content.textContent = '플랑크톤을 클릭해서 침략을 막아주세요!';
          break;
        case 'enhancement':
          title.textContent = '캐릭터 강화';
          content.textContent = '강화석을 사용해 캐릭터 능력치를 상승시켜 보세요.';
          break;
        case 'attendance':
          title.textContent = '출석 보상';
          content.textContent = '매일 접속 시 보상을 획득할 수 있어요!';
          break;
        case 'gag':
          title.textContent = '개그 코너';
          content.textContent = '집게사장의 유쾌한 개그를 들어보세요!';
          break;
        case 'skill':
          title.textContent = '캐릭터 스킬';
          content.textContent = '강화에 따라 다양한 스킬이 해금됩니다.';
          break;
      }
      popup.style.display = 'block';
    }

    function closePopup() {
      document.getElementById('popup').style.display = 'none';
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
