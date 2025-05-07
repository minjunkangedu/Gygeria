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
    .hidden { display: none; }
    .burger-count { font-weight: bold; }
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
  <button onclick="showSection('planktonGame')">⚔️ 플랑크톤의 침략</button>
  <button onclick="showSection('enhancement')">⚒️ 강화</button>
  <button onclick="showSection('enhanceGacha')">✨ 강화 가챠</button>
  <button onclick="showSection('burgerGame')">🍳 햄버거 생산</button>
  <button onclick="showAdmin()">🔑 관리자</button>

  <!-- 사용자 이름 입력 -->
  <div id="namePopup" class="hidden">
    <p>처음 오셨군요! 이름을 정해주세요:</p>
    <input id="usernameInput" placeholder="이름 입력"/>
    <button onclick="saveName()">저장</button>
  </div>

  <!-- 관리자 비밀번호 입력창 -->
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

<script>
  function showAdmin() {
    document.getElementById("adminPopup").classList.remove("hidden");
  }

  function verifyAdmin() {
    const password = document.getElementById("adminPassword").value;
    if (password === "komq3244") {
      document.getElementById("adminPopup").classList.add("hidden");
      document.getElementById("adminPanel").classList.remove("hidden");
    } else {
      alert("비밀번호가 틀렸습니다.");
    }
  }

  function giveCoinsToUser() {
    const target = document.getElementById("targetUser").value.trim();
    const amount = parseInt(document.getElementById("coinAmount").value);
    if (!target || isNaN(amount)) {
      alert("유저 이름과 코인 수를 정확히 입력해주세요.");
      return;
    }
    const userRef = firebase.database().ref("users/" + target);
    userRef.once("value").then(snapshot => {
      const data = snapshot.val();
      if (data) {
        const newCoins = (data.coins || 0) + amount;
        userRef.update({ coins: newCoins });
        alert(`${target}님에게 ${amount} 코인을 지급했습니다.`);
      } else {
        alert("해당 유저를 찾을 수 없습니다.");
      }
    });
  }
</script>
  
  <!-- 각 섹션들 -->
  <section id="menu" class="hidden">
    <h2>오늘의 메뉴</h2>
    <button onclick="orderMenu('크래비 버거', 5)">크래비 버거 (5코인)</button>
    <button onclick="orderMenu('씨위드 샐러드', 4)">씨위드 샐러드 (4코인)</button>
    <h3>🌟 특별 메뉴</h3>
    <button onclick="orderMenu('황금버거', 10)">황금버거 (10코인)</button>
  </section>

  <section id="gacha" class="hidden">
    <h2>캐릭터 가챠</h2>
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
      <!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>플랑크톤 침략 게임</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #e0f7fa;
      padding: 20px;
      text-align: center;
    }
    button {
      margin: 10px;
      padding: 12px 20px;
      background-color: #26c6da;
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 16px;
      cursor: pointer;
    }
    button:hover {
      background-color: #0097a7;
    }
    #planktonStatus {
      font-size: 18px;
      margin-top: 20px;
      color: #004d40;
    }
  </style>
</head>
<body>

  <h1>플랑크톤의 침략! 게살버거를 지켜라!</h1>
  <p>플랑크톤이 침략 중입니다! 아래 버튼을 눌러 막아주세요!</p>

  <button onclick="stopPlankton()">플랑크톤 막기</button>

  <div id="planktonStatus">아직 막은 적이 없습니다.</div>

  <script>
    let planktonAttacks = 0;

    function stopPlankton() {
      planktonAttacks++;
      document.getElementById("planktonStatus").innerText = `플랑크톤 ${planktonAttacks}번 막음!`;
    }
  </script>

</body>
</html>
  <section id="enhancement" class="hidden">
    <h2>캐릭터 강화</h2>
    <p>현재 강화 레벨: <span id="enhanceLevelDisplay">0</span></p>
    <p>능력치 증가: <span id="enhancePowerDisplay">+0%</span></p>
    <button onclick="upgradeCharacter()">강화하기 (강화석 1개)</button>
  </section>

  <section id="enhanceGacha" class="hidden">
    <h2>강화 가챠</h2>
    <button onclick="drawEnhanceGacha()">강화 가챠 뽑기 (5코인)</button>
    <p id="enhanceGachaResult"></p>
  </section>

  <section id="burgerGame" class="hidden">
    <h2>스폰지밥 햄버거 생산</h2>
    <p>햄버거 개수: <span id="burgerCount">0</span></p>
    <p>강화 효과: <span id="burgerRateDisplay">1개/초</span></p>
  </section>

  <script>
    // Firebase 설정 (당신의 정보로 교체)
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
    let burgerCount = 0;
    let enhanceLevel = 0;
    let enhanceStones = 0;

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

    function stopPlankton() {
      planktonAttacks++;
      document.getElementById("planktonStatus").innerText = `플랑크톤 ${planktonAttacks}번 막음!`;
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
      database.ref("users/" + user).set({
        coins: coins,
        pokedex: pokedex,
        enhanceLevel: enhanceLevel,
        enhanceStones: enhanceStones
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
          updateCoins();
          updatePokedex();
          updateEnhancementDisplay();
        }
      });
    }

    function upgradeCharacter() {
      if (enhanceStones < 1) return alert("강화석이 부족합니다!");
      enhanceStones--;
      const success = Math.random() < 0.8;
      if (success) enhanceLevel++;
      updateEnhancementDisplay();
      saveToServer();
    }

    function updateEnhancementDisplay() {
      document.getElementById("enhanceLevelDisplay").innerText = enhanceLevel;
      document.getElementById("enhancePowerDisplay").innerText = `+${enhanceLevel}%`;
      document.getElementById("burgerRateDisplay").innerText = `${1 + enhanceLevel}% 속도`;
    }

    function drawEnhanceGacha() {
      if (coins < 5) return alert("코인이 부족합니다!");
      coins -= 5;
      const chance = Math.random();
      if (chance < 0.7) {
        enhanceStones++;
        document.getElementById("enhanceGachaResult").innerText = "강화석 획득!";
      } else {
        coins += 3;
        document.getElementById("enhanceGachaResult").innerText = "보너스 코인 +3!";
      }
      updateCoins();
      saveToServer();
    }

    // 햄버거 생산 루프
    setInterval(() => {
      burgerCount += 1 + Math.floor(enhanceLevel / 10);
      document.getElementById("burgerCount").innerText = burgerCount;
      if (Math.random() < 0.05) {
        coins += 3;
        alert("뚱이 or 만수르 등장! 보너스 3코인!");
        updateCoins();
      }
    }, 1000);

    // 이름 확인
    if (!user) {
      document.getElementById("namePopup").classList.remove("hidden");
    } else {
      document.getElementById("displayName").innerText = user;
      loadFromServer();
    }
  </script>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>
</body>
</html>
