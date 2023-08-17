# Docker nâng cao

**Bài viết trước chúng ta đã tìm hiểu:**

- Docker là gì
- Ưu điểm của Docker
- Các thành phần chính của Docker
- Sự khác biệt giữa Docker Images và Docker Containers
- Cài đặt Docker
- Quy trình thực thi của một hệ thống sử dụng Docker
- Các lệnh cơ bản trong docker

## Docker nâng cao

### Dockerfile

![Dockerfile](/Image/Dockerfile.png)

**Dockerfile** là **file config** cho **Docker** để **build** ra **image**. Nó dùng một **image** cơ bản để xây dựng lớp **image** ban đầu. Một số **image** cơ bản: **python**, **unbutu** và **alpine**. Sau đó nếu có các lớp bổ sung thì nó được xếp chồng lên lớp cơ bản. Cuối cùng một lớp mỏng có thể được xếp chồng lên nhau trên các lớp khác trước đó.

### Các config

- FROM — chỉ định image gốc: python, unbutu, alpine…
- LABEL — cung cấp metadata cho image. Có thể sử dụng để add thông tin maintainer. Để xem các label của images, dùng lệnh docker inspect.
- ENV — thiết lập một biến môi trường.
- RUN — Có thể tạo một lệnh khi build image. Được sử dụng để cài đặt các package vào container.
- COPY — Sao chép các file và thư mục vào container.
- ADD — Sao chép các file và thư mục vào container.
- CMD — Cung cấp một lệnh và đối số cho container thực thi. Các tham số có thể được ghi đè và chỉ có một CMD.
- WORKDIR — Thiết lập thư mục đang làm việc cho các chỉ thị khác như: RUN, CMD, ENTRYPOINT, COPY, ADD,…
- ARG — Định nghĩa giá trị biến được dùng trong lúc build image.
- ENTRYPOINT — cung cấp lệnh và đối số cho một container thực thi.
- EXPOSE — khai báo port lắng nghe của image.
- VOLUME — tạo một điểm gắn thư mục để truy cập và lưu trữ data.

### Demo tạo file Dockerfile

Một số bước thực hiện:

- Đầu tiên chúng ta sẽ viết Dockerfile để tạo nên image rồi tạo nên container, sau khi tạo được container rồi thì đồng nghĩa là đã tạo ra được máy ảo để bạn có thể khởi chạy ứng dụng của bạn trên máy ảo đó.
- Thư mục webroot chứa mã nguồn chương trình, có thể là một c++ app, java app hoặc web app được viết bằng php hoặc ruby,.... (Ở đây, để cho đơn giản, chúng ta chỉ đặt file hello.html, chạy trên trình duyệt sẽ hiển thị dòng Hello Word)
- Sau này, bạn dùng editor để lập trình trên máy thật, chỉnh sửa mã nguồn trong thư mục này, mọi sự thay đổi được cập nhật ngay lập tức trên máy ảo.
- File start.sh chứa những câu lệnh được chạy khi bật container (có thể dùng để start mysql, nginx, redis ...)  

Nội dung file *Dockerfile*

```js
FROM ubuntu:22.04

MAINTAINER HauTran<hautph@gmail.com>

RUN DEBIAN_FRONTEND=noninteractive

RUN apt-get update

RUN apt-get install -y nginx

RUN echo "mysql-server mysql-server/root_password password root" | debconf-set-selections \
    && echo "mysql-server mysql-server/root_password_again password root" | debconf-set-selections \
    && apt-get install -y mysql-server

WORKDIR /venv

COPY start.sh /venv

RUN chmod a+x /venv/*

ENTRYPOINT ["/venv/start.sh"]

EXPOSE 80

```  
Tạo file `hello.html` trong thư mục **webroot**:

```js
<h1>Hello word</h1>
```  
Tạo file `start.sh` như sau

```js
#!/bin/bash
service nginx start
exec $@

```  

Tiến hành **build** file **Dockerfile**

```js
$ sudo docker build -t ubuntu-nginx . 

```  

Tạo **container** từ **image**:

