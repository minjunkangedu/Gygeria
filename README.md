<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>크러스티 크랩 웹 게임</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background: #f0f8ff;
      text-align: center;
      margin: 0;
      padding: 0;
    }
    header, nav, section {
      padding: 20px;
    }
    nav button {
      margin: 5px;
      padding: 10px 20px;
      font-size: 16px;
    }
    .game-section {
      display: none;
    }
    .visible {
      display: block;
    }
    .hidden {
      display: none;
    }
    /* 애니메이션 효과 */
    .fade-in {
      animation: fadeIn 1s ease-in;
    }
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
    /* 캐릭터 이미지 스타일 */
    .character-img {
      width: 100px;
      height: auto;
    }
  </style>
</head>
<body>

  <header>
    <h1>🏖️ 크러스티 크랩 게임 센터</h1>
    <p>보유 코인: <span id="coinDisplay">0</span></p>
    <p>보유 햄버거 수: <span id="burgerCount">0</span></p>
  </header>

  <nav>
    <button onclick="showSection('menu')">메인 메뉴</button>
    <button onclick="showSection('planktonGame')">플랑크톤 침략</button>
    <button onclick="showSection('burgerGame')">햄버거 만들기</button>
    <button onclick="showSection('enhancementGacha')">강화 가챠</button>
    <button onclick="showSection('enhancement')">캐릭터 강화</button>
  </nav>

  <section id="menu" class="game-section visible">
    <h2>게임을 선택하세요!</h2>
  </section>

  <section id="planktonGame" class="game-section hidden">
    <h2>플랑크톤 침략!</h2>
    <p>게살버거를 지켜라! 제한 시간 내 최대한 많이 막아보세요.</p>
    <p id="planktonCount">막은 횟수: 0</p>
    <p>남은 시간: <span id="planktonTimer">10</span>초</p>
    <button onclick="blockPlankton()">막기!</button>
  </section>

  <section id="burgerGame" class="game-section hidden">
    <h2>스폰지밥 햄버거 만들기</h2>
    <p>제시된 재료 순서대로 클릭하세요!</p>
    <div id="orderDisplay"></div>
    <div id="ingredientButtons">
      <button onclick="clickIngredient('빵')">빵</button>
      <button onclick="clickIngredient('야채')">야채</button>
      <button onclick="clickIngredient('패티')">패티</button>
      <button onclick="clickIngredient('치즈')">치즈</button>
      <button onclick="clickIngredient('완성')">완성</button>
    </div>
    <p id="burgerMessage"></p>
    <p>남은 시간: <span id="burgerTimer">30</span>초</p>
  </section>

  <section id="enhancementGacha" class="game-section hidden">
    <h2>강화 가챠</h2>
    <p>코인 5개로 강화석 또는 집게의 축복을 뽑습니다.</p>
    <button onclick="drawEnhancementGacha()">가챠 뽑기</button>
    <p id="enhanceGachaResult"></p>
  </section>

  <section id="enhancement" class="game-section hidden">
    <h2>캐릭터 강화</h2>
    <p>보유 강화석: <span id="enhanceStones">0</span></p>
    <p>집게의 축복: <span id="krabBlessings">0</span></p>
    <label for="characterSelect">강화할 캐릭터 선택:</label>
    <select id="characterSelect">
      <option value="spongebob">스폰지밥</option>
      <option value="squidward">징징이</option>
      <option value="krabs">집게사장</option>
    </select>
    <p>현재 강화 단계: <span id="enhanceLevel">0</span></p>
    <button onclick="enhanceCharacter()">강화하기</button>
    <p id="enhanceResult"></p>
  </section>

  <script>
    let coin = 0;
    let burgers = 0;
    let enhanceStones = 0;
    let krabBlessings = 0;
    let enhanceLevel = 0;
    let unlockedAbilities = [];

    function updateCoinDisplay() {
      document.getElementById("coinDisplay").textContent = coin.toFixed(1);
    }

    function updateBurgersDisplay() {
      document.getElementById("burgerCount").textContent = burgers;
    }

    function showSection(id) {
      document.querySelectorAll(".game-section").forEach(s => s.classList.add("hidden"));
      document.getElementById(id).classList.remove("hidden");
    }

    function produceBurger() {
      burgers++;
      updateBurgersDisplay();
    }

    function sellBurger() {
      if (burgers > 0) {
        burgers--;
        coin += 0.1;
        updateCoinDisplay();
        checkSpecialCustomers();
        updateBurgersDisplay();
      }
    }

    function checkSpecialCustomers() {
      const rand = Math.random();
      if (rand < 0.01) {
        coin += 3;
        alert("만수르 등장! 코인 +3");
        updateCoinDisplay();
      } else if (rand < 0.02) {
        coin += 3;
        alert("뚱이 등장! 코인 +3");
        updateCoinDisplay();
      }
    }

    function drawEnhancementGacha() {
      if (coin < 5) {
        alert("코인이 부족합니다.");
        return;
      }
      coin -= 5;
      updateCoinDisplay();
      const isBlessing = Math.random() < 0.3;
      if (isBlessing) {
        krabBlessings++;
        document.getElementById("enhanceGachaResult").textContent = "집게의 축복 획득!";
      } else {
        enhanceStones++;
        document.getElementById("enhanceGachaResult").textContent = "강화석 획득!";
      }
      document.getElementById("enhanceStones").textContent = enhanceStones;
      document.getElementById("krabBlessings").textContent = krabBlessings;
    }

    function enhanceCharacter() {
      if (enhanceStones <= 0) {
        alert("강화석이 부족합니다.");
        return;
      }
      const failRate = enhanceLevel >= 50 ? 0.3 : 0.2;
      const resetRate = enhanceLevel >= 50 ? 0.1 : 0;
      enhanceStones--;
      if (Math.random() < failRate) {
        if (Math.random() < resetRate && krabBlessings <= 0) {
          enhanceLevel = 0;
          document.getElementById("enhanceResult").textContent = "강화 실패! 초기화됨!";
        } else {
          document.getElementById("enhanceResult").textContent = "강화 실패!";
        }
      } else {
        enhanceLevel++;
        document.getElementById("enhanceResult").textContent = `강화 성공! 현재 ${enhanceLevel}강`;
        checkAbilities("선택된 캐릭터", enhanceLevel);
      }
      document.getElementById("enhanceStones").textContent = enhanceStones;
      document.getElementById("krabBlessings").textContent = krabBlessings;
      document.getElementById("enhanceLevel").textContent = enhanceLevel;
    }

    function checkAbilities(character, level) {
      const unlocks = {
        10: "속도 증가",
        20: "추가 수익",
        30: "강화 성공률 상승",
        40: "햄버거 자동 판매 강화",
        50: "만수르 등장 확률 +1%",
        60: "집게의 축복 소비 확률 50%",
        70: "생산 속도 +50%",
        80: "판매 수익 2배",
        90: "가챠에서 축복 확률 증가",
        100: "모든 능력 최대 강화"
      };
      if (unlocks[level] && !unlockedAbilities.includes(`${character}-${level}`)) {
        unlockedAbilities.push(`${character}-${level}`);
        alert(`${character}이 ${level}강에서 능력 해금! (${unlocks[level]})`);
      }
    }

    // 루프 시작
    setInterval(produceBurger, 1000);
    setInterval(sellBurger, 2000);
  </script>

</body>
</html>
