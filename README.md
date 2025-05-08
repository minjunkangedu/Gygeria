<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Gygeria: Krusty Krab Web Game</title>
  <style>
    body {
      margin: 0;
      font-family: 'Comic Sans MS', cursive, sans-serif;
      background: url('krustykrab_bg.jpg') no-repeat center center fixed;
      background-size: cover;
      color: white;
    }
    .container {
      max-width: 960px;
      margin: auto;
      padding: 20px;
      background-color: rgba(0, 0, 0, 0.7);
      border-radius: 20px;
    }
    h1 {
      text-align: center;
    }
    .section {
      margin-top: 30px;
    }
    button {
      margin: 5px;
      padding: 10px 20px;
      font-size: 16px;
      border: none;
      border-radius: 8px;
      background-color: #fbbd08;
      color: black;
      cursor: pointer;
    }
    button:hover {
      background-color: #f2711c;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Gygeria: Krusty Krab Web Game</h1>
    <div id="game">
      <!-- 메인 게임 메뉴 및 상태 출력 -->
    </div>
    <div class="section">
      <button onclick="playBurgerGame()">스폰지밥 햄버거 미니게임</button>
      <button onclick="playPlanktonGame()">플랑크톤 침략 게임</button>
      <button onclick="openEnhanceGacha()">강화 가챠</button>
      <button onclick="runIdleMode()">잠수 채널</button>
      <button onclick="viewAchievements()">업적 보기</button>
      <button onclick="showLoyalCustomers()">단골 손님</button>
      <button onclick="useCharacterSkill()">캐릭터 고유 스킬</button>
    </div>
  </div>

  <audio id="bgm" loop autoplay>
    <source src="bgm.mp3" type="audio/mpeg" />
  </audio>
  <audio id="effect">
    <source src="effect.mp3" type="audio/mpeg" />
  </audio>

  <script type="module">
    // Firebase 연동
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
    import { getDatabase, ref, set, get, child, update } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";

    const firebaseConfig = {
      apiKey: "YOUR_API_KEY_HERE",
      authDomain: "gygeria-9f319.firebaseapp.com",
      databaseURL: "https://gygeria-9f319-default-rtdb.firebaseio.com",
      projectId: "gygeria-9f319",
      storageBucket: "gygeria-9f319.appspot.com",
      messagingSenderId: "570080414698",
      appId: "YOUR_APP_ID_HERE"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    // 게임 관련 함수들 간략 예시 (실제 내용은 기능에 맞게 세부 구현 필요)
    function playBurgerGame() {
      alert("[스폰지밥 햄버거 게임] 시간 내에 재료를 조합하세요!");
      // 게임 로직 구현 필요
    }

    function playPlanktonGame() {
      alert("[플랑크톤 침략 게임] 플랑크톤을 막아주세요!");
      // AI 패턴 기반 방어 구현 필요
    }

    function openEnhanceGacha() {
      alert("[강화 가챠] 강화석 혹은 축복 아이템 획득!");
      // 강화 시스템, 확률, 실패 등 적용 필요
    }

    function runIdleMode() {
      alert("[잠수 채널] 황금 스폰지밥이 코인을 생산합니다 (1시간마다 1코인)");
      // 타이머 기반 수동 코인 지급 구현
    }

    function viewAchievements() {
      alert("[업적 시스템] 업적을 달성해보세요!");
      // 업적 조건 확인 및 보상 시스템 필요
    }

    function showLoyalCustomers() {
      alert("[단골 손님] 특별한 손님 등장! 코인을 보상으로 획득하세요.");
      // 등장 확률 및 보상 처리
    }

    function useCharacterSkill() {
      alert("[캐릭터 스킬] 캐릭터 고유 능력을 발동합니다!");
      // 캐릭터별 스킬 효과 발동 구현 필요
    }
  </script>
</body>
</html>
