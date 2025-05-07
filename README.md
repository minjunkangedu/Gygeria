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
  <button onclick="showSection('gacha')">🎰 가챠</button>
  <button onclick="showSection('pokedex')">📘 도감</button>
  <button onclick="showSection('shop')">🛒 상점</button>
  <button onclick="showSection('jokes')">🎭 개그</button>
  <button onclick="showSection('regulars')">🌟 단골</button>
  <button onclick="showSection('planktonGame')">⚔️ 플랑크톤의 침략</button>
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

  <!-- 게임 섹션들 -->
  <section id="menu" class="hidden"><h2>오늘의 메뉴</h2></section>
  <section id="gacha" class="hidden"><h2>가챠</h2></section>
  <section id="pokedex" class="hidden"><h2>도감</h2></section>
  <section id="shop" class="hidden"><h2>상점</h2></section>
  <section id="jokes" class="hidden"><h2>개그 코너</h2></section>
  <section id="regulars" class="hidden"><h2>단골 손님</h2></section>
  <section id="planktonGame" class="hidden"><h2>플랑크톤의 침략</h2></section>

  <script>
    let user = localStorage.getItem("userName") || "";
    let coins = parseInt(localStorage.getItem("coins")) || 0;

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
      if (target === user && !isNaN(amount)) {
        coins += amount;
        updateCoins();
        alert(`${target}님에게 ${amount} 코인을 지급했습니다.`);
      } else {
        alert("대상 이름이 현재 사용자와 일치하지 않거나 금액이 유효하지 않습니다.");
      }
    }

    function updateCoins() {
      document.getElementById("coinDisplay").innerText = coins;
      localStorage.setItem("coins", coins);
    }

    // 페이지 로드시
    if (!user) {
      document.getElementById("namePopup").classList.remove("hidden");
    } else {
      document.getElementById("displayName").innerText = user;
      updateCoins();
    }
  </script>
</body>
</html>
<!-- 햄버거 생산 시스템 -->
<section id="burgerProduction" class="hidden">
  <h2>🍔 스폰지밥의 햄버거 생산</h2>
  <p>생산된 햄버거 수: <span id="burgerCount">0</span></p>
  <button onclick="produceBurger()">즉시 생산</button>
</section>

<script>
  let burgerCount = parseInt(localStorage.getItem("burgers")) || 0;

  function produceBurger() {
    burgerCount++;
    updateBurgerCount();
  }

  function updateBurgerCount() {
    document.getElementById("burgerCount").innerText = burgerCount;
    localStorage.setItem("burgers", burgerCount);
  }

  // 자동 생산 (1초마다 1개씩)
  setInterval(() => {
    burgerCount++;
    updateBurgerCount();
  }, 1000);
</script>
<!-- 사용자 이름 설정 창 -->
<div id="namePopup" class="popup">
  <h3>처음 오셨군요!</h3>
  <p><strong>당신의 이름을 정해주세요:</strong></p>
  <input id="usernameInput" placeholder="이름 입력"/>
  <button onclick="saveName()">저장</button>
</div>

<!-- 관리자 모드 -->
<section id="adminPanel" class="hidden">
  <h2>🔒 관리자 패널</h2>
  <p>유저 이름:</p>
  <input id="targetUser" placeholder="지급할 유저 이름" />
  <p>코인 수:</p>
  <input type="number" id="coinAmount" placeholder="코인 수" />
  <button onclick="addCoins()">코인 지급</button>
</section>

<!-- 관리자 인증 팝업 -->
<div id="adminPopup" class="popup hidden">
  <h3>관리자 비밀번호 입력</h3>
  <input type="password" id="adminPassword" placeholder="비밀번호 입력" />
  <button onclick="verifyAdmin()">입력</button>
</div>

