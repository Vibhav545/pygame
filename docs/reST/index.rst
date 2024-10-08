import pygame
import random

# Initialize Pygame
pygame.init()

# Screen dimensions
SCREEN_WIDTH = 400
SCREEN_HEIGHT = 600

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)

# Bird settings
BIRD_WIDTH = 40
BIRD_HEIGHT = 40
BIRD_X = 50
BIRD_Y = 300
BIRD_VELOCITY = 0
GRAVITY = 0.5
JUMP_STRENGTH = -10

# Pipe settings
PIPE_WIDTH = 70
PIPE_HEIGHT = 400
PIPE_GAP = 150
PIPE_VELOCITY = -5

# Create screen
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Flappy Bird")

# Load bird image
bird_image = pygame.Surface((BIRD_WIDTH, BIRD_HEIGHT))
bird_image.fill(GREEN)

# Function to draw pipes
def draw_pipes(pipes):
    for pipe in pipes:
        pygame.draw.rect(screen, BLACK, pipe)

# Function to check for collisions
def check_collision(bird_rect, pipes):
    for pipe in pipes:
        if bird_rect.colliderect(pipe):
            return True
    return False

# Main game loop
def main():
    global BIRD_Y, BIRD_VELOCITY

    clock = pygame.time.Clock()
    pipes = []
    score = 0
    running = True

    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    BIRD_VELOCITY = JUMP_STRENGTH

        # Bird movement
        BIRD_VELOCITY += GRAVITY
        BIRD_Y += BIRD_VELOCITY
        bird_rect = pygame.Rect(BIRD_X, BIRD_Y, BIRD_WIDTH, BIRD_HEIGHT)

        # Pipe movement
        if len(pipes) == 0 or pipes[-1].x < SCREEN_WIDTH - 200:
            pipe_height = random.randint(100, 400)
            pipes.append(pygame.Rect(SCREEN_WIDTH, pipe_height - PIPE_HEIGHT, PIPE_WIDTH, PIPE_HEIGHT))
            pipes.append(pygame.Rect(SCREEN_WIDTH, pipe_height + PIPE_GAP, PIPE_WIDTH, PIPE_HEIGHT))

        for pipe in pipes:
            pipe.x += PIPE_VELOCITY

        pipes = [pipe for pipe in pipes if pipe.x + PIPE_WIDTH > 0]

        # Check for collisions
        if BIRD_Y > SCREEN_HEIGHT or BIRD_Y < 0 or check_collision(bird_rect, pipes):
            running = False

        # Draw everything
        screen.fill(WHITE)
        screen.blit(bird_image, (BIRD_X, BIRD_Y))
        draw_pipes(pipes)
        pygame.display.flip()

        clock.tick(30)

    pygame.quit()

if __name__ == "__main__":
    main()
