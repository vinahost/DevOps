# Lập Trình Game Đua Xe Với Pygame

Chào mọi người! Trong bài hướng dẫn này, mình và các bạn sẽ cùng lập trình **game** đua xe đơn giản với **pygame**. Bài hướng dẫn cũng khá đơn giản, phù hợp với các bạn mới học lập trình **game**. Chúng ta cùng bắt đầu thôi!

## Kiến thức cần thiết

Trước tiên, các bạn cần có những kiến thức cơ bản về **python** và **pygame**. Nếu các bạn chưa biết về **pygame** thì có thể xem qua 2 bài hướng dẫn **pygame** cơ bản của mình ([phần 1](Python-Pygame01.md), [phần 2](Python-Pygame02.md))

Nếu các bạn chưa xem bài hướng dẫn lập trình game **Flappy Bird**  thì có thể xem [tại đây](Flappy-Bird-Pygame.md). Trong bài hướng dẫn này có một số đoạn **code** và cách thức hoạt động tương tự với bài hướng dẫn lập trình game **Flappy Bird**. Vì thế, các bạn có thể tham khảo bài hướng dẫn đó để hiểu rõ hơn.

Các bạn hãy tải **source code** đầy đủ [tại đây](/Scripts/Python/Racing.zip). Trong lúc viết **code**, các bạn có thể tham khảo phần **code** đầy đủ nhe.

Bây giờ chúng ta cùng code thôi.

## Tạo thư mục và file

Các bạn hãy tạo thư mục có cấu trúc như sau:

![Racing Pygame](/Image/Racing-Pygame01.png)

**racing.py** là file **code** của chúng ta.

Thư mục **img** chứa các hình ảnh dùng cho **game**. Các hình đó mình lấy trên **OpenGameArt.org** và có chỉnh lại cho phù hợp với **game**.

Các bạn có thể tải **source code đầy đủ** về để lấy các hình ảnh nhe.

Nếu đã chuẩn bị xong thì mở file **racing.py** lên rồi **code** thôi.

## Tạo cửa sổ game và những màn chơi

Các bạn gõ **code** này vào nhé, mình sẽ giải thích sau.

```python
import pygame, sys, random
from pygame.locals import *

WINDOWWIDTH = 400
WINDOWHEIGHT = 600

pygame.init()

FPS = 60
fpsClock = pygame.time.Clock()

DISPLAYSURF = pygame.display.set_mode((WINDOWWIDTH, WINDOWHEIGHT))
pygame.display.set_caption('RACING')

def gameStart():
    pass

def gamePlay():
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

        pygame.display.update()
        fpsClock.tick(FPS)

def gameOver():
    pass

def main():
    gameStart()
    while True:
        gamePlay()
        gameOver()

if __name__ == '__main__':
    main()
```

Những dòng **code** này mình đã giải thích rất kĩ ở những bài hướng dẫn **pygame** cơ bản rồi. Các bạn cần chú ý 3 hàm `gameStart()`, `gamePlay()`, `gameOver()`.

- `gameStart()` là phần chuẩn bị khi vừa mở game lên.
- `gamePlay()` là phần chơi chính.
- `gameOver()` là phần xuất hiện khi thua 1 màn chơi.

Từ đây, các bạn hãy chú ý vào phần `gamePlay` nhé. Chúng ta sẽ **code** phần này trước.

## Tạo nền và cuộn nền

Khai báo một số hằng số như sau:

```python
BGSPEED = 1.5
BGIMG = pygame.image.load('img/background.png')
```

Tạo một **class** như sau:

```python
class Background():
    def __init__(self):
        self.x = 0
        self.y = 0
        self.speed = BGSPEED
        self.img = BGIMG
        self.width = self.img.get_width()
        self.height = self.img.get_height()
    def draw(self):
        DISPLAYSURF.blit(self.img, (int(self.x), int(self.y)))
        DISPLAYSURF.blit(self.img, (int(self.x), int(self.y-self.height)))
    def update(self):
        self.y += self.speed
        if self.y > self.height:
            self.y -= self.height
```

