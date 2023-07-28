# Một số lệnh Linux phổ biến được sử dụng trong DevOps:

## Linux là gì?

**Linux** là một hệ điều hành mã nguồn mở và miễn phí được phát triển bởi **Linus Torvalds** vào năm **1991**. **Linux** là một hệ điều hành đa nhiệm, có nghĩa là nó có thể chạy nhiều chương trình cùng một lúc. **Linux** cũng là một hệ điều hành linh hoạt, có thể được cài đặt trên nhiều loại thiết bị, bao gồm máy tính để bàn, máy chủ, máy tính xách tay, máy tính bảng và điện thoại thông minh.

**Linux** được biết đến với tính bảo mật, hiệu suất và tính linh hoạt của nó. **Linux** cũng là một hệ điều hành phổ biến đối với các nhà phát triển phần mềm, vì nó có thể được sử dụng để tạo ra các ứng dụng có hiệu suất cao và bảo mật.

## Tính năng vượt trội của Linux

**Linux** có nhiều tính năng vượt trội so với các hệ điều hành khác, bao gồm:

- **Linh hoạt**: Linux có thể được tùy chỉnh để phù hợp với nhiều nhu cầu khác nhau.
- **Mạnh mẽ**: Linux có thể chạy trên nhiều loại phần cứng, từ máy tính để bàn đến máy chủ.
- **An toàn**: Linux được thiết kế để bảo mật và chống lại các cuộc tấn công.
- **Miễn phí**: Linux là một hệ điều hành miễn phí, có nghĩa là bạn có thể sử dụng nó và sửa đổi nó mà không phải trả bất kỳ khoản phí nào.

## Lệnh cơ bản trên Linux

- wget: Lệnh này được sử dụng để tải xuống các tệp từ internet.
- curl: Lệnh này cũng được sử dụng để tải xuống các tệp từ internet, nhưng nó có nhiều tính năng hơn lệnh wget.
- tar: Lệnh này được sử dụng để giải nén các tệp đã được nén.
- zip: Lệnh này được sử dụng để nén các tệp.
- ssh: Lệnh này được sử dụng để kết nối với các máy tính từ xa.
- scp: Lệnh này được sử dụng để sao chép các tệp giữa các máy tính từ xa.
- rsync: Lệnh này cũng được sử dụng để sao chép các tệp giữa các máy tính từ xa, nhưng nó có nhiều tính năng hơn lệnh scp.
- grep: Lệnh này được sử dụng để tìm kiếm các chuỗi văn bản trong các tệp.
- find: Lệnh này được sử dụng để tìm các tệp trong một thư mục.
- sed: Lệnh này chỉnh sửa văn bản trong một tệp.
- awk: Lệnh này xử lý văn bản trong một tệp theo cách được xác định trước.
- sort: Lệnh này được sử dụng để sắp xếp các tệp theo thứ tự.
- uniq: Lệnh này được sử dụng để loại bỏ các bản sao của các dòng trong một tệp.
- wc: Lệnh này được sử dụng để đếm số lượng dòng, từ và byte trong một tệp.
- ls: Lệnh này liệt kê các tệp và thư mục trong thư mục hiện tại.
- cd: Lệnh này thay đổi thư mục hiện tại.
- cp: Lệnh này sao chép một tệp hoặc thư mục.
- mkdir: Lệnh này tạo một thư mục mới.
- mv: Lệnh này di chuyển hoặc đổi tên một tệp hoặc thư mục.
- rm: Lệnh này xóa một tệp hoặc thư mục.

## Chi tiết những câu lệnh trên Linux thường sử dụng trong DevOps

Sau đây sẽ là những câu lệnh trên **Linux** thường sử dụng trong công việc của **DevOps**.

### Lệnh ls

Câu lệnh này sẽ liệt kê toàn bộ nội dung tại thư mục đang làm việc.

**Cú pháp:**

    ls <option>

|Câu lệnh|Chi tiết|
|--------|--------|
|ls <đường dẫn>|Liệt kê các nội dung đang được lưu trữ bên trong đường dẫn chỉ định.|
|ls -l|Liệt kê toàn bộ nội dung có trong thư mục kèm theo các trường thông tin như người sở hữu, các quyền trên nội dung, thời gian chỉnh sửa gần nhất.|
|ls -a|Liệt kê toàn bộ nội dung bên trong thư mục, bao gồm các tập tin ẩn.|
 
### Lệnh sudo

