<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>크러스티 크랩 게임</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-image: url('https://i.imgur.com/VHQiVRv.jpg');
      background-size: cover;
      color: #fff;
      text-align: center;
      margin: 0;
      padding: 0;
    }
    h1 {
      background: rgba(0,0,0,0.6);
      padding: 10px;
    }
    .menu button {
      margin: 10px;
      padding: 10px 20px;
      font-size: 16px;
    }
    section {
      display: none;
      margin-top: 20px;
    }
    .character-card {
      background: rgba(255, 255, 255, 0.1);
      padding: 5px;
      margin: 5px;
      border-radius: 5px;
    }
    .gacha-result {
      font-weight: bold;
      color: gold;
    }
    #nameInputContainer {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0,0,0,0.8);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 9999;
      flex-direction: column;
    }
    #username {
      padding: 10px;
      font-size: 18px;
    }
  </style>
</head>
<body>
  <div id="nameInputContainer">
    <h2>이름을 입력하세요</h2>
    <input type="text" id="username" placeholder="닉네임">
    <button onclick="setName()">입력</button>
  </div>

  <h1>크러스티 크랩 웹 게임</h1>
  <div id="coinDisplay">로딩 중...</div>
  <div class="menu">
    <button onclick="showSection('gacha')">가챠</button>
    <button onclick="showSection('burger')">햄버거 게임</button>
    <button onclick="showSection('plankton')">플랑크톤 침공</button>
    <button onclick="showSection('enhance')">캐릭터 강화</button>
    <button onclick="showSection('admin')">관리자</button>
  </div>

  <section id="gacha">
    <h2>가챠 뽑기 (5코인)</h2>
    <button onclick="rollGacha()">뽑기</button>
    <div id="gachaResult"></div>
  </section>

  <section id="burger">
    <h2>햄버거 조합 게임</h2>
    <button onclick="makeBurger()">버거 조합</button>
    <div id="burgerOutput"></div>
  </section>

  <section id="plankton">
    <h2>플랑크톤 침공</h2>
    <button onclick="defendKrab()">방어!</button>
    <div id="planktonOutput"></div>
  </section>

  <section id="enhance">
    <h2>캐릭터 강화</h2>
    <button onclick="enhanceCharacter()">전체 강화</button>
    <div id="characterList"></div>
  </section>

  <section id="admin">
    <h2>관리자 코인 지급</h2>
    <input type="text" id="adminTarget" placeholder="유저 이름">
    <input type="number" id="adminAmount" placeholder="코인 수">
    <button onclick="grantCoin()">코인 지급</button>
  </section>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-firestore-compat.js"></script>

  <script>
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_PROJECT_ID.appspot.com",
      messagingSenderId: "YOUR_SENDER_ID",
      appId: "YOUR_APP_ID"
    };

    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();

    let currentUser = null;
    let coin = 0;
    let characters = [];
    let isGolden = false;

    document.addEventListener('DOMContentLoaded', () => {
      const savedName = localStorage.getItem('username');
      if (savedName) {
        loadUser(savedName);
        document.getElementById("nameInputContainer").style.display = "none";
      } else {
        document.getElementById("nameInputContainer").style.display = "flex";
      }
    });

    function setName() {
      const name = document.getElementById("username").value.trim();
      if (name) {
        localStorage.setItem("username", name);
        loadUser(name);
        document.getElementById("nameInputContainer").style.display = "none";
      }
    }

    async function loadUser(name) {
      const doc = await db.collection("users").doc(name).get();
      if (doc.exists) {
        const data = doc.data();
        coin = data.coin || 0;
        characters = data.characters || [];
      } else {
        await db.collection("users").doc(name).set({ coin: 0, characters: [] });
      }
      currentUser = name;
      updateUI();
    }

    function updateUI() {
      document.getElementById("coinDisplay").textContent = `${currentUser}: ${coin}코인`;
    }

    function showSection(id) {
      document.querySelectorAll('section').forEach(sec => sec.style.display = 'none');
      document.getElementById(id).style.display = 'block';
    }

    function rollGacha() {
      if (coin < 5) return alert("코인이 부족합니다!");
      coin -= 5;
      const rand = Math.random();
      let result;
      if (rand < 0.05) {
        result = "황금 스폰지밥";
        isGolden = true;
      } else {
        const names = ["뚱이", "징징이", "스폰지밥", "플랑크톤", "집게사장"];
        result = names[Math.floor(Math.random() * names.length)];
      }
      characters.push({ name: result, level: 1 });
      saveData();
      document.getElementById("gachaResult").innerHTML = `<span class='gacha-result'>[${result}] 획득!</span>`;
    }

    function saveData() {
      db.collection("users").doc(currentUser).set({
        coin, characters
      });
      updateUI();
    }

    let burgerCount = 0;
    function makeBurger() {
      burgerCount++;
      if (burgerCount % 5 === 0) {
        coin++;
        saveData();
      }
      document.getElementById("burgerOutput").textContent = `버거 ${burgerCount}개 조합함!`;
    }

    function defendKrab() {
      const success = Math.random() > 0.3;
      document.getElementById("planktonOutput").textContent =
        success ? "플랑크톤 격퇴 성공!" : "플랑크톤이 주방을 침범함!";
      if (success) {
        coin += 2;
        saveData();
      }
    }

    function enhanceCharacter() {
      const charList = document.getElementById("characterList");
      charList.innerHTML = '';
      characters.forEach((char, i) => {
        const success = Math.random() < 0.7;
        if (success) {
          char.level++;
          if (char.level % 10 === 0) {
            char.special = true;
          }
        } else if (char.level >= 50 && Math.random() < 0.2) {
          char.level = 0;
        }
        const el = document.createElement("div");
        el.className = 'character-card';
        el.textContent = `${char.name} - Lv.${char.level}${char.special ? " (특수 능력 해금!)" : ""}`;
        charList.appendChild(el);
      });
      saveData();
    }

    setInterval(() => {
      if (isGolden) {
        coin++;
        saveData();
      }
    }, 3600000);

    function grantCoin() {
      const pw = prompt("관리자 비밀번호를 입력하세요:");
      if (pw !== "komq3244") return alert("비밀번호가 틀렸습니다.");
      const target = document.getElementById("adminTarget").value;
      const amount = parseInt(document.getElementById("adminAmount").value);
      if (!target || isNaN(amount)) return alert("입력 오류!");
      db.collection("users").doc(target).get().then(doc => {
        if (!doc.exists) {
          db.collection("users").doc(target).set({ coin: amount, characters: [] });
        } else {
          const data = doc.data();
          db.collection("users").doc(target).update({ coin: (data.coin || 0) + amount });
        }
        alert(`관리자: ${target}에게 ${amount}코인 지급 완료`);
      });
    }
  </script>
</body>
</html>
