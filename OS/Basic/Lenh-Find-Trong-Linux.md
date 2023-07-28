# Câu lệnh Find trong Linux

Câu lệnh `find` là một công cụ mạnh mẽ được sử dụng để tìm kiếm các tệp và thư mục trong hệ thống **Linux**. Nó có thể được sử dụng để tìm kiếm các tệp dựa trên tên, ngày tạo, quyền và các thuộc tính khác. Câu lệnh `find` cũng có thể được sử dụng để thực hiện các hành động đối với các tệp, chẳng hạn như xóa, sao chép và di chuyển.

Trong **DevOps**, câu lệnh `find` được sử dụng để tìm kiếm các tệp cần được xử lý trong quá trình xây dựng hoặc triển khai. Nó cũng có thể được sử dụng để tìm kiếm các tệp cần được xóa hoặc sao lưu.

## Tìm kiếm theo tên file, thư mục
 
###  Tìm file có trong thư mục hiện tại

Câu lệnh tìm file có tên là *config.txt* bằng lệnh `find` như sau:

    find . -name config.txt
    ./config.txt

Dấu chấm tương ứng với vị trí đang đứng

### Tìm file có trong thư mục bất kì
 

Câu lệnh tìm file có tên *config.txt* có trong thư mục */home* theo tên như sau:

    find /home -name config.txt
    /home/config.txt
 
### Tìm kiếm file theo định dạng tên

Tìm tất cả các tệp có tên là *config.txt* và chứa cả chữ hoa và chữ thường trong thư mục */home*.

    find /home -iname config.txt
    /home/config.txt
    /home/data/CONFIG.txt

### Tìm kiếm thư mục theo tên

Tìm thư mục có tên devops có trong thư mục /

    find / -type d -name devops
    /home/devops
    /usr/src/devops
    ...

### Tìm file theo định dạng file
 

Tìm kiếm tất cả các file có định dạng *.php* trong thư mục */home*

    find /home -type f -name "*.php"
    /home/data/index.php
    /home/data/login.php
    /home/data/data.php
 

## Tìm kiếm dựa trên phân quyền

### Tìm tất cả các file có quyền 700

Để tìm tất cả các file có quyền *700* trong thư mục */var/www/html* ta dùng câu lệnh sau:

    find /var/www/html -type f -perm 0700 -print
    /var/www/html/login.php

### Tìm tất cả các file không có quyền 700

Để tìm tất cả các file không có quyền *700* trong thư mục */var/www/html* ta dùng câu lệnh sau:

    find /var/www/html -type f ! -perm 0700 -print
    /var/www/html/index.php
 

### Tìm File SGID có quyền 644
 
Tìm tất cả các file SGID có quyền 644

    find /home -perm 2644

### Tìm file SUID

Tìm tất cả các file *SUID* có trong thư mục */home*

    find /home -perm /u=s
 
### Tìm file SGID

Tìm tất cả các file *SGID* có trong thư mục */home*

    find /home -perm /g=s

### Tìm file chỉ có quyền đọc (readonly)

Tìm tất cả các file chỉ có quyền đọc (*readonly*) có trong thư mục */home*

    find /home -perm /u=r

### Tìm file có quyền thực thi

Tìm tất cả các file có quyền thực thi có trong thư mục */home*

    find /home -perm /a=x

### Tìm file có quyền 777 và đổi quyền thành 644

Tìm tất cả các file có quyền 777 và sau đó đổi thành 644 có trong thư mục */home*

    find /home -type f -perm 0777 -print -exec chmod 644 {} \;

### Tìm và xóa một file

Tìm và xóa một file có tên *devops.txt* có trong thư mục */home*

    find /home -type f -name "tel4vn.txt" -exec rm -f {} \;

### Tìm và xóa nhiều file

Tìm và xóa nhiều file có định dạng *.txt* trong thư mục */home*

    find /home -type f -name "*.txt" -exec rm -f {} \;

**Chú ý:** những lệnh thế này hơi nguy hiểm cho những bạn mới làm quen với **Linux**. Nếu bạn gõ sai đường dẫn **/home** thành đường dẫn khác thì sẽ xóa nhầm file. Hoặc xóa tất cả các file dẫn đến lỗi server. Một lời khuyên cho bạn là nên **backup** dữ liệu hằng ngày để tránh những sai sót nghiêm trọng.

### Tìm tất cả những file trống 

Tìm tất cả những file không chứa dữ liệu có trong thư mục */home*

    find /home -type f -empty

### Tìm tất cả những thư mục trống

Tìm tất cả những thu mục trống có trong server của bạn bằng lệnh sau:

    find / -type d -empty

### Tìm tất cả những file ẩn

Tìm tất cả những file ẩn có trong thư mục */home*. File ẩn thường là những file có tên bằng đầu bằng dấu chấm “.”

    find /home -type f -name ".*"

## Tìm kiếm theo Owners và Groups

### Tìm file với user
 
Tìm file có tên là *devops.txt* thuộc sở hữu của user **root** trong thư mục */home*

    find / -user root -name devops.txt
 
### Tìm nhiều file với user

Tìm tất cả file thuộc sở hữu của user **root** trong thư mục */home*

    find /home -user root

### Tìm nhiều file với group

Tìm tất cả các file thuộc sở hữu của *group dev* nằm trong thư mục */home*

    find /home -group dev

### Tìm file theo định dạng, user, thư mục

Tìm file có định dạng là *.txt* có thuộc user root trong thư mục */home*

    find /home -user root -iname "*.txt"

## Tìm kiếm file và thư mục theo thời gian
 
### Tìm file được chỉnh sửa thời gian gần nhất

    find /home -newermt '1 day ago'

### Tìm tất cả các file được chỉnh sửa trong thời gian 7 ngày gần nhất. 

    find /home -mtime 7
 
### Tìm tất cả các file được truy cập theo thời gian

Tìm tất cả các file được truy cập trong thời gian 7 ngày gần nhất. 

    find /home -atime 7

### Tìm tất cả file được chỉnh sửa trong mốc thời gian

Tìm tất cả các file được chỉnh sửa trong khoảng 1 tháng trước.

    find /home -mtime +30 -mtime -60

### Tìm file được chỉnh sửa theo giờ

Tìm tất cả các file được chỉnh sửa trong khoảng 2 tiếng trước.

    find /home -mmin -120

## Tìm file theo dung lượng 

### Tìm tất cả các file có dung lượng lớn hơn *10M* trong thư mục */home*

    find /home -size +10M

### Tìm và xóa tất cả các file có dung lượng lớn hơn 50M trong thư mục /home

    find /home -size +50M -exec rm -rf {} \;

### Tìm xóa file theo định dạng và dung lượng

Tìm file có định dạng mp3 có dung lượng lớn hơn 10M sau đó xóa 

    find /home -type f -name *.mp3 -size +10M -exec rm {} \;

Bài viết đã hướng dẫn cho bạn một số câu lệnh `find` thường dùng. Bạn có thể kết hợp các lệnh tùy vào mục đích của bạn. 