```js
sudo docker run -v <forder_in_computer>:<forder_in_container> -p <port_in_computer>:<port_in_container> -it <image_name> /bin/bash

```  
**Trong đó:**

- -v : Thể hiện việc mount volume, dữ liệu từ thư mục từ máy thật có thể được truy cập từ thư mục của máy ảo.
- -p: Cổng mạng từ máy thật để dẫn tới cổng mạng của máy ảo đang chạy.
- -t: Chạy container và mở terminal bằng /bin/bash

Ví dụ vào **localhost** mặc định của **nginx**:  

```js
sudo docker run -p 9000:80 -it ubuntu-nginx /bin/bash

```  
hoặc

```js
sudo docker run -v  /home/webroot:/var/www/html -p 9000:80 -it ubuntu-nginx /bin/bash

```  

### Các lệnh thường dùng

Build **container** từ **image**

```js
$ docker run --name {container_name} -it {image_id/name:tag} /bin/bash

```  

**Một số option**:

- -it để cho phép container vừa tạo ra có thể nhận tương tác (-i), và kết nối với terminal (-t).
- --name container_name của container mà bạn muốn sau khi khởi chạy. Mặc định khi không có thì nó sẽ tự đặt tên cho container là 1 tên nào đó, nên tốt nhất mình tự đặt cho dễ nhớ
- -p {host_port}:{container_port} , -h {container_host} optional
- Còn nhiều option khác, các bạn có thể tìm hiểu thêm

`start/stop` một **container** khởi chạy **container** đã **build**

```js
docker start {container_id/name} # Khởi động lại container
docker attach {container_id/name} # access vào container đang start
docker exec -it {container_id/name} /bin/bash #khởi động container và access vào container đó luôn
docker start {container_id/name}
```  

**Note:**

- Có thể dùng docker exec để đứng từ host và chỉ định cho container thực thi lệnh.
- Vd: Đứng từ host, và liệt kê tất cả các file có trong container_1 thì ta có lệnh: 

```js
docker exec container_1 ls  
``` 

Thoát khỏi **container** đang **access**

```js
exit  # thoát và stop luôn container  
ctrl + d # thoát và stop luôn container 
ctrl + p, ctrl + q # dùng cả 2 tỏ hợp phím này để thoát contraner mà vẫn giữ cho container chạy

```  
Liệt kê các **container**

```js
docker ps # Liệt kê các container đang chaỵ
docker ps -a #Liệt kê tất các container bao gồm container đã tắt
```  
Xóa một **container**

```js
docker rm -f {container_id/name}
```  

Đổi tên một **container**

```js
docker rename {old_container_name} {new_container_name}
```  

Xem các thay đổi trên **container**

```js
docker diff {container_name}

```  

Lưu **container** thành **image**: trong trường hợp bạn muốn lưu **container** của bạn thành **image** để thuận tiện **share** cho người khác hoặc đem đi cài trên máy khác thì bạn dùng lệnh sau

```js
docker commit {container_id/name} {image_name}:{tag}

```  

**Note:**

- Phải stop container trước khi bạn lưu.
- {image_name}:{tag} đặt tên image và version cho container sau khi lưu.  

Lưu **image** thành **file** để tiện **share**

```js
docker save --output filename.tar {image_id/name}

```  
**Note:**

- filename.tar sẽ được lưu ở vị trí bạn đang đứng trong terminal  

**Load image** từ **file** ra để sử dụng

```js
docker load -i filename.tar

```  

**Note:**

- khi load image từ file thì image_name và image_tag sẽ là rỗng để đặt tên và tag cho image ta dùng lệnh  

```js
docker tag {image_id} {new_name:new_version}

```  

### Data Volume

**Data volume** dùng để chia sẻ dữ liệu, thông thường ta sẽ dùng cho những trường hợp sau:

- Để giữ lai dữ liệu của container
- Để chia sẻ dữ liệu giữa host và doker container
- Để chia sẻ dữ liệu giữa các container khác nhau

Chia sẻ dữ liệu thông qua thư mục

*Giữa host và container*

