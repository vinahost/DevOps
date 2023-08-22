# Lập Trình Game Flappy Bird với Pygame

Chào mọi người! Trong bài hướng dẫn lần này mình sẽ cùng các bạn tạo ra *game* **Flappy Bird** với **pygame**. **Flappy Bird** là một **game** mà hầu như ai cũng biết, cách chơi đơn giản và gần như ai học lập trình **game** cũng từng thử làm tựa **game** này. Không nói dài dòng nữa, chúng ta cùng bắt đầu luôn nha!

## Kiến thức căn bản

Đầu tiên, các bạn cần phải biết cơ bản về **python** và **pygame**. Bạn nào chưa tìm hiểu **pygame** thì có thể xem qua 2 bài hướng dẫn cơ bản của mình ([phần 1](Python-Pygame01.md), [phần 2](Python-Pygame02.md)) hoặc có thể học trên những nguồn khác nhé. Bài này khá dài nên các bạn phải cố gắng theo dõi.

## Tạo thư mục và file cho game

Các bạn hãy tạo thư mục có cấu trúc như sau:

![Flappy Bird](/Image/Flappy-Bird01.png)

`flappyBird.py` là file **code** chính của chúng ta. Thư mục **img** chứa những hình ảnh trong **game**.

Các hình ảnh mình lấy [ở đây](https://opengameart.org/) và mình có chỉnh lại cho phù hợp với **game**.

Các hình ảnh đã có trong phần **source code** đầy đủ rồi. Các bạn nhớ tải về và thêm vào.

Như vậy là chúng ta đã chuẩn bị xong những thứ cần thiết. Bây giờ các bạn mở **file** `flappyBird.py` rồi **code** thôi!

## Tạo cửa sổ game và vẽ nền cho game

Phần này khá đơn giản nên mình đi nhanh qua nhe. Mọi thứ trong này mình đã giới thiệu ở những bài trước rồi.

À mà do mình đang **code** từng bước nên nhiều khi những dòng **code** có sự khác biệt so với đoạn **code** đầy đủ nhe!

``` python
import pygame, sys, random
from pygame.locals import *

WINDOWWIDTH = 400
WINDOWHEIGHT = 600

BACKGROUND = pygame.image.load('img/background.png')

pygame.init()
FPS = 60
fpsClock = pygame.time.Clock()

DISPLAYSURF = pygame.display.set_mode((WINDOWWIDTH, WINDOWHEIGHT))
pygame.display.set_caption('Flappy Bird')

def main():
    while True:
        for event in pygame.event.get():
            if event.type == QUIT:
                pygame.quit()
                sys.exit()
        
        DISPLAYSURF.blit(BACKGROUND, (0, 0))

        pygame.display.update()
        fpsClock.tick(FPS)
if __name__ == '__main__':
    main()
```

## Tạo con chim và điểu khiển nó

**Tạo con chim và vẽ lên cửa sổ**

Ở đầu đoạn **code**, chúng ta sẽ tạo một số hằng số để tiện cho việc thay đổi, kiểm tra. Các bạn tham khảo ở **code** đầy đủ nhe!

``` python
BIRDWIDTH = 60
BIRDHEIGHT = 45
G = 0.5
SPEEDFLY = -8
BIRDIMG = pygame.image.load('img/bird.png')
```

Tạo **class** `Bird` như sau:

``` python
class Bird():
    def __init__(self):
        self.width = BIRDWIDTH
        self.height = BIRDHEIGHT
        self.x = (WINDOWWIDTH - self.width)/2
        self.y = (WINDOWHEIGHT- self.height)/2
        self.speed = 0
        self.suface = BIRDIMG

    def draw(self):
        DISPLAYSURF.blit(self.suface, (int(self.x), int(self.y)))
```

Có lẽ các bạn cũng đã hiểu vai trò của các biến trong `__init__` rồi! Các bạn có thể xem hình dưới

![Flappy Bird](/Image/Flappy-Bird02.png)

Các bạn cần chú ý `self.speed` là tốc độ bay của chim. Cái này mình sẽ nói ở phần sau.

Trong hàm `main` tạo thêm 1 biến `bird` và gọi hàm `bird.draw()` trong vòng lặp **game**.

``` python
def main():
    bird = Bird()
    while True:
        for event in pygame.event.get():
            if event.type == QUIT:
                pygame.quit()
                sys.exit()
        
        DISPLAYSURF.blit(BACKGROUND, (0, 0))

        bird.draw()

        pygame.display.update()
        fpsClock.tick(FPS)
```

Các bạn chạy thử nhe. Con chim đã được vẽ lên màn hình. Bây giờ chúng ta hãy tìm cách làm cho con chim rơi xuống.

## Chuyển động rơi tự do của chim

Bây giờ chúng ta hãy **code** trước rồi mình sẽ giải thích sau.

Như bài hướng dẫn trước, để thay đổi vị trí của chim thì cần phải có thêm hàm `update`. Các bạn hãy tạo hàm `update` của **class** `Bird` như sau:

``` python
def update(self):
    self.y += self.speed + 0.5*G
    self.speed += G
```

Trong vòng lặp **game** các bạn nhớ gọi hàm `bird.update()` sau dòng `bird.draw()`

``` python
while True:
    for event in pygame.event.get():
        if event.type == QUIT:
            pygame.quit()
            sys.exit()
    
    DISPLAYSURF.blit(BACKGROUND, (0, 0))

    bird.draw()
    bird.update()
    
    pygame.display.update()
    fpsClock.tick(FPS)
```

Chạy thử và xem kết quả nhé. Bây giờ chúng ta cần biết những gì đã xảy ra bên trong hàm `update`.

Nhưng trước hết, các bạn cần phải biết những điều cơ bản về **chuyển động rơi tự do**. Thực chất, rơi tự do là chuyển động **nhanh dần đều** theo **gia tốc trọng trường**. Các bạn cần phải biết các phương trình của chuyển động rơi tự do **y = y0 + v0t + 0.5gt2** và **v = v0 + gt**. Nếu chưa rõ thì các bạn có thể tự tìm hiểu nhe, mình không nói kỹ ở đây.

![Flappy Bird](/Image/Flappy-Bird03.png)

Trong hàm `update` chúng ta sẽ tính **tung độ** (`self.y`) và **tốc độ** (`self.speed`) của chim ứng với từng khung hình. Như các bạn đã biết, các khung hình xuất hiện cách nhau một khoảng thời gian xác định. Để cho đơn giản thì **cho những khoảng thời gian đều bằng 1** và khi thay vào phương trình rơi tự do sẽ được 2 dòng **code** trên. Trong đó, **G** chính là **gia tốc trọng trường**.

## Chuyển động bay lên khi Click chuột

Để tạo được chuyển động bay lên khi **Click** chuột thì cần phải **bắt sự kiện Click chuột** và **thay đổi tốc độ** của chim. Tốc độ bay lên tất nhiên phải có giá trị âm, trong **code** của chúng ta đã một hằng số là `SPEEDFLY = -8`.

Bây giờ cần phải thêm một số thứ vào hàm `update`.

``` python
def update(self, mouseClick):
    self.y += self.speed + 0.5*G
    self.speed += G
    if mouseClick == True:
        self.speed = SPEEDFLY
```

Có thêm 1 tham số là `mouseClick`, biến này dùng để kiểm tra việc **Click chuột**.

Dòng `if` được thêm vào khá đơn giản. Nếu **Click chuột** thì đặt tốc độ chim là `SPEEDFLY`.

Bây giờ, việc cần làm là bắt sự kiện **Click chuột** và truyền vào hàm `update`, thêm những dòng **code** bên trong vòng lặp **game** như sau:

``` python
while True:
    mouseClick = False
    for event in pygame.event.get():
        if event.type == QUIT:
            pygame.quit()
            sys.exit()
        if event.type == MOUSEBUTTONDOWN:
            mouseClick = True

    DISPLAYSURF.blit(BACKGROUND, (0, 0))

    bird.draw()
    bird.update(mouseClick)
    
    pygame.display.update()
    fpsClock.tick(FPS)
```

Biến `mouseClick` để kiểm tra sự kiện **Click chuột** và truyền vào hàm `update`. Trong vòng `for` thì thực hiện việc bắt sự kiện `MOUSEBUTTONDOWN`, sự kiện này xảy ra khi nút chuột được ấn vào.

Vậy là chúng ta đã hoàn thành xong phần của con chim. Trước khi sang phần mới, các bạn nên nhìn lại đoạn **code**, nghĩ lại những dòng **code** xem chúng có chức năng gì, hoạt động ra sao. Nếu hiểu rõ mọi thứ rồi thì chúng ta cùng sang phần tiếp theo, đó là tạo những cái cột.

## Tạo cột và chuyển động của cột

**Tạo cột**

Như đã biết thì sẽ có những phần cột bên trên và bên dưới. Khi mình nói đến 1 cái "**cột**" thì các bạn hãy hiểu là có cả 2 phần và khoảng trống giữa 2 phần nhe!

Bây giờ, các bạn hãy gõ **code** trước đi. Mình sẽ giải thích những đoạn **code** đó sau.

Thêm một số hằng số:

``` python
COLUMNWIDTH = 60
COLUMNHEIGHT = 500
BLANK = 160
DISTANCE = 200
COLUMNSPEED = 2
COLUMNIMG = pygame.image.load('img/column.png')
```

Tạo class `Columns`

``` python
class Columns():
    def __init__(self):
        self.width = COLUMNWIDTH
        self.height = COLUMNHEIGHT
        self.blank = BLANK
        self.distance = DISTANCE
        self.speed = COLUMNSPEED
        self.surface = COLUMNIMG
        self.ls = []
        for i in range(3):
            x = i*self.distance
            y = random.randrange(60, WINDOWHEIGHT - self.blank - 60, 20)
            self.ls.append([x, y])
        
    def draw(self):
        for i in range(3):
            DISPLAYSURF.blit(self.surface, (self.ls[i][0], self.ls[i][1] - self.height))
            DISPLAYSURF.blit(self.surface, (self.ls[i][0], self.ls[i][1] + self.blank))
```

Trong hàm `main` làm tương tự như phần con chim nhe. Tạo biến `columns`, thêm hàm `columns.draw()` vào vòng lặp **game**.

``` python
def main():
    bird = Bird()
    columns = Columns()
    while True:
        mouseClick = False
        for event in pygame.event.get():
            if event.type == QUIT:
                pygame.quit()
                sys.exit()
            if event.type == MOUSEBUTTONDOWN:
                mouseClick = True

        DISPLAYSURF.blit(BACKGROUND, (0, 0))

        columns.draw()

        bird.draw()
        bird.update(mouseClick)

        pygame.display.update()
        fpsClock.tick(FPS)
```

Rồi bây giờ thì hãy chạy thử và xem thành quả. Sau đó, đến phần tìm hiểu **code** thôi!

Bây giờ hãy nhìn lại **class** `Columns`, trong hàm `__init__` có khởi tạo một số biến:

- `self.width` là chiều rộng của cột.
- `self.height` là chiều cao của **1 phần cột**.
- `self.blank` là chiều cao khoảng trống giữa 2 phần cột.
- `self.distance` là khoảng cách giữa các cột.
- `self.speed` là tốc độ di chuyển của cột (cột sẽ di chuyển sang trái).
- `self.surface` là surface của **1 phần cột**.
- `self.ls` là một **list** chứa thông tin về **vị trí cột và vị trí khoảng trống**. Cụ thể: một phần tử trong list chứ thông tin của một cột, phần tử đó có dạng [**x, y**], trong đó x là **hoành độ** của cột, y là **tung độ** của khoảng trống.

Vòng lặp `for` trong `__init__` dùng để tạo 3 cột đầu tiên.

``` python
for i in range(3):
    x = i*self.distance
    y = random.randrange(60, WINDOWHEIGHT - self.blank - 60, 20)
    self.ls.append([x, y])
```

- Biến `x` là hoành độ của cột. Cột đầu tiên được vẽ sát lề bên trái, tức là **x = 0**. Các cột tiếp theo cách nhau một khoảng bằng `self.distance`.
- Biến `y` là vị trí của khoảng trống. Biến này nhận giá trị **random** để tạo những khoảng trống có vị trí khác nhau.

![Flappy Bird](/Image/Flappy-Bird04.png)

Bây giờ hãy xem hàm `draw` của **class** `Columns`.

``` python
def draw(self):
    for i in range(3):
        DISPLAYSURF.blit(self.surface, (self.ls[i][0], self.ls[i][1] - self.height))
        DISPLAYSURF.blit(self.surface, (self.ls[i][0], self.ls[i][1] + self.blank))
```

Vòng lặp `for` dùng để vẽ 3 cột, mỗi cột có 2 phần nên có 2 dòng để vẽ phần trên và phần dưới.

Như vậy là chúng ta đã tạo được các cột, bây giờ hãy sang phần tiếp theo để tìm hiểu cách cho những cái cột di chuyển.

## Tạo chuyển động cho cột

Như thường lệ, chúng ta sẽ thêm hàm `update` cho **class** `Columns` và ở vòng lặp **game** thì gọi hàm `columns.update()` sau dòng `columns.draw()`. Ý tưởng là dùng vòng lặp `for` để lặp qua 3 cột, đối với mỗi cột thì hoành độ của nó sẽ giảm xuống. Nghe có vẻ đơn giản nhỉ.

``` python
def update(self):
    for i in range(3):
        self.ls[i][0] -= self.speed
```

Chạy thử để xem kết quả nhé. Những cái cột đã di chuyển sang trái rồi.

Tuy nhiên, có một vấn đề ở đây là chúng ta mới chỉ có 3 cột, trong khi đó game cần phải có rất nhiều (nếu không muốn nói là vô hạn) cột. Các bạn có thể tạo thật nhiều phần tử cho `self.ls`. Tuy nhiên, điều này nghe có vẻ không hiệu quả chút nào. Hãy nghĩ ra 1 ý tưởng mới nào, các cột sẽ di chuyển sang trái và khi một cột nào đó đi ra khỏi màn hình thì nó không còn công dụng gì nữa, chúng ta có thể xoá cái cột đó đi và tạo một cái cột khác nối tiếp theo. Theo vậy thì `self.ls` sẽ vẫn chỉ có 3 phần tử, khi đến những thời điểm thích hợp sẽ có **1 cột bị xoá đi** và **1 cột được thêm vào**.

![Flappy Bird](/Image/Flappy-Bird05.png)

Nếu có ý tưởng rồi thì thêm **code** vào hàm `update` thôi.

``` python
def update(self):
    for i in range(3):
        self.ls[i][0] -= self.speed
    if self.ls[0][0] < -self.width:
        self.ls.pop(0)
        x = self.ls[1][0] + self.distance
        y = random.randrange(60, WINDOWHEIGHT - self.blank - 60, 10)
        self.ls.append([x, y])
```

Dòng `if` được thêm vào để kiểm tra xem cột có đi ra ngoài màn hình hay chưa.

Dòng s`elf.ls.pop(0`) để xoá cột ở vị trí đầu tiên.

