Git là một hệ thống quản lý phiên bản phân tán được sử dụng để theo dõi các thay đổi đối với các tệp và thư mục trong một dự án. Nó được sử dụng rộng rãi bởi các nhà phát triển phần mềm để theo dõi các thay đổi đối với mã của họ, nhưng nó cũng có thể được sử dụng bởi bất kỳ ai cần theo dõi các thay đổi đối với một dự án.

Git hoạt động bằng cách lưu trữ các thay đổi đối với các tệp và thư mục trong các đơn vị được gọi là "**commit**". Mỗi commit có một ID duy nhất và nó chứa thông tin về các thay đổi đã được thực hiện đối với các tệp và thư mục. Git cũng lưu trữ một lịch sử của các commit, cho phép bạn xem các thay đổi đã được thực hiện đối với dự án theo thời gian.

Git là một công cụ mạnh mẽ có thể được sử dụng để theo dõi các thay đổi đối với các dự án lớn và phức tạp. Nó cũng rất dễ sử dụng và có thể được cài đặt trên hầu hết các hệ điều hành.

Git có thể được cài đặt trên hệ điều hành Linux bằng cách sử dụng các lệnh sau:

- Ubuntu:

    sudo apt-get install git

- Fedora:

    sudo dnf install git

- CentOS:

    sudo yum install git

- Arch Linux:

- sudo pacman -S git

Sau khi Git đã được cài đặt, bạn có thể cấu hình nó bằng cách chạy lệnh sau:

    git config --global user.name "Your Name"
    git config --global user.email "your_email@example.com"

Các lệnh trên sẽ cấu hình tên người dùng và địa chỉ email của bạn cho Git. Tên người dùng và địa chỉ email này sẽ được sử dụng để ghi lại các thay đổi của bạn trong Git.

Để kiểm tra cài đặt Git, hãy chạy lệnh sau:

    git --version

Bạn sẽ thấy một phản hồi tương tự như sau:

    git version 2.41.0

Sau khi Git đã được cài đặt và cấu hình, bạn có thể bắt đầu sử dụng nó để theo dõi các thay đổi đối với các dự án của mình.

Để bắt đầu sử dụng Git, hãy tạo một kho lưu trữ mới. Để làm điều này, hãy làm theo các bước sau:

- Mở thư mục mà bạn muốn tạo kho lưu trữ trong cửa sổ dòng lệnh.
- Gõ lệnh sau:

    git init

Kho lưu trữ mới sẽ được tạo trong thư mục hiện tại.

Bạn có thể bắt đầu thêm các tệp vào kho lưu trữ bằng cách sử dụng lệnh *git add*. Để đẩy các thay đổi của bạn lên kho lưu trữ từ xa, hãy sử dụng lệnh *git push*. Để kéo các thay đổi từ kho lưu trữ từ xa xuống, hãy sử dụng lệnh *git pull*.

Git là một công cụ mạnh mẽ có thể được sử dụng để theo dõi các thay đổi đối với các dự án. Nó rất dễ sử dụng và có thể được cài đặt trên hầu hết các hệ điều hành.