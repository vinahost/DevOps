# Quản lý cơ sở dữ liệu (Database) RDS với Alibaba Cloud DMS

## Data Management Tools trên Alibaba Cloud là gì ?

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud.jpg)

**Data Management Tools** (*DBMS*) trên **Alibaba Cloud** là một tập hợp các dịch vụ giúp bạn quản lý dữ liệu của mình một cách hiệu quả. Giải pháp **DBMS** của **Alibaba Cloud** hiện nay cung cấp nhiều sản phẩm, dịch vụ đa dạng với nhiều nhu cầu xây dựng hệ thống dữ liệu. Người dùng có thể gần như làm mọi thứ với dữ liệu trên **Alibaba Cloud**, có thể kể đến như: xây dựng cơ sở dữ liệu, thêm dữ liệu hiện có, phân tích dữ liệu, quản lý toàn hệ thống dữ liệu, chạy các mô hình học máy phức tạp…

Vào tháng **12/2022**, các giải pháp về **Cloud DBMS** của **Alibaba Cloud** đã được định vị vào vị trí "*Leaders*" trong báo cáo lần thứ ba trong ba năm liên tiếp.

Đối với việc quản lý và thao tác với các cơ sở dữ liệu, **Alibaba Cloud** cung cấp các sản phẩm:

- **Data Transmission Service (DTS)**: dịch vụ giúp di chuyển và đồng bộ hóa giữa các data storage khác nhau.
- **Database Backup (DBS)**: Sao lưu cơ sở dữ liệu và bảo vệ dữ liệu tối ưu trên Alibaba Cloud, IDC,...
- **Data Management (DMS)**: Một dịch vụ quản lý dữ liệu trên Alibaba Cloud đảm bảo các yếu tố bảo mật, hiệu quả.
- **Database Autonomy Service (DAS)**: Nền tảng cơ sở dữ liệu tự động: tự sửa chữa, tự tối ưu hóa và tự bảo mật.

Trong bài viết này, chúng ta sẽ cùng tìm hiểu về **DMS** và một số chức năng cơ bản của **DMS** trong việc thao tác và quản lý cơ sở dữ liệu host trên **Alibaba Cloud**.

## DMS là gì?

**Alibaba Cloud DMS (Data Management Service)** là một dịch vụ quản lý dữ liệu toàn diện được cung cấp bởi **Alibaba Cloud**. **DMS** cung cấp một nền tảng đa chức năng để quản lý và vận hành dữ liệu trong môi trường đám mây.

**DMS** cho phép người dùng quản lý toàn bộ quy trình quản lý dữ liệu, từ việc thiết kế và triển khai cơ sở dữ liệu cho đến quản lý vòng đời dữ liệu. Với **DMS**, người dùng có thể thực hiện các tác vụ quan trọng như tạo và quản lý cơ sở dữ liệu, sao lưu và khôi phục dữ liệu, điều chỉnh và tối ưu hóa hiệu suất cơ sở dữ liệu, kiểm tra và bảo mật dữ liệu, và tích hợp dữ liệu từ nhiều nguồn khác nhau.

**DMS** cung cấp giao diện đồ họa trực quan và dễ sử dụng, cho phép người dùng quản lý dữ liệu một cách thuận tiện và hiệu quả. **DMS** hỗ trợ nhiều loại cơ sở dữ liệu phổ biến như **MySQL**, **SQL Server**, **Oracle**, **PostgreSQL** và **MongoDB**, và cung cấp tính năng tự động hóa và tự động sửa chữa giúp tối ưu hóa các tác vụ quản lý dữ liệu.

## Quản lý cơ sở dữ liệu DMS với Alibaba Cloud DMS

Để tiến hành sử dụng **DMS** để quản lý cơ sở dữ liệu **RDS**, ta cần tạo ra 1 **instance ApsaraDB RDS**. Trong bài hướng dẫn này, chúng tôi sẽ xem như bạn đã tạo được một cơ sở dữ liệu của riêng mình trên **RDS**. 

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud01.png)

Tiếp theo tạo **user** và **database** trên **instance RDS**

Nhấn vào tên **instance RDS** > **Account** > **Create Account**.

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud02.png)
![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud03.png)
![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud04.png)

**RDS** hỗ trợ tạo 2 dạng user: **Privileged** và **Standard**. Ở đây chúng ta sẽ tạo ra một tài khoản **Privileged**. Nhập các thông tin **username** và **password** đăng nhập cho tài khoản.

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud05.png)

Sau khi nhập xong các thông tin cho tài khoản, chọn **Ok** để tạo.

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud06.png)

*Account sau khi tạo thành công*

