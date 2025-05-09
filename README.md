<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>크러스티 크랩 게임</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: url('https://i.imgur.com/4ZQ0fUQ.jpg') no-repeat center center fixed;
            background-size: cover;
            color: white;
            text-align: center;
            padding: 20px;
        }

        h1 {
            font-size: 40px;
            color: yellow;
            text-shadow: 2px 2px black;
        }

        .menu-btn {
            margin: 5px;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }

        .section {
            display: none;
            margin-top: 20px;
        }

        .visible {
            display: block !important;
        }

        .btn {
            padding: 10px;
            margin: 5px;
            background: orange;
            border: none;
            color: white;
            cursor: pointer;
            border-radius: 5px;
        }

        #plankton-area {
            position: relative;
            width: 320px;
            height: 200px;
            margin: 0 auto;
            background-color: rgba(0,0,0,0.4);
        }

        .plankton {
            width: 30px;
            height: 30px;
            background: green;
            color: white;
            border-radius: 50%;
            text-align: center;
            line-height: 30px;
            font-weight: bold;
            cursor: pointer;
            position: absolute;
        }

        .ingredients {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            margin-bottom: 30px;
        }

        .ingredient {
            width: 100px;
            height: 100px;
            margin: 10px;
            background-color: #ffcc00;
            border-radius: 8px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 18px;
            color: white;
            cursor: pointer;
        }

        .ingredient:hover {
            background-color: #ff9900;
        }

        .ingredient-row {
            margin-top: 20px;
        }

        .timer {
            font-size: 24px;
            color: #ff3333;
        }

        .status {
            font-size: 20px;
            margin-top: 20px;
        }

        .result {
            font-size: 24px;
            color: green;
            font-weight: bold;
        }

        .failed {
            color: red;
        }

        .button {
            padding: 12px 20px;
            background-color: #00cc66;
            color: white;
            font-size: 20px;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            margin-top: 20px;
        }

        .button:hover {
            background-color: #009955;
        }

        .section h2 {
            font-size: 30px;
            margin-bottom: 20px;
        }

        .battle-info {
            margin-top: 20px;
        }

        .battle-btn {
            padding: 12px 20px;
            background-color: #00cc66;
            color: white;
            font-size: 20px;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            margin-top: 20px;
        }
        
        .battle-btn:hover {
            background-color: #009955;
        }

        .progress-bar {
            height: 20px;
            width: 100%;
            background: #ccc;
            margin-top: 10px;
            position: relative;
        }

        .progress {
            height: 100%;
            background: green;
        }

        .battle-log {
            margin-top: 10px;
        }

        .battle-log p {
            font-size: 16px;
        }
    </style>