Thực thi câu lệnh kèm theo với quyền `root` / `superuser`.

**Cú pháp:**

    sudo - execute a command as another user

|Câu lệnh|Chi tiết|
|--------|--------|
|sudo useradd <username>|Thêm mới user vào hệ thống.|
|sudo passwd <username>|Thiết lập mật khẩu cho user.|
|sudo userdel <username>|Xóa user ra khỏi hệ thống.|
|sudo groupadd <groupname>|Thêm mới group vào hệ thống.|
|sudo groupdel <groupname>|Xóa group ra khỏi hệ thống.|
|sudo usermod -g <groupname> <username>|Thêm user vào một group primary.|
 
### Lệnh cat

Câu lệnh này sẽ giúp người dùng có thể đọc, điều chỉnh hoặc ghép các tập tin định dạng **text**.

**Cú pháp:**

    cat <option> <tên file>

|Câu lệnh|Chi tiết|
|--------|--------|
|cat -b|Đánh số thứ tự cho các dòng có ký tự.|
|cat -n|Đánh số thứ tự cho toàn bộ các dòng trong tập tin.|
|cat -s|Gộp những dòng không có ký tự lại thành 1 dòng.|
|cat -E|Hiển thị dấu $ ở cuối dùng.|

### Lệnh grep

Câu lệnh này sẽ giúp người dùng có thể tìm kiếm một từ khóa hoặc một đoạn từ khóa trong một tập tin. Tương tự chức năng `Ctrl + F` nhưng thực thi bằng **CLI**.

**Cú pháp:** 

    grep <option hoặc từ khóa cần tìm> <tên file>

|Câu lệnh|Chi tiết|
|--------|--------|
|grep -i|Hiện thị kết quả tìm kiếm không phân biệt in hoa và in thường.|
|grep -n|Hiển thị kết quả tìm kiếm cùng với số thứ tự của dòng.|
|grep -v|Hiển thị kết quả những dòng không giống với cụm từ cần tìm.|
|grep -c|Hiển thị số lượng dòng trùng khớp với cụm từ cần tìm.|

Trong thực tế, `grep` thường được lồng ghép với các câu lệnh liệt kê thông tin để lọc kết quả cần hiển thị. Ví dụ như bạn cần tìm `PID` của tiến trình **python** đang hoạt động trong hệ thống, thì sẽ gõ câu lệnh như sau: `ps ux | grep python`

### Lệnh sort
Câu lệnh này sẽ sắp xếp kết quả tìm kiếm theo thứ tự alphabet hoặc theo số tăng dần. Ngoài ra, câu lệnh này cũng sẽ sắp xếp tập tin, nội dung tập tin và thư mục.

**Cú pháp:**

    sort <option> <tên file>

|Câu lệnh|Chi tiết|
|--------|--------|
|sort -r|Đảo ngược kết quả sắp xếp.|
|sort -f|Sắp xếp kết quả không phân biệt chữ hoa và thường.|
|sort -n|Sắp xếp kết quả dựa theo số tăng dần.|

### Lệnh tail

Câu lệnh này mặc định sẽ hiển thị 10 dòng cuối cùng của tập tin được chỉ định ra màn hình. Người dùng cũng có thể xem cùng lúc nội dung của nhiều tập tin.

**Cú pháp:**

    tail <option> <tên file>

|Câu lệnh|Chi tiết|
|--------|--------|
|tail -n|Hiển thị n dòng kết quả cuối cùng của tập tin.|
|tail +<số>|Hiển thị kết quả từ dòng <số>.|
|tail -c <số>|Hiển thị <số> byte cuối cùng từ tập tin được chỉ định.|
 
### Lệnh chown

Lệnh này được sử dụng để thay đổi **user** hoặc nhóm sở hữu tập tin hoặc thư mục. Bất cứ khi nào bạn muốn thay đổi quyền sở hữu, bạn có thể sử dụng lệnh `chown`.

**Cú pháp:**

    chown <option>… <OWNER><:[GROUP]> <tên file>…
    chown <option>… –reference=RFILE <tên file>…

### Lệnh chmod
Lệnh này được sử dụng để thay đổi quyền truy cập của các tập tin và thư mục.

**Cú pháp:**

    chmod <quyền của user,group,"khác"> <tên file>

**Quyền**