- `x` là **hoành độ** cột thêm vào. Cột được thêm vào sẽ cách cột cuối một khoảng bằng `self.distance`. Do vừa xoá 1 cột rồi nên `self.ls` chỉ còn 2 cột thôi.

- `y` là **tung độ** khoảng trống của cột thêm vào, cũng lấy giá trị `random` như những cột khác.

Các bạn hãy chạy thử và xem kết quả nhe.

À mà lúc nãy mình code cái cột đầu tiên ở sát lề bên trái để tiện theo dõi thôi. Thực tế, trong **game** các cột sẽ ở bên phải màn hình rồi mới di chuyển qua. Nên ở vòng lặp `for` trong hàm `__init__` các bạn sửa lại dòng `x = i*self.distance` thành `x = WINDOWWIDTH + i*self.distance` để cột đầu tiên nằm bên phải màn hình

Như vậy là chúng ta đã hoàn thành xong phần cột rồi! Các bạn xem lại **code** để chuẩn bị sang phần tiếp theo nhé!

## Xử lý va chạm

**Va chạm giữa hai hình chữ nhật**

Xử lý va chạm là một phần không thể thiếu trong lập trình **game**. Trong **Flappy Bird** sẽ có sự va chạm giữa chim và cột, vì thế chúng ta cần phải biết rõ khi nào chim sẽ va chạm với cột, từ đó viết những đoạn **code** để xử lý. Để cho đơn giản thì hãy xem chim là một hình chữ nhật, một phần của cột (trên hoặc dưới) là một hình chữ nhật. Chúng ta cần viết một hàm để kiểm tra xem hai hình chữ nhật có va chạm với nhau hay không. Hàm cần viết sẽ có 2 tham số truyền vào là 2 **list** chứa những thông số hình chữ nhật (hoành độ, tung độ, chiều rộng, chiều cao).