<script>
  let user = localStorage.getItem("userName") || "";
  if (!user) {
    document.getElementById("namePopup").classList.remove("hidden");
  } else {
    document.getElementById("displayName").innerText = user;
    loadFromServer();
  }

  function saveName() {
    const input = document.getElementById("usernameInput").value.trim();
    if (input === "") return alert("이름을 입력해주세요.");
    user = input;
    localStorage.setItem("userName", user);
    document.getElementById("namePopup").classList.add("hidden");
    document.getElementById("displayName").innerText = user;
    loadFromServer();
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
    const name = document.getElementById("targetUser").value.trim();
    const amount = parseInt(document.getElementById("coinAmount").value);
    if (!name || isNaN(amount)) return alert("이름과 코인을 정확히 입력해주세요.");
    
    const ref = firebase.database().ref("users/" + name);
    ref.once("value").then(snapshot => {
      const data = snapshot.val() || {};
      const currentCoins = data.coins || 0;
      ref.set({
        ...data,
        coins: currentCoins + amount
      });
      alert(`${name}님에게 ${amount}코인을 지급했습니다.`);
    });
  }
</script>
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
  <button onclick="showSection('burgerGame')">🍳 햄버거 미니게임</button>
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
  document.getElementById("namePopup").classList.add("hidden");
  document.getElementById("displayName").innerText = user;
  loadFromServer();
}

function drawGacha() {
  if (coins < 10) return alert("코인이 부족합니다!");
  const characters = ["스폰지밥", "징징이", "뚱이", "Mr. Krabs", "샌디", "플랑크톤"];
  const picked = characters[Math.floor(Math.random() * characters.length)];
  if (!pokedex.includes(picked)) pokedex.push(picked);
  coins -= 10;
  updateUI();
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
  if (pw === "komq3244") {
    document.getElementById("adminPanel").classList.remove("hidden");
    document.getElementById("adminPopup").classList.add("hidden");
  } else {
    alert("비밀번호가 틀렸습니다.");
  }
}

function addCoins() {
  const target = document.getElementById("adminUser").value.trim();
  const amount = parseInt(document.getElementById("coinAmount").value);
  if (!target || isNaN(amount)) return alert("이름과 코인 수를 확인해주세요.");
  const ref = database.ref("users/" + target);
  ref.once("value").then(snapshot => {
    const data = snapshot.val() || { coins: 0 };
    ref.update({ coins: (data.coins || 0) + amount });
    alert(`${target}에게 ${amount}코인 지급 완료`);
  });
}

function stopPlankton() {
  planktonAttacks++;
  document.getElementById("planktonStatus").innerText = `플랑크톤 ${planktonAttacks}번 막음!`;
}

function drawEnhanceGacha() {
  if (coins < 5) return alert("코인이 부족합니다!");
  coins -= 5;
  const roll = Math.random();
  let result = "";
  if (roll < 0.6) {
    enhanceStones++;
    result = "강화석 1개 획득!";
  } else {
    blessings++;
    result = "집게의 축복 1개 획득!";
  }
  updateUI();
  document.getElementById("enhanceGachaResult").innerText = result;
}

function upgradeCharacter() {
  if (enhanceStones < 1) return alert("강화석이 부족합니다!");
  enhanceStones--;
  const failChance = enhanceLevel >= 50 ? 0.3 : 0.1;
  const resetChance = enhanceLevel >= 50 ? 0.05 : 0;
  const roll = Math.random();
  if (roll < resetChance) {
    enhanceLevel = 0;
    alert("강화 실패! 강화 레벨이 초기화되었습니다.");
  } else if (roll < failChance) {
    alert("강화 실패! 레벨 유지.");
  } else {
    enhanceLevel++;
    alert(`강화 성공! 현재 레벨: ${enhanceLevel}`);
  }
  updateUI();
}

// 햄버거 생산 루프: 스폰지밥이 초당 1코인 생성
setInterval(() => {
  if (pokedex.includes("스폰지밥")) {
    coins += 1;
    updateCoins();
  }
}, 1000);

// 손님 이벤트: 뚱이 또는 만수르 등장 시 3코인 보상
setInterval(() => {
  const chance = Math.random();
  if (chance < 0.05) {
    coins += 3;
    alert("손님 이벤트! 뚱이 또는 만수르가 방문해 3코인을 획득!");
    updateCoins();
  }
}, 10000);

// 페이지 로드시 이름 확인 및 강조
window.onload = () => {
  if (!user) {
    document.getElementById("namePopup").classList.remove("hidden");
    document.getElementById("usernameInput").focus();
  } else {
    document.getElementById("displayName").innerText = user;
    loadFromServer();
  }
};
