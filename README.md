<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>크러스티 크랩 웹게임</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-image: url('https://i.imgur.com/kwkYZzr.jpg'); /* 집게리아 스타일 배경 */
      background-size: cover;
      background-position: center;
      color: #333;
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
    section {
      background-color: rgba(255, 255, 255, 0.85);
      border-radius: 10px;
      margin: 15px auto;
      padding: 15px;
      width: 90%;
      max-width: 600px;
    }
    #namePopup {
      background-color: #fff3cd;
      border: 2px solid #ffa000;
      padding: 20px;
      font-weight: bold;
      color: #d17b00;
      border-radius: 10px;
    }
  </style>
</head>
<body>
  <h1>🏖️ 크러스티 크랩에 오신 걸 환영합니다!</h1>
  <p>손님: <span id="displayName"></span> / 코인: <span id="coinDisplay">0</span></p>

<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>크러스티 크랩 웹게임</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      margin: 0;
      padding: 0;
      background-image: url('https://i.imgur.com/jv1EMsY.jpg'); /* 집게리아 배경 */
      background-size: cover;
      background-position: center;
      color: #222;
    }
    header {
      background-color: rgba(255, 255, 255, 0.9);
      padding: 15px;
      text-align: center;
      border-bottom: 3px solid #ffca28;
    }
    h1 {
      margin: 0;
    }
    nav {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      background-color: rgba(255, 255, 255, 0.85);
      padding: 10px;
      border-bottom: 2px solid #fbc02d;
    }
    nav button {
      margin: 5px;
      padding: 10px 15px;
      background-color: #ffca28;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    nav button:hover {
      background-color: #f57f17;
      color: white;
    }
    .hidden {
      display: none;
    }
    section {
      padding: 20px;
      background-color: rgba(255, 255, 255, 0.9);
      margin: 15px;
      border-radius: 10px;
    }
    input, button {
      font-size: 1em;
    }
    #namePopup {
      background-color: #fff8e1;
      border: 2px solid #ffb300;
      padding: 20px;
      font-weight: bold;
      color: #d17b00;
    }
  </style>
</head>
<body>
  <header>
    <h1>🏖️ 크러스티 크랩에 오신 걸 환영합니다!</h1>
    <p>손님: <span id="displayName"></span> / 코인: <span id="coinDisplay">0</span></p>
  </header>
  <nav>
    <button onclick="showSection('menu')">🍔 오늘의 메뉴</button>
    <button onclick="showSection('burgerGame')">🍳 햄버거 게임</button>
    <button onclick="showSection('planktonGame')">⚔️ 플랑크톤의 침략</button>
    <button onclick="showSection('gacha')">🎰 가챠</button>
    <button onclick="showSection('shop')">🛒 상점</button>
    <button onclick="showSection('pokedex')">📘 도감</button>
    <button onclick="showSection('jokes')">🎭 개그</button>
    <button onclick="showSection('regulars')">🌟 단골</button>
    <button onclick="showSection('afk')">📺 잠수 채널</button>
    <button onclick="showAdmin()">🔑 관리자</button>
  </nav>
    <div id="namePopup" class="hidden">
    <h2>처음 방문하셨군요!</h2>
    <p>이름을 입력하세요:</p>
    <input type="text" id="nameInput" placeholder="닉네임 입력" />
    <button onclick="saveName()">시작하기</button>
  </div>

  <section id="menu" class="hidden">
    <h2>🍽 오늘의 메뉴</h2>
    <p id="todayMenu">메뉴를 불러오는 중...</p>
  </section>

  <section id="burgerGame" class="hidden">
    <h2>🍔 햄버거 조합 미니게임</h2>
    <p>시간 내에 올바른 재료 조합을 선택하세요!</p>
    <div id="burgerIngredients"></div>
    <button onclick="startBurgerGame()">게임 시작</button>
    <p id="burgerStatus"></p>
 
  <section id="planktonGame" class="hidden">
    <h2>🦠 플랑크톤 침략</h2>
    <button onclick="startPlanktonGame()">게임 시작</button>
    <div id="planktonArea"></div>
    <p id="planktonStatus"></p>
  </section>

  <section id="gacha" class="hidden">
    <h2>🎰 가챠 뽑기</h2>
    <button onclick="rollGacha()">뽑기 (5 코인)</button>
    <p id="gachaResult"></p>
  </section>

  <section id="shop" class="hidden">
    <h2>🏪 상점</h2>
    <p>캐릭터 강화석과 아이템을 구매하세요.</p>
    <!-- 상점 내용 추가 가능 -->
  </section>

  <section id="pokedex" class="hidden">
    <h2>📖 캐릭터 도감</h2>
    <div id="pokedexList"></div>
  </section>

  <section id="jokes" class="hidden">
    <h2>🎭 개그 코너</h2>
    <button onclick="showJoke()">하하하! 😂</button>
    <p id="jokeText"></p>
  </section>

  <section id="regulars" class="hidden">
    <h2>🌟 단골 손님 목록</h2>
    <ul id="regularList"></ul>
  </section>

  <section id="afk" class="hidden">
    <h2>📺 잠수 채널</h2>
    <p>황금 스폰지밥이 있다면 1시간마다 1코인을 받습니다.</p>
    <button onclick="activateAFK()">잠수 시작</button>
    <p id="afkStatus"></p>
  </section>

  <section id="adminSection" class="hidden">
    <h2>🔐 관리자 패널</h2>
    <input type="password" id="adminPass" placeholder="비밀번호 입력" />
    <button onclick="verifyAdmin()">확인</button>
    <div id="adminPanel" class="hidden">
      <input type="text" id="targetUser" placeholder="유저 이름" />
      <input type="number" id="coinAmount" placeholder="코인 수" />
      <button onclick="grantCoins()">코인 지급</button>
      <p id="adminStatus"></p>
    </div>
  </section>
