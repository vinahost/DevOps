# Triển khai Container lên “Alibaba Cloud Container Service for Kubernetes” (ACK) bằng GitHub  Actions

## GitHub Actions là gì?

**GitHub Actions** là một công cụ giúp tự động hóa quy trình phát triển phần mềm tại nơi lưu trữ code **GitHub**. Khi bạn nhận được một **event**, ví dụ như một **developer** tạo một **pull request** cho một **repository** nào đó, **GitHub Actions** sẽ chạy một loạt các dòng lệnh được thiết lập trước để thực thi các công việc như **build** và **test** chương trình.

**GitHub Actions** cung cấp các tác nhân xây dựng và triển khai (**runners**), chạy trên các máy ảo hỗ trợ hệ điều hành **Linux**, **Windows** và **macOS**. Ngoài ra, bạn có thể **self-hosted runners** trên **Alibaba Cloud**.

## Các thành phần của GitHub Actions

- **Workflow**: là một thủ tục tự động, workflows được tạo bởi một hay nhiều job và các job này có thể chạy Theo định kỳ hoặc trigger chạy khi nhận được một event nào đó. 

- **Events**: Là một hành động ấn định để trigger một workflow, ví dụ khi một developer tạo một nhánh develop nào đó thì event này được gửi tới workflow, hoặc bạn có thể sử dụng webhook khi nhận một event bên ngoài trigger vào workflow.

- **Jobs**: Là một tập các steps để thực thi trên cùng một runner. Mặc định với một workflow chạy nhiều job thì các job này chạy song song (parallel) 

- **Runners**: Là một server mà GitHub Actions cài đặt các ứng dụng trong nó. Bạn có thể sử dụng runner của github hoặc bạn có thể sử dụng server riêng của bạn. Github runner hỗ trợ các hệ điều hành: Ubuntu Linux, Microsoft Windows, and macOS

- **Actions**: Là một building block, có tính portable được build sẵn,  bạn có thể tạo riêng một action riêng cho bạn. 

## Yêu cầu

- Alibaba Cloud Container Service for Kubernetes cluster (ACK) - https://cs.console.aliyun.com 

- Alibaba Cloud Container registry https://cr.console.aliyun.com

- Cấu hình GitHub reposiroty chứa Dockerfile, triển khai Kubernetes, services và files.

## Sử dụng GitHub Actions

Sử dụng **GitHub Actions** để **Deploy** một **Container** lên **ACK**

Tiếp theo, chúng ta sẽ triển khai 1 **container** chạy **Webserver Nginx** lên **Alibaba Cloud Container Service** trên **Kubernetes** bằng cách sử dụng **GitHub Actions**. Chúng ta sẽ sử dụng **ack-demo** cho bài hướng dẫn này

- **Bước 1**: Truy cập tính năng **GitHub Actions** trong phần kiển khai bằng **repo** chứa **Dockerfile**, triển khai **Kubernetes**, **service** và **files**. Chọn **Deploy to Alibaba Cloud ACK** trong mục **Actions** và click **Configure**

![GitHub Actions](/Image/GitHub-Actions01.png)

- **Bước 2**: Sau khi click vào **Configure**, hãy chỉnh file **workflow** bằng cách thay đổi biến môi trường sao cho phù hợp với môi trường **Alibaba Cloud**.

![GitHub Actions](/Image/GitHub-Actions02.png)

Bạn có thể tham khảo file cấu hình mẫu [tại đây](/Scripts/yml/workflows_alibabacloud.yml)

- **Bước 3**: Thêm biến môi trường **Access_Key_ID** & **Access_Key_Secret** của tài khoản **Alibaba Cloud** vào trong **Github** dưới **Settings**:

![GitHub Actions](/Image/GitHub-Actions03.png)

Xem thêm cách lấy **Access_Key_ID** & **Access_Key_Secret** tại bài viết **Alibaba Cloud Resource Access Management (RAM)**

- **Bước 4**: Sau khi **commit** lên **Github repository**, nó sẽ kích hoạt **trigger** trong file **workfow** và chạy. Sau khi thành công, tất cả các bước trong job sẽ đánh dấu **complete**

![GitHub Actions](/Image/GitHub-Actions04.png)

Cuối cùng, bạn có thể xác nhậm rằng **container** đã hoàn thành việc **deployed** bằng cách truy cập vào **Kubernetes cluster** và chạy lệnh `kubetcl get deployments`

![GitHub Actions](/Image/GitHub-Actions05.png)

Có thể kiểm tra bằng cách truy cập website từ internet

![GitHub Actions](/Image/GitHub-Actions06.png)

Chúc các bạn thành công

## Tham khảo

https://www.alibabacloud.com/blog/how-to-deploy-a-container-using-github-actions_598601

https://www.alibabacloud.com/vi/product/kubernetes 

https://blog.japan-itworks.vn/vi/github-actions-la-gi-1404