- 7	Đọc, ghi  và thực thi (read, write, execute).
- 6	Đọc và ghi.
- 5	Đọc và thực thi.
- 4	Chỉ đọc.
- 3	Ghi và thực thi.
- 2	Chỉ ghi.
- 1	Chỉ thực thi.
- 0	Không phân quyền.

### Lệnh lsof

**lsof** hay còn gọi là “**list open files**“, dùng để liệt kê thông tin về các tập tin trên hệ thống đang hoặc đã được mở bởi các tiến trình đang hoạt động.

**Cú pháp:**

    lsof <option> <username>

Kết quả sẽ hiển thị với các cột có các nội dung tương ứng như sau:

|Cột|Chú thích|
|---|---------|
|COMMAND|9 kí tự đầu tiên của tên chương trình lệnh tương ứng với tiến trình.|
|PID|Thông tin PID của tiến trình.|
|USER|User thực thi tiến trình đó. Có thể là UID hoặc username.|
|FD|File Descriptor của tập tin được liệt kê, hoặc các thông tin khác hay mode (w,u,r) của tập tin.|
|| cwd : là thư mục đang hoạt động của tiến trình.|
|| txt : program text (code và data).|
|| mmap : tập tin memory-mapped.|
|| rtd : root directory (thư mục root).|
|| DEL : Linux map tập tin đã bị xoá.|
|| w : đang truy cập ghi xuống dữ liệu.|
|| u : đang truy cập ghi và đọc dữ liệu.|
|| r : đang truy cập đọc dữ liệu.|
|TYPE|
 || REG : tập tin thông thường|
 || sock : socket.|
 || ipv4/ipv6 : socket IPv4/v6.|
 || DIR : thư mục.|
|DEVICE|Số đại diện của thiết bị như partition mà tập tin thuộc về.|
|SIZE/OFF|Dung lượng của tập tin.|
|NODE|Số node của tập tin.|
|NAME|Tên tập tin.|
 
**Ví dụ:**

    [root@Devops ~]# lsof
    COMMAND   PID            USER    FD      TYPE             DEVICE    SIZE/OFF     NODE NAME
    lsof      28207          root    0u      CHR              136,0       0t0          3 /dev/pts/0
    lsof      28207          root    1u      CHR              136,0       0t0          3 /dev/pts/0
    lsof      28207          root    2u      CHR              136,0       0t0          3 /dev/pts/0
    lsof      28207          root    3r      DIR                0,3         0          1 /proc
    lsof      28207          root    4r      DIR                0,3         0     231727 /proc/28207/fd
    lsof      28207          root    5w     FIFO                0,9       0t0     231732 pipe
    lsof      28207          root    6r     FIFO                0,9       0t0     231733 pipe
    lsof      28208          root  cwd       DIR                8,2      4096    2097217 /root
    lsof      28208          root  rtd       DIR                8,2       263         64 /
    lsof      28208          root  txt       REG                8,2    154184    6505411 /usr/sbin/lsof
    ...

### Lệnh ifconfig

`ifconfig` (**interface configuration**) là một trong những câu lệnh dùng để xem nhanh các địa chỉ **IP** và các thông tin khác của giao diện mạng (**interface**). Gõ ifconfig để xem trạng thái các **interface** hiện đang hoạt động, trong đó bao gồm tên, tình trạng thông tin địa chỉ IP, địa chỉ **MAC**. Bạn cũng có thể chỉ định tên một **interface** cần xem thông tin thay vì xem toàn bộ.

**Cú pháp:** 

    ifconfig <option> <interface>

|Câu lệnh|Chi tiết|
|--------|--------|
|ifconfig -a|	Liệt kê toàn bộ thông tin interface có trên hệ thống, kể cả chúng đã bị “tắt”.|
|ifconfig -s|	Liệt kê tóm tắt thông tin interface trên hệ thống|

### Lệnh id

Câu lệnh này sẽ liệt kê các thông tin (**name, group,…**) của **user** hiện tại hoặc **user** được chỉ định trên **server**.

**Cú pháp:**

    id <option> <username>

|Câu lệnh|Chi tiết|
|--------|--------|
|id -g <username>|	Liệt kê group ID của user được chỉ định.|
|id -G <username>|	Liệt kê toàn bộ group ID mà user “tham gia”.|
|id -n <username>|	Liệt kê tên của user được chỉ định thay vì ID.|
|id -r <username>|	Liệt kê real ID  của user được chỉ định.|
|id -u <username>|	Hiển thị user ID của user được chỉ định.|
 
