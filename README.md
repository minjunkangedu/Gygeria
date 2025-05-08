<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>크러스티 크랩 웹 게임</title>
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
</head>
<body>
  <div class="container">
    <h1>🍔 크러스티 크랩 웹 게임</h1>
    <div class="menu-buttons">
      <button onclick="showSection('plankton')">플랑크톤 침략</button>
      <button onclick="showSection('burger')">햄버거 만들기</button>
      <button onclick="showJoke()">개그 보기</button>
      <button onclick="showRegulars()">단골 손님</button>
    </div>

    <div class="info-bar" id="dailyMenu"></div>

    <!-- 플랑크톤 침략 -->
    <div id="plankton" class="game-section">
      <p>플랑크톤을 클릭해서 막으세요!</p>
      <div id="planktonTarget"></div>
      <div>점수: <span id="planktonScore">0</span></div>
    </div>

    <!-- 햄버거 미니게임 -->
    <div id="burger" class="game-section">
      <p>제한 시간 안에 재료를 조합해서 햄버거를 완성하세요!</p>
      <button onclick="makeBurger()">🍔 햄버거 만들기</button>
      <div id="burgerResult"></div>
    </div>

    <!-- 개그 -->
    <div id="jokeSection" class="game-section">
      <p id="jokeText"></p>
    </div>

    <!-- 단골 -->
    <div id="regularsSection" class="game-section">
      <ul id="regularList"></ul>
    </div>
  </div>

  <div id="plankton" onclick="hitPlankton()"></div>

  <button class="floating-button" onclick="showRandomMenu()">📋 오늘의 메뉴</button>

  <script>
    // 날짜별 오늘의 메뉴
    const dailyMenus = [
      "치즈버거 + 감자튀김",
      "크래비 패티 스페셜",
      "해파리 젤리 버거",
      "플랑크톤 스튜",
      "스폰지 소다와 세트",
      "뚱이 버거 콤보",
      "집게사장 럭셔리 세트"
    ];

    function showRandomMenu() {
      const today = new Date().getDay();
      document.getElementById('dailyMenu').innerText = "오늘의 메뉴 🍽️: " + dailyMenus[today];
    }

    showRandomMenu();

    function showSection(id) {
      const sections = document.querySelectorAll('.game-section');
      sections.forEach(s => s.classList.remove('active'));
      if (id === 'plankton') startPlanktonGame();
      if (id === 'burger') document.getElementById('burger').classList.add('active');
    }

    // 플랑크톤 게임
    let planktonScore = 0;

    function startPlanktonGame() {
      document.getElementById('plankton').classList.add('active');
      const plankton = document.getElementById('plankton');
      plankton.style.display = 'block';
      movePlankton();
    }

    function movePlankton() {
      const plankton = document.getElementById('plankton');
      const x = Math.random() * (window.innerWidth - 50);
      const y = Math.random() * (window.innerHeight - 50);
      plankton.style.left = `${x}px`;
      plankton.style.top = `${y}px`;
    }

    function hitPlankton() {
      planktonScore++;
      document.getElementById('planktonScore').innerText = planktonScore;
      movePlankton();
    }

    // 햄버거 만들기
    function makeBurger() {
      const ingredients = ["패티", "양상추", "치즈", "피클", "토마토", "빵"];
      const result = ingredients.sort(() => Math.random() - 0.5).slice(0, 3);
      document.getElementById("burgerResult").innerText = "🧾 조합된 재료: " + result.join(", ");
    }

    // 개그 기능
    const jokes = [
      "스폰지밥: '이 햄버거 이름은 뭔가요?' 집게사장: '너 월급 깎임 버거다!'",
      "징징이: '오늘은 일 안 할래요.' 집게사장: '좋아, 내일부터도 안 해도 돼. 해고야!'",
      "플랑크톤: '이번엔 진짜 레시피 훔칠 거야!' -> 5초 후 체포됨.",
      "스폰지밥: '이게 바로 웃픈 현실 버거야.'",
      "뚱이: '나는 생각하지 않아. 그래서 행복해.'"
    ];

    function showJoke() {
      const joke = jokes[Math.floor(Math.random() * jokes.length)];
      const section = document.getElementById("jokeSection");
      document.querySelectorAll('.game-section').forEach(s => s.classList.remove('active'));
      section.classList.add("active");
      document.getElementById("jokeText").innerText = "🤣 " + joke;
    }

    // 단골 손님
    const regulars = [
      "뚱이 - 단골 1호",
      "버블버디 - 단골 2호",
      "레이디 피쉬 - 단골 3호",
      "만수르 - 럭셔리 VIP",
      "노인 물고기 - 매일 오는 손님"
    ];

    function showRegulars() {
      const section = document.getElementById("regularsSection");
      document.querySelectorAll('.game-section').forEach(s => s.classList.remove('active'));
      section.classList.add("active");
      const list = document.getElementById("regularList");
      list.innerHTML = '';
      regulars.forEach(name => {
        const li = document.createElement("li");
        li.innerText = "⭐ " + name;
        list.appendChild(li);
      });
    }
  </script>
</body>
</html>
