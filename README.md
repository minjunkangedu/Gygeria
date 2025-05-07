<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>크러스티 크랩 웹게임</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #fffde7;
      padding: 20px;
      text-align: center;
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
      font-weight: bold;
      color: #d17b00;
    }
  </style>
</head>
<body>
  <h1>🏖️ 크러스티 크랩에 오신 걸 환영합니다!</h1>
  <p>손님: <span id="displayName"></span> / 코인: <span id="coinDisplay">0</span></p>

  <!-- 메뉴 버튼 -->
  <button onclick="showSection('menu')">🍔 메뉴</button>
  <button onclick="showSection('burgerGame')">🍳 햄버거 게임</button>
  <button onclick="showSection('planktonGame')">⚔️ 플랑크톤의 침략</button>
  <button onclick="showSection('gacha')">🎰 가챠</button>
  <button onclick="showSection('shop')">🛒 상점</button>
  <button onclick="showSection('pokedex')">📘 도감</button>
  <button onclick="showSection('jokes')">🎭 개그</button>
  <button onclick="showSection('regulars')">🌟 단골</button>
  <button onclick="showAdmin()">🔑 관리자</button>

  <!-- 이름 설정 -->
  <div id="namePopup" class="hidden">
    <h2>처음 오셨군요! 이름을 정해주세요:</h2>
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
    <input id="adminTargetUser" placeholder="지급 대상 이름"/>
    <input type="number" id="coinAmount" placeholder="코인 수">
    <button onclick="addCoins()">코인 지급</button>
  </section>

  <!-- 섹션들 -->
  <section id="menu" class="hidden"><h2>오늘의 메뉴</h2></section>
  <section id="gacha" class="hidden"><h2>가챠</h2></section>
  <section id="shop" class="hidden"><h2>상점</h2></section>
  <section id="pokedex" class="hidden"><h2>도감</h2></section>
  <section id="jokes" class="hidden"><h2>개그 코너</h2></section>
  <section id="regulars" class="hidden"><h2>단골 손님</h2></section>

  <!-- 햄버거 미니게임 -->
  <section id="burgerGame" class="hidden">
    <h2>스폰지밥이 햄버거를 만들고 있어요!</h2>
    <p>1초마다 코인이 자동으로 증가합니다.</p>
    <p>현재 코인: <span id="burgerCoinDisplay">0</span></p>
  </section>

  <!-- 플랑크톤 침략 게임 -->
  <section id="planktonGame" class="hidden">
    <h2>⚔️ 플랑크톤의 침략!</h2>
    <p id="planktonMessage">플랑크톤이 오기를 기다리는 중...</p>
    <button id="stopPlanktonBtn" onclick="stopPlankton()" disabled>막기!</button>
    <p>막은 횟수: <span id="planktonDefendCount">0</span></p>
  </section>

  <script>
    let user = localStorage.getItem("userName") || "";
    let coins = parseInt(localStorage.getItem("coins")) || 0;
    let planktonDefend = parseInt(localStorage.getItem("defendCount")) || 0;
    let planktonTimeout;

    function saveName() {
      user = document.getElementById("usernameInput").value;
      localStorage.setItem("userName", user);
      document.getElementById("namePopup").classList.add("hidden");
      document.getElementById("displayName").innerText = user;
      updateCoins();
    }

    function showSection(id) {
      document.querySelectorAll("section").forEach(sec => sec.classList.add("hidden"));
      document.getElementById(id).classList.remove("hidden");
    }

    function showAdmin() {
      document.getElementById("adminPopup").classList.remove("hidden");
    }

    function verifyAdmin() {
      const pw = document.getElementById("adminPassword").value;
      if (pw === "komq3244") {
        document.getElementById("adminPanel").classList.remove("hidden");
        document.getElementById("adminPopup").classList.add("hidden");
      } else {
        alert("비밀번호가 틀렸습니다.");
      }
    }

    function addCoins() {
      const target = document.getElementById("adminTargetUser").value;
      const amount = parseInt(document.getElementById("coinAmount").value);
      if (!target || isNaN(amount)) {
        alert("이름과 코인 수를 정확히 입력하세요.");
        return;
      }
      if (target === user) {
        coins += amount;
        updateCoins();
        alert(`${target}님에게 ${amount} 코인을 지급했습니다.`);
      } else {
        alert("현재는 본인만 코인을 받을 수 있습니다.");
      }
    }

    function updateCoins() {
      document.getElementById("coinDisplay").innerText = coins;
      document.getElementById("burgerCoinDisplay").innerText = coins;
      localStorage.setItem("coins", coins);
    }

    // 햄버거 게임: 1초마다 코인 증가
    setInterval(() => {
      if (document.getElementById("burgerGame").classList.contains("hidden")) return;
      coins++;
      updateCoins();
    }, 1000);

    // 플랑크톤 침공
    function startPlanktonInvasion() {
      document.getElementById("planktonMessage").innerText = "플랑크톤이 공격 중! 빨리 막으세요!";
      document.getElementById("stopPlanktonBtn").disabled = false;
      planktonTimeout = setTimeout(() => {
        document.getElementById("planktonMessage").innerText = "막지 못했어요!";
        document.getElementById("stopPlanktonBtn").disabled = true;
      }, 3000);
    }

    function stopPlankton() {
      clearTimeout(planktonTimeout);
      planktonDefend++;
      localStorage.setItem("defendCount", planktonDefend);
      document.getElementById("planktonDefendCount").innerText = planktonDefend;
      document.getElementById("planktonMessage").innerText = "플랑크톤을 막았어요!";
      document.getElementById("stopPlanktonBtn").disabled = true;
      if (planktonDefend % 5 === 0) {
        coins += 5;
        updateCoins();
        alert("5회 방어 성공! 보너스 5코인 지급!");
      }
    }

    setInterval(() => {
      if (document.getElementById("planktonGame").classList.contains("hidden")) return;
      startPlanktonInvasion();
    }, 5000);

    // 초기 로드
    if (!user) {
      document.getElementById("namePopup").classList.remove("hidden");
    } else {
      document.getElementById("displayName").innerText = user;
      updateCoins();
      document.getElementById("planktonDefendCount").innerText = planktonDefend;
    }
  </script>
</body>
</html>