### Lệnh cut

Câu lệnh này được sử dụng thao tác với tệp dựa trên cột và được thiết kế để trích xuất các cột cụ thể.

**Cú pháp:**

    cut -c<số thứ tự cột đầu tiên - số thứ tự cột cuối cùng> <tên file>

**Ví dụ:** Cần cắt 16 ký tự đầu tiên từ tập tin tập *tin.txt*:

    [root@DevOps ~]# cat file.txt
    Roses are red, Violets are blue,
    Sugar is sweet, And so are you.
    [root@DevOps ~]# cut -c 1-16 file.txt
    Roses are red, V
    Sugar is sweet,

### Lệnh sed

Ngoài việc lọc kết quả từ cụm từ khóa mà người dùng chỉ định, câu lệnh còn hỗ trợ người dùng thay thế các từ khóa đầu tiên xuất hiện trong mỗi dòng (hoặc tất cả) mà nó tìm thấy trong tập tin văn bản được chỉ định, và hiển thị kết quả ra màn hình.

**Cú pháp:**

    sed 's/<từ cần tìm>/<từ thay thế>/' <tên tập tin>

|Câu lệnh|Chi tiết|
|--------|--------|
|sed ‘s/<từ cần tìm>/<từ thay thế>/’ <tên tập tin>|	Thay thế chuỗi đầu tiên xuất hiện trong mỗi dòng từ tập tin được chỉ định|
|sed ‘s/<từ cần tìm>/<từ thay thế>/g’ <tên tập tin>|	Thay thế tất cả các lần xuất hiện trong mỗi dòng từ tập tin được chỉ định|
|sed ‘1,3s/<từ cần tìm>/<từ thay thế>/g’ <tên tập tin>|	Thay thế tất cả các lần xuất hiện chuỗi trong một loạt các dòng từ tập tin được chỉ định|

**Ví dụ:**

    [root@DevOps ~]# cat test.txt
    Hello Student 
    [root@DevOps ~]# sed 's/Student/STUDENT/' test.txt
    Hello STUDENT

### Lệnh diff

Câu lệnh này sẽ so sánh và hiển thị sự khác nhau giữa 2 tập tin chỉ định.

**Cú pháp:**

    diff <option> <tên file 1> <tên file 2>

Một vài tuỳ chọn (*option*) được dùng thường xuyên gồm:

|Câu lệnh|Chi tiết|
|--------|--------|
|-c|	Cung cấp 3 dòng trước và sau của sự khác nhau về nội dung.|
|-r|	Sử dụng để so sánh đệ quy các thư mục con, thư mục hiện tại.|
|-i|	Bỏ qua trường hợp chữ cái.|
|-w|	Bỏ qua sự khác biệt về tab ( khoảng trắng ).|
|-q|	Báo những tệp khác nhau mà không cần liệt kê sự khác biệt.|
 
**Ví dụ:**

    [root@DevOps ~]# cat test1.txt
    this is the original text
    line2
    line3
    line4
    happy hacking !
    [root@DevOps ~]# cat test2.txt
    this is the original text
    line2
    line4
    happy hacking !
    GNU is not UNIX
    [root@DevOps ~]#diff test1.txt test2.txt
    3d2
    < line3 
    6c5 
    > GNU is not UNIX

### Lệnh history

Câu lệnh `history` được sử dụng để xem các lệnh đã thực hiện trước đó.

**Cú pháp:**

    history <số lượng câu lệnh cần hiển thị - mặc định sẽ hiển thị toàn bộ>

Bạn cũng có thể sử dụng ký hiệu ! với lệnh `history` để lấy số thứ tự của một câu lệnh cụ thể. Ví dụ, để lấy số thứ tự của câu lệnh `ls`, bạn có thể sử dụng lệnh `!ls`.

### Lệnh dd

Câu lệnh này sử dụng trong các trường hợp sau:

- Sao lưu và phục hồi toàn bộ dữ liệu ổ cứng hoặc một partition
- Chuyển đổi định dạng dữ liệu từ ASCII sang EBCDIC hoặc ngược lại
- Sao lưu lại MBR trong máy (MBR là một tập tin dữ liệu rất quan trong nó chứa các lệnh để LILO hoặc GRUB nạp hệ điều hành)
- Chuyển đổi chữ thường sang chữ hoa và ngược lại
- Tạo một tập tin với kích cỡ cố định
- Tạo một tập tin ISO

