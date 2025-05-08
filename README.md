<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>스폰지밥 햄버거 게임</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-image: url('background.jpg');
            background-size: cover;
            color: white;
            margin: 0;
            padding: 0;
        }

        h1 {
            text-align: center;
            margin-top: 50px;
        }

        #menu {
            text-align: center;
            margin-top: 30px;
        }

        button {
            margin: 10px;
            padding: 15px;
            font-size: 16px;
            cursor: pointer;
            background-color: #f39c12;
            color: white;
            border: none;
            border-radius: 5px;
        }

        button:hover {
            background-color: #e67e22;
        }

        #main-content {
            display: flex;
            justify-content: center;
            align-items: center;
            margin-top: 100px;
            flex-wrap: wrap;
        }

        #main-content img {
            width: 20vw;
            height: auto;
            margin: 10px;
        }

        .notification {
            font-size: 24px;
            color: yellow;
            text-align: center;
            margin-top: 20px;
            font-weight: bold;
        }

        .order-container {
            text-align: center;
            margin-top: 20px;
        }

        #coin-display {
            position: absolute;
            top: 10px;
            right: 20px;
            font-size: 20px;
            color: yellow;
        }

        #burger-feedback, #gacha-result, #gacha-general-result, #pvp-result {
            text-align: center;
            margin-top: 20px;
            font-size: 20px;
            color: white;
        }

        .section {
            display: none;
        }

        #attendance-result {
            font-size: 20px;
            color: yellow;
            margin-top: 20px;
            text-align: center;
        }
    </style>
</head>
<body>
    <h1>스폰지밥 햄버거 만들기 게임</h1>
    <div id="coin-display">코인: 50</div>

    <div id="menu">
        <button onclick="showSection('burger-making')">🍔 햄버거 만들기</button>
        <button onclick="showSection('gacha-container')">🎲 강화 가차</button>
        <button onclick="showSection('gacha-general')">🎲 일반 가차</button>
        <button onclick="showSection('character-upgrade')">⚙️ 캐릭터 강화</button>
        <button onclick="showSection('shop')">🛒 상점</button>
        <button onclick="showSection('pvp-container')">⚔️ PVP</button>
        <button onclick="showSection('rankings')">🏆 단골 랭킹</button>
        <button onclick="toggleMusic()">🎵 배경음악 재생/정지</button>
    </div>

    <!-- 출석 시스템 -->
    <div id="attendance-system" class="section">
        <h3>📅 출석 보상</h3>
        <button onclick="attendanceGacha()">출석 가차 (1코인)</button>
        <p id="attendance-result"></p>
    </div>

    <!-- 햄버거 만들기 -->
    <div id="burger-making" class="section">
        <h3>🍔 햄버거 만들기</h3>
        <div id="main-content">
            <img src="spongebob.png" alt="스폰지밥" />
            <img src="squidward.png" alt="징징이" />
        </div>
        <div class="order-container">
            <button onclick="acceptOrder()">주문 받기!</button>
        </div>
        <p id="burger-feedback"></p>
    </div>

    <!-- 강화 가차 -->
    <div id="gacha-container" class="section">
        <h3>🎲 강화 가차</h3>
        <button onclick="pullGacha()">강화 가차 뽑기 (10코인)</button>
        <p id="gacha-result"></p>
    </div>

    <!-- 일반 가차 -->
    <div id="gacha-general" class="section">
        <h3>🎲 일반 가차</h3>
        <button onclick="pullGeneralGacha()">일반 가차 뽑기 (5코인)</button>
        <p id="gacha-general-result"></p>
    </div>

    <!-- PVP -->
    <div id="pvp-container" class="section">
        <h3>⚔️ PVP 전투</h3>
        <button onclick="startPVP()">PVP 전투 시작</button>
        <p id="pvp-result"></p>
    </div>

    <!-- 음악 -->
    <audio id="background-music" loop>
        <source src="background-music.mp3" type="audio/mp3">
        Your browser does not support the audio element.
    </audio>

    <script>
        const gameState = {
            coins: 50,
            ordersCompleted: 0,
            burgerOrders: ['빵', '고기', '치즈', '양상추'],
            currentOrder: [],
            makingBurger: false,
            squidwardOrderCount: 1,
            spongebobSpeed: 1000,
            gachaItems: ["강화석", "방어권", "꽝"],
            generalGachaItems: ["5코인", "10코인", "강화석", "캐릭터", "꽝"],
            attendanceCount: 0
        };

        function updateUI() {
            document.getElementById('coin-display').innerText = `코인: ${gameState.coins}`;
        }

        function showSection(id) {
            document.querySelectorAll('.section').forEach(section => {
                section.style.display = 'none';
            });
            document.getElementById(id).style.display = 'block';
        }

        function acceptOrder() {
            if (gameState.currentOrder.length === 0) {
                gameState.currentOrder = gameState.burgerOrders.slice();
            }

            let order = gameState.currentOrder.join(' + ');
            document.getElementById('burger-feedback').innerText = `주문: ${order}. 햄버거를 만드세요!`;
            createBurger();
        }

        function createBurger() {
            if (gameState.makingBurger) return;
            gameState.makingBurger = true;
            setTimeout(() => {
                gameState.ordersCompleted++;
                gameState.coins += 1;
                document.getElementById('burger-feedback').innerText = `햄버거 ${gameState.ordersCompleted}개가 만들어졌습니다.`;
                updateUI();
                gameState.makingBurger = false;
            }, gameState.spongebobSpeed);
        }

        function pullGacha() {
            if (gameState.coins >= 10) {
                gameState.coins -= 10;
                const item = getRandomItem(gameState.gachaItems);
                document.getElementById('gacha-result').innerText = `획득: ${item}`;
                alert(item === "꽝" ? "꽝! 다시 시도하세요." : `${item}을(를) 획득했습니다!`);
                updateUI();
            } else {
                alert("코인이 부족합니다.");
            }
        }

        function pullGeneralGacha() {
            if (gameState.coins >= 5) {
                gameState.coins -= 5;
                const item = getRandomItem(gameState.generalGachaItems);
                document.getElementById('gacha-general-result').innerText = `획득: ${item}`;
                if (item === "5코인") gameState.coins += 5;
                else if (item === "10코인") gameState.coins += 10;
                alert(item === "꽝" ? "꽝! 다시 시도하세요." : `${item}을(를) 획득했습니다!`);
                updateUI();
            } else {
                alert("코인이 부족합니다.");
            }
        }

        function getRandomItem(array) {
            return array[Math.floor(Math.random() * array.length)];
        }

        function startPVP() {
            const result = Math.random() < 0.5 ? "승리!" : "패배!";
            document.getElementById('pvp-result').innerText = `전투 결과: ${result}`;
        }

        function attendanceGacha() {
            if (gameState.attendanceCount < 7) {
                gameState.attendanceCount++;
                gameState.coins += 5;
                document.getElementById('attendance-result').innerText = `출석 보상: 5코인 획득!`;
            } else {
                document.getElementById('attendance-result').innerText = "출석 가챠 완료!";
            }
            updateUI();
        }

        function toggleMusic() {
            const music = document.getElementById('background-music');
            if (music.paused) {
                music.play();
            } else {
                music.pause();
            }
        }

        // 초기 화면 세팅
        showSection('burger-making');
    </script>
</body>
</html>
