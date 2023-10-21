# Lập Trình Game Rắn Săn Mồi Với Python

**Python** là một ngôn ngữ lập trình bậc cao với cú pháp dễ hiểu, dễ học, thân thiện với người mới bắt đầu. Vì vậy, hôm nay mình sẽ hướng dẫn các bạn lập trình **game** rắn săn mồi cực kỳ dễ hiểu với **Python**. 

Lần này mình có sử dụng thư viện **pygame**, một thư viện được thiết kế để hỗ trợ lập trình những **game** đơn giản.

## Setup môi trường

Để cài đặt **pygame**, các bạn truy cập vào **Command Promt** và nhập lệnh :

```
pip install pygame
```

Sau khi đã cài đặt xong, ta tiến hành khai báo những thư viện cần dùng:

```python
import pygame
import sys
import random
```

## Khai báo biến

Ta sẽ bắt đầu bằng cách khai báo một số biến cần dùng:

```python
SCREEN_WIDTH = 480
SCREEN_HEIGHT = 480

GRIDSIZE = 20
GRID_WIDTH = SCREEN_WIDTH / GRIDSIZE
GRID_HEIGHT = SCREEN_HEIGHT / GRIDSIZE

UP    = (0, -1)
DOWN  = (0, 1)
LEFT  = (-1, 0)
RIGHT = (1, 0)
```

## Tọa độ màn hình

`SCREEN_WIDTH `và `SCREEN_HEIGHT` là kích thước ô cửa sổ của game. `GRIDSIZE` là kích thước mỗi ô **caro**. `UP`, `DOWN`, `LEFT`, `RIGHT` dùng để lưu phương hướng của con rắn.

Công việc tiếp theo của ta là vẽ bàn **caro** lên cửa sổ. Việc đó có thể thực hiện qua hàm sau đây:

```python
def drawGrid(surface):
    for y in range(0, int(GRID_HEIGHT)):
        for x in range(0, int(GRID_WIDTH)):
            if (x + y) % 2 == 0:
                r = pygame.Rect((x*GRIDSIZE, y*GRIDSIZE), (GRIDSIZE, GRIDSIZE))
                pygame.draw.rect(surface, (93, 216, 228), r)
            else:
                rr = pygame.Rect((x*GRIDSIZE, y*GRIDSIZE), (GRIDSIZE, GRIDSIZE))
                pygame.draw.rect(surface, (84, 194, 205), rr)
```

Trong đoạn **code** trên, cứ vị trí ô vuông nào có tổng tọa độ `x` và `y` thì là chẵn thì ta sẽ tô một màu, những ô còn lại ta tô màu khác. Vậy là ta đã có một bàn **caro** để con rắn chạy trên đó.

## Giờ ta sẽ tạo ra con rắn của mình 

```python
class Snake(object):
    def __init__(self):
        self.length = 1
        self.positions = [((SCREEN_WIDTH / 2), (SCREEN_HEIGHT / 2))]
        self.direction = random.choice([UP, DOWN, LEFT, RIGHT])
        self.color = (17, 24, 47)

    def get_head_position(self):
        return self.positions[0]

    def turn(self, point):
        if self.length > 1 and (point[0] * -1, point[1] * -1) == self.direction:
            return
        else:
            self.direction = point

    def move(self):
        cur = self.get_head_position()
        x, y = self.direction
        new = (((cur[0] + (x*GRIDSIZE)) % SCREEN_WIDTH), (cur[1] + (y*GRIDSIZE)) % SCREEN_HEIGHT)
        if len(self.positions) > 2 and new in self.positions[2:]:
            self.reset()
        else:
            self.positions.insert(0, new)
            if len(self.positions) > self.length:
                self.positions.pop()            
            
    def reset(self):
        self.length = 1
        self.positions =  [((SCREEN_WIDTH / 2), (SCREEN_HEIGHT / 2))]
        self.direction = random.choice([UP, DOWN, LEFT, RIGHT])
    
    def draw(self, surface):
        for p in self.positions:
            r = pygame.Rect((p[0], p[1]), (GRIDSIZE, GRIDSIZE))
            pygame.draw.rect(surface, self.color, r)
            pygame.draw.rect(surface, (93, 216, 228), r, 1)
            
    def handle_keys(self):
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP:
                    self.turn(UP)
                elif event.key == pygame.K_DOWN:
                    self.turn(DOWN)
                elif event.key == pygame.K_LEFT:
                    self.turn(LEFT)
                elif event.key == pygame.K_RIGHT:
                    self.turn(RIGHT)
```

Trong `object` trên, rắn của ta là một mảng lưu vị trí những phần thân của nó. Ta có thể thấy hàm `turn` để hỗ trợ việc chuyển hướng của con rắn, hàm `draw` để vẽ những phần thân của rắn, `get_head_position` để lấy vị trí đầu rắn, `reset` để bắt đầu lại trò chơi và `handle_keys` để xử lý thao tác của người chơi.

