# Docker là gì ?

**Docker** là một nền tảng mở dành cho các lập trình viên, quản trị hệ thống dùng để xây dựng, vận chuyển và chạy các ứng dụng phân tán. Docker giúp đóng gói phần mềm vào các đơn vị tiêu chuẩn hóa và được gọi là container. Ban đầu viết bằng Python, hiện tại đã chuyển sang Golang.

![Docker là gì](/Image/Docker-la-gi.png)

### Ưu điểm của Docker

- Tiện lợi: Bình thường khi cần chạy ứng dụng chúng ta cần cài đầy đủ môi trường trên máy tính, chưa kể có sự xung đột, sự cố xảy ra với các ứng dụng. Với Docker, bạn có thể đóng gói tất cả các thành phần của ứng dụng vào một container và chạy nó trên bất kỳ máy tính nào mà không cần phải cài đặt lại môi trường.

- Dễ dàng sử dụng: Docker rất dễ cho mọi người sử dụng từ developers, systems admins, architects… nó tận dụng lợi thế của container để build, test nhanh chóng. Có thể đóng gói ứng dụng trên laptop của họ và chạy trên public cloud, private cloud… Câu thần chú là “Build once, run anywhere”.

- Tốc độ: Docker Containers tương đối nhẹ và có tốc độ rất nhanh. Bạn hoàn toàn có thể tạo và khởi chạy chỉ trong vài giây.

- Linh hoạt: Triển khai ở bất kỳ nơi đâu do sự phụ thuộc của ứng dụng vào tầng OS cũng như cơ sở hạ tầng được loại bỏ.
- Môi trường chạy và khả năng mở rộng: Bạn có thể chia nhỏ những chức năng của ứng dụng thành các container riêng lẻ. Ví dụng Database chạy trên một container và Redis cache có thể chạy trên một container khác trong khi ứng dụng Node.js lại chạy trên một cái khác nữa. Với Docker, rất dễ để liên kết các container với nhau để tạo thành một ứng dụng, làm cho nó dễ dàng scale, update các thành phần độc lập với nhau.

### Các thành phần chính của Docker

- Docker Engine: dùng để tạo ra Docker image và chạy Docker container. Là thành phần chính của Docker, như một công cụ để đóng gói ứng dụng.
- Docker Hub: dịch vụ lưu trữ giúp chứa các Docker image. Trên DockerHub có hàng ngàn public images được tạo bởi cộng đồng cho phép bạn dễ dàng tìm thấy những image mà bạn cần. Và chỉ cần pull về và sử dụng với một số config mà bạn mong muốn.
- Docker Image: một dạng tập hợp các tệp của ứng dụng, được tạo ra bởi Docker engine. Nội dung của các Docker image sẽ không bị thay đổi khi di chuyển. Docker image được dùng để chạy các Docker container. Bạn có thể tự build một image riêng cho mình hoặc sử dụng những image được chia sẽ từ cộng đồng Docker Hub. Một image sẽ được build dựa trên những chỉ dẫn của Dockerfile.
- Docker Container: một dạng runtime của các Docker image, dùng để làm môi trường chạy ứng dụng. Bạn có thể create, start, stop, move or delete container dựa trên Docker API hoặc Docker CLI.
- Dockerfile: là một tập tin bao gồm các chỉ dẫn để build một image.
- Volumes: là phần dữ liệu được tạo ra khi container được khởi tạo.
- Docker Registry: là nơi lưu trữ riêng của Docker Images. Images được push vào registry và client sẽ pull images từ registry. Có thể sử dụng registry của riêng bạn hoặc registry của nhà cung cấp như : Alibaba Cloud, AWS, Google Cloud, Microsoft Azure...Docker Networking: cho phép kết nối các container lại với nhau. Kết nối này có thể trên 1 host hoặc nhiều host.
- Docker Repository: là tập hợp các Docker Images cùng tên nhưng khác tags. VD: golang:1.11-alpine.
- Docker Machine: tạo ra các Docker engine trên máy chủ.
- Docker Client: là một công cụ giúp người dùng giao tiếp với Docker host thông qua command.
- Docker Daemon: lắng nghe các yêu cầu từ Docker Client để quản lý các đối tượng như Container, Image, Network và Volumes thông qua REST API. Các Docker Daemon cũng giao tiếp với nhau để quản lý các Docker Service.
- Docker Compose: là công cụ cho phép run app với nhiều Docker containers 1 cách dễ dàng hơn. Docker Compose cho phép bạn config các command trong file docker-compose.yml để sử dụng lại. Có sẵn khi cài Docker.
- Docker Swarm: để phối hợp triển khai container.
- Docker Services: là các containers trong production. 1 service chỉ run 1 image nhưng nó mã hoá cách thức để run image — sử dụng port nào, bao nhiêu bản sao container run để service có hiệu năng cần thiết và ngay lập tức.
- Docker Object: khi sử dụng docker, bạn có thể khở tạo hoặc sử các images, container, network, volumes, plugins hoặc các object khác. Những thành phần này được gọi chung là docker objects.

### Sự khác biệt giữa Docker Images và Docker Containers