Trong hàm `main`, tạo thêm biến `bg` và truyền vào hàm `gamePlay`:

```python
def main():
    bg = Background()
    gameStart()
    while True:
        gamePlay(bg)
        gameOver()
```

Trong hàm `gamePlay`: thêm tham số `bg`, đặt lại `bg` bằng cách dùng `bg.__init__()` và gọi hàm `bg.draw()`, `bg.update()` trong vòng lặp **game**.

```python
def gamePlay(bg):
    bg.__init__()
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
        bg.draw()
        bg.update()
        pygame.display.update()
        fpsClock.tick(FPS)
```

Phần cuộn nền này mình đã có một bài hướng dẫn rất kỹ rồi. Các bạn có thể xem bài hướng dẫn đó [tại đây](Scrolling-Background.md). Hiểu đơn giản là vẽ 2 hình nối tiếp nhau, thay đổi vị trí qua mỗi vòng lặp **game**, khi hình thứ nhất ra khỏi màn hình thì kéo cả 2 hình về sau sao cho hình thứ nhất ứng với hình thứ 2 lúc đầu.

Các bạn hãy chạy thử và đến phần tiếp theo nhé.

## Tạo chiếc xe

Chúng ta hãy vẽ ra 1 chiếc xe và điều khiển nó bằng các phím mũi tên.

Khai báo một số hằng số như sau:

```python
X_MARGIN = 80

CARWIDTH = 40
CARHEIGHT = 60
CARSPEED = 3
CARIMG = pygame.image.load('img/car.png')
```

Trong đó:

- `X_MARGIN` là lề hai bên trái và phải (xe không được vượt qua đó).
- `CARWIDTH` và `CARHEIGHT` là kích thước của xe.
- `CARSPEED` là tốc độ di chuyển (tiến, lùi, trái, phải) của xe.
- `CARIMG` là ảnh chiếc xe.

Tạo một `class` như sau:

```python
class Car():
    def __init__(self):
        self.width = CARWIDTH
        self.height = CARHEIGHT
        self.x = (WINDOWWIDTH-self.width)/2
        self.y = (WINDOWHEIGHT-self.height)/2
        self.speed = CARSPEED
        self.surface = pygame.Surface((self.width, self.height))
        self.surface.fill((255, 255, 255))
    def draw(self):
        DISPLAYSURF.blit(CARIMG, (int(self.x), int(self.y)))
    def update(self, moveLeft, moveRight, moveUp, moveDown):
        if moveLeft == True:
            self.x -= self.speed
        if moveRight == True:
            self.x += self.speed
        if moveUp == True:
            self.y -= self.speed
        if moveDown == True:
            self.y += self.speed
        
        if self.x < X_MARGIN:
            self.x = X_MARGIN
        if self.x + self.width > WINDOWWIDTH - X_MARGIN:
            self.x = WINDOWWIDTH - X_MARGIN - self.width
        if self.y < 0:
            self.y = 0
        if self.y + self.height > WINDOWHEIGHT :
            self.y = WINDOWHEIGHT - self.height
```

Các bạn hãy chú ý trong hàm `update`:

```python
if moveLeft == True:
    self.x -= self.speed
if moveRight == True:
    self.x += self.speed
if moveUp == True:
    self.y -= self.speed
if moveDown == True:
    self.y += self.speed
```

Những dòng **code** trên để điều chỉnh ví trí chiếc xe theo các hướng di chuyển.

```python
if self.x < X_MARGIN:
    self.x = X_MARGIN
if self.x + self.width > WINDOWWIDTH - X_MARGIN:
    self.x = WINDOWWIDTH - X_MARGIN - self.width
if self.y < 0:
    self.y = 0
if self.y + self.height > WINDOWHEIGHT :
    self.y = WINDOWHEIGHT - self.height
```

