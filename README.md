import pygame
import sys
import math

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 800, 600
FPS = 60
WHITE = (255, 255, 255)

# Set up the display
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Space Shooter")
# Load images
background_image = pygame.image.load("1920x1080.jpg")
background_image = pygame.transform.scale(background_image, (WIDTH, HEIGHT))

player_image = pygame.Surface((50, 50), pygame.SRCALPHA)
pygame.draw.polygon(player_image, WHITE, [(25, 0), (50, 50), (25, 40), (0, 50)])

# Set up initial player position, angle, and speed
player_pos = [WIDTH // 2, HEIGHT - 70]
player_angle = 0
player_speed = 0

# Set up bullet speed and list
bullet_speed = 7
bullet_list = []

# Clock to control the frame rate
clock = pygame.time.Clock()
frames = 0
frames_cooldown = 0

# Main game loop
while True:
    pygame.draw.rect(screen, WHITE, (0, 5, 750, 300))
    frames += 1/60
    frames_cooldown += 1/60

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    keys = pygame.key.get_pressed()

    # Player movement and rotation
    if keys[pygame.K_LEFT] or keys[pygame.K_a]:
        player_angle += 5
    if keys[pygame.K_RIGHT] or keys[pygame.K_d]:
        player_angle -= 5
    if keys[pygame.K_w]:
        player_speed = 5
    elif keys != pygame.K_w and player_speed >= 0.1:
        player_speed -= 0.1

    # Update player position based on angle
    player_pos[0] += player_speed * math.sin(math.radians(-player_angle))
    player_pos[1] -= player_speed * math.cos(math.radians(-player_angle))

    # Shoot bullets
    if keys[pygame.K_SPACE] and frames >= 0.1:
        if frames_cooldown < 2.5:
            frames = 0
            bullet_list.append([player_pos[0], player_pos[1], player_angle])

    # Update frames_cooldown
    # Reset frames_cooldown after a certain duration (adjust as needed)
    if frames_cooldown > 4.5:
        frames_cooldown = 0

    # Move and rotate bullets
    for bullet in bullet_list:
        bullet[0] += bullet_speed * math.sin(math.radians(-bullet[2]))
        bullet[1] -= bullet_speed * math.cos(math.radians(-bullet[2]))

    # Remove bullets that go off-screen
    bullet_list = [bullet for bullet in bullet_list if 0 < bullet[0] < WIDTH and 0 < bullet[1] < HEIGHT]

    # Draw everything
    screen.blit(background_image, (0, 0))

    rotated_player_image = pygame.transform.rotate(player_image, player_angle)
    player_rect = rotated_player_image.get_rect(center=(player_pos[0], player_pos[1]))
    screen.blit(rotated_player_image, player_rect.topleft)

    for bullet in bullet_list:
        pygame.draw.circle(screen, WHITE, (int(bullet[0]), int(bullet[1])), 5)

    pygame.display.flip()

    # Cap the frame rate
    clock.tick(FPS)