**Cú pháp:**

    dd if=<địa chỉ đầu vào> of=<địa chỉ đầu ra> <option>

**Trong đó:**

- if= địa chỉ nguồn của dữ liệu nó sẽ bắt đầu đọc
- of= viết đầu ra của tập tin
- option : các tùy chọn cho câu lệnh

**Ý nghĩa các tham số:**

|Tùy chọn|Ý nghĩa|
|--------|--------|
|bs=Bytes|	Quá trình đọc (ghi) bao nhiêu byte một lần đọc (ghi)|
|cbs=Bytes|	Chuyển đổi bao nhiêu byte một lần|
|count=Blocks|	Thực hiện bao nhiêu Block trong quá trình thực thi câu lệnh|
|if|	Chỉ đường dẫn đọc đầu vào|
|of|	Chỉ đường dẫn ghi đầu ra|
|ibs=bytes|	Chỉ ra số byte một lần đọc|
|obs=bytes|	Chỉ ra số byte một lần ghi|
|skip=blocks|	Bỏ qua bao nhiêu block đầu vào|
|conv=Convs|	Chỉ ra tác vụ cụ thể của câu lệnh|

**Các tùy chọn (option) của conv**

|Tùy chọn|Ý nghĩa|
|--------|--------|
|ascii|	Chuyển đôi từ mã EBCDIC sáng ASCII|
|ebcdic|	Chuyển đổi từ mã ASCII sang EBCDIC|
|lcase|	Chuyển đổi từ chữ thường lên hết thành chữ in hoa|
|ucase|	Chuyển đổi từ chữ in hoa sang chữ thường|
|nocreat|	Không tạo ra tập tin đầu ra|
|noerror|	Tiếp tục sao chép dữ liệu khi đầu vào bị lỗi|
|sync|	Đồng bộ dữ liệu với ổ đang sao chép sang|

**Lưu ý**:  Mặc định nó được tính theo đơn vị là kb. Bạn có thể thêm một số trường sau để báo định dạng khác:

- c = 1 byte
- w = 2 byte
- b = 512 byte
- kB = 1000 byte
- K = 1024 byte
- MB = 1000000 byte
- M = (1024 * 1024) byte
- GB = (1000 * 1000 * 1000) byte
- G = (1024 * 1024 * 1024) byte

Ngoài ra, người dùng có thể sử dụng câu lệnh này để kiểm tra tốc độ đọc/ghi ổ cứng hiện tại của server:

**Cú pháp:**

    dd if=/dev/zero of=test bs=64k count=16k conv=fdatasync
 
### Lệnh find

Lệnh `find` được sử dụng để tìm kiếm và định vị danh sách các tập tin và thư mục dựa trên các điều kiện bạn chỉ định cho các tệp khớp với các đối số cần tìm: tên tập tin, tên thư mục, phân quyền, owner, groups,…

**Cú pháp:**

    find <vị trí con trỏ bắt đầu tìm kiếm> <đối số cần tìm> <option> <từ khóa cần tìm>

### Lệnh free

Câu lệnh này sẽ cung cấp thông tin về dung lượng bộ nhớ đã sử dụng và chưa sử dụng, kèm theo đó là thông tin về dung lượng **RAM** ảo **SWAP**.

**Cú pháp:**

    free <option>

Các tùy chọn (option) thường dùng

|Tùy chọn|Ý nghĩa|
|--------|--------|
|-b|	Hiển thị kết quả theo dung lượng byte|
|-k|	Hiển thị kết quả theo dung lượng kilobyte (mặc định)|
|-m| 	Hiển thị kết quả theo dung lượng megabyte|
|-g|	Hiển thị kết quả theo dung lượng gigabyte|

Ý nghĩa các cột trong kết quả hiển thị

|Cột|Ý nghĩa|
|--------|--------|
|total|Tổng dung lượng bộ nhớ hiện có trên thiết bị|
|used|Dung lượng bộ nhớ đang sử dụng|
|free|Dung lượng bộ nhớ còn trống|
|shared|Dung lượng bộ nhớ được sử dụng bởi tmpfs|
|buffers|Dung lượng bộ nhớ được sử dụng bởi kernel buffer|
|cache|Dung lượng bộ nhớ được sử dụng bởi page cache và slab|
|buffers/cache|Tổng dung lượng buffer và cache đang sử dụng|

### Lệnh ssh-keygen

