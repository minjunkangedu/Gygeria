<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>크러스티 크랩 게임</title>
  <style>
    body {
      margin: 0;
      font-family: 'Arial', sans-serif;
      background: url('krustykrab_bg.jpg') no-repeat center center fixed;
      background-size: cover;
      color: #fff;
      text-align: center;
    }
    header {
      padding: 1em;
      background-color: rgba(0,0,0,0.7);
    }
    .menu {
      display: flex;
      justify-content: center;
      gap: 1em;
      margin: 1em;
    }
    .menu button {
      padding: 1em;
      font-size: 1.2em;
      background-color: #f9a602;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      transition: transform 0.2s;
    }
    .menu button:hover {
      transform: scale(1.1);
    }
    #gameArea {
      padding: 2em;
      background-color: rgba(0,0,0,0.5);
      margin: 1em;
      border-radius: 20px;
    }
  </style>
</head>
<body>
  <header>
    <h1>크러스티 크랩: 궁극의 경영 게임</h1>
  </header>
  <div class="menu">
    <button onclick="startPlanktonInvasion()">플랑크톤 침략</button>
    <button onclick="startBurgerGame()">스폰지밥 햄버거 미니게임</button>
    <button onclick="openEnhancement()">캐릭터 강화</button>
    <button onclick="adminPanel()">관리자 모드</button>
  </div>
  <div id="gameArea">
    <p>게임을 시작하려면 메뉴를 선택하세요.</p>
  </div>

  <audio id="bgm" src="bgm.mp3" autoplay loop></audio>
  <audio id="effect" src="effect.mp3"></audio>

  <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-database-compat.js"></script>
  <script>
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "gygeria-9f319.firebaseapp.com",
      databaseURL: "https://gygeria-9f319-default-rtdb.firebaseio.com",
      projectId: "gygeria-9f319",
      storageBucket: "gygeria-9f319.appspot.com",
      messagingSenderId: "570080414698",
      appId: "YOUR_APP_ID"
    };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    function startPlanktonInvasion() {
      document.getElementById('gameArea').innerHTML = `<h2>플랑크톤 침략 게임</h2><p>게임 내용 구성 중...</p>`;
    }

    function startBurgerGame() {
      document.getElementById('gameArea').innerHTML = `<h2>스폰지밥 햄버거 미니게임</h2><p>게임 내용 구성 중...</p>`;
    }

    function openEnhancement() {
      document.getElementById('gameArea').innerHTML = `<h2>캐릭터 강화</h2><p>강화 시스템 구성 중...</p>`;
    }

    function adminPanel() {
      document.getElementById('gameArea').innerHTML = `
        <h2>관리자 패널</h2>
        <input id="adminName" placeholder="유저 이름" />
        <input id="adminCoins" placeholder="코인 수" type="number" />
        <button onclick="grantCoins()">지급</button>
      `;
    }

    function grantCoins() {
      const name = document.getElementById('adminName').value;
      const coins = parseInt(document.getElementById('adminCoins').value);
      if (name && !isNaN(coins)) {
        db.ref('users/' + name).update({ coins: coins });
        alert("지급 완료!");
        document.getElementById("effect").play();
      }
    }
  </script>
</body>
</html>