<script>
  let userName = localStorage.getItem("userName");
  let coins = parseInt(localStorage.getItem("coins")) || 0;
  let hasGoldenSponge = localStorage.getItem("goldenSponge") === "true";
  let afkActive = false;

  const menuList = [
    "치즈버거 세트", "불고기버거", "크랩버거 스페셜", "스폰지 세트",
    "징징이 정식", "황금 해파리버거", "해삼튀김 콤보", "킹크랩 라면"
  ];

  const jokeList = [
    "스폰지밥이 햄버거를 못 만들면? 크랩패닉!",
    "징징이는 왜 항상 화났을까? 감정 조절기가 없었대!",
    "플랑크톤이 성공 못 하는 이유? 마케팅 전공 안 했대!",
    "집게사장이 좋아하는 음악? 돈돈돈~♪"
  ];

  const regularList = ["만수르", "뚱이", "해파리광", "조이 보이", "진격의 손님"];

  function saveName() {
    const input = document.getElementById("nameInput").value.trim();
    if (input) {
      userName = input;
      localStorage.setItem("userName", userName);
      localStorage.setItem("coins", coins);
      document.getElementById("namePopup").classList.add("hidden");
      showMainUI();
    }
  }

  function showMainUI() {
    document.querySelectorAll("section").forEach(s => s.classList.remove("hidden"));
    document.getElementById("todayMenu").innerText =
      menuList[new Date().getDate() % menuList.length];
    updateRegulars();
  }

  function showJoke() {
    const joke = jokeList[Math.floor(Math.random() * jokeList.length)];
    document.getElementById("jokeText").innerText = joke;
  }

  function updateRegulars() {
    const list = document.getElementById("regularList");
    list.innerHTML = "";
    regularList.forEach(name => {
      const li = document.createElement("li");
      li.innerText = name;
      list.appendChild(li);
    });
  }

  window.onload = () => {
    if (!userName) {
      document.getElementById("namePopup").classList.remove("hidden");
    } else {
      showMainUI();
    }
  };
</script>

<script>
  // 관리자 기능
  function openAdminPanel() {
    const pw = prompt("관리자 비밀번호를 입력하세요:");
    if (pw === "komq3244") {
      const target = prompt("지급할 유저 이름:");
      const amount = parseInt(prompt("지급할 코인 수:"));
      if (target && !isNaN(amount)) {
        if (target === userName) {
          coins += amount;
          localStorage.setItem("coins", coins);
          updateCoinDisplay();
          alert(`${amount} 코인이 지급되었습니다.`);
        } else {
          alert("이름이 현재 유저와 일치하지 않습니다.");
        }
      }
    } else {
      alert("비밀번호가 틀렸습니다.");
    }
  }

  function updateCoinDisplay() {
    document.getElementById("coinDisplay").innerText = `코인: ${coins}`;
  }

  // 햄버거 생산
  let burgerCount = 0;
  function produceBurger() {
    burgerCount++;
    document.getElementById("burgerCount").innerText = `버거: ${burgerCount}`;
  }

  function sellBurger() {
    if (burgerCount > 0) {
      const earn = burgerCount;
      coins += earn;
      burgerCount = 0;
      document.getElementById("burgerCount").innerText = `버거: 0`;
      updateCoinDisplay();
    }
  }

  // 황금 스폰지밥 잠수 채널
  function startAFK() {
    if (!hasGoldenSponge) {
      alert("황금 스폰지밥이 필요합니다!");
      return;
    }
    if (!afkActive) {
      afkActive = true;
      alert("잠수 채널 시작! 1시간마다 코인 획득.");
      setInterval(() => {
        coins += 1;
        localStorage.setItem("coins", coins);
        updateCoinDisplay();
      }, 3600000); // 1시간마다
    }
  }

  // 가챠 시스템
  function pullGacha() {
    if (coins < 5) {
      alert("코인이 부족합니다!");
      return;
    }
    coins -= 5;
    updateCoinDisplay();

    const result = Math.random();
    let reward = "일반 스폰지밥";
    if (result < 0.05) {
      reward = "황금 스폰지밥";
      hasGoldenSponge = true;
      localStorage.setItem("goldenSponge", "true");
    }
    alert(`획득한 캐릭터: ${reward}`);
  }

  // DOM 연동
  setInterval(produceBurger, 1000); // 1초마다 자동 생산
