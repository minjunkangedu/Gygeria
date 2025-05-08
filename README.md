<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>스폰지밥 햄버거 게임</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f0f8ff;
      padding: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    h1 {
      color: #333;
      font-size: 2em;
    }
    .section {
      margin-bottom: 30px;
      width: 100%;
      max-width: 500px;
      text-align: center;
    }
    h2 {
      color: #333;
      font-size: 1.5em;
    }
    button {
      margin: 5px;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
      border-radius: 5px;
      background-color: #4CAF50;
      color: white;
      transition: 0.2s;
    }
    button:hover {
      background-color: #45a049;
    }
    p {
      font-size: 18px;
    }
    ul {
      list-style-type: none;
      padding: 0;
    }
    li {
      font-size: 18px;
    }
    #gacha-effect {
      font-size: 24px;
      font-weight: bold;
      color: #ff4500;
    }
    .hidden {
      display: none;
    }
  </style>
</head>
<body>

  <h1>스폰지밥 햄버거 게임</h1>

  <!-- 햄버거 만들기 미니게임 -->
  <div class="section">
    <h2>1️⃣ 햄버거 만들기 미니게임</h2>
    <p>랜덤한 햄버거를 만들고, 10개를 만들면 보상을 얻습니다!</p>
    <button onclick="startBurgerMaking()">햄버거 만들기 시작</button>
    <p id="burger-making-status"></p>
  </div>

  <!-- 플랑크톤의 침략 미니게임 -->
  <div class="section">
    <h2>2️⃣ 플랑크톤의 침략</h2>
    <p>집게리아를 침략하려는 플랑크톤의 계획을 막으세요! 문제를 풀어야 합니다.</p>
    <button onclick="startPlanktonInvasion()">플랑크톤 침략 막기</button>
    <p id="plankton-invasion-status"></p>
  </div>

  <!-- 강화 시스템 -->
  <div class="section">
    <h2>3️⃣ 강화 시스템 (최대 100강)</h2>
    <p>스폰지밥 강화 단계: <span id="spongebob-level">0</span>강</p>
    <p>징징이 강화 단계: <span id="squidward-level">0</span>강</p>
    <p>🎯 강화석 보유: <span id="stones">5</span>개</p>
    <button onclick="enhance('spongebob')">스폰지밥 강화</button>
    <button onclick="enhance('squidward')">징징이 강화</button>
    <p id="enhance-result"></p>
  </div>

  <!-- 가챠 시스템 -->
  <div class="section">
    <h2>4️⃣ 가챠 시스템 (코인 10 사용)</h2>
    <button onclick="pullGacha()">가챠 뽑기</button>
    <p id="gacha-result"></p>
    <div id="gacha-effect"></div>
    <h3>🎒 보유 캐릭터</h3>
    <ul id="character-list"></ul>
  </div>

  <!-- 단골 랭킹 시스템 -->
  <div class="section">
    <h2>5️⃣ 단골 랭킹 시스템</h2>
    <p>현재 랭킹:</p>
    <div id="rank-list"></div>
    <p>🛒 햄버거 구매 횟수: <span id="burger-purchases">0</span></p>
    <button onclick="buyBurger()">햄버거 구매 (10코인)</button>
    <p id="rank-message"></p>
  </div>

  <!-- 상점 시스템 -->
  <div class="section">
    <h2>6️⃣ 상점</h2>
    <p>오늘의 추천 메뉴:</p>
    <button onclick="buySpecialItem()">추천 메뉴 구매 (20코인)</button>
    <p id="shop-status"></p>
  </div>

  <!-- PVP 시스템 -->
  <div class="section">
    <h2>7️⃣ PVP 시스템</h2>
    <p>🔮 선택 가능한 전투 모드:</p>
    <button onclick="startPvP('ai')">AI 전투</button>
    <button onclick="startPvP('user')">유저 간 전투</button>
    <button onclick="startPvP('mirror')">미러전</button>
    <div id="pvp-result"></div>
  </div>

  <script>
    // 초기화 변수
    let coins = localStorage.getItem('coins') || 100; // 초기 코인 100
    let burgerPurchases = localStorage.getItem('burgerPurchases') || 0;
    let playerRank = localStorage.getItem('playerRank') || '';
    let topRank = { name: '뚱이', purchases: 300, title: '뚱이를 이긴 자' };

    // 강화 시스템 변수
    let spongebobLevel = localStorage.getItem('spongebobLevel') || 0;
    let squidwardLevel = localStorage.getItem('squidwardLevel') || 0;
    let stones = localStorage.getItem('stones') || 5;

    // 캐릭터 설정
    const characters = [
      { name: '황금 스폰지밥', grade: 'SSR', rate: 0.5, color: 'gold' },
      { name: '산디', grade: 'SR', rate: 5, color: 'purple' },
      { name: '징징이', grade: 'R', rate: 20, color: 'blue' },
      { name: '플랑크톤', grade: 'N', rate: 74.5, color: 'gray' }
    ];

    const characterInventory = JSON.parse(localStorage.getItem('characterInventory')) || [];

    // 게임 상태 저장
    function saveGameState() {
      localStorage.setItem('coins', coins);
      localStorage.setItem('burgerPurchases', burgerPurchases);
      localStorage.setItem('playerRank', playerRank);
      localStorage.setItem('spongebobLevel', spongebobLevel);
      localStorage.setItem('squidwardLevel', squidwardLevel);
      localStorage.setItem('stones', stones);
      localStorage.setItem('characterInventory', JSON.stringify(characterInventory));
    }

    // 햄버거 만들기 미니게임
    function startBurgerMaking() {
      let successCount = 0;
      let interval = setInterval(() => {
        if (successCount >= 10) {
          clearInterval(interval);
          document.getElementById('burger-making-status').innerText = '성공! 10개의 햄버거를 만들었습니다. 코인 2개 획득!';
          coins += 2;
          saveGameState();
          return;
        }
        successCount++;
      }, 1000);
    }

    // 플랑크톤의 침략 미니게임
    function startPlanktonInvasion() {
      const question = "스폰지밥의 직업은 무엇인가요?";
      const answer = "집게리아 직원";

      const userAnswer = prompt(question);
      if (userAnswer === answer) {
        document.getElementById('plankton-invasion-status').innerText = '성공! 플랑크톤의 침략을 막았습니다.';
      } else {
        document.getElementById('plankton-invasion-status').innerText = '실패! 플랑크톤이 집게리아를 침략했습니다.';
      }
    }

    // 강화 시스템
    function enhance(character) {
      if (stones <= 0) {
        document.getElementById('enhance-result').innerText = '❌ 강화석이 부족합니다!';
        return;
      }

      let currentLevel = character === 'spongebob' ? spongebobLevel : squidwardLevel;
      let maxLevel = 100;

      if (currentLevel >= maxLevel) {
        document.getElementById('enhance-result').innerText = '이미 최고 레벨입니다!';
        return;
      }

      stones--;
      const upgradeChance = Math.random();

      if (upgradeChance < 0.5) {
        document.getElementById('enhance-result').innerText = `${character === 'spongebob' ? '스폰지밥' : '징징이'} 강화 실패!`;
      } else {
        if (character === 'spongebob') {
          spongebobLevel++;
          document.getElementById('spongebob-level').innerText = spongebobLevel;
        } else {
          squidwardLevel++;
          document.getElementById('squidward-level').innerText = squidwardLevel;
        }
        document.getElementById('enhance-result').innerText = `${character === 'spongebob' ? '스폰지밥' : '징징이'} 강화 성공!`;
      }
      document.getElementById('stones').innerText = stones;
      saveGameState();
    }

    // 가챠 시스템
    function pullGacha() {
      if (coins < 10) {
        document.getElementById('gacha-result').innerText = '❌ 코인이 부족합니다!';
        return;
      }

      coins -= 10;

      const roll = Math.random() * 100;
      let cumulative = 0;
      let result;

      for (const char of characters) {
        cumulative += char.rate;
        if (roll <= cumulative) {
          result = char;
          break;
        }
      }

      characterInventory.push(result);
      saveGameState();

      document.getElementById('gacha-result').innerText = `🎉 ${result.name} (등급: ${result.grade})을/를 획득했습니다!`;
      document.getElementById('gacha-effect').innerText = `🔥 ${result.grade} 등급 획득!`;
      document.getElementById('gacha-effect').style.color = result.color;

      updateCharacterList();
    }

    // 캐릭터 목록 업데이트
    function updateCharacterList() {
      const characterList = document.getElementById('character-list');
      characterList.innerHTML = '';
      for (const char of characterInventory) {
        const li = document.createElement('li');
        li.textContent = `${char.name} (${char.grade})`;
        characterList.appendChild(li);
      }
    }

    // 단골 랭킹 시스템
    function buyBurger() {
      if (coins >= 10) {
        coins -= 10;
        burgerPurchases++;
        playerRank = burgerPurchases > topRank.purchases ? topRank.title : '';
        document.getElementById('burger-purchases').innerText = burgerPurchases;
        document.getElementById('rank-message').innerText = playerRank
          ? `축하합니다! ${topRank.title} 칭호를 획득하셨습니다.`
          : `지금까지 ${burgerPurchases}번 구매하셨습니다.`;

        saveGameState();
      } else {
        alert('❌ 코인이 부족합니다!');
      }
    }

    // 상점 시스템
    function buySpecialItem() {
      if (coins >= 20) {
        coins -= 20;
        document.getElementById('shop-status').innerText = '✅ 추천 메뉴를 구매했습니다!';
        saveGameState();
      } else {
        alert('❌ 코인이 부족합니다!');
      }
    }

    // PVP 시스템
    function startPvP(mode) {
      let result;

      if (mode === 'ai') {
        result = 'AI와의 전투에서 승리했습니다!';
      } else if (mode === 'user') {
        result = '유저와의 전투에서 승리했습니다!';
      } else {
        result = '미러전에서 승리했습니다!';
      }

      document.getElementById('pvp-result').innerText = result;
    }

    // 게임 상태 초기화
    saveGameState();
    updateCharacterList();
  </script>

</body>
</html>
