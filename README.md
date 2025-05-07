# Create a full HTML file with integrated JavaScript for the Krusty Krab game
html_code = """
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
  <button onclick="showSection('enhancement')">⚒️ 강화</button>
  <button onclick="showSection('enhanceGacha')">✨ 강화 가챠</button>
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

  <section id="enhancement" class="hidden">
    <h2>캐릭터 강화</h2>
    <p>현재 강화 레벨: <span id="enhanceLevelDisplay">0</span></p>
    <p>성공 시 능력치 증가: <span id="enhancePowerDisplay">+0%</span></p>
    <button onclick="upgradeCharacter()">강화하기 (강화석 1개)</button>
  </section>

  <section id="enhanceGacha" class="hidden">
    <h2>강화 가챠</h2>
    <button onclick="drawEnhanceGacha()">강화 가챠 뽑기 (5코인)</button>
    <p id="enhanceGachaResult"></p>
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

  <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
  <script>
    // JavaScript 통합 로직은 다음 셀에서 이어서 제공됩니다.
  </script>
</body>
</html>
"""

# Save the HTML file for the user
file_path = "/mnt/data/krusty_krab_game.html"
with open(file_path, "w", encoding="utf-8") as f:
    f.write(html_code)

file_path  # Return the file path to the user for download
