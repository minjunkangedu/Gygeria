<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>크러스티 크랩 게임</title>
  <link href="https://fonts.googleapis.com/css2?family=Jua&display=swap" rel="stylesheet">
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Jua', sans-serif;
    }
    body {
      background: url('krustykrab_bg.jpg') no-repeat center center fixed;
      background-size: cover;
      color: #fff;
    }
    #game-container {
      width: 100%;
      height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      backdrop-filter: blur(6px);
      background-color: rgba(0,0,0,0.6);
      padding: 20px;
    }
    button {
      padding: 10px 20px;
      margin: 10px;
      font-size: 1.2rem;
      border: none;
      border-radius: 10px;
      background-color: #f1c40f;
      color: #222;
      cursor: pointer;
      transition: 0.3s;
    }
    button:hover {
      background-color: #f39c12;
    }
    .section {
      display: none;
      margin-top: 20px;
    }
    .visible {
      display: block;
    }
  </style>
</head>
<body>
  <div id="game-container">
    <h1>크러스티 크랩 게임</h1>
    <input type="text" id="username" placeholder="이름을 입력하세요">
    <button onclick="startGame()">게임 시작</button>

    <div id="main-menu" class="section">
      <h2>메인 메뉴</h2>
      <button onclick="playBurgerGame()">스폰지밥 햄버거 미니게임</button>
      <button onclick="playPlanktonGame()">플랑크톤 침략 게임</button>
      <button onclick="openEnhancement()">캐릭터 강화</button>
      <button onclick="openGacha()">강화 가챠</button>
      <button onclick="openAttendance()">출석 보상</button>
      <button onclick="checkAchievements()">업적 확인</button>
      <button onclick="viewRegulars()">단골 손님 보기</button>
    </div>

    <!-- 여기에 각각의 게임과 시스템 섹션이 추가될 수 있습니다 -->
  </div>

  <!-- Firebase SDK -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
    import { getFirestore, doc, setDoc, getDoc, updateDoc } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "gygeria-9f319.firebaseapp.com",
      projectId: "gygeria-9f319",
      storageBucket: "gygeria-9f319.appspot.com",
      messagingSenderId: "570080414698",
      appId: "YOUR_APP_ID"
    };

    const app = initializeApp(firebaseConfig);
    const db = getFirestore(app);

    // 예시 저장 코드
    async function saveUserData(name) {
      await setDoc(doc(db, "users", name), {
        coin: 10,
        enhanceLevel: 0,
        attendance: new Date().toDateString()
      });
    }

    window.saveUserData = saveUserData;
  </script>

  <!-- 게임 로직 -->
  <script>
    let username = "";

    function startGame() {
      const input = document.getElementById("username");
      if (!input.value) {
        alert("이름을 입력해주세요.");
        return;
      }
      username = input.value;
      saveUserData(username);  // Firebase에 저장
      document.getElementById("main-menu").classList.add("visible");
      input.style.display = 'none';
      event.target.style.display = 'none';
    }

    function playBurgerGame() {
      alert("스폰지밥 미니게임이 실행됩니다. 제한 시간 안에 재료를 조합하세요!");
    }

    function playPlanktonGame() {
      alert("플랑크톤 침략 게임이 시작됩니다. 플랑크톤을 막으세요!");
    }

    function openEnhancement() {
      alert("강화 시스템입니다. 강화석과 축복으로 강화해보세요!");
    }

    function openGacha() {
      alert("가챠를 돌려 강화 아이템을 획득하세요!");
    }

    function openAttendance() {
      alert("출석 보상: 오늘도 접속해주셔서 감사합니다!");
    }

    function checkAchievements() {
      alert("업적 확인: 새로운 업적을 달성해보세요!");
    }

    function viewRegulars() {
      alert("단골 손님 목록을 확인합니다. 뚱이, 만수르 등이 올 수 있어요!");
    }
  </script>

  <!-- 배경 음악 -->
  <audio src="bgm.mp3" autoplay loop></audio>
</body>
</html>