``` python
def rectCollision(rect1, rect2):
    if rect1[0] <= rect2[0]+rect2[2] and rect2[0] <= rect1[0]+rect1[2] and rect1[1] <= rect2[1]+rect2[3] and rect2[1] <= rect1[1]+rect1[3]:
        return True
    return False
```

Cách sử dụng của hàm này rất đơn giản, truyền 2 "**hình chữ nhật**" vào hàm, hàm trả về `True` nếu 2 hình chữ nhật chạm nhau, ngược lại trả về `False`. Hàm này hoạt động thế nào thì các bạn tự đọc **code** rồi tìm hiểu nhe, mình đã có một bài riêng để nói về xử lý va chạm, bạn có thể tham khảo thêm [tại đây](Collision-Pygame.md)

## Kiểm tra gameover

Như các bạn đã biết, khi chim chạm vào cột, chạm đất hoặc đi quá cao ra khỏi màn hình thì **gameover**. Ở trên chúng ta đã có hàm để kiểm tra sự va chạm giữa 2 hình chữ nhật rồi, chúng ta sẽ dùng hàm đó trong hàm kiểm tra **gameover**. Các bạn thêm hàm `isGameOver` phía dưới hàm `rectCollision` nhe. Mình sẽ giải thích **code** sau.

```python
def isGameOver(bird, columns):
    for i in range(3):
        rectBird = [bird.x, bird.y, bird.width, bird.height]
        rectColumn1 = [columns.ls[i][0], columns.ls[i][1] - columns.height, columns.width, columns.height]
        rectColumn2 = [columns.ls[i][0], columns.ls[i][1] + columns.blank, columns.width, columns.height]
        if rectCollision(rectBird, rectColumn1) == True or rectCollision(rectBird, rectColumn2) == True:
            return True
    if bird.y + bird.height < 0 or bird.y + bird.height > WINDOWHEIGHT:
        return True
    return False
```

