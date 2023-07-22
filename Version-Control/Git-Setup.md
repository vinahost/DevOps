# Cài đặt & Cấu hình Git

Git là một hệ thống quản lý phiên bản phân tán được sử dụng để theo dõi các thay đổi đối với các tệp và thư mục trong một dự án. Nó được sử dụng rộng rãi bởi các nhà phát triển phần mềm để theo dõi các thay đổi đối với mã của họ, nhưng nó cũng có thể được sử dụng bởi bất kỳ ai cần theo dõi các thay đổi đối với một dự án.

![img](../Image/Git.png)

Git hoạt động bằng cách lưu trữ các thay đổi đối với các tệp và thư mục trong các đơn vị được gọi là "**commit**". Mỗi commit có một ID duy nhất và nó chứa thông tin về các thay đổi đã được thực hiện đối với các tệp và thư mục. Git cũng lưu trữ một lịch sử của các commit, cho phép bạn xem các thay đổi đã được thực hiện đối với dự án theo thời gian.

Git là một công cụ mạnh mẽ có thể được sử dụng để theo dõi các thay đổi đối với các dự án lớn và phức tạp. Nó cũng rất dễ sử dụng và có thể được cài đặt trên hầu hết các hệ điều hành.

Git có thể được cài đặt trên hệ điều hành Linux bằng cách sử dụng các lệnh sau:

Ubuntu:

    sudo apt-get install git

Fedora:

    sudo dnf install git

CentOS:

    sudo yum install git

Arch Linux:

    sudo pacman -S git

Sau khi Git đã được cài đặt, bạn có thể cấu hình nó bằng cách chạy lệnh sau:

    git config --global user.name "Your Name"
    git config --global user.email "your_email@example.com"

Các lệnh trên sẽ cấu hình tên người dùng và địa chỉ email của bạn cho Git. Tên người dùng và địa chỉ email này sẽ được sử dụng để ghi lại các thay đổi của bạn trong Git.

Để kiểm tra cài đặt Git, hãy chạy lệnh sau:

    git --version

Bạn sẽ thấy một phản hồi tương tự như sau:

    git version 2.41.0

Sau khi Git đã được cài đặt và cấu hình, bạn có thể bắt đầu sử dụng nó để theo dõi các thay đổi đối với các dự án của mình.

### Để bắt đầu sử dụng Git, hãy tạo một kho lưu trữ mới. Để làm điều này, hãy làm theo các bước sau:

**1. Tạo một kho lưu trữ Git**. Một kho lưu trữ Git là một thư mục chứa mã của bạn và lịch sử các thay đổi đối với mã đó. Để tạo một kho lưu trữ Git, hãy chạy lệnh sau trên máy của bạn:

    git init

Kho lưu trữ mới sẽ được tạo trong thư mục hiện tại.

**2. Thêm mã của bạn vào kho lưu trữ Git.** Để thêm mã của bạn vào kho lưu trữ Git, hãy chạy lệnh sau trên máy của bạn:

    git add .

Bạn có thể bắt đầu thêm các tệp vào kho lưu trữ bằng cách sử dụng lệnh *git add*. Để đẩy các thay đổi của bạn lên kho lưu trữ từ xa, hãy sử dụng lệnh *git push*. Để kéo các thay đổi từ kho lưu trữ từ xa xuống, hãy sử dụng lệnh *git pull*.

**3. Lưu trữ mã của bạn trong kho lưu trữ Git**. Để lưu trữ mã của bạn trong kho lưu trữ Git, hãy chạy lệnh sau trong thiết bị đầu cuối của bạn:

    git commit -m "Initial commit"

**4. Tạo một kho lưu trữ Git trên GitHub**: GitHub - https://github.com/ là một dịch vụ lưu trữ kho lưu trữ Git trực tuyến. Để tạo một kho lưu trữ Git trên GitHub, hãy tạo một tài khoản GitHub và sau đó tạo một kho lưu trữ mới.

**5. Đăng ký kho lưu trữ Git của bạn trên GitHub**: để đăng ký kho lưu trữ Git của bạn trên GitHub, hãy chạy lệnh sau trong thiết bị đầu cuối của bạn:

    git push origin master

Bây giờ, kho lưu trữ Git của bạn đã được đăng ký trên GitHub và có thể truy cập được bởi bất kỳ ai có quyền truy cập vào kho lưu trữ.

Một số công cụ DevOps phổ biến mà Git có thể được sử dụng cùng bao gồm:

- **Jenkins**: Jenkins là một công cụ tự động hóa xây dựng và triển khai có thể được sử dụng để tự động hóa các quy trình liên quan đến Git.
- **Travis CI**: Travis CI là một nền tảng tự động hóa xây dựng và triển khai có thể được sử dụng để tự động hóa các quy trình liên quan đến Git.
- **CircleCI**: CircleCI là một nền tảng tự động hóa xây dựng và triển khai có thể được sử dụng để tự động hóa các quy trình liên quan đến Git.

Việc cài đặt và cấu hình Git với DevOps là một quá trình quan trọng đối với bất kỳ tổ chức nào sử dụng DevOps. Bằng cách cài đặt và cấu hình Git, các tổ chức có thể tận dụng các tính năng và lợi ích của Git để cải thiện quy trình phát triển và triển khai của họ.

Bên dưới là một số lợi ích của việc cài đặt và cấu hình Git với DevOps:

- **Cải thiện khả năng cộng tác**: Git có thể được sử dụng để cải thiện khả năng cộng tác giữa các nhà phát triển bằng cách cho phép họ theo dõi các thay đổi đối với mã và chia sẻ các thay đổi của họ với những người khác.
- **Cải thiện khả năng tái sử dụng mã**: Git có thể được sử dụng để cải thiện khả năng tái sử dụng mã bằng cách cho phép các nhà phát triển lưu trữ mã của họ trong một kho lưu trữ chung và sau đó truy cập mã đó từ bất kỳ dự án nào.
- **Cải thiện khả năng theo dõi các thay đổi**: Git có thể được sử dụng để cải thiện khả năng theo dõi các thay đổi đối với mã bằng cách cho phép các nhà phát triển theo dõi các thay đổi đối với mã và khôi phục các thay đổi cũ nếu cần.
- **Cải thiện khả năng kiểm tra chất lượng**: Git có thể được sử dụng để cải thiện khả năng kiểm tra chất lượng bằng cách cho phép các nhà phát triển chạy các bài kiểm tra tự động trên mã của họ và theo dõi kết quả của các bài kiểm tra đó.
- **Cải thiện khả năng triển khai**: Git có thể được sử dụng để cải thiện khả năng triển khai bằng cách cho phép các nhà phát triển triển khai mã của họ lên các môi trường sản xuất một cách an toàn và hiệu quả.

Git là một công cụ mạnh mẽ có thể được sử dụng để theo dõi các thay đổi đối với các dự án. Nó rất dễ sử dụng và có thể được cài đặt trên hầu hết các hệ điều hành.