```js
docker run -it -v {host path}:{container path} --name {container name} {image_id/name}

docker run -it -v /home/hautp/share_data:/home/share_data --name C1 ubuntu:22.04 # vd minh họa

```  

*Giữa containter với nhau:*

Để tạo ra một **container** có tên là **C2** và cũng cùng chia sẻ giữ liệu của **C1** (*/home/share_data*) ta dùng lệnh

```js
docker run -it --name C2 --volumes-from C1 ubuntu:22.04

```  
Chia sẻ dữ liệu thông qua ổ đĩa

*Kiểm tra ổ đĩa hiện có*
```js
docker volume ls

```  

*Tạo mới ổ đĩa Volume*

```js
docker volume create {volume name}

```  
*Kiểm tra thông tin ổ đĩa Volume*

```js
docker volume inspect {volume name}

```  

*Xóa ổ đĩa Volume*
```js
docker volume rm {volume name}

```  
*Chia sẻ dữ liệu thông qua ổ đĩa Volume*
```js
docker run -it --name C1 --mount source=Disk_1,target=/home/disk_1 ubuntu:22.04

```  

**Note**:

- --mount tham số để gán ổ đĩa vào
- source=Disk_1 tên ổ đĩa mà mình muốn gán
- target=/home/disk_1 vị trí mà ổ đĩa Disk_1 ánh xạ vào thư mục /home/disk_1 của container  

Chia sẻ dữ liệu ổ đĩa **Volume** với **host** và **containter**

*Tạo ổ đĩa ánh xạ với host*
```js
docker volume create --opt device={host path} --opt type=none --opt o=bind {volume name}

```  
*Gán ổ đĩa cho container*

```js
docker run -it --name C2 -v Disk_2:/home/disk_2 ubuntu:22.04

```  
> Note: Khi đã ánh xạ ổ đĩa với host thì mình sẽ ko sử dụng --mount mà dùng

## Khái niệm và các default Docker Network

### Docker network là gì?

**Docker network** là nơi sẽ đảm nhiệm nhiệm vụ cho **container** kết nối vào **network**

- Các container cùng một network có thể liên lạc với nhau qua tên của container và cổng (port) được lắng nghe của container trên mạng đó

- Kết nối trên 1 host hoặc nhiều host,

- Kết nối giữa các cụm (swarm) docker containers.

- Kết nối container với các mạng khác nằm ngoài docker.

- Có thể cung cấp hầu hết các chức năng mà một hệ thống mạng bình thường cần có.

### Các loại docker network

Có 3 loại **networks** được tự động tạo ra trong **docker** là **bridge**, **none**, **host** ta có thể xem bằng lệnh

```js
docker network ls

```  

- bridge đây là driver mạng mặc định của Docker, bridge là driver phù hợp nhất cho việc giao tiếp các containers độc lập. các container trong cùng mạng có thể giao tiếp với nhau qua địa chỉ IP, nếu không chỉ định driver thì bridge sẽ là driver mạng mặc định khi khởi tạo.

- none driver cung cấp cho một container networking stack và không gian mạng riêng của nó, thường được dùng với mạng tùy chỉnh, driver này không thể dùng trong cụm swarm.

- host dùng khi container cần giao tiếp với host và sử dụng trực tiếp mạng của máy chủ đang chạy

### Làm việc với **docker network**

*Tạo network*
```js
docker network create --driver bridge network1

```  
Lệnh trên tạo ra mạng **network1** ta kiểm tra bằng lệnh

```js
docker network ls

```  
*Xóa network*

```js
docker network rm name_network

```  
*Tạo container kết nối vào một network được chỉ định*

```js
docker run -it --name B4 --network network1 busybox

```  

Lệnh trên ta tạo một **container** tên là **B4** từ **image busybox** và có kết nối trong mạng **network1** mới tạo ở trên

*kiểm tra bằng lệnh*

    docker network inspect network1
 
chúng ta sẽ thấy có **container B4** kết nối

### Kết nối trong docker network

Kết nối một **container** đang chạy với một mạng khác

Ví dụ: ta có 2 **network** là **network1** và **network2** có một **container B5** đang kết nối với mạng **network1** và ta muốn **container** này kết nối với cả **network2** thì ta chạy lệnh

    docker network connect network2 B5

