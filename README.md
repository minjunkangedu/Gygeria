<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>크러스티 크랩 사이트</title>
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
  </style>
</head>
<body>
  <h1>🏖️ 크러스티 크랩에 오신 걸 환영합니다!</h1>
  <p>손님: <span id="displayName"></span> / 코인: <span id="coinDisplay">0</span></p>

  <button onclick="showSection('menu')">🍔 메뉴</button>
  <button onclick="showSection('gacha')">🎰 가챠</button>
  <button onclick="showSection('pokedex')">📘 도감</button>
  <button onclick="showSection('shop')">🛒 상점</button>
  <button onclick="showSection('jokes')">🎭 개그</button>
  <button onclick="showSection('regulars')">🌟 단골</button>
  <button onclick="showAdmin()">🔑 관리자</button>
  <button onclick="showSection('planktonGame')">⚔️ 플랑크톤의 침략</button>

  <div id="namePopup" class="hidden">
    <p>처음 오셨군요! 이름을 정해주세요:</p>
    <input id="usernameInput" placeholder="이름 입력"/>
    <button onclick="saveName()">저장</button>
  </div>

  <div id="adminPopup" class="hidden">
    <p>관리자 비밀번호 입력:</p>
    <input type="password" id="adminPassword" placeholder="비밀번호"/>
    <button onclick="verifyAdmin()">입력</button>
  </div>

  <section id="menu" class="hidden">
    <h2>오늘의 메뉴</h2>
    <button onclick="orderMenu('크래비 버거', 5)">크래비 버거 (5코인)</button>
    <button onclick="orderMenu('씨위드 샐러드', 4)">씨위드 샐러드 (4코인)</button>
    <h3>🌟 특별 메뉴</h3>
    <button onclick="orderMenu('황금버거', 10)">황금버거 (10코인)</button>
  </section>

  <section id="gacha" class="hidden">
    <h2>스폰지밥 캐릭터 가챠</h2>
    <button onclick="drawGacha()">가챠 뽑기 (10코인)</button>
    <div id="gachaResult"></div>
  </section>

  <section id="pokedex" class="hidden">
    <h2>도감</h2>
    <ul id="pokedexList"></ul>
  </section>

  <section id="shop" class="hidden">
    <h2>상점</h2>
    <button onclick="buyItem('행운버거', 5)">행운버거 (5코인)</button>
    <button onclick="buyItem('경험치 스프', 8)">경험치 스프 (8코인)</button>
  </section>

  <section id="jokes" class="hidden">
    <h2>개그 코너</h2>
    <button onclick="tellJoke()">개그 듣기</button>
    <p id="jokeOutput"></p>
  </section>

  <section id="regulars" class="hidden">
    <h2>단골 손님</h2>
    <ul>
      <li>뚱이</li>
      <li>징징이</li>
      <li>플랑크톤</li>
    </ul>
  </section>

  <section id="adminPanel" class="hidden">
    <h2>관리자 메뉴</h2>
    <input type="number" id="coinAmount" placeholder="코인 수">
    <button onclick="addCoins()">코인 지급</button>
  </section>

  <section id="planktonGame" class="hidden">
    <h2>플랑크톤의 침략! 게살버거 비법을 지켜라!</h2>
    <p>플랑크톤이 침략 중입니다! 클릭해서 막아주세요!</p>
    <button onclick="stopPlankton()">플랑크톤 막기</button>
    <div id="planktonStatus"></div>
  </section>

  <!-- Firebase + Logic Script -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>
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
    let planktonAttacks = 0;

    function saveName() {
      user = document.getElementById("usernameInput").value;
      localStorage.setItem("userName", user);
      document.getElementById("namePopup").classList.add("hidden");
      document.getElementById("displayName").innerText = user;
      loadFromServer();
    }

    function showSection(id) {
      document.querySelectorAll("section").forEach(sec => sec.classList.add("hidden"));
      document.getElementById(id).classList.remove("hidden");
    }

    function updateCoins() {
      document.getElementById("coinDisplay").innerText = coins;
      localStorage.setItem("coins", coins);
      saveToServer();
    }

    function addCoins() {
      const amount = parseInt(document.getElementById("coinAmount").value);
      if (!isNaN(amount)) {
        coins += amount;
        updateCoins();
      }
    }

    function drawGacha() {
      const characters = ["스폰지밥", "징징이", "뚱이", "Mr. Krabs", "샌디", "플랑크톤"];
      if (coins < 10) return alert("코인이 부족합니다!");
      coins -= 10;
      const picked = characters[Math.floor(Math.random() * characters.length)];
      if (!pokedex.includes(picked)) pokedex.push(picked);
      updateCoins();
      updatePokedex();
      localStorage.setItem("pokedex", JSON.stringify(pokedex));
      saveToServer();
      document.getElementById("gachaResult").innerText = `축하합니다! ${picked} 획득!`;
    }

    function updatePokedex() {
      const list = document.getElementById("pokedexList");
      list.innerHTML = "";
      pokedex.forEach(char => {
        const li = document.createElement("li");
        li.textContent = char;
        list.appendChild(li);
      });
    }

    function orderMenu(item, price) {
      if (coins < price) return alert("코인이 부족합니다!");
      coins -= price;
      updateCoins();
      alert(`${item} 주문 완료!`);
    }

    function buyItem(item, price) {
      if (coins < price) return alert("코인이 부족합니다!");
      coins -= price;
      updateCoins();
      alert(`${item} 구매 완료!`);
    }

    function tellJoke() {
      const jokes = [
        "뚱이: '왜 의자가 하나도 없어?' 스폰지밥: '그건 널 위한 자리야!'",
        "징징이: '오늘도 일해야 해...' 플랑크톤: '난 또 망했지롱~'",
        "스폰지밥: '버거 100개요!' / 집게사장: '돈은?' / 스폰지밥: 'ㅋㅋ;;;'"
      ];
      const joke = jokes[Math.floor(Math.random() * jokes.length)];
      document.getElementById("jokeOutput").innerText = joke;
    }

    function showAdmin() {
      document.getElementById("adminPopup").classList.remove("hidden");
    }

    function verifyAdmin() {
      const pw = document.getElementById("adminPassword").value;
      if (pw === "krabby123") {
        document.getElementById("adminPanel").classList.remove("hidden");
        document.getElementById("adminPopup").classList.add("hidden");
      } else {
        alert("비밀번호가 틀렸습니다.");
      }
    }

    function saveToServer() {
      if (!user) return;
      const ref = database.ref("users/" + user);
      ref.set({
        coins: coins,
        pokedex: pokedex
      });
    }

    function loadFromServer() {
      if (!user) return;
      const ref = database.ref("users/" + user);
      ref.once("value").then(snapshot => {
        const data = snapshot.val();
        if (data) {
          coins = data.coins || 0;
          pokedex = data.pokedex || [];
          updateCoins();
          updatePokedex();
        }
      });
    }

    function stopPlankton() {
      planktonAttacks++;
      document.getElementById("planktonStatus").innerText = `플랑크톤 ${planktonAttacks}번 막음!`;
    }

    // 페이지 로드 시 사용자 이름을 확인하고, 없는 경우 입력 받기
    if (!user) {
      document.getElementById("namePopup").classList.remove("hidden");
    } else {
      document.getElementById("displayName").innerText = user;
      loadFromServer();
    }
  </script>
</body>
</html>
