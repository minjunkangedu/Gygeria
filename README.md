import pygame
import random

# 초기화
pygame.init()

# 화면 설정
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("스폰지밥 타워 디펜스")

# 색상 정의
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# 기본 폰트 설정
font = pygame.font.SysFont('arial', 20)

# 타워 기본 클래스
class Tower(pygame.sprite.Sprite):
    def __init__(self, x, y, damage, range, cooldown):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill(GREEN)
        self.rect = self.image.get_rect()
        self.rect.center = (x, y)
        self.damage = damage
        self.range = range
        self.cooldown = cooldown
        self.last_shot = pygame.time.get_ticks()

    def update(self, enemies):
        now = pygame.time.get_ticks()
        if now - self.last_shot > self.cooldown:
            for enemy in enemies:
                if self.rect.colliderect(enemy.rect):  # 적이 타워 범위 내에 있으면
                    enemy.hp -= self.damage
                    self.last_shot = now  # 타워가 공격 후 쿨타임 리셋

# 특수 능력을 가진 타워 (스폰지밥 예시)
class SpecialTower(Tower):
    def __init__(self, x, y, damage, range, cooldown):
        super().__init__(x, y, damage, range, cooldown)
        self.special_ability_ready = True
        self.special_ability_time = 5000  # 5초 후 특수 능력 리셋

    def special_ability(self, enemies):
        if self.special_ability_ready:
            for enemy in enemies:
                enemy.hp -= 20  # 모든 적에게 20의 피해를 입힘
            self.special_ability_ready = False
            pygame.time.set_timer(pygame.USEREVENT, self.special_ability_time)

    def reset_special_ability(self):
        self.special_ability_ready = True  # 특수 능력 리셋

# 적 기본 클래스
class Enemy(pygame.sprite.Sprite):
    def __init__(self, x, y, hp, speed):
        super().__init__()
        self.image = pygame.Surface((40, 40))
        self.image.fill(RED)
        self.rect = self.image.get_rect()
        self.rect.center = (x, y)
        self.hp = hp
        self.speed = speed

    def update(self):
        self.rect.x -= self.speed  # 적이 왼쪽으로 이동

        # 적이 화면 밖으로 나가면 게임에서 제거
        if self.rect.right < 0:
            self.kill()

# 회복하는 적 (특수 능력)
class HealingEnemy(Enemy):
    def __init__(self, x, y, hp, speed):
        super().__init__(x, y, hp, speed)
        self.heal_timer = 0

    def update(self):
        super().update()
        if self.heal_timer >= 300:  # 300 프레임마다 체력 회복
            self.hp += 10
            self.heal_timer = 0
        self.heal_timer += 1

# 기후 시스템
class Weather:
    def __init__(self):
        self.weather_type = random.choice(["Sunny", "Rainy", "Windy"])  # 날씨를 무작위로 선택
        self.weather_effect = self.apply_weather_effect()

    def apply_weather_effect(self):
        if self.weather_type == "Sunny":
            return "타워 공격력 +10%"
        elif self.weather_type == "Rainy":
            return "타워 공격 범위 -20%"
        elif self.weather_type == "Windy":
            return "타워 공격 속도 +15%"

    def get_weather_info(self):
        return f"날씨: {self.weather_type}, 효과: {self.weather_effect}"

# 게임 설정
clock = pygame.time.Clock()
all_sprites = pygame.sprite.Group()
enemies = pygame.sprite.Group()
towers = pygame.sprite.Group()

# 타워, 적 생성 예시
spongebob_tower = SpecialTower(400, 300, damage=10, range=100, cooldown=1000)
all_sprites.add(spongebob_tower)
towers.add(spongebob_tower)

enemy1 = HealingEnemy(800, 300, hp=50, speed=2)
all_sprites.add(enemy1)
enemies.add(enemy1)

# 날씨 객체 생성
current_weather = Weather()

# 메인 게임 루프
running = True
while running:
    screen.fill(WHITE)

    # 이벤트 처리
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.USEREVENT:  # 특수 능력 리셋
            spongebob_tower.reset_special_ability()

    # 게임 업데이트
    all_sprites.update()

    # 타워의 특수 능력 발동 (예: 'S'키를 누르면)
    keys = pygame.key.get_pressed()
    if keys[pygame.K_s]:
        spongebob_tower.special_ability(enemies)

    # 날씨 정보 출력
    weather_text = font.render(current_weather.get_weather_info(), True, BLACK)
    screen.blit(weather_text, (10, 10))

    # 적과 타워의 상태 출력
    for tower in towers:
        pygame.draw.rect(screen, GREEN, tower.rect)
    for enemy in enemies:
        pygame.draw.rect(screen, RED, enemy.rect)

    # 화면 업데이트
    pygame.display.flip()

    # FPS 설정
    clock.tick(60)

pygame.quit()
