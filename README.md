<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>크러스티 크랩 게임</title>
  <link href="https://fonts.googleapis.com/css2?family=Jua&display=swap" rel="stylesheet">
  <style>
    body {
      margin: 0;
      font-family: 'Jua', sans-serif;
      background: url('https://i.ibb.co/sJ3y3Dk/krusty-krab-bg.jpg') no-repeat center center fixed;
      background-size: cover;
      color: #fff;
    }
    #container {
      padding: 20px;
      background-color: rgba(0,0,0,0.5);
    }
    h1, h2 {
      text-align: center;
      text-shadow: 2px 2px 4px black;
    }
    button {
      font-family: 'Jua', sans-serif;
      margin: 5px;
      padding: 10px 20px;
      font-size: 18px;
      cursor: pointer;
    }
    .section {
      margin-bottom: 20px;
    }
    #gameArea, #planktonGame, #adminPanel {
      display: none;
    }
    input {
      font-size: 16px;
      padding: 5px;
    }
  </style>
</head>
<body>
  <div id="container">
    <h1>크러스티 크랩 월드</h1>

    <div id="nameInputSection">
      <h2>이름을 입력하세요:</h2>
      <input type="text" id="playerNameInput" placeholder="닉네임">
      <button onclick="setPlayerName()">확인</button>
    </div>

    <div id="mainMenu" style="display:none">
      <h2 id="welcomeMessage"></h2>
      <p>코인: <span id="coinDisplay">0</span> / <span id="specialStatus"></span></p>

      <button onclick="startBurgerGame()">[1] 햄버거 만들기</button>
      <button onclick="startPlanktonGame()">[2] 플랑크톤 침략</button>
      <button onclick="openEnhance()">[3] 강화 / 가챠</button>
      <button onclick="openIdleChannel()">[4] 잠수 채널</button>
      <button onclick="toggleAdmin()">관리자</button>
    </div>

    <!-- 햄버거 미니게임 -->
    <div id="gameArea">
      <h2>햄버거 만들기 게임</h2>
      <p>시간 내에 올바른 재료 조합을 클릭하세요!</p>
      <div id="burgerGameContent"></div>
      <button onclick="endBurgerGame()">돌아가기</button>
    </div>

    <!-- 플랑크톤 미니게임 -->
    <div id="planktonGame">
      <h2>플랑크톤 침략!</h2>
      <p>플랑크톤을 클릭해서 막아라!</p>
      <div id="planktonArea"></div>
      <button onclick="endPlanktonGame()">돌아가기</button>
    </div>

    <!-- 강화 / 가챠 -->
    <div id="enhanceSection" style="display:none;">
      <h2>강화 / 가챠</h2>
      <p>강화석: <span id="enhanceStones">0</span></p>
      <button onclick="pullGacha()">가챠 돌리기 (3코인)</button>
      <button onclick="doEnhance()">강화하기</button>
      <p>현재 강화 레벨: <span id="enhanceLevel">0</span></p>
      <button onclick="closeEnhance()">돌아가기</button>
    </div>

    <!-- 잠수 채널 -->
    <div id="idleChannel" style="display:none;">
      <h2>잠수 채널 (황금 스폰지밥 전용)</h2>
      <p>1시간마다 자동으로 1코인을 얻습니다.</p>
      <p>최근 수령 시각: <span id="lastIdleTime">-</span></p>
      <button onclick="closeIdleChannel()">돌아가기</button>
    </div>

    <!-- 관리자 -->
    <div id="adminPanel">
      <h2>관리자 패널</h2>
      <input type="text" id="adminUser" placeholder="유저 이름">
      <input type="number" id="adminCoins" placeholder="코인 수">
      <button onclick="giveCoins()">코인 지급</button>
      <button onclick="toggleAdmin()">닫기</button>
    </div>
  </div>

  <script>
    let playerName = '';
    let coins = 0;
    let enhanceLevel = 0;
    let enhanceStones = 0;
    let hasGoldSpongeBob = false;
    let lastIdleTimestamp = 0;

    function setPlayerName() {
      const input = document.getElementById('playerNameInput');
      if (input.value.trim()) {
        playerName = input.value.trim();
        document.getElementById('nameInputSection').style.display = 'none';
        document.getElementById('mainMenu').style.display = 'block';
        document.getElementById('welcomeMessage').innerText = `${playerName}님, 환영합니다!`;
        updateUI();
      }
    }

    function updateUI() {
      document.getElementById('coinDisplay').innerText = coins;
      document.getElementById('enhanceStones').innerText = enhanceStones;
      document.getElementById('enhanceLevel').innerText = enhanceLevel;
      document.getElementById('specialStatus').innerText = hasGoldSpongeBob ? '황금 스폰지밥 보유' : '-';
    }

    // 햄버거 게임 로직
    function startBurgerGame() {
      document.getElementById('mainMenu').style.display = 'none';
      document.getElementById('gameArea').style.display = 'block';
      document.getElementById('burgerGameContent').innerText = "햄버거 게임 작동 중! (개선 가능)";
      coins += 1;
      updateUI();
    }

    function endBurgerGame() {
      document.getElementById('gameArea').style.display = 'none';
      document.getElementById('mainMenu').style.display = 'block';
    }

    // 플랑크톤 게임 로직
    function startPlanktonGame() {
      document.getElementById('mainMenu').style.display = 'none';
      document.getElementById('planktonGame').style.display = 'block';
      document.getElementById('planktonArea').innerText = "플랑크톤 게임 작동 중! (개선 가능)";
      coins += 1;
      updateUI();
    }

    function endPlanktonGame() {
      document.getElementById('planktonGame').style.display = 'none';
      document.getElementById('mainMenu').style.display = 'block';
    }

    // 강화 / 가챠
    function openEnhance() {
      document.getElementById('mainMenu').style.display = 'none';
      document.getElementById('enhanceSection').style.display = 'block';
    }

    function closeEnhance() {
      document.getElementById('enhanceSection').style.display = 'none';
      document.getElementById('mainMenu').style.display = 'block';
    }

    function pullGacha() {
      if (coins >= 3) {
        coins -= 3;
        const rand = Math.random();
        if (rand < 0.05) {
          hasGoldSpongeBob = true;
          alert("황금 스폰지밥 획득!");
        } else {
          enhanceStones += 1;
          alert("강화석 획득!");
        }
        updateUI();
      } else {
        alert("코인이 부족합니다.");
      }
    }

    function doEnhance() {
      if (enhanceStones > 0) {
        enhanceStones -= 1;
        const success = Math.random() < 0.7;
        if (success) {
          enhanceLevel += 1;
          alert("강화 성공!");
        } else {
          alert("강화 실패...");
        }
        updateUI();
      } else {
        alert("강화석이 부족합니다.");
      }
    }

    // 잠수 채널
    function openIdleChannel() {
      if (!hasGoldSpongeBob) {
        alert("황금 스폰지밥이 필요합니다!");
        return;
      }
      document.getElementById('mainMenu').style.display = 'none';
      document.getElementById('idleChannel').style.display = 'block';
      const now = Date.now();
      if (now - lastIdleTimestamp >= 3600000) {
        coins += 1;
        lastIdleTimestamp = now;
        alert("1코인 획득!");
      }
      document.getElementById('lastIdleTime').innerText = new Date(lastIdleTimestamp).toLocaleTimeString();
      updateUI();
    }

    function closeIdleChannel() {
      document.getElementById('idleChannel').style.display = 'none';
      document.getElementById('mainMenu').style.display = 'block';
    }

    // 관리자
    function toggleAdmin() {
      const panel = document.getElementById('adminPanel');
      if (panel.style.display === 'block') {
        panel.style.display = 'none';
      } else {
        const pw = prompt("관리자 비밀번호를 입력하세요:");
        if (pw === 'komq3244') {
          panel.style.display = 'block';
        } else {
          alert("비밀번호가 틀렸습니다.");
        }
      }
    }

    function giveCoins() {
      const targetName = document.getElementById('adminUser').value.trim();
      const amount = parseInt(document.getElementById('adminCoins').value, 10);
      if (targetName === playerName && !isNaN(amount)) {
        coins += amount;
        updateUI();
        alert(`${amount} 코인이 지급되었습니다.`);
      } else {
        alert("이름 확인 또는 숫자를 올바르게 입력하세요.");
      }
    }
  </script>
</body>
</html>