Sử dụng lệnh `ssh-keygen` để tạo cặp khóa xác thực **public/private**, giúp người dùng kết nối với hệ thống từ xa mà không cần cung cấp mật khẩu. Các khóa phải được tạo riêng cho từng người dùng. Nếu bạn tạo các cặp khóa xác thực là với quyền user root thì chỉ user root mới có thể sử dụng các khóa đã tạo.

**Cú pháp:**

    ssh-keygen -t <option>

Các option thường sử dụng bao gồm:

- rsa1
- dsa
- ecdsa
- rsa

Để tránh việc khóa xác thực bị tin tặc (*hacker*) chiếm đoạn và sử dụng, người dùng sẽ được yêu cầu nhập mật khẩu để sử dụng khóa xác thực trong bước tạo khóa. Điều này giúp hạn chế được việc tin tặc lấy được khóa xác thực và chiếm toàn bộ quyền trên server.

### Lệnh ip
Câu lệnh này thuộc gói `net-tools` được cài đặt trên **Linux**, giúp người dùng có thể thao tác, quản trị hệ thống mạng trên thiết bị. Có chức năng gần như tương tự với câu lệnh `ifconfig`: hiển thị thông số, điều chỉnh các giao diện mạng (**interface**).

**Cú pháp:**

    ip <option> <các đối tượng mạng trên hệ thống> <COMMAND | help>

Các tùy chọn (*option*) thường dùng

|Tùy chọn|Ý nghĩa|
|--------|--------|
|address|	Hiển thị toàn bộ thông tin của các interface hiện có trên thiết bị|
|link|	Hiển thị địa chỉ MAC của các interface hiện có trên thiết bị|
|route|	Hiển thị thông tin định tuyến|

### Lệnh nslookup

`nslookup` được sử dụng để truy vấn các **Internet Domain Name Servers (DNS)**. `Nslookup` có hai chế độ: tương tác (**interactive**) và không tương tác (**non-interactive**). Chế độ tương tác cho phép người dùng truy vấn các tên máy chủ để biết thông tin về các máy chủ và tên miền khác nhau hoặc để in danh sách các máy chủ trong một miền. Chế độ không tương tác được sử dụng để chỉ in tên và thông tin được yêu cầu cho máy chủ lưu trữ hoặc tên miền.

**Cú pháp:**

    nsloopkup <đường dẫn cần kiểm tra>

**Ví dụ:**

    root@vinahost:~/devops# nslookup
    > google.com
    Server:         127.0.0.53
    Address:        127.0.0.53#53

    Non-authoritative answer:
    Name:   google.com
    Address: 74.125.68.102
    Name:   google.com
    Address: 74.125.68.100
    Name:   google.com
    Address: 74.125.68.101
    Name:   google.com
    Address: 74.125.68.139
    Name:   google.com
    Address: 74.125.68.138
    Name:   google.com
    Address: 74.125.68.113
    Name:   google.com
    Address: 2404:6800:4003:c06::66
    Name:   google.com
    Address: 2404:6800:4003:c06::8b
    Name:   google.com
    Address: 2404:6800:4003:c06::65
    Name:   google.com
    Address: 2404:6800:4003:c06::64
    >
Để cài trên **Red Hat Linux**/ **CentOS** sử dụng lệnh

    dnf install bind-utils

hoặc

    dnf install bind-utils

Để cài trên **Debian/Ubuntu** sử dụng lệnh

    apt install dnsutils

### Lệnh curl

Một trong những điều cơ bản nhất bạn có thể làm với `curl` là tải xuống một trang web hoặc tập tin.

**Cú pháp:**

    curl <option> <đường dẫn / URL>

Nếu bạn muốn lưu kết quả sau khi “curl” được thì chỉ cần thêm option “-o <tên tập tin>” vào trước URL để lưu kết quả vào tập tin mong muốn. Ví dụ:

    curl -o hello.zip ftp://speedtest.tele2.net/1MB.zip

### Lệnh tr

Câu lệnh này được sử dụng trên hệ điều hành **Linux** giúp người dùng có thể thay đổi hoặc xóa các ký tự.

**Cú pháp:**

    tr <option> <set1> <set2>

**Ví dụ:** Bạn cần thay đổi các ký tự trong tập tin từ chữ thường sang chữ hoa

    [root@DevOps ~]# cat test.txt
    devops
    [root@DevOps ~]# cat test.txt | tr [:lower:] [:upper:]
    DEVOPS