Hàm `move` điều khiển cách con rắn của chúng ta di chuyển. Hàm này hoạt động bằng cách tính toán vị trí tiếp theo của đầu rắn, nếu nó không trùng với vị trí của phần thân nào thì sẽ được thêm vào rắn, đồng thời bỏ đi phần thân cuối, còn nếu trùng thì trò chơi sẽ bắt đầu lại.

## Tiếp đến ta sẽ tạo thức ăn cho rắn

```python
class Food(object):
    def __init__(self):
        self.position = (0, 0)
        self.color = (233, 163, 49)
        self.randomize_position()

    def randomize_position(self):
        self.position = (random.randint(0, GRID_WIDTH-1) * GRIDSIZE, random.randint(0, GRID_HEIGHT-1) * GRIDSIZE)

    def draw(self, surface):
        r = pygame.Rect((self.position[0], self.position[1]), (GRIDSIZE, GRIDSIZE))
        pygame.draw.rect(surface, self.color, r)
        pygame.draw.rect(surface, (93, 216, 228), r, 1)
```

Với **object** `Food`, ta có hàm `randomize_position` để những ô đồ ăn xuất hiện ngẫu nhiên trên bàn **caro**, hàm `draw` để vẽ ô thức ăn đó.

Vậy là ta đã có đủ thành phần để lắp ghép thành **game** rắn săn mồi hoàn chỉnh. Thứ tiếp theo ta làm sau đây là vòng lặp **game**, nơi những quy định của **game** được cấu hình.

```python
def main():
    pygame.init()
    
    clock = pygame.time.Clock()
    screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT), 0, 32)
    
    surface = pygame.Surface(screen.get_size())
    surface = surface.convert()
    drawGrid(surface)

    snake = Snake()
    food = Food()
    
    myfont = pygame.font.SysFont("monospace", 16)
    
    score = 0 
    while True:
        clock.tick(10)
        snake.handle_keys()
        drawGrid(surface)
        snake.move()
        if snake.get_head_position() == food.position:
            snake.length += 1
            score += 1
            food.randomize_position()
        snake.draw(surface)
        food.draw(surface) 
        screen.blit(surface, (0,0))
        text = myfont.render("Score {0}".format(score), 1, (0,0,0))
        screen.blit(text, (5, 10))
        pygame.display.update()
```

## Theo dõi thời gian

Việc làm đầu tiên của ta là bao gồm những **module** của **pygame**, tạo một **Clock** để theo dõi thời gian và tạo một cửa sổ game.

```python
pygame.init()
    
    clock = pygame.time.Clock()
    screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT), 0, 32)
    
    surface = pygame.Surface(screen.get_size())
    surface = surface.convert()
```

Vẽ caro

```python
drawGrid(surface)
```

Tạo rắn và thức ăn

```python
snake = Snake()
food = Food()
```

Đây sẽ là vòng lặp chính của **game**

```python
while True:
        clock.tick(10)
        snake.handle_keys()
        drawGrid(surface)
        snake.move()
        if snake.get_head_position() == food.position:
            snake.length += 1
            score += 1
            food.randomize_position()
        snake.draw(surface)
        food.draw(surface) 
        screen.blit(surface, (0,0))
        text = myfont.render("Score {0}".format(score), 1, (0,0,0))
        screen.blit(text, (5, 10))
        pygame.display.update()
```

Ta sẽ quy định **game** chạy với tốc độ **10 khung hình 1 giây**.

```python
clock.tick(10)
```

Nếu vị trí đầu rắn trùng với vị trí của thức ăn, ta tăng độ dài rắn lên một, đồng thời tạo ô thức ăn mới ngẫu nhiên

```python
if snake.get_head_position() == food.position:
            snake.length += 1
            score += 1
            food.randomize_position()
```

Vẽ lại rắn và đồ ăn

```python
snake.draw(surface)
food.draw(surface)
```

Tạo một ô nhỏ để hiển thị điểm số

```python
screen.blit(surface, (0,0))
text = myfont.render("Score {0}".format(score), 1, (0,0,0))
screen.blit(text, (5, 10))
pygame.display.update()
```

Vậy là xong! Việc duy nhất ta cần làm là gọi lại hàm `main` của ta rồi chạy **game** thôi!

```python
main()
```

## Kết

Sau đây là toàn bộ **code** của trò chơi: [Game snake với python](/Scripts/Python/Snake.py)

Có chỗ nào cần giải đáp hoặc bổ sung, các bạn để lại **comment** dưới bài viết nha. Chúc các bạn vận dụng được kiến thức và thực hiện thành công **game** này

- https://codelearn.io/sharing/game-ran-san-moi-voi-python-3