Trong hàm này các bạn có thể thấy một vòng lặp `for` dùng để chạy qua 3 cột, chim chạm bất kì cột nào cũng **gameover**. Các bạn xem những dòng **code** bên trong `for` nhé. Biến `rectBird` được tạo ra để chứa thông số hình chữ nhật của chim. Tương tự thế, 2 biến `rectColumn1` và `rectColumn2` là thông số 2 hình chữ nhật của cột (mỗi cột có phần trên và phần dưới). Dòng `if` bên trong vòng lặp `for` để kiểm tra xem chim có chạm vào 1 trong 2 phần cột hay không. Hàm `rectCollision` được dùng để kiểm tra sự va chạm giữa chim và 1 phần cột. Nếu chim chạm vào 1 trong 2 phần cột thì hàm `isGameOver` trả về giá trị `True`.

Dòng `if` bên dưới vòng `for` dùng để kiểm tra thêm trường hợp chim chạm đất hoặc bay quá cao. Dòng này cũng đơn giản thôi!

Vậy là chúng ta đã có một hàm để biết lúc nào thì **gameover**. Hãy kiểm tra hoạt động của hàm này nhe! Các bạn hãy thêm đoạn **code** này vào trong vòng lặp **game**.

```python
if isGameOver(bird, columns) == True:
    pygame.quit()
    sys.exit()
```