Các tùy chọn (*option*) thường dùng

|Tùy chọn|Ý nghĩa|
|--------|--------|
|-c|	Áp dụng các option khác đối với các ký tự không có trong bộ lọc “set1”|
|-d|	Xóa ký tự trong chuỗi|
|-s|	Bỏ các ký tự lặp liên tiếp|
 
### Lệnh iptables

`iptables` là ứng dụng tường lửa miễn phí trong **Linux**, cho phép thiết lập các quy tắc riêng để kiểm soát truy cập, tăng tính bảo mật cho hệ thống. Về cơ bản, `iptables` chỉ là giao diện dòng lệnh để tương tác với **packet filtering** của **netfilter framework**. Trong đó:

- Table: được iptables sử dụng để định nghĩa các rule(quy tắc) dành cho các gói tin.
- Chain: được tạo ra với một số lượng nhất định ứng với mỗi Table, giúp lọc gói tin tại các điểm khác nhau.
- Rule: các điều kiện ứng với các loại gói tin.
- Tagert: có thể được hiểu là hành động dành cho các gói tin khi gói tin thỏa mãn các rule đã quy định.
- Policy: cơ chế mặc định đối với các trường hợp gói tin không trùng khớp với các Chain hiện có.

**Cú pháp:**

    iptables --table TABLE -A/-C/-D... CHAIN rule --jump Target

### Lệnh apt-get

`apt-get` là một công cụ **command-line** giúp xử lý các gói cài đặt trong **Debian/Ubuntu**. Nhiệm vụ chính của nó là lấy thông tin và các gói từ các nguồn được xác thực để cài đặt, nâng cấp và loại bỏ các gói cài đặt và các thành phần kèm theo của chúng. **APT** là viết tắt của **Advanced Packaging Tool**.

    apt-get <option> command

`apt-get update`: lệnh này được sử dụng để đồng bộ hóa các tệp chỉ mục gói cài đặt từ các nguồn của chúng một lần nữa. Người dùng cần thực hiện cập nhật trước khi nâng cấp các gói cài đặt trên hệ thống của mình.

### Lệnh df, du

Lệnh `df` (**disk free**) hiển thị tình trạng dung lượng ổ đĩa trống đang được sử dụng bởi các tập tin của hệ thống. Lệnh `du` (**disk usage**) tình trạng dung lượng của cây thư mục bao gồm tất cả nội dung của chúng và kích thước của các tệp riêng lẻ. Công cụ này giúp người dùng có thể quản lý được tài nguyên đang sử dụng trên hệ thống, giảm tình thiểu tình trạng hệ thống thiếu hụt tài nguyên.

**Cú pháp:**

    df -h 
    du -h

### Lệnh htop

Đây là công cụ giúp người dùng có thể theo các tiến trình đang hoạt động trên hệ thống, tổng quan các tài nguyên của hệ thống như **CPU**, lượng **RAM** và **Swap** sử dụng theo thời gian thực (**real-time**).

**Cú pháp:**

    htop <option>

Các tùy chọn (*option*) thường dùng:

|Tùy chọn|Ý nghĩa|
|--------|--------|
| –d / –delay|	Hiển thị độ trễ giữa các bản cập nhật, tính bằng 1/10 giây|
| –C / –no-color|	Chế độ đơn sắc|
| –h / –help|	Hiển thị thông báo trợ giúp và thoát|
| –u / –user=USERNAME|	Chỉ hiển thị các PID đã chỉ định|
| –v / –version|	Hiển thị thông tin phiên bản và thoát|
 
### Lệnh ps

Câu lệnh này sẽ hiển thị danh sách các tiến trình đang hoạt động trên hệ thống.

**Cú pháp:**

    ps <option>

Các tùy chọn (*option*) thường dùng:

|Tùy chọn|Ý nghĩa|
|--------|--------|
|a|	Hiển thị tất cả các tiến trình của toàn bộ user trên hệ thống|
|u|	Hiển thị người khởi tạo tiến trình|
|x|	Hiển thị các tiến trình không attach terminal|
 
### Lệnh kill

Lệnh `kill` có thể được dùng để ngắt một hoặc nhiều tiến trình đang hoạt động trên hệ thống.

**Cú pháp:**

    kill <option> <ID của tiến trình>

### Lệnh telnet

