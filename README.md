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
      display: flex;
      justify-content: center;
      align-items: flex-end;
      position: relative;
      width: 100vw;
      height: 60vh;
      padding-bottom: 2rem;
    }
    .character {
      transition: all 0.3s ease;
      margin: 0 3rem;
    }
    #squidward {
      width: 180px;
    }
    #spongebob {
      width: 180px;
    }
    .guest {
      position: absolute;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%);
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
    .popup {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: white;
      border: 2px solid #333;
      padding: 2rem;
      z-index: 100;
      display: none;
    }
    .popup h2 { margin-top: 0; }
    .overlay {
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background: rgba(0,0,0,0.5);
      z-index: 90;
      display: none;
    }
  </style>
</head>
<body>
  <header>집게리아</header>
  <nav>
    <button onclick="showPopup('burgerGame')">버거 미니게임</button>
    <button onclick="showPopup('planktonGame')">플랑크톤 침략</button>
    <button onclick="showPopup('enhancement')">강화</button>
    <button onclick="showPopup('attendance')">출석 보상</button>
    <button onclick="showPopup('gag')">개그 코너</button>
    <button onclick="showPopup('skill')">캐릭터 스킬</button>
  </nav>

  <div id="mainScene" onclick="handleGuestClick()">
    <img id="squidward" class="character" src="squidward.png" loading="lazy" alt="징징이">
    <img id="spongebob" class="character" src="spongebob.png" loading="lazy" alt="스폰지밥">
    <img id="guest" class="guest" src="guest.png" loading="lazy" style="display:none" alt="손님">
    <div class="coin-display">코인: <span id="coinCount">0</span></div>
  </div>

  <audio id="bgm" src="bgm.mp3" autoplay loop></audio>
  <audio id="effect" src="effect.mp3"></audio>

  <!-- 팝업들 -->
  <div class="overlay" id="overlay" onclick="hidePopup()"></div>
  <div class="popup" id="burgerGame">
    <h2>버거 미니게임</h2>
    <p>재료를 순서대로 클릭하세요! (추가 구현 가능)</p>
    <button onclick="hidePopup()">닫기</button>
  </div>
  <div class="popup" id="planktonGame">
    <h2>플랑크톤 침략</h2>
    <p>플랑크톤을 막아라! (추가 구현 가능)</p>
    <button onclick="hidePopup()">닫기</button>
  </div>
  <div class="popup" id="enhancement">
    <h2>강화</h2>
    <p>캐릭터 강화 진행! (추가 구현 가능)</p>
    <button onclick="hidePopup()">닫기</button>
  </div>
  <div class="popup" id="attendance">
    <h2>출석 보상</h2>
    <p>하루 1회 보상 획득! (추가 구현 가능)</p>
    <button onclick="hidePopup()">닫기</button>
  </div>
  <div class="popup" id="gag">
    <h2>개그 코너</h2>
    <p>웃긴 대사 or 장면 표시! (추가 구현 가능)</p>
    <button onclick="hidePopup()">닫기</button>
  </div>
  <div class="popup" id="skill">
    <h2>캐릭터 스킬</h2>
    <p>스킬 목록 및 효과 설명 (추가 구현 가능)</p>
    <button onclick="hidePopup()">닫기</button>
  </div>

  <script>
    let coins = 0;
    let guestVisible = false;

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

    function showPopup(id) {
      document.getElementById('overlay').style.display = 'block';
      document.getElementById(id).style.display = 'block';
    }

    function hidePopup() {
      document.getElementById('overlay').style.display = 'none';
      const popups = document.querySelectorAll('.popup');
      popups.forEach(p => p.style.display = 'none');
    }

    spawnGuestWithDelay();
  </script>
</body>
</html>
