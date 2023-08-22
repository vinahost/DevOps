# Game Cờ Caro Qua Mạng LAN Bằng Python

Cờ **Caro** là một **Game** trí tuệ đã rất quen thuộc với mỗi chúng ta. Với lối chơi đơn giản nhưng yếu tố trí tuệ lại rất cao nên cờ **Caro** được rất nhiều người yêu thích đặc biệt là các bạn học sinh, sinh viên và dân văn phòng. **Game** cờ **Caro** không chỉ mang tính chất giải trí đơn thuần mà là một cuộc đấu trí vô cùng gay cấn.

Hãy cùng mình làm một chương trình chơi cờ **Caro** đơn giản để cùng chơi với bạn bè của mình nhé.

## Thuật ngữ và cách chơi trong Game

### Thuật ngữ

- Quân cờ: trong game sử dụng bộ dấu ký hiệu X/O
- Bàn cờ Caro
- - Sử dụng bàn cờ kích thước X×Y (ô vuông)
- - Trên một bàn cờ chỉ cho phép 2 người tham gia.

- Lượt chơi (tick X/O)
- - Một ván cờ có thể gồm nhiều lượt chơi.
- - Mỗi lượt người chơi thực hiện tick các dấu X/O vào hình vuông trên bàn cờ.

### Luật chơi

Luật chơi cũng như cách chơi cờ **caro** hết sức đơn giản, hai bên thay phiên nhau tích vào những ô vuông trên bàn cờ, để tạo thành đường 5 quân theo hàng dọc, hàng ngang hoặc đường chéo là thắng. Tuy nhiên, nếu muốn chiến thắng khi chơi cờ **caro** bạn phải tạo ra những thế cờ độc, dùng chiến thuật và kinh nghiệm để đánh lừa và hạ gục đối thủ.

Bài viết mặc định yêu cầu bạn đã có kiến thức về **Python** cơ bản nhé

## Cài đặt Python và thư viện cần thiết.

- Cài đặt python :

Các bạn click [vào đây](Introduction-to-Python.md) để được hướng dẫn cài đặt **Python**

- Cài đặt thư viện cho Python

```
pip install tk
```

## Bắt đầu lập trình thôi nào.

### Import các thư viện cần thiết

Mình dùng các thư viện  `tkinter`, `threading`, `socket` để tạo giao diện, xử lý đa luồng và kết nối hai chương trình với nhau.

```python
import tkinter as tk
from functools import partial
import threading
import socket
from tkinter import messagebox
```

### Tạo giao diện cho chương trình

Mình tạo khởi tạo một lớp `Window` kế thừa từ `tk.TK`.

```python
class Window(tk.Tk):
```

Hàm khởi tạo và tạo các thành phần trong **GUI** như `Frame`, `Button`, `Label`,....

```python
def __init__(self):
        super().__init__()
        self.title("Caro Tkinter - codelearn.io")
        self.Buts = {}
        self.memory = []
        self.Threading_socket = Threading_socket(self)
        print(self.Threading_socket.name)

    def showFrame(self):
        frame1 = tk.Frame(self)
        frame1.pack()
        frame2 = tk.Frame(self)
        frame2.pack()

        Undo = tk.Button(frame1, text="Undo", width=10,  # nút quay lại
                         command=partial(self.Undo, synchronized=True))
        Undo.grid(row=0, column=0, padx=30)

        tk.Label(frame1, text="IP", pady=4).grid(row=0, column=1)
        inputIp = tk.Entry(frame1, width=20)  # Khung nhập địa chỉ ip
        inputIp.grid(row=0, column=2, padx=5)
        connectBT = tk.Button(frame1, text="Connect", width=10,
                              command=lambda: self.Threading_socket.clientAction(inputIp.get()))
        connectBT.grid(row=0, column=3, padx=3)

        makeHostBT = tk.Button(frame1, text="MakeHost", width=10,  # nút tạo host
                               command=lambda: self.Threading_socket.serverAction())
        makeHostBT.grid(row=0, column=4, padx=30)
        for x in range(Ox):   # tạo ma trận button Ox * Oy
            for y in range(Oy):
                self.Buts[x, y] = tk.Button(frame2, font=('arial', 15, 'bold'), height=1, width=2,
                                            borderwidth=2, command=partial(self.handleButton, x=x, y=y))
                self.Buts[x, y].grid(row=x, column=y)
```

Chúng ta tạo các hàm xử lý **Button**, kiểm tra người chiến thắng, thông báo và quay lại nước đánh trước.