`Telnet` là một giao thức mạng (**network protocol**) và cũng là công cụ quản trị server từ xa thông qua **command line**. **TELNET** (viết tắt của **TErminaL NETwork**) được dùng phổ biến trong mạng máy tính cục bộ (**LAN**). Ngoài ra, bạn có thể sử dụng câu lệnh này để kiểm tra **TCP** port của server hiện có đang mở hay không.

Cú pháp: 

    telnet <hostname / địa chỉ ip>

## Một số lệnh của các phần mềm trên Linux

- pip install: Tải xuống và cài đặt một gói phần mềm Python.
- npm install: Tải xuống và cài đặt một gói phần mềm JavaScript.
- composer install: Tải xuống và cài đặt một gói phần mềm PHP.
- gradlew build: Xây dựng một dự án Gradle.
- maven install: Xây dựng một dự án Maven.
- sbt compile: Xây dựng một dự án SBT.

## Một số lệnh dành cho Docker

- docker run: Chạy một container Docker.
- docker build: Xây dựng một hình ảnh Docker từ một tệp Dockerfile.
- docker push: Đẩy một hình ảnh Docker lên kho lưu trữ Docker.
- docker pull: Kéo một hình ảnh Docker từ kho lưu trữ Docker.
- docker stop: Dừng một container Docker đang chạy.
- docker start: Khởi động lại một container Docker đã dừng.
- docker rm: Xóa một container Docker.
- docker images: Liệt kê tất cả các hình ảnh Docker.
- docker ps: Liệt kê tất cả các container Docker đang chạy.
- docker logs: Xem nhật ký của một container Docker.
- docker network ls: Liệt kê tất cả các mạng Docker.
- docker volume ls: Liệt kê tất cả các khối lượng Docker.
- docker compose up: Chạy một ứng dụng Docker từ tệp docker-compose.yml.

## Một số lệnh dành cho Kubernetes

- kubectl create deployment: Tạo một triển khai Kubernetes.
- kubectl scale deployment: Thay đổi quy mô một triển khai Kubernetes.
- kubectl rollout restart deployment: Khởi động lại một triển khai Kubernetes.
- kubectl get pods: Lấy danh sách các pod Kubernetes.
- kubectl logs pod: Lấy nhật ký của một pod Kubernetes.
- kubectl describe pod: Mô tả chi tiết về một pod Kubernetes.
- kubectl get services: Lấy danh sách các dịch vụ Kubernetes.
- kubectl expose deployment: Tạo một dịch vụ cho một triển khai Kubernetes.
- kubectl get nodes: Lấy danh sách các nút Kubernetes.
- kubectl get nodes -o wide: Lấy danh sách các nút Kubernetes với thông tin chi tiết.
- kubectl get cluster-autoscaler: Lấy trạng thái của trình tự mở rộng cụm Kubernetes.
- kubectl get pods --all-namespaces: Lấy danh sách tất cả các pod Kubernetes trong tất cả các không gian tên.
- kubectl logs pod --all-namespaces: Lấy nhật ký của tất cả các pod Kubernetes trong tất cả các không gian tên.
- kubectl describe pod --all-namespaces: Mô tả chi tiết về tất cả các pod Kubernetes trong tất cả các không gian tên.
- kubectl get services --all-namespaces: Lấy danh sách tất cả các dịch vụ Kubernetes trong tất cả các không gian tên.
- kubectl expose deployment --all-namespaces: Tạo một dịch vụ cho một triển khai Kubernetes trong tất cả các không gian tên.
- kubectl get nodes --all-namespaces: Lấy danh sách tất cả các nút Kubernetes trong tất cả các không gian tên.
- kubectl get nodes --all-namespaces -o wide: Lấy danh sách tất cả các nút Kubernetes trong tất cả các không gian tên với thông tin chi tiết.
- kubectl get cluster-autoscaler --all-namespaces: Lấy trạng thái của trình tự mở rộng cụm Kubernetes trong tất cả các không gian tên.

Trường hợp cần thông tin chi tiết, các bạn có thể sử dụng lệnh *man* để có thêm thông tin của các lệnh tương ứng, cấu trúc lệnh man <command>

    man ls

Đây chỉ là một số lệnh Linux phổ biến được sử dụng trong DevOps. Có nhiều lệnh khác có thể được sử dụng, tùy thuộc vào các nhiệm vụ cụ thể mà bạn đang thực hiện.

## Tham khảo

- https://github.com/trinib/Linux-Bash-Commands
- https://github.com/sk3pp3r/cheat-sheet-pdf