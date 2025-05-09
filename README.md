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

        .rank-display {
            margin-top: 20px;
        }

        .shop-item {
            padding: 10px 20px;
            background-color: #ff6600;
            color: white;
            font-size: 18px;
            border-radius: 8px;
            margin-top: 10px;
            cursor: pointer;
        }

        .shop-item:hover {
            background-color: #ff3300;
        }

        .order-container {
            margin-top: 20px;
        }

        .order-item {
            padding: 12px 20px;
            background-color: #ffcc00;
            color: white;
            font-size: 18px;
            border-radius: 8px;
            margin-top: 10px;
            cursor: pointer;
        }

        .order-item:hover {
            background-color: #ff9900;
        }
    </style>
</head>
<body>

    <h1>크러스티 크랩 게임</h1>
    <div>
        <button class="menu-btn" onclick="showSection('hamburger-game')">햄버거 게임</button>
        <button class="menu-btn" onclick="showSection('shop')">상점</button>
        <button class="menu-btn" onclick="showSection('gacha-section')">가챠</button>
        <button class="menu-btn" onclick="showSection('battlepass')">배틀패스</button>
        <button class="menu-btn" onclick="showSection('pvp-section')">PvP</button>
        <button class="menu-btn" onclick="showSection('rank')">단골 랭크</button>
    </div>

    <div>
        <p>코인: <span id="coin-display">0</span> | 강화석: <span id="stones">0</span> | Lv.<span id="level">0</span></p>
    </div>

    <!-- 햄버거 게임 -->
    <div class="section" id="hamburger-game">
        <h2>햄버거 만들기</h2>
        <p>주어진 재료로 햄버거를 만들어 보세요!</p>
        <div class="ingredient" onclick="addIngredient('상추')">상추</div>
        <div class="ingredient" onclick="addIngredient('치즈')">치즈</div>
        <div class="ingredient" onclick="addIngredient('패티')">패티</div>
        <div class="ingredient" onclick="addIngredient('토마토')">토마토</div>
        <div class="ingredient" onclick="addIngredient('양파')">양파</div>
        <div class="ingredient" onclick="addIngredient('치킨패티')">치킨 패티</div>
        <div class="ingredient" onclick="addIngredient('양상추')">양상추</div>
        <div class="ingredient" onclick="addIngredient('피클')">피클</div>
        <div class="ingredient" onclick="addIngredient('소스')">소스</div>
        <div class="ingredient-row">
            <p>조합 순서: <span id="ingredient-sequence">상추, 치즈, 패티, 토마토, 양파</span></p>
            <button class="btn" onclick="makeBurger()">햄버거 만들기</button>
        </div>
    </div>

    <!-- 단골 랭크 -->
    <div class="section" id="rank">
        <h2>단골 랭크</h2>
        <p>단골 랭크: <span id="rank-display">뚱이: 300</span></p>
        <p>목표: 뚱이의 구매 개수를 넘기세요!</p>
    </div>

    <!-- 상점 -->
    <div class="section" id="shop">
        <h2>상점</h2>
        <p>오늘의 추천 메뉴:</p>
        <div class="shop-item" onclick="buyItem('햄버거')">햄버거 (가격: 50 코인)</div>
        <div class="shop-item" onclick="buyItem('음료수')">음료수 (가격: 30 코인)</div>
        <div class="shop-item" onclick="buyItem('감자튀김')">감자튀김 (가격: 40 코인)</div>
        <p>주간 메뉴:</p>
        <div class="shop-item" onclick="buyItem('버거 세트')">버거 세트 (가격: 100 코인)</div>
        <p>특별 메뉴:</p>
        <div class="shop-item" onclick="buyItem('특별버거')">특별버거 (가격: 200 코인)</div>
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
        <div id="battlepass-info">
            <p>배틀패스 레벨에 따라 보상이 다릅니다.</p>
        </div>
        <button class="btn" onclick="levelUpBattlepass()">배틀패스 레벨 업</button>
    </div>

    <!-- PvP 전투 -->
    <div class="section" id="pvp-section">
        <h2>PvP 전투</h2>
        <button class="battle-btn" onclick="startPvP()">PvP 시작</button>
        <div id="battle-log" class="battle-log"></div>
    </div>

    <script>
        let playerCharacter = { name: '플래여', health: 100, attack: 20, defense: 15 };
        let playerCoins = 0;
        let playerRank = 0;
        let battlepassLevel = 1;

        // 재료 추가 및 랜덤 조합 순서 생성
        let ingredients = ['상추', '치즈', '패티', '토마토', '양파', '치킨패티', '양상추', '피클', '소스'];
        let ingredientSequence = generateRandomSequence();

        function generateRandomSequence() {
            let shuffled = ingredients.sort(() => Math.random() - 0.5);
            return shuffled.slice(0, 4); // 랜덤으로 4개의 재료를 선택
        }

        function addIngredient(ingredient) {
            currentIngredients.push(ingredient);
            document.getElementById('ingredient-sequence').innerText = currentIngredients.join(', ');
        }

        function makeBurger() {
            let success = true;
            if (currentIngredients.length !== ingredientSequence.length) {
                success = false;
            } else {
                for (let i = 0; i < ingredientSequence.length; i++) {
                    if (currentIngredients[i] !== ingredientSequence[i]) {
                        success = false;
                        break;
                    }
                }
            }

            if (success) {
                playerCoins += 10;
                document.getElementById('coin-display').innerText = playerCoins;
                alert("햄버거 조합 성공! 10 코인을 얻었습니다.");
            } else {
                alert("햄버거 조합 실패! 다시 시도해 보세요.");
            }

            // 새로운 랜덤 조합 생성
            ingredientSequence = generateRandomSequence();
            currentIngredients = [];
        }

        // 상점 아이템 구매
        function buyItem(item) {
            let cost = 0;
            switch (item) {
                case '햄버거':
                    cost = 50;
                    break;
                case '음료수':
                    cost = 30;
                    break;
                case '감자튀김':
                    cost = 40;
                    break;
                case '버거 세트':
                    cost = 100;
                    break;
                case '특별버거':
                    cost = 200;
                    break;
            }

            if (playerCoins >= cost) {
                playerCoins -= cost;
                document.getElementById('coin-display').innerText = playerCoins;
                alert(`${item}를 구매했습니다!`);
            } else {
                alert("코인이 부족합니다.");
            }
        }

        // 가챠
        function rollGacha() {
            const characters = ['스폰지밥', '징징이', '파트너플래여'];
            const character = characters[Math.floor(Math.random() * characters.length)];
            document.getElementById('gacha-result').innerText = `새로운 캐릭터: ${character}`;
        }

        // PvP
        function startPvP() {
            const enemy = { name: '라이벌1', health: 100, attack: 30, defense: 15 };
            let playerDamage = playerCharacter.attack - enemy.defense;
            let enemyDamage = enemy.attack - playerCharacter.defense;

            if (playerDamage < 0) playerDamage = 0;
            if (enemyDamage < 0) enemyDamage = 0;

            enemy.health -= playerDamage;
            playerCharacter.health -= enemyDamage;

            const battleLog = `전투 결과: ${playerCharacter.name}가 ${enemy.name}에게 ${playerDamage} 피해를 주었습니다.`;
            document.getElementById('battle-log').innerText = battleLog;

            if (enemy.health <= 0) {
                alert(`${playerCharacter.name} 승리!`);
            } else if (playerCharacter.health <= 0) {
                alert(`${enemy.name} 승리!`);
            }
        }

        // 배틀패스 레벨업
        function levelUpBattlepass() {
            battlepassLevel += 1;
            document.getElementById('battlepass-level').innerText = battlepassLevel;
            alert(`배틀패스 레벨 ${battlepassLevel}로 업그레이드되었습니다!`);
        }

        // 단골 랭크
        function updateRank() {
            document.getElementById('rank-display').innerText = `뚱이: ${playerRank}`;
        }
    </script>
</body>
</html>
