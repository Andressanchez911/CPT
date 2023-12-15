# CPT
My compsci CPT
import pygame
import sys
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
# Replace with your image file
background_image = pygame.transform.scale(background_image, (WIDTH, HEIGHT))

player_image = pygame.Surface((50, 50))
player_image.fill(WHITE)

# Set up initial player position
player_pos = [WIDTH // 2 - 25, HEIGHT - 70]

# Set up player movement speed
player_speed = 5

# Set up bullet speed and list
bullet_speed = 7
bullet_list = []

# Clock to control the frame rate
clock = pygame.time.Clock()
frames = 0
# Main game loop
while True:
    frames += 1/60
    print(frames)
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    keys = pygame.key.get_pressed()

    # Player movement
    if (keys[pygame.K_LEFT] or keys[pygame.K_a]) and player_pos[0] > 0:
        player_pos[0] -= player_speed
    if (keys[pygame.K_RIGHT] or keys[pygame.K_d]) and player_pos[0] < WIDTH - 50:
        player_pos[0] += player_speed
    if (keys[pygame.K_UP] or keys[pygame.K_w]) and player_pos[1] > 0:
        player_pos[1] -= player_speed
    if (keys[pygame.K_DOWN] or keys[pygame.K_s]) and player_pos[1] < HEIGHT - 50:
        player_pos[1] += player_speed

    # Shoot bullets
    if keys[pygame.K_SPACE] and frames >= 0.5:
       frames = 0
       bullet_list.append([player_pos[0] + 25, player_pos[1]])
          

    # Move bullets
    for bullet in bullet_list:
        bullet[1] -= bullet_speed

    # Remove bullets that go off-screen
    bullet_list = [bullet for bullet in bullet_list if bullet[1] > 0]

    # Draw everything
    screen.blit(background_image, (0, 0))
    screen.blit(player_image, tuple(player_pos))

    for bullet in bullet_list:
        pygame.draw.rect(screen, WHITE, pygame.Rect(bullet[0], bullet[1], 5, 10))

    pygame.display.flip()

    # Cap the frame rate
    clock.tick(FPS)
