<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>크러스티 크랩: 궁극의 경영 시뮬레이션</title>
  <link href="https://fonts.googleapis.com/css2?family=Jua&display=swap" rel="stylesheet">
  <style>
    body {
      margin: 0;
      font-family: 'Jua', sans-serif;
      background: url('https://static.wikia.nocookie.net/spongebob/images/f/fb/Krusty_Krab_Interior.png') no-repeat center center fixed;
      background-size: cover;
      color: #fff;
      text-align: center;
    }

    #game-container {
      background: rgba(0, 0, 0, 0.5);
      padding: 30px;
      margin: 30px auto;
      border-radius: 15px;
      width: 90%;
      max-width: 800px;
    }

    h1 {
      font-size: 2.5em;
      margin-bottom: 20px;
    }

    .menu-btn {
      font-size: 1.2em;
      padding: 15px 25px;
      margin: 10px;
      border: none;
      border-radius: 10px;
      background-color: #f9d342;
      color: #000;
      cursor: pointer;
      transition: transform 0.2s;
    }

    .menu-btn:hover {
      transform: scale(1.05);
    }

    #output, #minigame-area {
      margin-top: 20px;
    }

    .hidden {
      display: none;
    }

    #coin-display {
      font-size: 1.4em;
      margin-bottom: 10px;
      color: #ffeb3b;
    }
  </style>
</head>
<body>
  <div id="game-container">
    <h1>크러스티 크랩: 궁극의 경영</h1>
    <div id="coin-display">코인: <span id="coin-count">0</span></div>
    <button class="menu-btn" onclick="startBurgerGame()">스폰지밥 버거 미니게임</button>
    <button class="menu-btn" onclick="startPlanktonDefense()">플랑크톤 침략 방어</button>
    <button class="menu-btn" onclick="earnCoin()">코인 획득</button>
    <div id="output"></div>
    <div id="minigame-area" class="hidden"></div>
  </div>

  <script>
    let coins = 0;

    function updateCoins() {
      document.getElementById('coin-count').textContent = coins;
    }

    function earnCoin() {
      const randomGuest = Math.random();
      if (randomGuest < 0.05) {
        coins += 3;
        document.getElementById('output').innerText = "만수르가 왔다! +3 코인!";
      } else if (randomGuest < 0.1) {
        coins += 3;
        document.getElementById('output').innerText = "뚱이가 한가득 주문했다! +3 코인!";
      } else {
        coins += 1;
        document.getElementById('output').innerText = "오늘도 평범한 하루! +1 코인.";
      }
      updateCoins();
    }

    function startBurgerGame() {
      const area = document.getElementById('minigame-area');
      area.classList.remove('hidden');
      area.innerHTML = `
        <h2>스폰지밥 버거 만들기!</h2>
        <p>제한 시간 내에 아래 재료를 순서대로 클릭하세요: 빵 → 고기 → 야채 → 빵</p>
        <div id="ingredients"></div>
        <div id="burger-result"></div>
      `;

      const sequence = ["빵", "고기", "야채", "빵"];
      let index = 0;

      const ingredientDiv = document.getElementById('ingredients');
      sequence.forEach(ing => {
        const btn = document.createElement('button');
        btn.className = "menu-btn";
        btn.textContent = ing;
        btn.onclick = () => {
          if (btn.textContent === sequence[index]) {
            index++;
            btn.disabled = true;
            if (index === sequence.length) {
              document.getElementById('burger-result').textContent = "완벽한 버거! +2 코인!";
              coins += 2;
              updateCoins();
            }
          } else {
            document.getElementById('burger-result').textContent = "순서가 틀렸어요!";
          }
        };
        ingredientDiv.appendChild(btn);
      });
    }

    function startPlanktonDefense() {
      const area = document.getElementById('minigame-area');
      area.classList.remove('hidden');
      area.innerHTML = `
        <h2>플랑크톤 방어 게임</h2>
        <p>5초 동안 플랑크톤이 나타나면 클릭해서 막으세요!</p>
        <div id="plankton-area"></div>
        <div id="plankton-result"></div>
      `;

      let hits = 0;
      const planktonArea = document.getElementById('plankton-area');

      const spawn = () => {
        const plankton = document.createElement('div');
        plankton.textContent = "플랑크톤!";
        plankton.className = "menu-btn";
        plankton.onclick = () => {
          hits++;
          plankton.remove();
        };
        planktonArea.appendChild(plankton);

        setTimeout(() => {
          if (document.body.contains(plankton)) {
            plankton.remove();
          }
        }, 1000);
      };

      const gameDuration = 5000;
      const interval = setInterval(spawn, 700);

      setTimeout(() => {
        clearInterval(interval);
        document.getElementById('plankton-result').textContent = `방어 성공: ${hits}회`;
        if (hits >= 5) {
          coins += 3;
          document.getElementById('plankton-result').textContent += " +3 코인!";
        } else {
          document.getElementById('plankton-result').textContent += " 더 노력하세요!";
        }
        updateCoins();
      }, gameDuration);
    }

    updateCoins(); // 초기 코인 표시
  </script>
</body>
</html>