Những dòng **code** trên để giới hạn phạm vi di chuyển của xe (xe không đi quá lề trái, phải và không đi ra khỏi màn hình).

Trong hàm `main`, tạo thêm biến `car` và truyền vào hàm `gamePlay`.

```python
def main():
    bg = Background()
    car = Car()
    gameStart()
    while True:
        gamePlay(bg, car)
        gameOver()
```

Trong hàm `gamePlay`, thêm tham số `car` và thực hiện việc bắt sự kiện (phần bắt sự kiện này mình đã hướng dẫn ở bài hướng dẫn **python** cơ bản rồi). Những thứ khác làm tương tự với phần nền (gọi các hàm `__init__`, `draw`, `update`).

```python
def gamePlay(bg, car):
    bg.__init__()
    car.__init__()
    moveLeft = False
    moveRight = False
    moveUp = False
    moveDown = False
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == KEYDOWN:
                if event.key == K_LEFT:
                    moveLeft = True
                if event.key == K_RIGHT:
                    moveRight = True
                if event.key == K_UP:
                    moveUp = True
                if event.key == K_DOWN:
                    moveDown = True
            if event.type == KEYUP:
                if event.key == K_LEFT:
                    moveLeft = False
                if event.key == K_RIGHT:
                    moveRight = False
                if event.key == K_UP:
                    moveUp = False
                if event.key == K_DOWN:
                    moveDown = False
        bg.draw()
        bg.update()
        car.draw()
        car.update(moveLeft, moveRight, moveUp, moveDown)
        pygame.display.update()
        fpsClock.tick(FPS)
```

Vậy là chúng ta đã xong phần chiếc xe rồi. Các bạn hãy chạy thử, xem lại **code** và chuẩn bị sang phần tiếp theo nhé!

## Tạo chướng ngại vật

Các chướng ngại vật là những chiếc xe chuyển động ngược chiều. Các hãy gõ **code** trước nhé, mình sẽ giải thích sau.

Khai báo một số hằng số:

```python
LANEWIDTH = 60

DISTANCE = 200
OBSTACLESSPEED = 2
CHANGESPEED = 0.001
OBSTACLESIMG = pygame.image.load('img/obstacles.png')
```

Tạo một **class** như sau:

```python
class Obstacles():
    def __init__(self):
        self.width = CARWIDTH
        self.height = CARHEIGHT
        self.distance = DISTANCE
        self.speed = OBSTACLESSPEED
        self.changeSpeed = CHANGESPEED
        self.ls = []
        for i in range(5):
            y = -CARHEIGHT-i*self.distance
            lane = random.randint(0, 3)
            self.ls.append([lane, y])
    def draw(self):
        for i in range(5):
            x = int(X_MARGIN + self.ls[i][0]*LANEWIDTH + (LANEWIDTH-self.width)/2)
            y = int(self.ls[i][1])
            DISPLAYSURF.blit(OBSTACLESIMG, (x, y))
    def update(self):
        for i in range(5):
            self.ls[i][1] += self.speed
        self.speed += self.changeSpeed
        if self.ls[0][1] > WINDOWHEIGHT:
            self.ls.pop(0)
            y = self.ls[3][1] - self.distance
            lane = random.randint(0, 3)
            self.ls.append([lane, y])
```

Tạo thêm biến `obstacles` trong hàm `main` và truyền vào hàm `gamePlay`.

```python
def main():
    bg = Background()
    car = Car()
    obstacles = Obstacles()
    gameStart()
    while True:
        gamePlay(bg, car, obstacles)
        gameOver()
```

Thêm tham số vào hàm `gamePlay` và làm tương tự như những phần trên.

