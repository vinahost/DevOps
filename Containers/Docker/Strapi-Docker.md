# Strapi là gì?

Đây là một **NodeJS application framework** cho phép tạo nhanh **API server** và xuất bản **API** thông qua **REST** hoặc **GraphQL**.

Điểm khác biệt của **Strapi** chính là nó hỗ trợ thiết kế **model** bằng giao diện, và tạo sẵn cho bạn một trang **admin** để bạn quảy lý cá **model** đó. Muốn **customize** thêm để phù hợp yêu cầu thì bạn sẽ viết theo mô hình **plugin** mà nó cung cấp.

Ngoài ra nếu chúng ta cần **custom** theo những ứng dụng đặc thì **Strapi** cung cấp giải pháp viết **plugin** để cài cắm vào hệ thống.

Điểm hay nữa là **strapi** hỗ trợ làm việc với hàng loạt các **framework frontend** như:

- Angular CMS
- React CMS
- Next.js CMS
- Vue.js CMS
- Nuxt.js CMS

## Tại sao lại sử dụng docker để cài đặt strapi

Việc cài đặt **strapi** một cách thông thường qua **cli** bạn có thể tham khảo qua [link sau](https://strapi.io/documentation/3.0.0-beta.x/getting-started/quick-start.html)

Thế nhưng, với các bản **strapi** hiện tại việc cài đặt bằng **yarn** hay **npx** sẽ yêu cầu khá nhiều **requirement** về **version** của các **dependence**, các **plugin** hỗ trợ... Bạn đều phải cài bằng tay và **bug** sẽ xuất hiện. Thay vào việc phải sử dụng khoảng thời gian lãng phí đó các nhà phát triển đã tạo ra một **image** cho riêng **strapi** trên **dockerHub** để ta tạo một **container** dựa sẵn vào **image** đó chạy riêng cho **strapi**. Thật là tiện phải không nào ta không phải tốn thời gian hàng giờ ngồi chỉnh `package.json` và việc **deploy** với **docker** cũng rất nhiều tài liệu hướng dẫn vì thế mình sẽ sử dụng **docker** để chạy **strapi**.

## Nhắc lại một số kiến thức cơ bản của docker

**Docker** là một nền tảng cho **developers** và **sysadmin** để **develop**, **deploy** và **run application** với **container**. Nó cho phép tạo các môi trường độc lập và tách biệt để khởi chạy và phát triển ứng dụng và môi trường này được gọi là **container**. Khi cần **deploy** lên bất kỳ **server** nào chỉ cần **run container** của **Docker** thì **application** của bạn sẽ được khởi chạy ngay lập tức.

**Docker container** được khởi tạo bởi một **docker image**. Bạn có thể tìm kiếm những **image** có sẵn trên **dockerhub**, ví dụ như:

![Strapi Docker](/Image/Strapi-Docker01.png)

Từ đó có thể **pull image** về và tạo ra **container** mà bạn mong muốn...

## Bắt đầu nào

Hãy tạo ra một thư mục làm việc mà bạn mong muốn nhé. ở đây mình tạo thư mục **test** như sau:

![Strapi Docker](/Image/Strapi-Docker02.png)

Bạn không cần phải tạo **2 folder app** hay **db** như trên đó là kết quả do mình **volume** các **folder** bằng kịch bản trong **file** `docker-compose.yml` ở dưới. Mình chỉ viết file **docker-compose.yml** như sau

```yaml
version: '3'

services:
strapi:
  container_name: strapi
  image: strapi/strapi
  environment:
    - DATABASE_CLIENT=postgres
    - DATABASE_HOST=db
    - DATABASE_PORT=5432
    - DATABASE_NAME=strapi
    - DATABASE_USERNAME=strapi
    - DATABASE_PASSWORD=strapi
  ports:
    - 1337:1337
  volumes:
    - ./app:/srv/app
  depends_on:
    - db

db:
  container_name: postgres
  image: postgres
  restart: always
  volumes:
    - ./db:/var/lib/postgresql/data
  environment:
    POSTGRES_USER: strapi
    POSTGRES_PASSWORD: strapi
    POSTGRES_DB: strapi
```

Mình đọc qua kịch bản của `docker-compose` nhé: Ở đây mình sẽ sử dụng **2 docker image** là `postgres` và `strapi` để tạo ra 2 **container** chứa **database** và **strapi app**.

Đối với **service strapi** sẽ truy cập vào bằng `port 1337` của máy, các biến môi trường sẽ được định nghĩa trong **environment** như trên. **strapi container** sẽ liên kết với **db container** thông qua `depends_on`.

Khi viết xong kịch bản như trên bạn chạy câu lệnh:

    docker-compose up

Bạn sẽ thấy trên màn hình **console** hiển thị một loạt các thông tin về **pull** các **images** bạn định nghĩa và khởi tạo **image** mới do bạn định nghĩa tạo ra **2 container** có tên như sau (sử dụng câu lệnh `docker ps` để xem các **container** đang chạy):

![Strapi Docker](/Image/Strapi-Docker03.png)

Bạn sẽ phải chờ khoảng 5 phút để cài đặt các dependences trong strapi, Khi xong các kết quả màn hình sẽ hiển thị như sau:

![Strapi Docker](/Image/Strapi-Docker04.png)

Bạn truy vào **http://localhost:1337/admin/auth/register** để xem kết quả nhé.

![Strapi Docker](/Image/Strapi-Docker05.png)