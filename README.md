<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>크러스티 크랩 게임</title>
  <style>
    body {
      margin: 0;
      background: url('krustykrab_bg.jpg') no-repeat center center fixed;
      background-size: cover;
      font-family: 'Arial', sans-serif;
      color: #fff;
    }
    .container {
      background-color: rgba(0,0,0,0.7);
      padding: 20px;
      max-width: 800px;
      margin: auto;
      margin-top: 50px;
      border-radius: 16px;
      box-shadow: 0 0 10px #333;
    }
    input, button {
      padding: 10px;
      margin: 5px;
      border-radius: 8px;
    }
    .highlight { animation: blink 1s infinite; color: yellow; }
    @keyframes blink {
      0%, 100% { opacity: 1 }
      50% { opacity: 0.2 }
    }
  </style>
</head>
<body>
  <div class="container" id="app">
    <h1>크러스티 크랩 게임</h1>
    <div id="namePrompt">
      <p class="highlight">이름을 입력하세요 (최초 방문):</p>
      <input id="usernameInput" placeholder="닉네임 입력"/>
      <button onclick="submitName()">시작</button>
    </div>

    <div id="mainGame" style="display:none;">
      <p>안녕하세요, <span id="playerName"></span>님!</p>
      <p>보유 코인: <span id="coinCount">0</span>개</p>
      <p>강화 레벨: <span id="enhanceLevel">0</span></p>
      <button onclick="openEnhance()">강화하기</button>
      <button onclick="gachaDraw()">강화 가챠</button>
      <button onclick="startBurgerGame()">햄버거 만들기</button>
      <button onclick="startDefenseGame()">플랑크톤 침공</button>
      <button onclick="openSubChannel()">잠수 채널</button>
      <button onclick="openAdmin()">관리자</button>
    </div>

    <div id="adminPanel" style="display:none;">
      <h3>관리자 로그인</h3>
      <input id="adminName" placeholder="지급할 유저 이름">
      <input id="adminCoin" type="number" placeholder="코인 수">
      <button onclick="giveCoin()">코인 지급</button>
    </div>
  </div>

  <!-- 음악 및 효과음 -->
  <audio id="bgm" src="bgm.mp3" loop autoplay></audio>
  <audio id="effect" src="effect.mp3"></audio>

  <!-- Firebase 연동 -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
    import { getDatabase, ref, set, get, update } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-database.js";

    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "gygeria-9f319.firebaseapp.com",
      databaseURL: "https://gygeria-9f319-default-rtdb.firebaseio.com",
      projectId: "gygeria-9f319",
      storageBucket: "gygeria-9f319.appspot.com",
      messagingSenderId: "570080414698",
      appId: "YOUR_APP_ID"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    let username = localStorage.getItem('kk_name');
    let coin = 0;
    let enhance = 0;

    const namePrompt = document.getElementById('namePrompt');
    const mainGame = document.getElementById('mainGame');
    const coinCount = document.getElementById('coinCount');
    const enhanceLevel = document.getElementById('enhanceLevel');
    const playerName = document.getElementById('playerName');

    if (username) {
      namePrompt.style.display = 'none';
      mainGame.style.display = 'block';
      playerName.textContent = username;
      loadData();
    }

    window.submitName = () => {
      username = document.getElementById('usernameInput').value;
      if (username) {
        localStorage.setItem('kk_name', username);
        namePrompt.style.display = 'none';
        mainGame.style.display = 'block';
        playerName.textContent = username;
        saveData();
      }
    };

    function saveData() {
      set(ref(db, 'users/' + username), {
        coin, enhance
      });
    }

    function loadData() {
      get(ref(db, 'users/' + username)).then((snap) => {
        if (snap.exists()) {
          const data = snap.val();
          coin = data.coin || 0;
          enhance = data.enhance || 0;
          updateUI();
        }
      });
    }

    function updateUI() {
      coinCount.textContent = coin;
      enhanceLevel.textContent = enhance;
    }

    window.openEnhance = () => {
      const cost = 5;
      if (coin >= cost) {
        const success = Math.random() < 0.8;
        if (success) enhance++;
        else if (enhance >= 50 && Math.random() < 0.3) enhance = 0;
        coin -= cost;
        updateUI();
        saveData();
        document.getElementById("effect").play();
      }
    };

    window.gachaDraw = () => {
      const cost = 10;
      if (coin >= cost) {
        coin -= cost;
        const reward = Math.random() < 0.1 ? '강화석' : '코인';
        if (reward === '강화석') enhance++;
        else coin += 5;
        updateUI();
        saveData();
        document.getElementById("effect").play();
      }
    };

    window.startBurgerGame = () => {
      alert('제한 시간 내에 재료를 조합하는 햄버거 게임 시작!');
    };

    window.startDefenseGame = () => {
      alert('AI 기반 플랑크톤 침공 방어 게임 시작!');
    };

    window.openSubChannel = () => {
      setInterval(() => {
        coin++;
        updateUI();
        saveData();
      }, 3600000); // 1시간
      alert('잠수 채널 ON: 1시간마다 코인 +1');
    };

    window.openAdmin = () => {
      const pass = prompt("관리자 비밀번호 입력:");
      if (pass === 'komq3244') {
        document.getElementById('adminPanel').style.display = 'block';
      }
    };

    window.giveCoin = () => {
      const target = document.getElementById('adminName').value;
      const amount = parseInt(document.getElementById('adminCoin').value);
      if (target && amount) {
        update(ref(db, 'users/' + target), {
          coin: amount
        });
        alert(`${target}에게 ${amount}코인 지급됨`);
      }
    };
  </script>
</body>
</html>