</head>
<body>

    <h1>크러스티 크랩 게임</h1>
    <div>
        <button class="menu-btn" onclick="showSection('hamburger-game')">햄버거 게임</button>
        <button class="menu-btn" onclick="showSection('plankton-game')">플랑크톤 침략</button>
        <button class="menu-btn" onclick="showSection('gacha-section')">가챠</button>
        <button class="menu-btn" onclick="showSection('battlepass')">배틀패스</button>
        <button class="menu-btn" onclick="showSection('pvp-section')">PvP</button>
    </div>
    <p>코인: <span id="coin-display">0</span> | 강화석: <span id="stones">0</span> | Lv.<span id="level">0</span></p>

    <!-- 햄버거 게임 -->
    <div class="section" id="hamburger-game">
        <h2>햄버거 만들기</h2>
        <p>주어진 재료로 햄버거를 만들어 보세요!</p>
        <div class="ingredients">
            <div class="ingredient" id="lettuce">상추</div>
            <div class="ingredient" id="cheese">치즈</div>
            <div class="ingredient" id="patty">패티</div>
            <div class="ingredient" id="tomato">토마토</div>
            <div class="ingredient" id="onion">양파</div>
        </div>
        <div class="ingredient-row">
            <p>조합 순서: <span id="ingredient-sequence">상추, 치즈, 패티, 토마토, 양파</span></p>
            <button class="btn" onclick="makeBurger()">햄버거 만들기</button>
        </div>
    </div>

    <!-- 플랑크톤 침략 -->
    <div class="section" id="plankton-game">
        <h2>플랑크톤 침략</h2>
        <p>플랑크톤이 집게리아를 침략하려고 합니다. 제한 시간 내에 문제를 풀어 플랑크톤을 막아주세요!</p>
        <div id="plankton-area">
            <!-- 플랑크톤이 들어갈 공간 -->
        </div>
        <button class="btn" onclick="startPlanktonAttack()">침략 시작</button>
    </div>

    <!-- 가챠 시스템 -->
    <div class="section" id="gacha-section">
        <h2>가챠 시스템</h2>
        <p>캐릭터 가챠를 통해 새로운 캐릭터를 획득할 수 있습니다!</p>
        <button class="btn" onclick="rollGacha()">가챠 돌리기</button>
        <div id="gacha-result"></div>
    </div>

    <!-- 배틀패스 -->
    <div class="section" id="battlepass">
        <h2>배틀패스</h2>
        <p>현재 배틀패스 레벨: <span id="battlepass-level">1</span></p>
        <div id="battlepass-rewards">
            <!-- 보상 항목들이 동적으로 생성됩니다 -->
        </div>
        <button class="btn" onclick="levelUpBattlepass()">배틀패스 레벨업</button>
    </div>

    <!-- PvP -->
    <div class="section" id="pvp-section">
        <h2>PvP 전투</h2>
        <p>상대 유저의 정보:</p>
        <div class="battle-info" id="battle-info">
            <!-- 상대 정보가 여기에 표시됩니다 -->
        </div>
        <button class="battle-btn" onclick="startPvP()">PvP 시작</button>
        <div class="battle-log" id="battle-log"></div>
    </div>

    <script>
        let coins = 0;
        let stones = 0;
        let level = 0;
        let battlepassLevel = 1;
        let playerCharacter = {
            name: "스폰지밥",
            health: 100,
            attack: 20,
            defense: 10,
            specialAbility: "버거 조리 속도 증가"
        };

        const characters = [
            { name: "스폰지밥", health: 100, attack: 20, defense: 10, specialAbility: "버거 조리 속도 증가" },
            { name: "징징이", health: 120, attack: 25, defense: 15, specialAbility: "공격력 증가" },
            { name: "뚱이", health: 150, attack: 30, defense: 20, specialAbility: "방어력 증가" },
            { name: "황금 스폰지밥", health: 250, attack: 40, defense: 30, specialAbility: "모든 능력치 증가" }
        ];

        function updateDisplay() {
            document.getElementById('coin-display').innerText = coins;
            document.getElementById('stones').innerText = stones;
            document.getElementById('level').innerText = level;
        }

        function showSection(id) {
            document.querySelectorAll('.section').forEach(div => div.style.display = 'none');
            document.getElementById(id).style.display = 'block';
        }

        // 햄버거 만들기
        let ingredientSequence = ['상추', '치즈', '패티', '토마토', '양파'];
        let userSequence = [];
        function makeBurger() {
            if (JSON.stringify(ingredientSequence) === JSON.stringify(userSequence)) {
                alert("햄버거 만들기 성공!");
                coins += 10;
                updateDisplay();
            } else {
                alert("햄버거 만들기 실패!");
            }
        }

        function selectIngredient(ingredient) {
            userSequence.push(ingredient);
            if (userSequence.length === ingredientSequence.length) {
                makeBurger();
            }
        }

        // 플랑크톤 침략
        function startPlanktonAttack() {
            alert("플랑크톤의 침략을 막아주세요!");
        }

        // 가챠 시스템
        function rollGacha() {
            const randomIndex = Math.floor(Math.random() * characters.length);
            const selectedCharacter = characters[randomIndex];
            document.getElementById('gacha-result').innerText = `새로운 캐릭터 획득: ${selectedCharacter.name}`;
        }

        // 배틀패스
        function levelUpBattlepass() {
            if (battlepassLevel < 10) {
                battlepassLevel++;
                document.getElementById('battlepass-level').innerText = battlepassLevel;
                displayBattlepassRewards();
            } else {
                alert("배틀패스 레벨이 이미 최고입니다!");
            }
        }

        function displayBattlepassRewards() {
            const rewards = [
                { level: 1, reward: "코인 100" },
                { level: 2, reward: "강화석 10개" },
                { level: 3, reward: "새로운 캐릭터: 징징이" },
                { level: 4, reward: "코인 200" },
                { level: 5, reward: "새로운 캐릭터: 똑똑이" },
                { level: 6, reward: "강화석 20개" },
                { level: 7, reward: "황금 스폰지밥 캐릭터" },
                { level: 8, reward: "코인 500" },
                { level: 9, reward: "강화석 50개" },
                { level: 10, reward: "최고의 보상: 모든 캐릭터 강화를 위한 특수 아이템" }
            ];

            const rewardElement = rewards.filter(r => r.level <= battlepassLevel).map(r => `<p>레벨 ${r.level}: ${r.reward}</p>`).join('');
            document.getElementById('battlepass-rewards').innerHTML = rewardElement;
        }

        // PvP
        function startPvP() {
            const opponent = characters[Math.floor(Math.random() * characters.length)];
            const battleResult = battleSimulation(playerCharacter, opponent);
            document.getElementById('battle-info').innerHTML = `
                <p>상대: ${opponent.name}</p>
                <p>체력: ${opponent.health}</p>
                <p>공격력: ${opponent.attack}</p>
                <p>방어력: ${opponent.defense}</p>
                <p>특수 능력: ${opponent.specialAbility}</p>
            `;
            document.getElementById('battle-log').innerHTML = `<p>${battleResult}</p>`;
        }

        function battleSimulation(player, enemy) {
            let playerHealth = player.health;
            let enemyHealth = enemy.health;

            while (playerHealth > 0 && enemyHealth > 0) {
                enemyHealth -= (player.attack - enemy.defense);
                playerHealth -= (enemy.attack - player.defense);
            }

            if (playerHealth > 0) {
                return `${player.name} 승리!`;
            } else {
                return `${enemy.name} 승리!`;
            }
        }

        updateDisplay();
        displayBattlepassRewards();
    </script>
</body>
</html>