- Docker Images: Là một template chỉ cho phép đọc, ví dụ một image có thể chứa hệ điều hành Ubuntu và web app. Images được dùng để tạo Docker container. Docker cho phép chúng ta build và cập nhật các image có sẵn một cách cơ bản nhất, hoặc bạn có thể download Docker images của người khác.
- Docker Containers: Docker container có nét giống với các directory. Một Docker container giữ mọi thứ chúng ta cần để chạy một app. Mỗi container được tạo từ Docker image. Docker container có thể có các trạng thái run, started, stopped, moved và deleted.

### Cài đặt Docker

Docker hỗ trợ nhiều nền tảng hệ điều hành khác nhau bao gồm Linux, Windows và cả Ma. Ngoài ra, Docker còn hỗ trợ nhiều dịch vụ điện toán đám mây nổi tiếng như Alibaba Cloud, Microsoft Azure, Amazon Web Services.. Lưu ý là ban đầu nó được xây dựng trên nền tảng Linux.

Bên dưới sẽ hướng dẫn các bạn cách cài đặt Docker trên Ubuntu 22.4, trong trường hợp bạn cần cài trên OS khác vui lòng tham khảo link https://docs.docker.com/get-docker/

*1. Cập nhật hệ thống*

Các bạn tiến hành cập nhật các gói (package) đã được cài đặt trên hệ thống Ubuntu bằng lệnh:

```js
sudo apt update -y && apt upgrade -y

```  
*2. Cài đặt Docker*

Để cài đặt bản Docker mới nhất, các bạn nên cài đặt trực tiếp từ kho lưu trữ của Docker.

Cài đặt một số gói cho phép sử dụng HTTPS.
```js
sudo apt install apt-transport-https ca-certificates curl software-properties-common

```  
Thêm khóa GPG của kho lưu trữ Docker.
```js
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

```  
Bây giờ hãy thêm kho lưu trữ Docker của Ubuntu vào các apt sources.
```js
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```  
Cập nhật packages và thiết lập để cài đặt Docker từ kho lưu trữ chính thức.
```js
sudo apt update
sudo apt-cache policy docker-ce

```  
Cài đặt Docker
```js
sudo apt install docker-ce -y

```  
Kiểm tra trạng thái của Docker

```js
sudo systemctl status docker

```  
*3. Cấu hình quyền Sudo cho user sử dụng Docker*

Các Docker yêu cầu chỉ được thực thi với tư cách người dùng root theo mặc định. Do đó nếu các bạn sử dụng các user khác thì cần phải thêm user đó vào nhóm Docker thì mới có quyền thao tác.
```js
sudo usermod -aG docker username

```  
*4. Sử dụng lệnh Docker*

Để xem các thông tin về Docker
```js
docker info
docker --version
```  
Tải xuống Docker Images

```js
docker run hello-world

```  
Chạy lệnh dưới đây để xem các images đã tải xuống.

```js
docker images

```  
### Quy trình thực thi của một hệ thống sử dụng Docker

![quy_trinh_thuc_thi_docker](/Image/Docker-process.png)

Như trong hình vẽ, một hệ thống Docker được thực thi với 3 bước chính :

***Build -> Push -> Pull,Run***

- Build: Đầu tiên tạo một dockerfile, trong dockerfile này chính là code của chúng ta. Dockerfile này sẽ được Build tại một máy tính đã cài đặt Docker Engine. Sau khi build ta sẽ có được Container, trong Container này chứa ứng dụng kèm bộ thư viện của chúng ta.

- Push: Sau khi có được Container, chúng ta thực hiện push Container này lên cloud và lưu tại đó.

- Pull, Run: Nếu một máy tính khác muốn sử dụng Container chúng ta thì bắt buộc máy phải thực hiện việc Pull container này về máy, tất nhiên máy này cũng phải cài Docker Engine. Sau đó thực hiện Run Container này.


### Các lệnh cơ bản trong docker

List image/container:
```js
$ docker image/container ls
```  
Delete image/container:
```js
$ docker image/container rm <tên image/container >

```  
Delete all image hiện có:
```js
$ docker image rm $(docker images –a –q)
```  
List all container hiện có:
```js
$ docker ps –a
```  
Stop a container cụ thể:
```js
$ docker stop <tên container>
```  
Run container từ image và thay đổi tên container:
```js
$ docker run –name <tên container> <tên image>
```  
Stop all container:
```js
docker stop $(docker ps –a –q)
```  
Delete all container hiện có:

```js
docker image rm $(docker images –a –q)
```  
Show log a container:
```js
$ docker logs <tên container>
```  
Build một image từ container:
```js
$ docker build -t <tên container>
```  
Tạo một container chạy ngầm:
```js
$ docker run -d <tên image>
```  
Tải một image trên docker hub:
```js
$ docker pull <tên image>
```  
Start một container:
```js
$ docker start <tên container>
```  
### Tham khảo

- https://docs.docker.com/get-docker/
- https://www.alibabacloud.com/help/en/elastic-compute-service/latest/deploy-and-use-docker
- https://www.alibabacloud.com/help/en/container-registry/latest/basic-operations-on-docker-images
- https://www.alibabacloud.com/blog/run-docker-on-alibaba-clouds-elastic-compute-service-ecs_594306
- https://edu.alibabacloud.com/certification/clouder_docker