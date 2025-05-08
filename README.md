<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>크러스티 크랩 게임</title>
  <style>
    body {
      font-family: 'Arial';
      background: url('https://i.imgur.com/4ZQ0fUQ.jpg') no-repeat center center fixed;
      background-size: cover;
      color: white;
      text-align: center;
      padding: 20px;
    }
    h1 {
      font-size: 40px;
      color: yellow;
      text-shadow: 2px 2px black;
    }
    .menu-btn {
      margin: 5px;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
    }
    .section {
      display: none;
      margin-top: 20px;
    }
    .visible {
      display: block !important;
    }
    .btn {
      padding: 10px;
      margin: 5px;
      background: orange;
      border: none;
      color: white;
      cursor: pointer;
      border-radius: 5px;
    }
    #plankton-area {
      position: relative;
      width: 320px;
      height: 200px;
      margin: 0 auto;
      background-color: rgba(0,0,0,0.4);
    }
    .plankton {
      width: 30px;
      height: 30px;
      background: green;
      color: white;
      border-radius: 50%;
      text-align: center;
      line-height: 30px;
      font-weight: bold;
      cursor: pointer;
      position: absolute;
    }
  </style>
</head>
<body>
  <h1>크러스티 크랩 게임</h1>
  <div>
    <button class="menu-btn" onclick="showSection('hamburger-game')">햄버거 게임</button>
    <button class="menu-btn" onclick="showSection('plankton-game')">플랑크톤 침략</button>
    <button class="menu-btn" onclick="showSection('gacha-section')">가챠</button>
    <button class="menu-btn" onclick="showSection('enhance-section')">강화</button>
    <button class="menu-btn" onclick="showSection('attendance-section')">출석</button>
    <button class="menu-btn" onclick="showSection('admin-panel')">관리자</button>
  </div>

  <p>코인: <span id="coin-display">0</span> | 강화석: <span id="stones">0</span> | Lv.<span id="level">0</span></p>

  <!-- 햄버거 미니게임 -->
  <div class="section" id="hamburger-game">
    <h2>햄버거 조합 게임</h2>
    <button class="btn" onclick="acceptOrder()">주문 받기</button>
    <p id="current-order">주문 없음</p>
    <button class="btn" onclick="document.getElementById('burger-feedback').innerText='완성! 보상 +2코인'; coins+=2; updateDisplay();">재료 클릭</button>
    <p id="burger-feedback"></p>
  </div>

  <!-- 플랑크톤 침략 -->
  <div class="section" id="plankton-game">
    <h2>플랑크톤 침략</h2>
    <button class="btn" onclick="startInvasion()">방어 시작</button>
    <div id="plankton-area"></div>
    <p id="invasion-result"></p>
  </div>

  <!-- 가챠 -->
  <div class="section" id="gacha-section">
    <h2>가챠</h2>
    <button class="btn" onclick="pullGeneralGacha()">일반 가챠 (5코인)</button>
    <button class="btn" onclick="pullEnhanceGacha()">강화 가챠 (10코인)</button>
    <p id="gacha-result"></p>
    <p id="enhance-result"></p>
  </div>

  <!-- 강화 -->
  <div class="section" id="enhance-section">
    <h2>캐릭터 강화</h2>
    <button class="btn" onclick="enhanceCharacter()">강화 시도</button>
    <p id="upgrade-result"></p>
  </div>

  <!-- 출석 -->
  <div class="section" id="attendance-section">
    <h2>출석 보상</h2>
    <button class="btn" onclick="claimAttendance()">출석하기</button>
    <p id="attendance-result"></p>
  </div>

  <!-- 관리자 패널 -->
  <div class="section" id="admin-panel">
    <h2>관리자 도구</h2>
    <input id="admin-pass" placeholder="비밀번호"><br>
    <input id="admin-user" placeholder="유저 이름"><br>
    <input id="admin-coins" type="number" placeholder="코인 수"><br>
    <button class="btn" onclick="grantCoins()">코인 지급</button>
    <p id="admin-result"></p>
  </div>

  <!-- 음악 및 효과음 -->
  <audio id="bgm" src="https://example.com/bgm.mp3" loop autoplay></audio>
  <audio id="click-sound" src="https://example.com/click.mp3"></audio>

  <!-- 스크립트 -->
  <script>
    let coins = 0;
    let stones = 0;
    let level = 0;
    let userName = '';
    let goldenSponge = false;
    let lastAttendance = null;

    function updateDisplay() {
      document.getElementById('coin-display').innerText = coins;
      document.getElementById('stones').innerText = stones;
      document.getElementById('level').innerText = level;
    }

    function showSection(id) {
      document.querySelectorAll('.section').forEach(div => div.style.display = 'none');
      document.getElementById(id).style.display = 'block';
      document.getElementById('click-sound').play();
    }

    function acceptOrder() {
      const orders = ['양상추-패티-빵', '빵-치즈-패티', '패티-양상추-토마토'];
      const order = orders[Math.floor(Math.random() * orders.length)];
      document.getElementById('current-order').innerText = order;
      document.getElementById('burger-feedback').innerText = '재료를 클릭해 조합하세요!';
    }

    function startInvasion() {
      const area = document.getElementById('plankton-area');
      area.innerHTML = '';
      let count = 0;
      for (let i = 0; i < 10; i++) {
        const plankton = document.createElement('div');
        plankton.innerText = '플';
        plankton.className = 'plankton';
        plankton.style.left = Math.random() * 300 + 'px';
        plankton.style.top = Math.random() * 150 + 'px';
        plankton.onclick = () => {
          plankton.remove();
          count++;
          if (count === 10) {
            coins += 3;
            updateDisplay();
            document.getElementById('invasion-result').innerText = '모두 처치 성공! 코인 +3';
          }
        };
        area.appendChild(plankton);
      }
    }

    function pullGeneralGacha() {
      if (coins < 5) return alert('코인이 부족해요!');
      coins -= 5;
      const characters = ['스폰지밥', '징징이', '집게사장', '황금 스폰지밥'];
      const result = characters[Math.floor(Math.random() * characters.length)];
      document.getElementById('gacha-result').innerText = result + ' 획득!';
      if (result === '황금 스폰지밥') goldenSponge = true;
      updateDisplay();
    }

    function pullEnhanceGacha() {
      if (coins < 10) return alert('코인이 부족해요!');
      coins -= 10;
      stones += 1;
      document.getElementById('enhance-result').innerText = '강화석 획득!';
      updateDisplay();
    }

    function enhanceCharacter() {
      if (stones <= 0) return alert('강화석이 부족해요!');
      stones--;
      const success = Math.random() < (level >= 50 ? 0.6 : 0.8);
      if (success) {
        level++;
        document.getElementById('upgrade-result').innerText = `강화 성공! Lv.${level}`;
      } else if (level >= 50 && Math.random() < 0.2) {
        level = 0;
        document.getElementById('upgrade-result').innerText = `강화 실패로 초기화!`;
      } else {
        document.getElementById('upgrade-result').innerText = `강화 실패!`;
      }
      updateDisplay();
    }

    function claimAttendance() {
      const today = new Date().toDateString();
      if (lastAttendance === today) {
        document.getElementById('attendance-result').innerText = '이미 출석했어요!';
        return;
      }
      lastAttendance = today;
      coins += 5;
      document.getElementById('attendance-result').innerText = '출석 보상! 코인 +5';
      updateDisplay();
    }

    function grantCoins() {
      const pass = document.getElementById('admin-pass').value;
      const user = document.getElementById('admin-user').value;
      const amount = parseInt(document.getElementById('admin-coins').value);
      if (pass !== 'komq3244') {
        document.getElementById('admin-result').innerText = '비밀번호 오류!';
        return;
      }
      if (user === userName) {
        coins += amount;
        document.getElementById('admin-result').innerText = `${amount} 코인을 지급했습니다.`;
        updateDisplay();
      } else {
        document.getElementById('admin-result').innerText = '사용자 이름이 일치하지 않습니다.';
      }
    }

    window.onload = () => {
      userName = prompt("이름을 입력하세요:");
      if (!userName) userName = '손님';
      updateDisplay();
      setInterval(() => {
        if (goldenSponge) {
          coins++;
          updateDisplay();
        }
      }, 3600000); // 1시간마다 1코인
    };
  </script>
</body>
</html>
