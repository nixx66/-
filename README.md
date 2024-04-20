# -
在这个游戏中，玩家控制一个“蛇”，通过键盘控制蛇的移动方向，吃到食物时身体会变长，如果蛇头碰到身体或墙壁，则游戏结束。
import pygame
import random

# 游戏窗口大小
WIDTH, HEIGHT = 800, 600
# 蛇和食物的大小
BLOCK_SIZE = 20
# 蛇的移动速度
SPEED = 10

# 定义颜色
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# 初始化pygame
pygame.init()
# 创建游戏窗口
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("贪食蛇")

# 蛇类
class Snake:
    def __init__(self):
        self.body = [(WIDTH / 2, HEIGHT / 2)]
        self.direction = random.choice(["UP", "DOWN", "LEFT", "RIGHT"])

    def move(self):
        x, y = self.body[0]
        if self.direction == "UP":
            y -= BLOCK_SIZE
        elif self.direction == "DOWN":
            y += BLOCK_SIZE
        elif self.direction == "LEFT":
            x -= BLOCK_SIZE
        elif self.direction == "RIGHT":
            x += BLOCK_SIZE
        self.body.insert(0, (x, y))
        self.body.pop()

    def grow(self):
        x, y = self.body[-1]
        if self.direction == "UP":
            y += BLOCK_SIZE
        elif self.direction == "DOWN":
            y -= BLOCK_SIZE
        elif self.direction == "LEFT":
            x += BLOCK_SIZE
        elif self.direction == "RIGHT":
            x -= BLOCK_SIZE
        self.body.append((x, y))

    def draw(self):
        for x, y in self.body:
            pygame.draw.rect(screen, GREEN, (x, y, BLOCK_SIZE, BLOCK_SIZE))

# 食物类
class Food:
    def __init__(self):
        self.x = random.randrange(0, WIDTH-BLOCK_SIZE, BLOCK_SIZE)
        self.y = random.randrange(0, HEIGHT-BLOCK_SIZE, BLOCK_SIZE)

    def draw(self):
        pygame.draw.rect(screen, RED, (self.x, self.y, BLOCK_SIZE, BLOCK_SIZE))

# 主函数
def main():
    clock = pygame.time.Clock()
    snake = Snake()
    food = Food()

    running = True
    while running:
        screen.fill(WHITE)

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP and snake.direction != "DOWN":
                    snake.direction = "UP"
                elif event.key == pygame.K_DOWN and snake.direction != "UP":
                    snake.direction = "DOWN"
                elif event.key == pygame.K_LEFT and snake.direction != "RIGHT":
                    snake.direction = "LEFT"
                elif event.key == pygame.K_RIGHT and snake.direction != "LEFT":
                    snake.direction = "RIGHT"

        snake.move()
        snake.draw()
        food.draw()

        if snake.body[0] == (food.x, food.y):
            snake.grow()
            food = Food()

        for block in snake.body[1:]:
            if snake.body[0] == block:
                running = False

        if snake.body[0][0] < 0 or snake.body[0][0] >= WIDTH or snake.body[0][1] < 0 or snake.body[0][1] >= HEIGHT:
            running = False

        pygame.display.flip()
        clock.tick(SPEED)

    pygame.quit()

if __name__ == "__main__":
    main()
