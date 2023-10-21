# Server Migration Center (SMC) là gì ?

**Server Migration Center (SMC)** là một nền tảng giúp người dùng dễ dàng di chuyển dữ liệu đa nền tảng và môi trường được phát triển bởi [Alibaba Cloud](https://vn.alibabacloud.com/). **SMC** cho phép bạn di chuyển một hoặc nhiều máy chủ nguồn đến **Alibaba Cloud**. Máy chủ nguồn có thể là máy chủ trong các trung tâm dữ liệu (**servers in self-managed data centers**) , máy ảo (**virtual machines**) , máy chủ trên các nền tảng đám mây khác (**third-party cloud servers**) hoặc máy chủ của các loại khác nhau. Khi sử dụng **SMC** để di chuyển máy chủ, bạn sẽ bị tính phí cho các tài nguyên [Elastic Compute Service (ECS)](https://www.alibabacloud.com/vi/product/ecs) được sử dụng trong quá trình di chuyển. **ECS** bao gồm các trường hợp trung gian, bản **snapshot** và đĩa hệ thống và đĩa dữ liệu được gắn vào các trường hợp trung gian. 

### Lợi ích khi sử dụng Server Migration Center (SMC)

Bên cạnh đó, **SMC** còn mang đến nhiều lợi ích vượt trội:

- Độc lập với môi trường cơ bản của máy chủ nguồn: SMC hỗ trợ di chuyển các hình thức di chuyển như P2C, V2C và C2C và hoàn toàn tự động.
- Khả năng di chuyển mà không bị gián đoạn và không cần dừng các dịch vụ trên máy chủ nguồn.
- Cấu hình đơn giản, gọn nhẹ và linh hoạt, không yêu cầu cài đặt. Người dùng có thể lựa chọn phương pháp cho riêng mình. Đồng thời, SMC hỗ trợ quản lý toàn bộ tiến trình di chuyển bằng cách sử dụng SMC Console hoặc gọi các thao tác thông qua API.
- Các hệ điều hành chính bao gồm Windows 2003 trở lên, CentOS, Red Hat, Ubuntu, Debian, SUSE và openSUSE.
- Khả năng truyền tải dữ liệu an toàn, bảo mật. Nền tảng SMC sử dụng khóa RSA 2048-bit để mã hóa dữ liệu trong suốt quá trình truyền tải. Bên cạnh đó, SMC cho phép người dùng sử dụng mạng riêng như cổng VPN cũng như các kết nối vật lý do Alibaba Cloud Express Connect cung cấp.

### Quá trình Migration

**SMC** bao gồm **SMC client** và **SMC console**. Hình ảnh bên dưới cho thấy cách chuyển một máy chủ thành **Elastic Compute Service (ECS) image**.

![Server Migration Center (SMC) là gì ?](/Image/Server-Migration-Center-SMC-la-gi.png)

### Tài liệu tham khảo

- Link dịch vụ: https://smc.console.aliyun.com/
- https://www.alibabacloud.com/help/en/server-migration-center
- https://www.alibabacloud.com/help/en/server-migration-center/latest/what-is-smc
- https://www.alibabacloud.com/help/en/server-migration-center/latest/smc-faq
- Tài liệu giới thiệu: tham khảo [**tại đây**](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/download%2Fpdf%2F121557%2FProduct_Introduction_intl_en-US.pdf)
- User Guide: tham khảo [**tại đây**](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/download%2Fpdf%2F121561%2FUser_Guide_intl_en-US.pdf)
- Xem video giới thiệu [**tại đây**](https://cloud.video.taobao.com/play/u/null/p/1/e/6/t/1/d/ud/378567887228.mp4)