Đoạn này nhìn vào thì cũng hiểu ngay. Khi kiểm tra thấy **gameover** thì kết thúc **game** luôn. Các bạn tự chạy thử nhé!

Vậy là chúng ta đã xong phần xử lý va chạm rồi. Tiếp theo chúng ta sẽ tìm cách tính điểm trong **game** nhe!

## Tính điểm

Trong **game**, nếu con chim đi qua 1 cột thì sẽ được cộng thêm điểm. Vậy làm sao để biết chim đi qua 1 cột?? Trong phần trước, chúng ta đã có hàm `rectCollision` để kiểm tra sự va chạm của 2 hình chữ nhật. Vì vậy, chúng ta có thể tận dụng hàm này bằng cách tạo một hình chữ nhật phía sau cột, tạm gọi là "**hình chữ nhật tính điểm**", nếu chim chạm vào hình chữ nhật đó thì sẽ cộng thêm điểm. Các bạn có thể xem hình minh hoạ dưới đây.

![Flappy Bird](/Image/Flappy-Bird06.png)

Các bạn sẽ tạo một **class** `Score` như bên dưới. Nhìn nó có vẻ phức tạp đấy, mình sẽ giải thích sau.

```python
class Score():
    def __init__(self):
        self.score = 0
        self.addScore = True
    
    def draw(self):
        font = pygame.font.SysFont('consolas', 40)
        scoreSuface = font.render(str(self.score), True, (0, 0, 0))
        textSize = scoreSuface.get_size()
        DISPLAYSURF.blit(scoreSuface, (int((WINDOWWIDTH - textSize[0])/2), 100))
    
    def update(self, bird, columns):
        collision = False
        for i in range(3):
            rectColumn = [columns.ls[i][0] + columns.width, columns.ls[i][1], 1, columns.blank]
            rectBird = [bird.x, bird.y, bird.width, bird.height]
            if rectCollision(rectBird, rectColumn) == True:
                collision = True
                break
        if collision == True:
            if self.addScore == True:
                self.score += 1
            self.addScore = False
        else:
            self.addScore = True
```

