<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>집게리아 게임</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background: url('krustykrab_bg.jpg') no-repeat center center fixed;
      background-size: cover;
      font-family: 'Arial', sans-serif;
      color: #fff;
      text-align: center;
    }
    .container {
      background: rgba(0, 0, 0, 0.7);
      padding: 20px;
      margin: 40px auto;
      max-width: 600px;
      border-radius: 16px;
    }
    button {
      padding: 10px 20px;
      margin: 5px;
      border: none;
      background-color: #fcbf49;
      color: #000;
      font-weight: bold;
      border-radius: 10px;
      cursor: pointer;
    }
    #coinDisplay {
      font-size: 18px;
      margin-bottom: 10px;
      color: #f1fa8c;
    }
    #gameSection, #gagSection {
      display: none;
    }
  </style>
</head>
<body>

<audio id="bgm" src="bgm.mp3" autoplay loop></audio>

<div class="container">
  <h1>어서오세요, 집게리아입니다!</h1>
  <p id="coinDisplay">코인: 0</p>

  <div id="mainMenu">
    <button onclick="startBurgerGame()">스폰지밥 햄버거 게임</button>
    <button onclick="startPlanktonGame()">플랑크톤 침략</button>
    <button onclick="strengthenCharacter()">캐릭터 강화</button>
    <button onclick="drawEnhancement()">강화 가챠</button>
    <button onclick="checkAttendance()">출석 보상</button>
    <button onclick="showGag()">개그 코너</button>
  </div>

  <div id="gameSection">
    <p id="gameText">게임 영역</p>
    <button onclick="endGame()">돌아가기</button>
  </div>

  <div id="gagSection">
    <p id="gagText">개그가 여기에!</p>
    <button onclick="endGag()">돌아가기</button>
  </div>
</div>

<script type="module">
  // Firebase 설정
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
  import { getDatabase, ref, get, set, update } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-database.js";

  const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "gygeria-9f319.firebaseapp.com",
    projectId: "gygeria-9f319",
    storageBucket: "gygeria-9f319.appspot.com",
    messagingSenderId: "570080414698",
    appId: "YOUR_APP_ID",
    databaseURL: "https://gygeria-9f319-default-rtdb.firebaseio.com"
  };

  const app = initializeApp(firebaseConfig);
  const db = getDatabase(app);

  const username = prompt("이름을 입력하세요:");
  const userRef = ref(db, 'users/' + username);

  let coins = 0;

  get(userRef).then(snapshot => {
    if (snapshot.exists()) {
      coins = snapshot.val().coins || 0;
    } else {
      set(userRef, { coins: 0 });
    }
    updateCoinDisplay();
  });

  function updateCoinDisplay() {
    document.getElementById("coinDisplay").innerText = `코인: ${coins}`;
  }

  function addCoins(amount) {
    coins += amount;
    update(userRef, { coins });
    updateCoinDisplay();
  }

  // 게임 및 기능 실행 예시
  function startBurgerGame() {
    showSection("gameSection");
    document.getElementById("gameText").innerText = "스폰지밥이 햄버거를 만들고 있어요!";
    addCoins(1);
  }

  function startPlanktonGame() {
    showSection("gameSection");
    document.getElementById("gameText").innerText = "플랑크톤이 침략 중입니다!";
    addCoins(2);
  }

  function strengthenCharacter() {
    showSection("gameSection");
    document.getElementById("gameText").innerText = "캐릭터가 강화되고 있습니다!";
    addCoins(1);
  }

  function drawEnhancement() {
    showSection("gameSection");
    const results = ["강화석", "집게의 축복", "실패!"];
    const result = results[Math.floor(Math.random() * results.length)];
    document.getElementById("gameText").innerText = `강화 가챠 결과: ${result}`;
    if (result !== "실패!") addCoins(1);
  }

  function checkAttendance() {
    showSection("gameSection");
    document.getElementById("gameText").innerText = "출석 보상으로 3코인을 획득!";
    addCoins(3);
  }

  function showGag() {
    showSection("gagSection");
    const gags = [
      "스폰지밥이 만든 버거는 '치즈' 대신 '해파리젤리'가 들어가요!",
      "플랑크톤: \"이번엔 꼭 비법 레시피를 훔칠거야!\" → 실패",
      "징징이: \"오늘도 알바라니… 사장님 월급은요?\"",
      "뚱이: \"햄버거를 먹으려다 입이 햄이 됐어.\""
    ];
    document.getElementById("gagText").innerText = gags[Math.floor(Math.random() * gags.length)];
  }

  function endGame() {
    hideSections();
  }

  function endGag() {
    hideSections();
  }

  function hideSections() {
    document.getElementById("gameSection").style.display = "none";
    document.getElementById("gagSection").style.display = "none";
  }

  function showSection(id) {
    hideSections();
    document.getElementById(id).style.display = "block";
  }
</script>

</body>
</html>