```python
def gamePlay(bg, car, obstacles):
    bg.__init__()
    car.__init__()
    obstacles.__init__()
    moveLeft = False
    moveRight = False
    moveUp = False
    moveDown = False
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == KEYDOWN:
                if event.key == K_LEFT:
                    moveLeft = True
                if event.key == K_RIGHT:
                    moveRight = True
                if event.key == K_UP:
                    moveUp = True
                if event.key == K_DOWN:
                    moveDown = True
            if event.type == KEYUP:
                if event.key == K_LEFT:
                    moveLeft = False
                if event.key == K_RIGHT:
                    moveRight = False
                if event.key == K_UP:
                    moveUp = False
                if event.key == K_DOWN:
                    moveDown = False
        bg.draw()
        bg.update()
        car.draw()
        car.update(moveLeft, moveRight, moveUp, moveDown)
        obstacles.draw()
        obstacles.update()
        pygame.display.update()
        fpsClock.tick(FPS)
```

Bây giờ thì chúng ta tìm hiểu những dòng **code** thôi.

Các bạn chú ý một số hằng số được khai báo, những chiếc xe làm chướng ngại vật có kích thước bằng với chiếc xe chúng ta điều khiển nên không khai báo thêm kích thức của xe.

- `LANEWIDTH` là độ rộng của 1 làn xe (đường có 4 làn).
- `DISTANCE` là khoảng cách giữa các xe theo chiều dọc.
- `OBSTACLESSPEED` là tốc độ ban đầu của những chiếc xe.
- `CHANGESPEED` dùng để tăng tốc độ của những chiếc xe theo thời gian.
- `OBSTACLESIMG` là ảnh chiếc xe.

Trong hàm `__init__` của **class** `Obstacles`, các bạn hãy chú ý biến `self.ls`. Biến này là một **list** các xe, mỗi xe gồm 2 thông số có dạng [**lane, y**], `lane` là làn của xe đó (có 4 làn được đánh số từ 0 đến 3), y là hoành độ của xe đó.

Dòng `for` dưới đây dùng để tạo 5 chiếc xe đầu tiên:

```python
for i in range(5):
    y = -CARHEIGHT-i*self.distance
    lane = random.randint(0, 3)
    self.ls.append([lane, y])
```

Các xe được tạo ra cách nhau theo chiều dọc một khoảng bằng `self.distance`. Mỗi xe sẽ nằm ngẫu nhiên ở 1 trong 4 làn.

Các bạn có thể xem hình dưới đây để hiểu rõ hơn.

![Racing Pygame](/Image/Racing-Pygame02.png)

Trong hàm `draw`, vòng lặp `for` dùng để vẽ từng xe. Trong vòng `for`, hai biến `x` và `y` dùng để xác định **hoành độ** và **tung độ** của xe. Hàm này cũng không có gì phức tạp nên mình không giải thích nhiều.

Các bạn hãy chú ý vào hàm upd`ate nhe.

Vòng `for` dưới đây dùng để thay đổi vị trí xe dựa vào tốc độ:

```python
for i in range(5):
    self.ls[i][1] += self.speed
```

Dòng dưới đây dùng để tăng tốc độ những chiếc xe:

```python
self.speed += self.changeSpeed
```

Đoạn `if` dưới đây để kiểm tra xem chiếc xe ở đầu có ra khỏi màn hình chưa. Nếu xe đi ra khỏi màn hình thì **xoá chiếc xe đó** và **thêm một chiếc xe** khác nối tiếp theo.

```python
if self.ls[0][1] > WINDOWHEIGHT:
    self.ls.pop(0)
    y = self.ls[3][1] - self.distance
    lane = random.randint(0, 3)
    self.ls.append([lane, y])
```

Các bạn có thể xem hình dưới đây để hiểu rõ hơn.

![Racing Pygame](/Image/Racing-Pygame03.png)

Vậy là chúng ta đã xong phần tạo những chướng ngại vật rồi. Các bạn xem lại **code** nhé, nghỉ ngơi một chút rồi sang phần tiếp theo nào.

## Tính điểm

Để cho đơn giản thì chúng ta sẽ cho điểm tăng đều theo thời gian.

Các bạn hãy tạo **class** như sau:

```python
class Score():
    def __init__(self):
        self.score = 0
    def draw(self):
        font = pygame.font.SysFont('consolas', 30)
        scoreSuface = font.render('Score: '+str(int(self.score)), True, (0, 0, 0))
        DISPLAYSURF.blit(scoreSuface, (10, 10))
    def update(self):
        self.score += 0.02