Sau khi đã có **Account**, tạo một cơ sở dữ liệu trên **instance RDS** của bạn bằng cách chọn mục **Database** >** Create Database**.

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud07.png)
![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud08.png)

Nhập các thông tin cho **database** của bạn. Bạn có thể nhập giống như bên dưới và nhấn **Create** để hoàn tất.

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud09.png)

Sau khi hoàn tất tạo **User** và **Database**, nhấn vào **Log on to database** để vào **DMS**.

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud024.png)

Bạn sẽ được chuyển đến **console** của **DMS**, nhập các thông tin cơ sở dữ liệu và **user** để kết nối vào **DMS**.

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud011.png)

Chọn **Test connection** để kiểm tra các thông tin đăng nhập. Nếu các thông tin là đúng, thông báo như sau sẽ hiển thị.

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud012.png)

Chọn **Ok** và tiếp tục chọn **Log In để** đăng nhập.

Tại trang chủ **DMS**, chọn **SQL Console** và chọn cơ sở dữ liệu bạn muốn thao tác.

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud014.png)

Paste câu lệnh **SQL** sau đây vào **SQL Console** để tạo các bảng cho cơ sở dữ liệu. Nhấn **Execute(F8)** để chạy lệnh.

    CREATE TABLE `users` (
        `user_id` bigint NOT NULL,
        `name` varchar(32) NULL,
        `age` bigint NULL,
        `sex` varchar(32) NULL,
        `country` varchar(32) NULL,
        `country_code` varchar(32) NULL,
        PRIMARY KEY (`user_id`)
    ) ENGINE=InnoDB
    DEFAULT CHARACTER SET=utf8;

    CREATE TABLE `products` (
        `product_id` bigint NOT NULL,
        `product_name` varchar(32) NULL,
        `price` float NULL,
        PRIMARY KEY (`product_id`)
    ) ENGINE=InnoDB
    DEFAULT CHARACTER SET=utf8;

    CREATE TABLE `products` (
        `product_id` bigint NOT NULL,
        `product_name` varchar(32) NULL,
        `price` float NULL,
        PRIMARY KEY (`product_id`)
    ) ENGINE=InnoDB
    DEFAULT CHARACTER SET=utf8;

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud015.png)

Sau khi đã có các bảng, tiến hành **import data** vào các bảng. Trong bài hướng dẫn này, chúng tôi sẽ sử dụng dữ liệu **E-commerce** có sẵn trên **Github** của **Alibaba Cloud Academy**.

Truy cập vào **Github** chứa dữ liệu, tải ba file **csv** sau:

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud016.png)

Tại **DMS**, chọn **Data Import** để tiến hành đổ dữ liệu vào các bảng.

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud017.png)

Nhập các thông tin được yêu cầu và chọn từng file **.csv** tương ứng với mỗi bảng.

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud018.png)

Sau khi hoàn tất, nhấn **Submit**.

Lúc này bạn sẽ tạo ra một **ticket** trên **DMS** để đổ dữ liệu vào, sau khi **ticket** được **check** thành công, bạn có thể chọn **Execute Change** để đến bước tiếp theo.

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud019.png)

Tiếp tục chọn **Confirm Execution**.

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud020.png)

Nếu hiển thị như sau, bạn đã đổ dữ liệu thành công.

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud021.png)

Làm tương tự với 2 file **csv** còn lại để đổ vào 2 bảng còn lại trong cơ sở dữ liệu.

Sau khi đổ dữ liệu vào cả ba bảng, bạn có thể sử dụng **SQL Console** để truy vấn dữ liệu ngay trên **DMS**. Bạn có thể chạy lệnh **SELECT** đơn giản sau để kiểm tra dữ liệu:

    SELECT * FROM users LIMIT 10;
    SELECT * FROM products LIMIT 10;
    SELECT * FROM orders LIMIT 10;

![DMS Alibaba Cloud](/Image/DMS-Alibaba-Cloud022.png)

Như vậy là chúng ta đã đi qua các phần cơ bản để thao tác với dữ liệu của bạn trong **DMS**. Trong các bài tiếp theo, chúng ta sẽ cùng tìm hiểu cách tạo **report**, dựng **data warehouse** bằng các sản phẩm **Big Data** đến từ **Alibaba Cloud** như **DataWorks**, **QuickBI** …

*Author*: **Vo Thi Phuong Anh**

*Solution Architect* - **Alibaba Cloud Intelligence**

## Tham khảo

- https://www.alibabacloud.com/product/data-management-service
- https://www.alibabacloud.com/help/en/dms/product-overview/what-is-dms