<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>크러스티 크랩 게임</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #fffde7;
      padding: 20px;
    }
    button {
      margin: 5px;
      padding: 10px 15px;
      background-color: #ffca28;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    button:hover {
      background-color: #ffa000;
    }
    .hidden {
      display: none;
    }
    #namePopup {
      background-color: #fff3cd;
      border: 2px solid #ffa000;
      padding: 20px;
      margin: 20px 0;
    }
    input {
      padding: 6px;
      margin-top: 5px;
    }
  </style>
</head>
<body>
  <h1>🏖️ 크러스티 크랩에 오신 걸 환영합니다!</h1>
  <p>손님: <span id="displayName"></span> / 코인: <span id="coinDisplay">0</span></p>

  <!-- 메뉴 버튼 -->
  <button onclick="showSection('menu')">🍔 메뉴</button>
  <button onclick="showSection('gacha')">🎰 가챠</button>
  <button onclick="showSection('pokedex')">📘 도감</button>
  <button onclick="showSection('shop')">🛒 상점</button>
  <button onclick="showSection('jokes')">🎭 개그</button>
  <button onclick="showSection('regulars')">🌟 단골</button>
  <button onclick="showSection('enhancement')">⚒️ 강화</button>
  <button onclick="showSection('enhanceGacha')">✨ 강화 가챠</button>
  <button onclick="showSection('planktonGame')">⚔️ 플랑크톤 침략</button>
  <button onclick="showAdmin()">🔑 관리자</button>

  <!-- 이름 입력 팝업 -->
  <div id="namePopup" class="hidden">
    <h2>처음 오셨군요!</h2>
    <p><strong>당신의 이름을 정해주세요!</strong></p>
    <input id="usernameInput" placeholder="이름 입력"/>
    <button onclick="saveName()">저장</button>
  </div>

  <!-- 관리자 로그인 -->
  <div id="adminPopup" class="hidden">
    <p>관리자 비밀번호 입력:</p>
    <input type="password" id="adminPassword" placeholder="비밀번호"/>
    <button onclick="verifyAdmin()">입력</button>
  </div>

  <!-- 관리자 패널 -->
  <section id="adminPanel" class="hidden">
    <h2>관리자 메뉴</h2>
    <input id="targetUser" placeholder="유저 이름" />
    <input type="number" id="coinAmount" placeholder="코인 수">
    <button onclick="giveCoinsToUser()">코인 지급</button>
  </section>

  <!-- 메뉴 섹션 -->
  <section id="menu" class="hidden">
    <h2>오늘의 메뉴</h2>
    <button onclick="orderMenu('크래비 버거', 5)">크래비 버거 (5코인)</button>
    <button onclick="orderMenu('씨위드 샐러드', 4)">씨위드 샐러드 (4코인)</button>
    <h3>🌟 특별 메뉴</h3>
    <button onclick="orderMenu('황금버거', 10)">황금버거 (10코인)</button>
  </section>

  <!-- 가챠 섹션 -->
  <section id="gacha" class="hidden">
    <h2>스폰지밥 캐릭터 가챠</h2>
    <button onclick="drawGacha()">가챠 뽑기 (10코인)</button>
    <div id="gachaResult"></div>
  </section>

  <!-- 도감 -->
  <section id="pokedex" class="hidden">
    <h2>도감</h2>
    <ul id="pokedexList"></ul>
  </section>

  <!-- 상점 -->
  <section id="shop" class="hidden">
    <h2>상점</h2>
    <button onclick="buyItem('행운버거', 5)">행운버거 (5코인)</button>
    <button onclick="buyItem('경험치 스프', 8)">경험치 스프 (8코인)</button>
  </section>

  <!-- 개그 코너 -->
  <section id="jokes" class="hidden">
    <h2>개그 코너</h2>
    <button onclick="tellJoke()">개그 듣기</button>
    <p id="jokeOutput"></p>
  </section>

  <!-- 단골 손님 -->
  <section id="regulars" class="hidden">
    <h2>단골 손님</h2>
    <ul>
      <li>뚱이</li>
      <li>징징이</li>
      <li>플랑크톤</li>
    </ul>
  </section>

  <!-- 강화 섹션 -->
  <section id="enhancement" class="hidden">
    <h2>강화</h2>
    <p>현재 강화 레벨: <span id="enhanceLevelDisplay">0</span></p>
    <p>강화 파워: <span id="enhancePowerDisplay">+0%</span></p>
    <p>강화석: <span id="enhanceStoneDisplay">0</span></p>
    <p>집게의 축복: <span id="blessingDisplay">0</span></p>
    <button onclick="upgradeCharacter()">강화 시도</button>
  </section>

  <!-- 강화 가챠 -->
  <section id="enhanceGacha" class="hidden">
    <h2>강화 가챠</h2>
    <button onclick="drawEnhanceGacha()">강화 가챠 뽑기 (5코인)</button>
    <p id="enhanceGachaResult"></p>
  </section>

  <!-- 플랑크톤 침략 게임 -->
  <section id="planktonGame" class="hidden">
    <h2>플랑크톤 침략</h2>
    <p id="planktonStatus">플랑크톤 침략을 막아주세요!</p>
    <button onclick="stopPlankton()">막기</button>
  </section>

  <!-- Firebase 및 JavaScript -->
  <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
  <script>
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
      databaseURL: "https://YOUR_PROJECT_ID.firebaseio.com",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_PROJECT_ID.appspot.com",
      messagingSenderId: "YOUR_SENDER_ID",
      appId: "YOUR_APP_ID"
    };

    firebase.initializeApp(firebaseConfig);
    const database = firebase.database();

    let user = localStorage.getItem("userName") || "";
    let coins = parseInt(localStorage.getItem("coins")) || 0;
    let pokedex = JSON.parse(localStorage.getItem("pokedex")) || [];
    let enhanceLevel = parseInt(localStorage.getItem("enhanceLevel")) || 0;
    let enhanceStones = parseInt(localStorage.getItem("enhanceStones")) || 0;
    let blessings = parseInt(localStorage.getItem("blessings")) || 0;
    let planktonAttacks = 0;

    function showSection(id) {
      document.querySelectorAll("section").forEach(sec => sec.classList.add("hidden"));
      document.getElementById(id).classList.remove("hidden");
    }

    function updateCoins() {
      document.getElementById("coinDisplay").innerText = coins;
      localStorage.setItem("coins", coins);
      saveToServer();
    }

    function saveToServer() {
      if (!user) return;
      database.ref("users/" + user).set({
        coins,
        pokedex,
        enhanceLevel,
        enhanceStones,
        blessings
      });
    }

    function loadFromServer() {
      if (!user) return;
      database.ref("users/" + user).once("value").then(snapshot => {
        const data = snapshot.val();
        if (data) {
          coins = data.coins || 0;
          pokedex = data.pokedex || [];
          enhanceLevel = data.enhanceLevel || 0;
          enhanceStones = data.enhanceStones || 0;
          blessings = data.blessings || 0;
          updateUI();
        }
      });
    }

    function updateUI() {
      updateCoins();
      updatePokedex();
      document.getElementById("enhanceLevelDisplay").innerText = enhanceLevel;
      document.getElementById("enhancePowerDisplay").innerText = `+${enhanceLevel}%`;
      document.getElementById("enhanceStoneDisplay").innerText = enhanceStones;
      document.getElementById("blessingDisplay").innerText = blessings;
    }

    function saveName() {
      user = document.getElementById("usernameInput").value.trim();
      if (!user) return alert("이름을 입력해주세요!");
      localStorage.setItem("userName", user);
      document      
