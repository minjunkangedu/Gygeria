<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Krusty Krab Web Game</title>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap">
  <style>
    body {
      margin: 0;
      font-family: 'Press Start 2P', cursive;
      background: url('images/krustykrab_bg.jpg') no-repeat center center fixed;
      background-size: cover;
      color: #fff;
      text-align: center;
    }
    .container {
      padding: 20px;
      background-color: rgba(0,0,0,0.6);
      max-width: 960px;
      margin: 0 auto;
      border-radius: 16px;
    }
    button {
      font-family: inherit;
      background: #ffd700;
      border: none;
      padding: 10px 20px;
      margin: 10px;
      cursor: pointer;
      border-radius: 8px;
      box-shadow: 0 4px #b8860b;
    }
    input, select {
      padding: 8px;
      margin: 10px;
      border-radius: 8px;
    }
  </style>
  <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-database-compat.js"></script>
</head>
<body>
  <div class="container">
    <h1>크러스티 크랩 게임</h1>
    <p>이름: <input type="text" id="username"></p>
    <p><button onclick="startGame()">게임 시작</button></p>
    <p><button onclick="customizeStore()">가게 이름 설정</button></p>
    <p><button onclick="pvpBattle()">PVP 배틀</button></p>
    <p><button onclick="viewRankings()">랭킹 보기</button></p>
    <div id="gameArea"></div>
    <audio id="bgm" loop autoplay src="sounds/bgm.mp3"></audio>
  </div>

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

    const bgm = document.getElementById('bgm');
    const usernameInput = document.getElementById('username');

    function startGame() {
      const username = usernameInput.value.trim();
      if (!username) return alert("이름을 입력해주세요.");
      document.getElementById('gameArea').innerHTML = `<p>${username}님, 햄버거를 만들어주세요!</p>`;
      playEffect();
    }

    function customizeStore() {
      const name = prompt("가게 이름을 입력하세요:");
      if (name) alert(`가게 이름이 '${name}'(으)로 설정되었습니다.`);
    }

    function pvpBattle() {
      alert("PVP 배틀 모드 준비중입니다. 실제 게임 로직은 여기에 추가됩니다.");
    }

    function viewRankings() {
      db.ref('users').orderByChild('score').limitToLast(5).once('value', snapshot => {
        const scores = [];
        snapshot.forEach(child => {
          scores.push({ name: child.key, score: child.val().score });
        });
        scores.reverse();
        alert("Top 5 유저:\n" + scores.map(s => `${s.name}: ${s.score}`).join("\n"));
      });
    }

    function playEffect() {
      const audio = new Audio('sounds/effect.mp3');
      audio.play();
    }
  </script>
</body>
</html>
