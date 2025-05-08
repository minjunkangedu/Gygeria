<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>집게리아 웹 게임</title>
  <style>
    * {
      box-sizing: border-box;
    }
    body {
      margin: 0;
      font-family: 'Arial', sans-serif;
      background: url('https://i.ibb.co/qNYBtb1/krusty-krab-bg.jpg') no-repeat center center fixed;
      background-size: cover;
      overflow: hidden;
    }

    #mainMenuBtn {
      position: absolute;
      top: 20px;
      left: 20px;
      background-color: #ffd700;
      padding: 10px 20px;
      border-radius: 8px;
      border: none;
      cursor: pointer;
      font-weight: bold;
      z-index: 100;
    }

    #menuPanel {
      position: absolute;
      top: 60px;
      left: 20px;
      background: rgba(255, 255, 255, 0.95);
      padding: 12px;
      border-radius: 10px;
      display: none;
      flex-direction: column;
      gap: 10px;
      z-index: 99;
    }

    #menuPanel button {
      padding: 10px 14px;
      font-size: 15px;
      cursor: pointer;
      border-radius: 6px;
      background-color: #e0e0e0;
      border: 1px solid #ccc;
    }

    #spongebob {
      position: absolute;
      left: 5%;
      bottom: 0;
      height: 350px;
      z-index: 10;
    }

    #squidward {
      position: absolute;
      right: 5%;
      bottom: 0;
      height: 320px;
      z-index: 10;
    }

    #coinDisplay {
      position: absolute;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      background-color: rgba(255, 255, 255, 0.9);
      padding: 8px 16px;
      border-radius: 20px;
      font-weight: bold;
      z-index: 100;
    }

    #guestArea {
      position: absolute;
      bottom: 100px;
      right: 120px;
      height: 200px;
      width: 150px;
      z-index: 15;
    }
    .guest {
      position: absolute;
      bottom: 0;
      right: 0;
      height: 180px;
    }
  </style>
</head>
<body>

  <button id="mainMenuBtn">메뉴</button>

  <div id="menuPanel">
    <button onclick="openEnhance()">강화</button>
    <button onclick="openGag()">개그</button>
    <button onclick="openAttendance()">출석</button>
    <button onclick="checkCoins()">코인 확인</button>
  </div>

  <div id="coinDisplay">코인: 0</div>

  <img id="spongebob" src="https://i.ibb.co/9ybHChN/spongebob.png" alt="스폰지밥">
  <img id="squidward" src="https://i.ibb.co/xMKzMfR/squidward.png" alt="징징이">

  <div id="guestArea"></div>

  <script>
    let coins = 0;

    const mainMenuBtn = document.getElementById('mainMenuBtn');
    const menuPanel = document.getElementById('menuPanel');
    const coinDisplay = document.getElementById('coinDisplay');
    const guestArea = document.getElementById('guestArea');

    mainMenuBtn.addEventListener('click', () => {
      menuPanel.style.display = (menuPanel.style.display === 'none' || menuPanel.style.display === '') ? 'flex' : 'none';
    });

    function openEnhance() {
      alert("[강화] 시스템을 실행합니다. 강화석을 준비하세요!");
    }

    function openGag() {
      const gags = [
        "징징이: 오늘은 일 안 하고 싶다...",
        "스폰지밥: 주문이 들어왔다고요!", 
        "플랑크톤: 레시피는 내 것이다!",
        "게살버거는 사랑입니다."
      ];
      alert("[개그 코너] " + gags[Math.floor(Math.random() * gags.length)]);
    }

    function openAttendance() {
      alert("[출석] 보상 5코인을 획득했습니다!");
      coins += 5;
      updateCoinDisplay();
    }

    function checkCoins() {
      alert(`현재 보유 코인: ${coins}`);
    }

    function updateCoinDisplay() {
      coinDisplay.textContent = `코인: ${coins}`;
    }

    // 손님 등장 시스템
    function spawnGuest() {
      const guest = document.createElement('img');
      guest.src = 'https://i.ibb.co/YQ9nH3n/fish-customer.png';
      guest.classList.add('guest');
      guestArea.innerHTML = '';
      guestArea.appendChild(guest);
    }

    // 아무 곳이나 클릭 시 손님 처리
    document.body.addEventListener('click', (e) => {
      if (!menuPanel.contains(e.target) && e.target.id !== 'mainMenuBtn') {
        if (guestArea.children.length > 0) {
          coins++;
          updateCoinDisplay();
          guestArea.innerHTML = '';
        }
      }
    });

    // 일정 시간마다 손님 등장
    setInterval(spawnGuest, 10000); // 10초마다 손님 등장
  </script>
</body>
</html>