</script>
<script>
  // 메뉴 전환
  function showSection(sectionId) {
    const sections = document.querySelectorAll('.game-section');
    sections.forEach(sec => sec.classList.remove('active'));
    document.getElementById(sectionId).classList.add('active');
  }

  // 오늘의 메뉴
  function getTodayMenu() {
    const menus = ["더블 패티버거", "치즈 크랩버거", "집게치킨", "불고기 버거", "비밀 레시피 버거"];
    const today = new Date().getDate();
    return menus[today % menus.length];
  }
  document.getElementById("menuDisplay").innerText = `오늘의 메뉴: ${getTodayMenu()}`;

  // 단골 손님 목록
  const guests = ["뚱이", "만수르", "해파리 사장님", "할머니 손님", "바다곰"];
  function showFrequentGuests() {
    alert(`단골 손님 목록:\n- ${guests.join("\n- ")}`);
  }

  // 개그 코너
  function showJoke() {
    const jokes = [
      "스폰지밥이 버거 만들다 웃은 이유는? 패티보며 패티~하고 웃어서!",
      "플랑크톤이 가게 안 들어가는 이유는? 입장료가 있어서!",
      "징징이는 왜 항상 찡그릴까? 성격이 '징'징거리니까!"
    ];
    const rand = Math.floor(Math.random() * jokes.length);
    alert(jokes[rand]);
  }

  // 플랑크톤 침략 게임
  let planktonScore = 0;
  function startPlanktonGame() {
    const plankton = document.getElementById("plankton");
    plankton.style.display = "block";
    movePlankton();
  }

  function movePlankton() {
    const plankton = document.getElementById("plankton");
    const x = Math.random() * 80;
    const y = Math.random() * 80;
    plankton.style.left = x + "%";
    plankton.style.top = y + "%";
  }

  function hitPlankton() {
    planktonScore++;
    document.getElementById("planktonScore").innerText = `점수: ${planktonScore}`;
    movePlankton();
  }
</script>
<style>
  body {
    margin: 0;
    font-family: 'Arial', sans-serif;
    background: url('https://i.imgur.com/3TzK4d3.jpg') no-repeat center center fixed;
    background-size: cover;
    color: #fff;
  }

  .container {
    padding: 20px;
    background: rgba(0, 0, 0, 0.6);
    margin: 20px auto;
    max-width: 800px;
    border-radius: 12px;
    box-shadow: 0 0 12px #000;
  }

  h1 {
    text-align: center;
    color: #ffcc00;
    font-family: 'Comic Sans MS', cursive;
  }

  .menu-buttons {
    text-align: center;
    margin-bottom: 20px;
  }

  .menu-buttons button {
    padding: 10px 20px;
    margin: 5px;
    background-color: #ffcc00;
    border: none;
    border-radius: 8px;
    font-size: 16px;
    cursor: pointer;
    transition: 0.2s;
  }

  .menu-buttons button:hover {
    background-color: #ffaa00;
  }

  .game-section {
    display: none;
  }

  .game-section.active {
    display: block;
  }

  #plankton {
    width: 50px;
    height: 50px;
    background: url('https://i.imgur.com/bCzqW9g.png') no-repeat center/contain;
    position: absolute;
    cursor: pointer;
    display: none;
  }

  .info-bar {
    text-align: center;
    margin-top: 10px;
    font-size: 18px;
    color: #00ffcc;
  }

  .floating-button {
    position: fixed;
    bottom: 20px;
    right: 20px;
    background: #ff3399;
    color: white;
    padding: 12px 18px;
    border: none;
    border-radius: 50px;
    cursor: pointer;
    font-weight: bold;
    box-shadow: 0 0 10px #ff3399;
  }
</style>