Trong hàm `main`, các bạn nhớ tạo thêm biến `score` và gọi 2 hàm của nó trong vòng lặp **game**.

```python
def main():
    bird = Bird()
    columns = Columns()
    score = Score()
    while True:
        mouseClick = False
        for event in pygame.event.get():
            if event.type == QUIT:
                pygame.quit()
                sys.exit()
            if event.type == MOUSEBUTTONDOWN:
                mouseClick = True
        DISPLAYSURF.blit(BACKGROUND, (0, 0))
        columns.draw()
        columns.update()
        bird.draw()
        bird.update(mouseClick)
        
        score.draw()
        score.update(bird, columns)

        if isGameOver(bird, columns) == True:
            pygame.quit()
            sys.exit()
        pygame.display.update()
        fpsClock.tick(FPS)
```

Xong rồi, bây giờ chúng ta cùng nhìn lại **class**  . Trong hàm      có 2 biến,      chính là số điểm, còn biến      dùng để kiểm tra  xem có được cộng thêm điểm hay không.

Tại sao lại có biến      và biến này sử dụng như thế nào?? Trong **game**, khi chim đi qua cột sẽ chạm vào "**hình chữ nhật tính điểm**" rất nhiều lần. Tuy nhiên, mỗi lần chim qua cột chỉ tính 1 điểm thôi, vì thế biến này để chúng ta cộng thêm điểm vào lần chạm đầu tiên.

