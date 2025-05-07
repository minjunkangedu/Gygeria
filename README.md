<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>🧽 스폰지밥 월드</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background: #f0f8ff;
      text-align: center;
      padding: 20px;
    }
    button {
      margin: 5px;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
    }
    .hidden {
      display: none;
    }
    #gameArea {
      width: 300px;
      height: 300px;
      position: relative;
      border: 4px double #000;
      background: url('https://i.imgur.com/pymJJ3s.jpg') center/cover;
      margin: 0 auto;
    }
    .plankton {
      width: 40px;
      position: absolute;
      cursor: pointer;
      transition: transform 0.2s ease;
    }
    #gachaResult img {
      width: 100px;
      margin: 10px;
    }
    #collection img {
      width: 80px;
      margin: 5px;
    }
  </style>
</head>
<body>

  <h1>🧽 스폰지밥 월드</h1>
  <p>코인: <span id="coinCount">0</span></p>

  <button onclick="showSection('minigame')">🛡️ 플랑크톤의 침략</button>
  <button onclick="showSection('gacha')">🎰 뽑기</button>
  <button onclick="showSection('collection')">📘 도감</button>

  <!-- 플랑크톤 미니게임 -->
  <section id="minigame" class="hidden">
    <h2>🛡️ 플랑크톤의 침략!</h2>
    <p>게살버거 비법을 지키세요!</p>
    <p>남은 시간: <span id="gameTime">20</span>초</p>
    <p>막은 침입자: <span id="blockedCount">0</span> / 침입한 플랑크톤: <span id="escapedCount">0</span></p>
    <div id="gameArea"></div>
    <p id="gameMessage"></p>
    <audio id="popSound" src="https://freesound.org/data/previews/341/341695_3248244-lq.mp3"></audio>
  </section>

  <!-- 뽑기 시스템 -->
  <section id="gacha" class="hidden">
    <h2>🎰 뽑기</h2>
    <p>코인 5개로 뽑기를 진행합니다.</p>
    <button onclick="drawGacha()">뽑기 진행</button>
    <div id="gachaResult"></div>
  </section>

  <!-- 도감 -->
  <section id="collection" class="hidden">
    <h2>📘 도감</h2>
    <div id="collectionList"></div>
  </section>

  <script>
    let coins = 0;
    let collection = [];

    const characters = [
      { name: "꽝", image: "https://i.imgur.com/3sK3w5O.png", rarity: "common" },
      { name: "스폰지밥", image: "https://i.imgur.com/klZbphE.png", rarity: "common" },
      { name: "뚱이", image: "https://i.imgur.com/5xgXg8M.png", rarity: "common" },
      { name: "징징이", image: "https://i.imgur.com/1uZz3cL.png", rarity: "common" },
      { name: "광금 스폰지밥", image: "https://i.imgur.com/XYZ1234.png", rarity: "rare" }
    ];

    // 저장/로드 함수
    function saveData() {
      localStorage.setItem("spongeCoins", coins);
      localStorage.setItem("spongeCollection", JSON.stringify(collection));
    }

    function loadData() {
      const savedCoins = localStorage.getItem("spongeCoins");
      const savedCollection = localStorage.getItem("spongeCollection");

      coins = savedCoins ? parseInt(savedCoins) : 0;
      collection = savedCollection ? JSON.parse(savedCollection) : [];
      updateCoins();
    }

    function updateCoins() {
      document.getElementById("coinCount").innerText = coins;
      saveData();
    }

    function showSection(id) {
      document.querySelectorAll("section").forEach(sec => sec.classList.add("hidden"));
      document.getElementById(id).classList.remove("hidden");
      if (id === "minigame") startGame();
      if (id === "collection") updateCollection();
    }

    // 미니게임 로직
    let gameInterval;
    let spawnInterval;
    let blocked = 0;
    let escaped = 0;
    let gameTime = 20;

    function startGame() {
      blocked = 0;
      escaped = 0;
      gameTime = 20;
      document.getElementById("gameMessage").innerText = "";
      document.getElementById("blockedCount").innerText = blocked;
      document.getElementById("escapedCount").innerText = escaped;
      document.getElementById("gameTime").innerText = gameTime;
      document.getElementById("gameArea").innerHTML = "";

      gameInterval = setInterval(() => {
        gameTime--;
        document.getElementById("gameTime").innerText = gameTime;
        if (gameTime <= 0) endGame();
      }, 1000);

      spawnInterval = setInterval(spawnPlankton, 800);
    }

    function spawnPlankton() {
      const plankton = document.createElement("img");
      plankton.src = "https://i.imgur.com/klZbphE.png";
      plankton.className = "plankton";
      plankton.style.left = Math.random() * 260 + "px";
      plankton.style.top = Math.random() * 260 + "px";

      plankton.onclick = () => {
        blocked++;
        document.getElementById("blockedCount").innerText = blocked;
        document.getElementById("popSound").play();
        plankton.style.transform = "scale(0)";
        setTimeout(() => plankton.remove(), 200);
      };

      document.getElementById("gameArea").appendChild(plankton);

      setTimeout(() => {
        if (document.body.contains(plankton)) {
          escaped++;
          document.getElementById("escapedCount").innerText = escaped;
          plankton.remove();
        }
      }, 1500);
    }

    function endGame() {
      clearInterval(gameInterval);
      clearInterval(spawnInterval);
      document.getElementById("gameArea").innerHTML = "";

      if (escaped <= 5) {
        const reward = 10;
        coins += reward;
        updateCoins();
        document.getElementById("gameMessage").innerText = `🎉 성공! 플랑크톤을 막아냈어요! +${reward} 코인`;
      } else {
        document.getElementById("gameMessage").innerText = "😱 플랑크톤이 비법서를 가져갔어요!";
      }
    }

    // 뽑기 로직
    function drawGacha() {
      if (coins < 5) {
        alert("코인이 부족합니다!");
        return;
      }
      coins -= 5;
      updateCoins();

      const rand = Math.random();
      let selected;
      if (rand < 0.05) {
        selected = characters.find(c => c.name === "광금 스폰지밥");
      } else if (rand < 0.6) {
        selected = characters.find(c => c.name === "꽝");
      } else {
        const commons = characters.filter(c => c.rarity === "common" && c.name !== "꽝");
        selected = commons[Math.floor(Math.random() * commons.length)];
      }

      document.getElementById("gachaResult").innerHTML = `
        <h3>결과: ${selected.name}</h3>
        <img src="${selected.image}" alt="${selected.name}">
      `;

      if (selected.name !== "꽝" && !collection.includes(selected.name)) {
        collection.push(selected.name);
        saveData();
      }
    }

    // 도감 업데이트
    function updateCollection() {
      const collectionDiv = document.getElementById("collectionList");
      collectionDiv.innerHTML = "";
      if (collection.length === 0) {
        collectionDiv.innerText = "아직 획득한 캐릭터가 없습니다.";
        return;
      }
      collection.forEach(name => {
        const char = characters.find(c => c.name === name);
        const img = document.createElement("img");
        img.src = char.image;
        img.alt = char.name;
        img.title = char.name;
        collectionDiv.appendChild(img);
      });
    }

    // 페이지 로드 시 저장된 데이터 불러오기
    window.onload = loadData;
  </script>

</body>
</html>