```

Trong hàm `main`, tạo thêm biến `score` và truyền vào `gamePlay`:

```python
def main():
    bg = Background()
    car = Car()
    obstacles = Obstacles()
    score = Score()
    gameStart()
    while True:
        gamePlay(bg, car, obstacles, score)
        gameOver()
```

Thêm tham số vào hàm `gamePlay` và gọi các hàm `__init__`, `draw`, `update` như những phần trước:

```python
def gamePlay(bg, car, obstacles, score):
    bg.__init__()
    car.__init__()
    obstacles.__init__()
    score.__init__()
    moveLeft = False
    moveRight = False
    moveUp = False
    moveDown = False
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == KEYDOWN:
                if event.key == K_LEFT:
                    moveLeft = True
                if event.key == K_RIGHT:
                    moveRight = True
                if event.key == K_UP:
                    moveUp = True
                if event.key == K_DOWN:
                    moveDown = True
            if event.type == KEYUP:
                if event.key == K_LEFT:
                    moveLeft = False
                if event.key == K_RIGHT:
                    moveRight = False
                if event.key == K_UP:
                    moveUp = False
                if event.key == K_DOWN:
                    moveDown = False
        bg.draw()
        bg.update()
        car.draw()
        car.update(moveLeft, moveRight, moveUp, moveDown)
        obstacles.draw()
        obstacles.update()
        score.draw()
        score.update()
        pygame.display.update()
        fpsClock.tick(FPS)
```

Phần này không quá phức tạp nên mình không giải thích kỹ. Các bạn có thể tự xem **code** nhe.

## Xử lý va chạm

Để cho đơn giản thì hãy xem chiếc xe của chúng ta và những chiếc xe khác là những hình chữ nhật.

Các bạn viết hàm để kiểm tra sự va chạm giữa hai hình chữ nhật như sau:

```python
def rectCollision(rect1, rect2):
    if rect1[0] <= rect2[0]+rect2[2] and rect2[0] <= rect1[0]+rect1[2] and rect1[1] <= rect2[1]+rect2[3] and rect2[1] <= rect1[1]+rect1[3]:
        return True
    return False
```

Phần này mình đã giải thích rất kĩ ở bài hướng dẫn về xử lý va chạm rồi. Các bạn có thể xem bài hướng dẫn đó [tại đây](Collision-Pygame.md).

Nếu xe của chúng ta va chạm với 1 xe khác thì **gameover**. Vì thế, các bạn hãy viết hàm kiểm tra `gameover` như sau:

```python
def isGameover(car, obstacles):
    carRect = [car.x, car.y, car.width, car.height]
    for i in range(5):
        x = int(X_MARGIN + obstacles.ls[i][0]*LANEWIDTH + (LANEWIDTH-obstacles.width)/2)
        y = int(obstacles.ls[i][1])
        obstaclesRect = [x, y, obstacles.width, obstacles.height]
        if rectCollision(carRect, obstaclesRect) == True:
            return True
    return False
```

Trong hàm `gamePlay`, chúng ta dùng hàm `isGameover` để kiểm tra `gameover` và kết thúc hàm. Khi hàm `gamePlay` kết thúc thì sẽ chạy đến hàm `gameOver`.

```python
def gamePlay(bg, car, obstacles, score):
    car.__init__()
    obstacles.__init__()
    bg.__init__()
    score.__init__()
    moveLeft = False
    moveRight = False
    moveUp = False
    moveDown = False
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == KEYDOWN:
                if event.key == K_LEFT:
                    moveLeft = True
                if event.key == K_RIGHT:
                    moveRight = True
                if event.key == K_UP:
                    moveUp = True
                if event.key == K_DOWN:
                    moveDown = True
            if event.type == KEYUP:
                if event.key == K_LEFT:
                    moveLeft = False
                if event.key == K_RIGHT:
                    moveRight = False
                if event.key == K_UP:
                    moveUp = False
                if event.key == K_DOWN:
                    moveDown = False
        if isGameover(car, obstacles):
            return
        bg.draw()
        bg.update()
        car.draw()
        car.update(moveLeft, moveRight, moveUp, moveDown)
        obstacles.draw()
        obstacles.update()
        score.draw()
        score.update()
        pygame.display.update()
        fpsClock.tick(FPS)
