// game.js

let coins = 500;
let level = 1;
let towerCost = 100;
let upgradeCost = 100;
let towers = [];
let enemies = [];
let gameOver = false;

const coinDisplay = document.getElementById("coin-count");
const levelDisplay = document.getElementById("level-count");
const gameBoard = document.getElementById("game-board");
const startGameButton = document.getElementById("start-game");
const upgradeButton = document.getElementById("upgrade-tower");
const messageDisplay = document.getElementById("message");

// 게임 시작 버튼 클릭 시
startGameButton.addEventListener("click", startGame);

// 타워 업그레이드 버튼 클릭 시
upgradeButton.addEventListener("click", upgradeTower);

// 게임 시작 함수
function startGame() {
    gameOver = false;
    coins = 500;
    level = 1;
    towers = [];
    enemies = [];
    updateUI();
    setupBoard();
    spawnEnemies();
    messageDisplay.textContent = '';
}

// 게임 화면 업데이트 함수
function updateUI() {
    coinDisplay.textContent = coins;
    levelDisplay.textContent = level;
}

// 보드 셀 생성
function setupBoard() {
    gameBoard.innerHTML = '';
    for (let row = 0; row < 5; row++) {
        for (let col = 0; col < 5; col++) {
            const cell = document.createElement('div');
            cell.classList.add('grid-cell');
            cell.dataset.row = row;
            cell.dataset.col = col;
            cell.addEventListener('click', () => placeTower(row, col));
            gameBoard.appendChild(cell);
        }
    }
}

// 타워 배치 함수
function placeTower(row, col) {
    if (gameOver) return;
    
    const cell = document.querySelector(`[data-row='${row}'][data-col='${col}']`);
    if (cell.classList.contains('tower')) {
        messageDisplay.textContent = '이 자리에 이미 타워가 있습니다!';
        return;
    }
    
    if (coins >= towerCost) {
        coins -= towerCost;
        towers.push({ row, col });
        cell.classList.add('tower');
        updateUI();
        messageDisplay.textContent = `타워가 배치되었습니다! 남은 코인: ${coins}`;
    } else {
        messageDisplay.textContent = '코인이 부족합니다!';
    }
}

// 적 스폰 함수
function spawnEnemies() {
    let enemyInterval = setInterval(() => {
        if (gameOver) {
            clearInterval(enemyInterval);
            return;
        }

        const enemy = {
            row: 0,
            col: Math.floor(Math.random() * 5),
            health: 100
        };
        enemies.push(enemy);
        moveEnemies();
    }, 3000); // 3초마다 적 스폰
}

// 적 이동 및 타워 공격 함수
function moveEnemies() {
    let towerInterval = setInterval(() => {
        if (gameOver) {
            clearInterval(towerInterval);
            return;
        }

        enemies.forEach((enemy, index) => {
            if (enemy.row < 4) {
                enemy.row++;
            } else {
                enemies.splice(index, 1);
                messageDisplay.textContent = '적이 지나갔습니다! 게임 오버!';
                gameOver = true;
            }

            checkTowerAttack(enemy);
        });
    }, 1000); // 1초마다 적 이동
}

// 타워 공격 함수
function checkTowerAttack(enemy) {
    towers.forEach(tower => {
        const towerCell = document.querySelector(`[data-row='${tower.row}'][data-col='${tower.col}']`);
        const enemyCell = document.querySelector(`[data-row='${enemy.row}'][data-col='${enemy.col}']`);

        if (tower.row === enemy.row && tower.col === enemy.col) {
            enemy.health -= 20; // 타워 공격
            if (enemy.health <= 0) {
                messageDisplay.textContent = '적을 처치했습니다!';
                coins += 50; // 적 처치 후 코인 보상
                enemies = enemies.filter(e => e !== enemy);
                updateUI();
            }
        }
    });
}

// 타워 업그레이드 함수
function upgradeTower() {
    if (coins >= upgradeCost) {
        coins -= upgradeCost;
        upgradeCost += 50; // 업그레이드 비용 증가
        level++;
        updateUI();
        messageDisplay.textContent = `타워가 업그레이드 되었습니다! 새로운 레벨: ${level}`;
    } else {
        messageDisplay.textContent = '코인이 부족합니다!';
    }
}
