<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>스폰지밥 버거 제국</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background-color: #f4f4f9;
      text-align: center;
    }

    h1 {
      color: #f2b600;
    }

    #coin-status {
      font-size: 20px;
      margin-top: 20px;
    }

    #equipment-container {
      margin-top: 30px;
    }

    button {
      padding: 10px 20px;
      margin: 5px;
      font-size: 16px;
      cursor: pointer;
      background-color: #007BFF;
      color: white;
      border: none;
      border-radius: 5px;
    }

    button:disabled {
      background-color: #aaa;
      cursor: not-allowed;
    }

    #equipment-status {
      margin-top: 40px;
      font-size: 18px;
    }

    #story-container {
      margin-top: 30px;
      font-size: 16px;
    }

    #mini-game-container {
      margin-top: 30px;
      font-size: 16px;
    }

  </style>
</head>
<body>

  <h1>스폰지밥 버거 제국</h1>

  <div id="story-container">
    <h3>스토리 진행</h3>
    <p id="story-text">
      스폰지밥은 집게리아에서 최고의 버거를 만들고 있습니다. 하지만 어느 날, 
      집게사장이 비밀스러운 버거 레시피를 훔치려는 플랑크톤의 침략을 받게 됩니다.
      당신은 스폰지밥을 도와 버거를 만들고, 장비를 업그레이드하여 플랑크톤의 침략을 막아야 합니다!
    </p>
    <button onclick="startGame()">게임 시작</button>
  </div>

  <div id="coin-status">
    현재 코인: <span id="coin-count">1000</span> 코인
  </div>

  <div id="mini-game-container">
    <h3>미니게임</h3>
    <button onclick="startMiniGame()">버거 만들기 미니게임</button>
  </div>

  <div id="equipment-container">
    <h2>장비 구매</h2>
    <button onclick="purchaseEquipment(0)">버거 제작기 (200 코인)</button>
    <button onclick="purchaseEquipment(1)">코인 강화기 (300 코인)</button>
    <button onclick="purchaseEquipment(2)">주문 처리기 (500 코인)</button>

    <h3>장비 강화</h3>
    <button onclick="upgradeEquipment(0)">버거 제작기 강화</button>
    <button onclick="upgradeEquipment(1)">코인 강화기 강화</button>
    <button onclick="upgradeEquipment(2)">주문 처리기 강화</button>
  </div>

  <div id="equipment-status">
    <h3>장비 상태</h3>
    <p id="equipment-status-text"></p>
  </div>

  <script>
    // 기본 설정
    let coinCount = 1000;
    let burgerSpeed = 1;
    let miniGameActive = false;

    // 장비 시스템
    class Equipment {
      constructor(name, effect, price, level = 1) {
        this.name = name;
        this.effect = effect;
        this.price = price;
        this.level = level;
        this.upgradeCost = 100 * level;
      }

      upgrade() {
        if (coinCount >= this.upgradeCost) {
          coinCount -= this.upgradeCost;
          this.level++;
          this.effect *= 1.2;
          this.upgradeCost = Math.floor(this.upgradeCost * 1.5);
          updateGameStatus();
          logMessage(`${this.name}이(가) 레벨 ${this.level}로 강화되었습니다!`);
        } else {
          logMessage("코인이 부족합니다!");
        }
      }
    }

    // 장비 목록
    let equipmentList = [
      new Equipment("버거 제작기", 1.1, 200),
      new Equipment("코인 강화기", 1.2, 300),
      new Equipment("주문 처리기", 1.5, 500)
    ];

    // 게임 시작
    function startGame() {
      document.getElementById("story-container").style.display = "none";
      updateGameStatus();
      document.getElementById("coin-status").style.display = "block";
      document.getElementById("equipment-container").style.display = "block";
    }

    // 장비 구매 함수
    function purchaseEquipment(index) {
      const equipment = equipmentList[index];
      if (coinCount >= equipment.price) {
        coinCount -= equipment.price;
        logMessage(`${equipment.name}을(를) 구매하셨습니다!`);
        updateGameStatus();
      } else {
        logMessage("코인이 부족합니다!");
      }
    }

    // 장비 강화 함수
    function upgradeEquipment(index) {
      const equipment = equipmentList[index];
      equipment.upgrade();
    }

    // 미니게임 시작
    function startMiniGame() {
      if (!miniGameActive) {
        miniGameActive = true;
        logMessage("버거 만들기 미니게임이 시작되었습니다!");
        document.getElementById("mini-game-container").innerHTML = `
          <h3>버거 만들기 미니게임</h3>
          <p>버거를 만들고 코인을 획득하세요! 성공적으로 버거를 만들면 코인을 얻습니다.</p>
          <button onclick="playBurgerMiniGame()">버거 만들기</button>
        `;
      }
    }

    // 버거 만들기 미니게임
    function playBurgerMiniGame() {
      let randomSuccess = Math.random() < 0.7;
      if (randomSuccess) {
        coinCount += 10;
        logMessage("버거를 성공적으로 만들었습니다! 10 코인을 획득했습니다.");
      } else {
        logMessage("버거 만들기에 실패했습니다.");
      }
      updateGameStatus();
      endMiniGame();
    }

    // 미니게임 종료
    function endMiniGame() {
      setTimeout(() => {
        miniGameActive = false;
        document.getElementById("mini-game-container").innerHTML = `
          <h3>미니게임</h3>
          <button onclick="startMiniGame()">버거 만들기 미니게임</button>
        `;
      }, 2000);
    }

    // UI 업데이트 함수
    function updateGameStatus() {
      document.getElementById("coin-count").innerText = coinCount;
      document.getElementById("equipment-status-text").innerHTML = equipmentList.map(equipment =>
        `${equipment.name} - 레벨: ${equipment.level}, 효과: ${equipment.effect.toFixed(2)}`
      ).join('<br>');
    }

    // 로그 메시지 출력
    function logMessage(message) {
      console.log(message);
    }

    updateGameStatus();
  </script>
</body>
</html>