Điểm chỉ được cộng khi `self.addScore` là `True`. Ban đầu thì `self.addScore = True`, khi chim chạm vào "**hình chữ nhật tính điểm**" lần đầu thì sẽ được cộng điểm và `self.addScore` nhận giá trị `False`. Nếu chim đi qua khỏi "**hình chữ nhật tính điểm**", tức là không chạm vào nó nữa thì `self.addScore` nhận giá trị `True` để chuẩn bị cộng điểm cho lần chạm tiếp theo. Nếu các bạn thấy rối quá thì có thể xem hình bên dưới nhe!

![Flappy Bird](/Image/Flappy-Bird07.png)

Hàm `draw` trong **class** `Score` chỉ dùng để vẽ điểm thôi, không có gì phức tạp nên mình không giải thích hàm này.

Các bạn chú ý hàm `update` nhé. Biến `collision` để kiểm tra xem chim có chạm "**hình chữ nhật tính điểm**" hay không. Cũng tương tự như hàm `isGameOver`, có một vòng `for` để chạy qua 3 cột, biến `rectColumn` chính là "**hình chữ nhật tính điểm**", `rectBird` là hình chữ nhật của chim, vẫn dùng hàm `rectCollision` để kiểm tra va chạm.

Những dòng **code** bên dưới là để cộng thêm điểm cho biến `self.score` và điều chỉnh biến `self.addScore` cho phù hợp. Các dòng code hoạt động theo như phần giải thích ở trên.

Vậy là chúng ta đã gần hoàn chỉnh game **Flappy Bird** rồi. Các bạn chạy thử để kiểm tra nhé. Nếu thấy khó để kiểm tra thì có thể điều chỉnh các hằng số nhé (tăng chiều cao khoảng trống, giảm tốc độ cột ...). Nếu thấy ok rồi thì hãy sang phần cuối cùng nào!

## Tạo các màn cho game và hoàn chỉnh game

Như các bạn đã thấy thì **game** của chúng ta vẫn chưa hoàn chỉnh. Ví dụ như khi bắt đầu **game** thì cần có 1 màn hình bắt đầu, sau khi **Click chuột** thì mới vào chơi. Sau khi **gamover** thì cũng phải có màn hình **gameover** rồi có thể cho chơi lại chẳng hạn. Bây giờ chúng ta cùng tạo các màn như thế nhé.

Chúng ta sẽ tạo 3 phần giản thôi: **bắt đầu game**, **chơi**, **gameover**. Chúng ta sẽ viết 3 hàm tương ứng là `gameStart`, `gamePlay`, `gameOver`.

### gamePlay

Thực chất thì cái vòng lặp **game** chúng ta **code** từ trước đến giờ là của màn `gamePlay` thôi. Chúng ta sẽ tạo hàm `gamePlay` như sau:

```python
def gamePlay(bird, columns, score):
    bird.__init__()
    bird.speed = SPEEDFLY
    columns.__init__()
    score.__init__()
    while True:
        mouseClick = False
        for event in pygame.event.get():
            if event.type == QUIT:
                pygame.quit()
                sys.exit()
            if event.type == MOUSEBUTTONDOWN:
                mouseClick = True
        
        DISPLAYSURF.blit(BACKGROUND, (0, 0))
        columns.draw()
        columns.update()
        bird.draw()
        bird.update(mouseClick)
        score.draw()
        score.update(bird, columns)

        if isGameOver(bird, columns) == True:
            return

        pygame.display.update()
        fpsClock.tick(FPS)
```

Các bạn chỉnh lại hàm `main` như sau:

```python
def main():
    bird = Bird()
    columns = Columns()
    score = Score()
    while True:
        gamePlay(bird, columns, score)
```

Khi chạy hàm `gamePlay` thì các giá trị của `bird`, `columns`, `score` sẽ được đặt lại bằng việc gọi hàm `__init__` và chỗ con chim thì cho nó bay lên 1 cái. Khi kiểm tra thấy `gameOver` thì kết thúc hàm.

### gameStart