```python
def handleButton(self, x, y):
        if self.Buts[x, y]['text'] == "": #Kiểm tra ô có ký tự rỗng hay không
            if self.memory.count([x, y]) == 0:
                self.memory.append([x, y])
            if len(self.memory) % 2 == 1:
                self.Buts[x, y]['text'] = 'O'
                self.Threading_socket.sendData("{}|{}|{}|".format("hit", x, y))
                if(self.checkWin(x, y, "O")):
                    self.notification("Winner", "O")
                    self.newGame()
            else:
                print(self.Threading_socket.name)
                self.Buts[x, y]['text'] = 'X'
                self.Threading_socket.sendData("{}|{}|{}|".format("hit", x, y))
                if(self.checkWin(x, y, "X")):
                    self.notification("Winner", "X")
                    self.newGame()

    def notification(self, title, msg):
        messagebox.showinfo(str(title), str(msg))

    def checkWin(self, x, y, XO):
        count = 0
        i, j = x, y
        while(j < Ox and self.Buts[i, j]["text"] == XO):
            count += 1
            j += 1
        j = y
        while(j >= 0 and self.Buts[i, j]["text"] == XO):
            count += 1
            j -= 1
        if count >= 6:
            return True
        # check cột
        count = 0
        i, j = x, y
        while(i < Oy and self.Buts[i, j]["text"] == XO):
            count += 1
            i += 1
        i = x
        while(i >= 0 and self.Buts[i, j]["text"] == XO):
            count += 1
            i -= 1
        if count >= 6:
            return True
        # check cheo phai
        count = 0
        i, j = x, y
        while(i >= 0 and j < Ox and self.Buts[i, j]["text"] == XO):
            count += 1
            i -= 1
            j += 1
        i, j = x, y
        while(i <= Oy and j >= 0 and self.Buts[i, j]["text"] == XO):
            count += 1
            i += 1
            j -= 1
        if count >= 6:
            return True
        # check cheo trai
        count = 0
        i, j = x, y
        while(i < Ox and j < Oy and self.Buts[i, j]["text"] == XO):
            count += 1
            i += 1
            j += 1
        i, j = x, y
        while(i >= 0 and j >= 0 and self.Buts[i, j]["text"] == XO):
            count += 1
            i -= 1
            j -= 1
        if count >= 6:
            return True
        return False

    def Undo(self, synchronized):
        if(len(self.memory) > 0):
            x = self.memory[len(self.memory) - 1][0]
            y = self.memory[len(self.memory) - 1][1]
            # print(x,y)
            self.Buts[x, y]['text'] = ""
            self.memory.pop()
            if synchronized == True:
                self.Threading_socket.sendData("{}|".format("Undo"))
            print(self.memory)
        else:
            print("No character")

    def newGame(self):
        for x in range(Ox):
            for y in range(Oy):
                self.Buts[x, y]["text"] = ""
```

### Tạo lớp đồng bộ dữ liều người chơi giữa hai chương trình :

Khởi tạo **Class** `Threading_socket`.

```python
class Threading_socket():
```

Hàm `clientAction` và `serverAction` sử dụng **socket** gửi và nhận dữ liệu qua lại giữa hai chương trình.

```python
def __init__(self, gui):
        super().__init__()
        self.dataReceive = ""
        self.conn = None
        self.gui = gui
        self.name = ""

    def clientAction(self, inputIP):
        self.name = "client"
        print("client connect ...............")
        HOST = inputIP  # Cấu hình địa chỉ server
        PORT = 8000              # Cấu hình Port sử dụng
        self.conn = socket.socket(
            socket.AF_INET, socket.SOCK_STREAM)  # Cấu hình socket
        self.conn.connect((HOST, PORT))  # tiến hành kết nối đến server
        self.gui.notification("Đã kết nối tới", str(HOST))
        t1 = threading.Thread(target=self.client)  # tạo luồng chạy client
        t1.start()

    def client(self):
        while True:
            self.dataReceive = self.conn.recv(
                1024).decode()  # Đọc dữ liệu server trả về
            if(self.dataReceive != ""):
                friend = self.dataReceive.split("|")[0]
                action = self.dataReceive.split("|")[1]
                if(action == "hit" and friend == "server"):
                    #     print(self.dataReceive)
                    x = int(self.dataReceive.split("|")[2])
                    y = int(self.dataReceive.split("|")[3])
                    self.gui.handleButton(x, y)
                if(action == "Undo" and friend == "server"):

                    self.gui.Undo(False)
            self.dataReceive = ""

    def serverAction(self):
        self.name = "server"
        HOST = socket.gethostbyname(socket.gethostname())  # Láy  lập địa chỉ
        print("Make host.........." + HOST)
        self.gui.notification("Gui IP chp ban", str(HOST))
        PORT = 8000  # Thiết lập port lắng nghe
        # cấu hình kết nối
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.bind((HOST, PORT))  # lắng nghe
        s.listen(1)  # thiết lập tối ta 1 kết nối đồng thời
        self.conn, addr = s.accept()  # chấp nhận kết nối và trả về thông số
        t2 = threading.Thread(target=self.server, args=(addr, s))
        t2.start()

    def server(self, addr, s):
        try:
            # in ra thông địa chỉ của client
            print('Connected by', addr)
            while True:
                # Đọc nội dung client gửi đến
                self.dataReceive = self.conn.recv(1024).decode()
                if(self.dataReceive != ""):
                    friend = self.dataReceive.split("|")[0]
                    action = self.dataReceive.split("|")[1]
                    print(action)
                    if(action == "hit" and friend == "client"):
                        x = int(self.dataReceive.split("|")[2])
                        y = int(self.dataReceive.split("|")[3])
                        self.gui.handleButton(x, y)
                    if(action == "Undo" and friend == "client"):
                        self.gui.Undo(False)
                self.dataReceive = ""
        finally:
            s.close()  # đóng socket

    def sendData(self, data):
        # Gửi dữ liệu lên server`
        self.conn.sendall(str("{}|".format(self.name) + data).encode())
```

### Khởi tạo hàm `main` và chạy chương trình

```python
if __name__ == "__main__":
    Ox = 20  # Số lượng ô theo trục X
    Oy = 20 # Số lượng ô theo trục Y
    window = Window()
    window.showFrame()
    window.mainloop()
```

## Kết

Bạn có thể tải **source code** trên [tại đây](/Scripts/Python/Caro.py)

Trên đây là bài hướng dẫn lập trình **Caro** hai người chơi qua mạng **Lan** bằng **Python**. Rất mong sự ủng hộ của các bạn, nếu các bạn thấy bài viết có giá trị thì hãy share để ủng hộ mình nhé.

- https://codelearn.io/sharing/game-caro-qua-mang-lan-bang-python