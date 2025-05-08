<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Krusty Krab Web Game</title>
  <style>
    body {
      margin: 0;
      font-family: 'Arial', sans-serif;
      background: url('krustykrab_bg.jpg') no-repeat center center fixed;
      background-size: cover;
      color: #fff;
      text-align: center;
    }
    .container {
      background-color: rgba(0, 0, 0, 0.6);
      margin: 50px auto;
      padding: 20px;
      border-radius: 10px;
      width: 90%;
      max-width: 600px;
    }
    input, button {
      padding: 10px;
      margin: 5px;
      border: none;
      border-radius: 5px;
    }
    button {
      background-color: #f1c40f;
      cursor: pointer;
    }
    button:hover {
      background-color: #d4ac0d;
    }
    #gameArea {
      margin-top: 20px;
    }
    .hidden {
      display: none;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>🍔 Krusty Krab Web Game 🍟</h1>
    <div id="loginArea">
      <input type="text" id="username" placeholder="이름을 입력하세요" />
      <button onclick="login()">시작하기</button>
    </div>
    <div id="mainMenu" class="hidden">
      <p>환영합니다, <span id="displayName"></span>님!</p>
      <p>보유 코인: <span id="coinCount">0</span></p>
      <button onclick="startBurgerGame()">버거 만들기 게임</button>
      <button onclick="startPlanktonGame()">플랑크톤 침공 방어</button>
      <button onclick="openGacha()">가챠 돌리기</button>
      <button onclick="dailyCheckIn()">출석 체크</button>
      <button onclick="adminPanel()">관리자 패널</button>
      <div id="gameArea"></div>
    </div>
  </div>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-database-compat.js"></script>

  <script>
    // Firebase 설정
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_PROJECT_ID.appspot.com",
      messagingSenderId: "YOUR_SENDER_ID",
      appId: "YOUR_APP_ID"
    };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    let currentUser = null;
    let userData = {
      coins: 0,
      lastCheckIn: null
    };

    function login() {
      const name = document.getElementById('username').value.trim();
      if (name === "") {
        alert("이름을 입력해주세요.");
        return;
      }
      currentUser = name;
      document.getElementById('displayName').textContent = currentUser;
      document.getElementById('loginArea').classList.add('hidden');
      document.getElementById('mainMenu').classList.remove('hidden');
      loadUserData();
    }

    function loadUserData() {
      db.ref('users/' + currentUser).once('value').then(snapshot => {
        if (snapshot.exists()) {
          userData = snapshot.val();
        } else {
          db.ref('users/' + currentUser).set(userData);
        }
        updateUI();
      });
    }

    function updateUI() {
      document.getElementById('coinCount').textContent = userData.coins;
    }

    function startBurgerGame() {
      alert("버거 만들기 게임을 시작합니다! (기능 구현 예정)");
      // 게임 로직 구현
      userData.coins += 1;
      saveUserData();
    }

    function startPlanktonGame() {
      alert("플랑크톤 침공 방어 게임을 시작합니다! (기능 구현 예정)");
      // 게임 로직 구현
      const reward = Math.random() < 0.5 ? 3 : 0;
      userData.coins += reward;
      saveUserData();
    }

    function openGacha() {
      alert("가챠를 돌립니다! (기능 구현 예정)");
      // 가챠 로직 구현
      userData.coins -= 5;
      saveUserData();
    }

    function dailyCheckIn() {
      const today = new Date().toLocaleDateString();
      if (userData.lastCheckIn === today) {
        alert("오늘 이미 출석하셨습니다.");
        return;
      }
      userData.coins += 10;
      userData.lastCheckIn = today;
      saveUserData();
      alert("출석 체크 완료! 10코인을 받았습니다.");
    }

    function adminPanel() {
      const password = prompt("관리자 비밀번호를 입력하세요:");
      if (password === "komq3244") {
        const targetUser = prompt("코인을 지급할 유저 이름을 입력하세요:");
        const amount = parseInt(prompt("지급할 코인 수를 입력하세요:"), 10);
        if (targetUser && !isNaN(amount)) {
          db.ref('users/' + targetUser + '/coins').once('value').then(snapshot => {
            let currentCoins = snapshot.val() || 0;
            db.ref('users/' + targetUser).update({ coins: currentCoins + amount });
            alert(`${targetUser}님에게 ${amount}코인을 지급했습니다.`);
          });
        }
      } else {
        alert("비밀번호가 틀렸습니다.");
      }
    }

    function saveUserData() {
      db.ref('users/' + currentUser).set(userData);
      updateUI();
    }
  </script>
</body>
</html>