Hàm này thì quá đơn giản rồi, chỉ việc sắp xếp những hình ảnh, dòng chữ sao cho đẹp mắt thôi. Ngoài ra thì chúng ta chỉ cần thêm việc bắt sự kiện **Click chuột** rồi kết thúc hàm (chuyển sang `gamePlay`).

```python
def gameStart(bird):
    bird.__init__()

    font = pygame.font.SysFont('consolas', 60)
    headingSuface = font.render('FLAPPY BIRD', True, (255, 0, 0))
    headingSize = headingSuface.get_size()
    
    font = pygame.font.SysFont('consolas', 20)
    commentSuface = font.render('Click to start', True, (0, 0, 0))
    commentSize = commentSuface.get_size()
    
    while True:
        for event in pygame.event.get():
            if event.type == QUIT:
                pygame.quit()
                sys.exit()
            if event.type == MOUSEBUTTONDOWN:
                return

        DISPLAYSURF.blit(BACKGROUND, (0, 0))
        bird.draw()
        DISPLAYSURF.blit(headingSuface, (int((WINDOWWIDTH - headingSize[0])/2), 100))
        DISPLAYSURF.blit(commentSuface, (int((WINDOWWIDTH - commentSize[0])/2), 500))

        pygame.display.update()
        fpsClock.tick(FPS)
```

Trong `main` cũng nhớ gọi hàm này nhe:

```python
def main():
    bird = Bird()
    columns = Columns()
    score = Score()
    while True:
        gameStart(bird)
        gamePlay(bird, columns, score)
```

### gameOver

Các bạn thêm hàm `gameOver` vào luôn nhe, cũng chỉ là vẽ chữ, vẽ số điểm thôi. Hàm này thì bắt **sự kiện nhấn phím space** để kết thúc hàm.

```python
def gameOver(bird, columns, score):
    font = pygame.font.SysFont('consolas', 60)
    headingSuface = font.render('GAMEOVER', True, (255, 0, 0))
    headingSize = headingSuface.get_size()
    
    font = pygame.font.SysFont('consolas', 20)
    commentSuface = font.render('Press "space" to replay', True, (0, 0, 0))
    commentSize = commentSuface.get_size()

    font = pygame.font.SysFont('consolas', 30)
    scoreSuface = font.render('Score: ' + str(score.score), True, (0, 0, 0))
    scoreSize = scoreSuface.get_size()

    while True:
        for event in pygame.event.get():
            if event.type == QUIT:
                pygame.quit()
                sys.exit()
            if event.type == KEYUP:
                if event.key == K_SPACE:
                    return
        
        DISPLAYSURF.blit(BACKGROUND, (0, 0))
        columns.draw()
        bird.draw()
        DISPLAYSURF.blit(headingSuface, (int((WINDOWWIDTH - headingSize[0])/2), 100))
        DISPLAYSURF.blit(commentSuface, (int((WINDOWWIDTH - commentSize[0])/2), 500))
        DISPLAYSURF.blit(scoreSuface, (int((WINDOWWIDTH - scoreSize[0])/2), 160))

        pygame.display.update()
        fpsClock.tick(FPS)
```

Xong rồi thì nhớ chỉnh hàm `main` luôn.

```python
def main():
    bird = Bird()
    columns = Columns()
    score = Score()
    while True:
        gameStart(bird)
        gamePlay(bird, columns, score)
        gameOver(bird, columns, score)
```

Thế là xong rồi. Code của chúng ta đã giống với đoạn **code** đầy đủ rồi. Các bạn chạy thử và xem thành quả đi!

## Kết
Vậy là chúng ta đã hoàn thành xong game **Flappy Bird** với **pygame**. Các bạn cũng có thể thêm những tính năng mới (thêm điểm cao, thêm huy chương...) cho **game** thú vị hơn. 

Bạn có thể tham khảo đoạn **code** đầy đủ [tại đây](/Scripts/Python/FlappyBird.zip)

- https://codelearn.io/sharing/lam-game-flappy-bird-voi-pygame