# Billing Management trên Aliaba Cloud

Bài viết này hướng dẫn cách thiết lập giới hạn sử dụng tài nguyên trên **Alibaba Cloud**. Giới hạn sử dụng tài nguyên cho phép bạn kiểm soát việc sử dụng tài nguyên của mình và tránh bị tính phí quá mức.

1) **Budget Management trên Alibaba Cloud**

Từ trang **Console** của trang **Alibaba Cloud**, bạn có thể chọn **Expenses** để vào trang **Billing Management**. Tại trang này, bạn có thể xem các thông tin về số dư tài khoản, các dịch vụ sử dụng cùng với hóa đơn cụ thể, bạn có thể phân chia budget, quản lý coupon và tài nguyên…

Một trong số những tính năng được hỗ trợ trên trang **Billing Management** là cài đặt thông báo khi số dư trong tài khoản giảm đến một mức cố định. Trong bài viết này chúng tôi sẽ hướng dẫn bạn cài đặt alert đó để tự quản lý số dư trong tài khoản của mình.

2) **Set up thông báo cho budget**

Truy cập vào trang **Console** trên Alibaba Cloud, chọn **Expenses** để vào trang **Billing Management**.

![Billing Management](/Image/Billing-Management01.png)

Tại trang này, bạn sẽ thấy một số thông tin như số tiền sử dụng trong tháng, số tiền còn lại trong phần **Available Credit**. Chọn Setting ở dòng **Alert** như hình bên dưới để set up thông báo khi số credit chạm đến một ngưỡng nhất định.

![Billing Management](/Image/Billing-Management02.png)

Cửa sổ set up thông báo như hình dưới sẽ hiện ra. Chọn các thông tin tương ứng như sau:

![Billing Management](/Image/Billing-Management03.png)

![Billing Management](/Image/Billing-Management04.png)

Bạn có thể chọn **Message Settings** để cài đặt người nhận thông báo trên.

Sau khi hoàn tất, nhấn **Save** để tạo ra **Alert**.

*Author*: **Vo Thi Phuong Anh**

*Solution Architect* - **Alibaba Cloud Intelligence**

### Tham khảo

- https://www.alibabacloud.com/blog/set-billing-alert-on-alibaba-cloud_600156