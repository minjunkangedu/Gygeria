<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>집게리아 시뮬레이터</title>
  <style>
    body {
      margin: 0;
      font-family: 'Arial', sans-serif;
      background: url('krustykrab_bg.jpg') no-repeat center center fixed;
      background-size: cover;
      overflow: hidden;
    }
    #menuBar {
      position: fixed;
      top: 0;
      width: 100%;
      background: rgba(0, 0, 0, 0.7);
      color: white;
      display: flex;
      justify-content: space-around;
      padding: 10px;
      z-index: 1000;
    }
    .menu-button {
      cursor: pointer;
    }
    #gameArea {
      position: relative;
      width: 100vw;
      height: 100vh;
    }
    #squidward, #spongebob, .customer {
      position: absolute;
      transition: opacity 0.5s ease, transform 0.5s ease;
    }
    #squidward {
      bottom: 50px;
      left: 40%;
      width: 100px;
    }
    #spongebob {
      bottom: 100px;
      right: 40px;
      width: 100px;
      animation: cooking 2s infinite alternate;
    }
    .customer {
      bottom: 50px;
      left: 30%;
      width: 80px;
      opacity: 0;
    }
    .balloon {
      position: absolute;
      top: -50px;
      left: 20px;
      background: white;
      border-radius: 10px;
      padding: 5px 10px;
      font-size: 14px;
      color: black;
    }
    @keyframes cooking {
      0% { transform: translateY(0); }
      100% { transform: translateY(-5px); }
    }
    #coinDisplay {
      position: fixed;
      top: 50px;
      right: 20px;
      background: gold;
      color: black;
      padding: 10px;
      border-radius: 10px;
      font-weight: bold;
      z-index: 1000;
    }
  </style>
</head>
<body>
  <div id="menuBar">
    <span class="menu-button" onclick="toggleMenu()">메뉴</span>
    <span class="menu-button" style="display:none" onclick="showFeature('강화')">강화</span>
    <span class="menu-button" style="display:none" onclick="showFeature('개그')">개그</span>
    <span class="menu-button" style="display:none" onclick="showFeature('미니게임')">미니게임</span>
    <span class="menu-button" style="display:none" onclick="showFeature('출석')">출석</span>
    <span class="menu-button" style="display:none" onclick="showFeature('가챠')">가챠</span>
    <span class="menu-button" style="display:none" onclick="alert('코인: ' + coin)">코인확인</span>
  </div>

  <div id="coinDisplay">코인: 0</div>
  <div id="gameArea" onclick="collectOrder()">
    <img id="squidward" src="squidward.png" alt="징징이">
    <img id="spongebob" src="spongebob.png" alt="스폰지밥">
  </div>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
    import { getDatabase, ref, set, get } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";

    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT.firebaseapp.com",
      databaseURL: "https://YOUR_PROJECT.firebaseio.com",
      projectId: "gygeria-9f319",
      storageBucket: "gygeria-9f319.appspot.com",
      messagingSenderId: "570080414698",
      appId: "YOUR_APP_ID"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    let coin = 0;
    const coinDisplay = document.getElementById('coinDisplay');

    function updateCoinDisplay() {
      coinDisplay.textContent = `코인: ${coin}`;
    }

    function spawnCustomer() {
      const customer = document.createElement('img');
      customer.src = 'customer.png';
      customer.className = 'customer';
      const balloon = document.createElement('div');
      balloon.className = 'balloon';
      balloon.textContent = '햄버거 하나 주세요!';
      customer.appendChild(balloon);
      document.getElementById('gameArea').appendChild(customer);
      setTimeout(() => customer.style.opacity = 1, 100);
    }

    function collectOrder() {
      const customer = document.querySelector('.customer');
      if (customer) {
        customer.style.opacity = 0;
        setTimeout(() => customer.remove(), 500);
        coin += 1;
        updateCoinDisplay();
      }
    }

    function toggleMenu() {
      document.querySelectorAll('#menuBar .menu-button').forEach((btn, i) => {
        if (i !== 0) btn.style.display = (btn.style.display === 'none') ? 'inline' : 'none';
      });
    }

    function showFeature(name) {
      alert(`${name} 기능은 곧 추가됩니다!`);
    }

    spawnCustomer();
    setInterval(spawnCustomer, 15000); // 15초마다 손님 등장

  </script>
</body>
</html>
