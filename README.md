- 👋 Hi, I’m @Zekeyz
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
Zekeyz/Zekeyz is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import pygame
import random


# Initialize pygame
pygame.init()

# Colors
WHITE = (255, 255, 255)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)

# Screen dimensions
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600

# Create the screen
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption('Simple Shooter Game')

# Player
player_img = pygame.Surface((50, 50))
player_img.fill(GREEN)
player_rect = player_img.get_rect()
player_rect.center = (SCREEN_WIDTH // 2, SCREEN_HEIGHT - 50)

# Bullets
bullets = []

# Enemies
enemies = []
for _ in range(5):
    enemy_img = pygame.Surface((30, 30))
    enemy_img.fill(RED)
    enemy_rect = enemy_img.get_rect()
    enemy_rect.x = random.randint(0, SCREEN_WIDTH - enemy_rect.width)
    enemy_rect.y = random.randint(0, SCREEN_HEIGHT // 2)
    enemies.append(enemy_rect)

# Game loop
clock = pygame.time.Clock()
running = True

while running:
    screen.fill(WHITE)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                bullet_img = pygame.Surface((10, 10))
                bullet_img.fill(BLUE)
                bullet_rect = bullet_img.get_rect()
                bullet_rect.center = player_rect.center
                bullets.append(bullet_rect)

    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        player_rect.x -= 5
    if keys[pygame.K_RIGHT]:
        player_rect.x += 5

    # Move bullets
    for bullet in bullets:
        bullet.y -= 10
        if bullet.y < 0:
            bullets.remove(bullet)

    # Check bullet-enemy collision
    for bullet in bullets:
        for enemy in enemies:
            if bullet.colliderect(enemy):
                bullets.remove(bullet)
                enemies.remove(enemy)

    # Draw player, bullets, and enemies
    screen.blit(player_img, player_rect)
    for bullet in bullets:
        screen.blit(bullet_img, bullet)
    for enemy in enemies:
        screen.blit(enemy_img, enemy)

    pygame.display.flip()
    clock.tick(30)

pygame.quit()
