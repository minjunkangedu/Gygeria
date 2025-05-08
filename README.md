<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>크러스티 크랩 게임</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="mainMenu">
    <h1>크러스티 크랩</h1>
    <input type="text" id="playerNameInput" placeholder="이름을 입력하세요" />
    <button onclick="startGame()">게임 시작</button>
    <div id="menuButtons" style="display:none;">
      <button onclick="openGame('burger')">스폰지밥 햄버거 게임</button>
      <button onclick="openGame('plankton')">플랑크톤 침공</button>
      <button onclick="openGacha()">가챠</button>
      <button onclick="openEnhance()">캐릭터 강화</button>
      <button onclick="openIdle()">잠수 채널</button>
      <button onclick="openAdmin()">관리자 패널</button>
    </div>
  </div>

  <div id="gameContent" style="display:none;"></div>

  <div id="coinDisplay">코인: <span id="coinCount">0</span></div>

  <script src="script.js"></script>
</body>
</html>