```

Vậy là chúng ta đã hoàn thành hàm `gamePlay`. Tiếp theo, chúng ta sẽ viết hàm `gameOver`.

## Viết hàm gameOver

Hàm này không có gì phức tạp, chỉ cần dùng những hàm để vẽ và thêm việc bắt sự kiện nhấn phím **SPACE** để kết thúc hàm. Hàm `gameOver` được viết dưới đây:

```python
def gameOver(bg, car, obstacles, score):
    font = pygame.font.SysFont('consolas', 60)
    headingSuface = font.render('GAMEOVER', True, (255, 0, 0))
    headingSize = headingSuface.get_size()

    font = pygame.font.SysFont('consolas', 20)
    commentSuface = font.render('Press "space" to replay', True, (0, 0, 0))
    commentSize = commentSuface.get_size()
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYUP:
                if event.key == K_SPACE:
                    return
        bg.draw()
        car.draw()
        obstacles.draw()
        score.draw()
        DISPLAYSURF.blit(headingSuface, (int((WINDOWWIDTH - headingSize[0])/2), 100))
        DISPLAYSURF.blit(commentSuface, (int((WINDOWWIDTH - commentSize[0])/2), 400))
        pygame.display.update()
        fpsClock.tick(FPS)
```

Trong hàm `main`, các bạn nhớ truyền thêm các biến vào hàm `gameOver`.

```python
def main():
    bg = Background()
    car = Car()
    obstacles = Obstacles()
    score = Score()
    gameStart()
    while True:
        gamePlay(bg, car, obstacles, score)
        gameOver(bg, car, obstacles, score)
```

Vậy là chúng ta đã xong phần `gameOver`, chúng ta cùng sang phần cuối cùng nào!

## Viết hàm gameStart

Hàm `gameStart` cũng tương tự như hàm `gameOver`.

```python
def gameStart(bg):
    bg.__init__()
    font = pygame.font.SysFont('consolas', 60)
    headingSuface = font.render('RACING', True, (255, 0, 0))
    headingSize = headingSuface.get_size()

    font = pygame.font.SysFont('consolas', 20)
    commentSuface = font.render('Press "space" to play', True, (0, 0, 0))
    commentSize = commentSuface.get_size()
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYUP:
                if event.key == K_SPACE:
                    return
        bg.draw()
        DISPLAYSURF.blit(headingSuface, (int((WINDOWWIDTH - headingSize[0])/2), 100))
        DISPLAYSURF.blit(commentSuface, (int((WINDOWWIDTH - commentSize[0])/2), 400))
        pygame.display.update()
        fpsClock.tick(FPS)
```

Trong hàm `main`, các bạn nhớ truyền thêm biến vào hàm `gameStart` nhe!!

```python
def main():
    bg = Background()
    car = Car()
    obstacles = Obstacles()
    score = Score()
    gameStart(bg)
    while True:
        gamePlay(bg, car, obstacles, score)
        gameOver(bg, car, obstacles, score)
```

## Kết

Vậy là chúng ta đã hoàn thành xong **game** đua xe đơn giản rồi. **Game** này khá dễ nên phù hợp với những bạn mới học lập trình **game**. Trong **game** còn nhiều thứ chưa tốt, các bạn có thể tự cải thiện thêm.

- https://codelearn.io/sharing/lap-trinh-game-dua-xe-voi-pygame