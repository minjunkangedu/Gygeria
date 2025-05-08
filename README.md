<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>집게리아 고퀄 웹게임</title>
  <style>
    body {
      background-image: url('https://i.imgur.com/oU5FbdA.jpg');
      background-size: cover;
      background-attachment: fixed;
      font-family: 'Arial', sans-serif;
      color: #fff;
      text-align: center;
      padding: 20px;
    }
    #main-menu button, #game-section button {
      padding: 10px 20px;
      margin: 5px;
      font-size: 16px;
      background-color: #ffcc00;
      border: none;
      cursor: pointer;
    }
    #log {
      background: rgba(0,0,0,0.6);
      height: 150px;
      overflow-y: auto;
      margin-top: 20px;
      padding: 10px;
    }
    #admin-panel {
      background: rgba(0,0,0,0.4);
      padding: 10px;
      display: none;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <h1>집게리아 고퀄 게임</h1>

  <div id="name-section">
    <p>이름을 입력하세요:</p>
    <input id="player-name" placeholder="예: 스폰지밥" />
    <button onclick="startGame()">시작</button>
  </div>

  <div id="main-menu" style="display:none;">
    <h2 id="welcome-msg"></h2>
    <button onclick="startBurgerGame()">햄버거 조합</button>
    <button onclick="startInvasionGame()">플랑크톤 침공</button>
    <button onclick="openMenuShop()">오늘의 메뉴</button>
    <button onclick="showFavorites()">단골 손님</button>
    <button onclick="showGags()">개그 코너</button>
    <button onclick="claimAttendance()">출석 보상</button>
    <button onclick="checkAchievements()">업적 확인</button>
    <button onclick="openEnhance()">강화</button>
    <button onclick="openIdle()">잠수 채널</button>
    <button onclick="toggleAdmin()">관리자</button>
  </div>

  <div id="game-section" style="display:none;"></div>

  <div id="log"></div>

  <div id="admin-panel">
    <h3>관리자 패널</h3>
    <input id="admin-user" placeholder="유저 이름" />
    <input id="admin-coins" placeholder="코인 수" type="number" />
    <button onclick="giveCoins()">코인 지급</button>
  </div>

  <script>
    let playerName = "";
    let coins = 0;
    let burgerCombo = [];
    let attendanceClaimed = false;
    let planktonHP = 5;

    let character = {
      level: 0,
      attack: 10,
      defense: 5,
      stones: 0
    };

    const gags = [
      "문을 밀어야 하는데 당겼어요!",
      "오늘도 해파리에 쏘였어요!",
      "스폰지밥이 버거에 치약을 넣었대요!",
      "징징이는 오늘도 불평 중!",
    ];

    function startGame() {
      const input = document.getElementById("player-name").value.trim();
      if (input === '') return alert("이름을 입력하세요!");
      playerName = input;
      document.getElementById("name-section").style.display = "none";
      document.getElementById("main-menu").style.display = "block";
      document.getElementById("welcome-msg").innerText = `어서오세요, ${playerName}님!`;
      log(`${playerName}님이 입장했습니다.`);
    }

    function log(msg) {
      const logDiv = document.getElementById("log");
      const p = document.createElement("p");
      p.innerText = msg;
      logDiv.appendChild(p);
      logDiv.scrollTop = logDiv.scrollHeight;
    }

    function toggleAdmin() {
      const password = prompt("관리자 비밀번호를 입력하세요:");
      if (password === "komq3244") {
        document.getElementById("admin-panel").style.display = "block";
        log("관리자 패널 열림");
      } else {
        alert("비밀번호가 틀렸습니다.");
      }
    }

    function giveCoins() {
      const user = document.getElementById("admin-user").value.trim();
      const amount = parseInt(document.getElementById("admin-coins").value);
      if (!user || isNaN(amount)) return alert("입력 오류");
      coins += amount;
      log(`${user}에게 ${amount} 코인을 지급했습니다. 현재 보유: ${coins}`);
    }

    const ingredients = ["빵", "고기", "치즈", "야채", "소스"];
    function startBurgerGame() {
      document.getElementById("game-section").style.display = "block";
      document.getElementById("game-section").innerHTML = `<h2>햄버거 조합 게임</h2>
        <p>정해진 순서대로 재료를 클릭하세요! 시간 제한: <span id="burger-timer">10</span>초</p>
        <div id="ingredients"></div>`;
      burgerCombo = [...ingredients];
      const ingDiv = document.getElementById("ingredients");
      ingredients.forEach(ing => {
        const btn = document.createElement("button");
        btn.innerText = ing;
        btn.onclick = () => checkBurger(ing);
        ingDiv.appendChild(btn);
      });
      let timeLeft = 10;
      const timer = setInterval(() => {
        document.getElementById("burger-timer").innerText = timeLeft;
        if (timeLeft-- <= 0) {
          clearInterval(timer);
          log("시간 초과! 실패!");
          document.getElementById("game-section").style.display = "none";
        }
      }, 1000);
    }

    function checkBurger(ing) {
      if (ing === burgerCombo[0]) {
        burgerCombo.shift();
        log(`재료 ${ing} 선택`);
        if (burgerCombo.length === 0) {
          log("햄버거 완성! 보상: 3 코인");
          coins += 3;
          document.getElementById("game-section").style.display = "none";
        }
      } else {
        log(`틀린 재료! 실패`);
        document.getElementById("game-section").style.display = "none";
      }
    }

    function startInvasionGame() {
      document.getElementById("game-section").style.display = "block";
      document.getElementById("game-section").innerHTML = `<h2>플랑크톤 침공</h2>
        <p>플랑크톤의 체력: <span id="plankton-hp">5</span></p>
        <button onclick="attackPlankton()">공격!</button>`;
      planktonHP = 5;
      planktonAttackPattern();
    }

    function attackPlankton() {
      planktonHP--;
      document.getElementById("plankton-hp").innerText = planktonHP;
      log("플랑크톤을 공격했습니다!");
      if (planktonHP <= 0) {
        log("플랑크톤 처치! 보상: 5 코인");
        coins += 5;
        document.getElementById("game-section").style.display = "none";
      }
    }

    function planktonAttackPattern() {
      const interval = setInterval(() => {
        if (planktonHP <= 0) return clearInterval(interval);
        if (Math.random() < 0.4) {
          log("플랑크톤이 반격했다! 코인 -1");
          coins = Math.max(0, coins - 1);
        }
      }, 2000);
    }

    function openMenuShop() {
      const items = ["강화석", "황금버거", "스페셜 세트"];
      const prices = [5, 10, 15];
      let html = "<h2>오늘의 메뉴</h2>";
      items.forEach((item, i) => {
        html += `<p>${item} - ${prices[i]} 코인 <button onclick="buyItem(${prices[i]}, '${item}')">구매</button></p>`;
      });
      document.getElementById("game-section").innerHTML = html;
      document.getElementById("game-section").style.display = "block";
    }

    function buyItem(price, item) {
      if (coins >= price) {
        coins -= price;
        if (item === "강화석") character.stones += 1;
        log(`${item} 구매 성공!`);
      } else {
        log("코인이 부족합니다!");
      }
    }

    function showFavorites() {
      const guests = ["뚱이", "만수르", "펄", "해파리", "퍼프 선생님"];
      const guest = guests[Math.floor(Math.random() * guests.length)];
      log(`단골 손님 ${guest} 등장! 2 코인 보너스!`);
      coins += 2;
    }

    function showGags() {
      const gag = gags[Math.floor(Math.random() * gags.length)];
      log(`개그 코너: ${gag}`);
    }

    function claimAttendance() {
      if (attendanceClaimed) return alert("이미 출석했습니다!");
      log("출석 보상 획득! 5 코인");
      coins += 5;
      attendanceClaimed = true;
    }

    function checkAchievements() {
      let msg = "업적 확인:";
      if (coins >= 10) msg += "\n- 부자 손님!";
      if (attendanceClaimed) msg += "\n- 성실한 출석!";
      alert(msg);
    }

    function openEnhance() {
      document.getElementById("game-section").style.display = "block";
      document.getElementById("game-section").innerHTML = `
        <h2>강화 시스템</h2>
        <p>현재 강화 단계: +${character.level}</p>
        <p>공격력: ${character.attack} / 방어력: ${character.defense}</p>
        <p>보유 강화석: ${character.stones}</p>
        <button onclick="tryEnhance()">강화 시도</button>`;
    }

    function tryEnhance() {
      if (character.stones <= 0) return log("강화석이 없습니다!");
      character.stones--;
      const successRate = character.level < 50 ? 0.8 : 0.6;
      const resetRate = character.level >= 50 ? 0.2 : 0;
      const rand = Math.random();
      if (rand < successRate) {
        character.level++;
        character.attack += 2;
        character.defense += 1;
        log(`강화 성공! 현재 +${character.level}`);
      } else if (rand < successRate + resetRate) {
        character.level = 0;
        log("강화 실패! 단계 초기화됨!");
      } else {
        character.level = Math.max(0, character.level - 1);
        log("강화 실패! 1단계 하락!");
      }
      openEnhance();
    }

    function openIdle() {
      log("잠수 채널: 1분 후 자동 보상 지급 예정...");
      setTimeout(() => {
        coins += 1;
        log("잠수 보상: 1 코인 지급!");
      }, 60000);
    }
  </script>
</body>
</html>
