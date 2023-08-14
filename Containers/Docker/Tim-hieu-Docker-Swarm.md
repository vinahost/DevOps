# Docker Swarm là gì ?

**Docker Swarm** là công cụ tạo ra một **clustering Docker**. Cho phép ta có thể kết nối các **docker host** với nhau tạo thành một cụm các máy, khi tạo được hệ thống **Docker Swarm** thì chúng ta có thể quản lý và chạy các dịch vụ trên hệ thống này một cách dẽ dàng. Giả sử ta có các hệ thống **docker** chạy trên các **vps** khác nhau thì ta có kể kết nối chúng tạo thành một cụm **docker**.

Chúng ta cần dùng **Docker Swarm** khi **project** của bạn cần phát triển, quản lý, **deploy** trên nhiều nhiều **host** thì đó là lúc cần dùng đến **Docker Swarm**.

## Tính năng nổi bật

- Cluster management integrated with Docker Engine : Sử dụng bộ Docker Engine CLI để tạo swarm một cách dễ dàng

- Decentralized design: Docker Swarm được thiết kế dạng phân cấp. Thay vì xử lý sự khác biệt giữa các roles của node tại thời điểm triển khai, Docker xử lý bất kỳ tác vụ nào khi runtime. Ta có thể triển khai node managers và worker bằng Docker Engine.

- Declarative service model: Docker Engine sử dụng phương thức khai báo để cho phép bạn define trạng thái mong muốn của các dịch vụ khác nhau trong stack ứng dụng của bạn

- Scaling: với mỗi service có thể khai báo số lượng task mà ta muốn chạy, Scale up, down replicas của 1 service một cách dễ dàng

- Desired state reconciliation: Swarm đảm bảo 1 service hoạt động ổn định bằng cách tự động thay 1 replicas crash bằng 1 replicas mới cho các worker đang run

- Multi-host networking: Swarm manager có thể tự động gán IP cho mỗi service khi nó khởi tạo và cập nhật application.

- Service discovery: Swarm manager node gán mỗi service trong swarm một DNS server riêng. Do đó bạn có thể truy xuất thông qua DNS này

- Load balancing: Có thể expose các port cho các services tới load balance. tích hợp cân bằng tải sử dujgn thuật toán thuật toán Round-robin

- Secure by default: Các service giao tiếp với nhau sử dụng giao thức bảo mật TLS

- Rolling updates: ASwarm giúp update image của service một cách hoàn toàn tự động. Swarm manager giúp bạn kiểm soát độ trễ giữa service deploy tới các node khác nhau và bạn có thể rolling back bất cứ lúc nào.

## Kiến trúc của Docker Swarm

![swarm_diagram](/Image/Docker-Swarm.png)

Kiến trúc **Docker Swarm** bao gồm một tập hợp các **node** có ít nhất một nút chính (**Manager-Leader**) và một số **node worker** có thể là máy ảo hoặc vật lý.

- Swarm: là một cluster của một node trong chế độ Swarm, thay vì phải chạy các container bằng câu lệnh thì ta sẽ thiết lập các services để phân bổ các bản replicas tới các node.

- Manager Node: Là node nhận các define service từ user, quản lý theo dõi các service và tác vụ đang chạy trong Swarm, điều phối và chỉ định các node worker làm việc

- Worker Node: là một máy vật lý hay máy ảo chạy các tác vụ được chỉ định bới node manager

- Task: là các Docker containers thực thi các lệnh bạn đã định nghĩa trong service. Tác vụ này sẽ do node Manager phân bổ xuống, và sau khi việc phân bổ này task không thể chuyển sang một worker khác. Nếu task thất bại, node Manager sẽ chỉ định một phiên bản mới của tác vụ đó cho một node có sẵn khác trong Swarm.

- Service: Một service xác định image của container và số lượng các replicas (bản sao) mong muốn khởi chạy trong Swarm.

## Khởi tạo Docker Swarm

Phần này chúng ta sẽ tạo một cụm **docker** với 2 máy **host** làm **node worker** và 1 host làm **node manager**

Tạo máy ảo cho **Swarm manager** bằng lệnh

```js
$ docker-machine create manager
```  
Tạo 2 máy ảo cho **Swarm node worker** bằng lệnh

```js
$ docker-machine create worker1

$ docker-machine create worker2
```  

Kiểm tra danh sách máy ảo

```js
$ docker-machine ls
```  
Cần kiểm tra thông tin 1 máy ảo thì ta có thể dùng lệnh

```js
$ docker-machine inspect manager
```  
Khởi tạo **Swarn** trên máy ảo **manager**. Để truy cập vào máy **manager** thì ta **SSH** vào bằng lệnh

```js
$ docker-machine ssh manager
```  
Ở **local host** khởi tạo **Swarm** với **node manager** là **IP** máy chọn làm **manager**,

```js
$ docker swarm init --advertise-addr <IP Machine>

```  
Ở đây <IP Machine> là **IP** máy mình chọn làm **node manager** , **IP** này mình có thể lấy ở lệnh *$ docker-machine ls* hoặc *$ docker-machine inspect manager*

Tạo xong thì sẽ như này, dòng bôi trắng là lệnh dùng để các **worker join** vào **Swarm** này, lệnh này để dùng vào phần sau

Kiểm tra **list node** hiện tại đang có trong **Swarm**

```js
docker node ls

```  
**Join** một máy áo khác vào **Swarm** vừa tạo

Bật một **terminal** mới và **ssh** vào **worker1**, và sử dụng lệnh **join** ở trên khi khởi tạo **Swarm**. Giải thích 1 chút về lệnh **join**

```js
$ docker swarm join --token <token> <host>:<port>

```  
-  < host >: Địa chỉ ip của con manager
-  < port>: Cổng port của con manager

## So sánh giữa Kubernetes và Docker Swarm

**Kubernetes** và **Docker Swarm** đều là các công cụ quản lý **container orchestration** cho phép ta có thể quản lý các **container** trên nhiều máy chủ vật lý khác nhau.

Tuy nhiên, **Kubernetes** là công cụ phổ biến hơn và có nhiều tính năng hơn so với **Docker Swarm**.

## Một số điểm khác biệt giữa **Kubernetes** và **Docker Swarm**:

- Kubernetes có thể quản lý các container của nhiều nhà cung cấp khác nhau, trong khi Docker Swarm chỉ quản lý các container của Docker.
- Kubernetes có thể tự động phân phối tải cho các container, trong khi Docker Swarm cần một công cụ bên ngoài để phân phối tải.
- Kubernetes có thể tự động khôi phục các container bị lỗi, trong khi Docker Swarm không có tính năng này.

## Tài liệu tham khảo

- https://docs.docker.com/engine/swarm/