Lệnh trên là kết nối **container B5** vào mạng **network2**

## Docker Hub là gì?

**Docker Hub** là một dịch vụ do **Docker** cung cấp, cho phép tìm kiếm và chia sẻ các **container images**. Các tính năng chính của **Docker Hub** là:

- Repositories: Push và pull container images.
- Teams & Organizations: Quản lý quyền truy cập vào private repositories của container images.
- Official Images: Pull sử dụng container images chất lượng cao của Docker.
- Publisher Images: Pull và sử dụng container images được cung cấp bởi vendors khác.
- Builds: Tự động tạo container images từ GitHub và Bitbucket. Push chúng lên Docker Hub.
- Webhooks: Kích hoạt các actions sau khi push thành công một repository lên Docker Hub với các dịch vụ khác.

Để sử dụng **Docker Hub**, bạn hãy đăng ký một tài khoản [tại đây](https://hub.docker.com/).

## Docker compose là gì ?

Là công cụ giúp ta thiết lập và quản lý nhiều **container**, **network**, **volume** (gọi chung là các **service**) và thiết lập cấu hình cho các **service** một cách nhanh chóng và đơn giản bằng việc chạy theo các chỉ định trong file `docker-compose.yml`

*Những tính năng chính của Compose bao gồm:*

- Tạo và quản lý nhiều môi trường độc lập trong một máy host đảm bảo độc lập các phân vùng ổ nhớ tránh say ra xung đột
- Chỉ tạo lại container thay đổi, nhận biết các container không thay đổi và sử dụng lại
- Định nghĩa và sử dụng biến môi trường trong file YAML

*Docker-compose.yml*

Là một file lưu dạng **yaml**, file này lưu các chỉ thị để **docker compose** đọc file này và thực thi các chỉ thị đó, các chỉ thị như tạo **container** từ **image**, tạo **network**, cấu hình cho các dịch vụ. VD: file `docker-compose.yml` như sau

```js
version: "3" #là phiên bản docker composer

# Tạo mạng tên là my-network
networks:
    my-network:
        driver: bridge

# Tạo các dịch vụ (container)
services:

    # Tạo container my-php từ imgae php:latest có kết nối với mạng my-network
    my-php:
        container_name: php-product
        image: 'php:latest'
        hostname: php
        restart: always
        networks:
            - my-network

    # Tạo container my-httpd từ imgae httpd:latest có kết nối với mạng my-network, ánh xạ cổng 9999 của máy host vào cổng 80
    my-httpd:
        container_name: c-httpd01
        image: 'httpd:latest'
        hostname: httpd
        restart: always
        networks:
            - my-network
        ports:
            - "9999:80"
            - "443:443"
            
     # Tạo container my-mysql từ imgae mysql:latest có kết nối với mạng my-network,config các biến môi trường
    my-mysql:
        container_name: myql-product
        image: "mysql:latest"
        hostname: mysql
        restart: always
        networks:
            - my-network
        environment:
            - MYSQL_ROOT_PASSWORD=123abc
            - MYSQL_DATABASE=db_site
            - MYSQL_USER=sites
            - MYSQL_PASSWORD=123abc

```  
Vào thư mục chứa file `docker-compose.yml` và chạy lệnh

```js
docker-compose up

```  
Vậy là xong rồi, giờ ta bật một **terminal** khác để kiểm tra xem đã có các **container** và **network** theo như mục tiêu đề ra hay chưa

Chạy lệnh *docker ps* và *docker network ls* để xem danh sách **container** đang chạy và **network**

Muốn dừng các **services** đang chạy thì ta dùng lệnh

```js
docker-compose stop

```  
Để kết thúc các **services** đang chạy và xóa hoàn toàn **container** ta dùng lệnh

```js
docker-compose down

```  
Theo dõi **Logs** các **services**

```js
docker-compose logs [SERVICES]
```  

## Tài liệu tham khảo

- https://docs.docker.com/engine/reference/builder/
- https://docs.docker.com/docker-hub/