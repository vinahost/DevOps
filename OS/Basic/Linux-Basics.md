# Một số lệnh Linux phổ biến được sử dụng trong DevOps:

#### Lệnh cho Debian - Ubuntu ...
- apt-get install: Lệnh này được sử dụng để cài đặt các gói phần mềm.
- apt-get remove: Lệnh này được sử dụng để gỡ cài đặt các gói phần mềm.
- apt-get update: Lệnh này được sử dụng để cập nhật danh sách các gói phần mềm có sẵn.
- apt-get upgrade: Lệnh này được sử dụng để nâng cấp các gói phần mềm hiện có.

#### Lệnh cho Centos - Redhat ...
- yum install: Lệnh này được sử dụng để cài đặt các gói phần mềm trên hệ điều hành Fedora hoặc Red Hat.
- yum remove: Lệnh này được sử dụng để gỡ cài đặt các gói phần mềm trên hệ điều hành Fedora hoặc Red Hat.
- yum update: Lệnh này được sử dụng để cập nhật danh sách các gói phần mềm có sẵn trên hệ điều hành Fedora hoặc Red Hat.
- yum upgrade: Lệnh này được sử dụng để nâng cấp các gói phần mềm hiện có trên hệ điều hành Fedora hoặc Red Hat.
- dnf install: Tải xuống và cài đặt một gói phần mềm trên hệ thống Fedora 30 trở lên hoặc RHEL 8 trở lên.

#### Lệnh chung trên Linux
- wget: Lệnh này được sử dụng để tải xuống các tệp từ internet.
- curl: Lệnh này cũng được sử dụng để tải xuống các tệp từ internet, nhưng nó có nhiều tính năng hơn lệnh wget.
- tar: Lệnh này được sử dụng để giải nén các tệp đã được nén.
- zip: Lệnh này được sử dụng để nén các tệp.
- ssh: Lệnh này được sử dụng để kết nối với các máy tính từ xa.
- scp: Lệnh này được sử dụng để sao chép các tệp giữa các máy tính từ xa.
- rsync: Lệnh này cũng được sử dụng để sao chép các tệp giữa các máy tính từ xa, nhưng nó có nhiều tính năng hơn lệnh scp.
- grep: Lệnh này được sử dụng để tìm kiếm các chuỗi văn bản trong các tệp.
- find: Lệnh này được sử dụng để tìm các tệp trong một thư mục.
- sed: Lệnh này chỉnh sửa văn bản trong một tệp.
- awk: Lệnh này xử lý văn bản trong một tệp theo cách được xác định trước.
- sort: Lệnh này được sử dụng để sắp xếp các tệp theo thứ tự.
- uniq: Lệnh này được sử dụng để loại bỏ các bản sao của các dòng trong một tệp.
- wc: Lệnh này được sử dụng để đếm số lượng dòng, từ và byte trong một tệp.
- ls: Lệnh này liệt kê các tệp và thư mục trong thư mục hiện tại.
- cd: Lệnh này thay đổi thư mục hiện tại.
- cp: Lệnh này sao chép một tệp hoặc thư mục.
- mkdir: Lệnh này tạo một thư mục mới.
- mv: Lệnh này di chuyển hoặc đổi tên một tệp hoặc thư mục.
- rm: Lệnh này xóa một tệp hoặc thư mục.

#### Một số lệnh cho các phần mềm trên Linux
- pip install: Tải xuống và cài đặt một gói phần mềm Python.
- npm install: Tải xuống và cài đặt một gói phần mềm JavaScript.
- composer install: Tải xuống và cài đặt một gói phần mềm PHP.
- gradlew build: Xây dựng một dự án Gradle.
- maven install: Xây dựng một dự án Maven.
- sbt compile: Xây dựng một dự án SBT.

#### Một số lệnh dành cho Docker
- docker build: Xây dựng một hình ảnh Docker.
- docker run: Chạy một hình ảnh Docker.
- docker push: Đẩy một hình ảnh Docker lên kho lưu trữ Docker.

#### Một số lệnh dành cho Kubernetes
- kubectl create deployment: Tạo một triển khai Kubernetes.
- kubectl scale deployment: Thay đổi quy mô một triển khai Kubernetes.
- kubectl rollout restart deployment: Khởi động lại một triển khai Kubernetes.
- kubectl get pods: Lấy danh sách các pod Kubernetes.
- kubectl logs pod: Lấy nhật ký của một pod Kubernetes.
- kubectl describe pod: Mô tả chi tiết về một pod Kubernetes.
- kubectl get services: Lấy danh sách các dịch vụ Kubernetes.
- kubectl expose deployment: Tạo một dịch vụ cho một triển khai Kubernetes.
- kubectl get nodes: Lấy danh sách các nút Kubernetes.
- kubectl get nodes -o wide: Lấy danh sách các nút Kubernetes với thông tin chi tiết.
- kubectl get cluster-autoscaler: Lấy trạng thái của trình tự mở rộng cụm Kubernetes.
- kubectl get pods --all-namespaces: Lấy danh sách tất cả các pod Kubernetes trong tất cả các không gian tên.
- kubectl logs pod --all-namespaces: Lấy nhật ký của tất cả các pod Kubernetes trong tất cả các không gian tên.
- kubectl describe pod --all-namespaces: Mô tả chi tiết về tất cả các pod Kubernetes trong tất cả các không gian tên.
- kubectl get services --all-namespaces: Lấy danh sách tất cả các dịch vụ Kubernetes trong tất cả các không gian tên.
- kubectl expose deployment --all-namespaces: Tạo một dịch vụ cho một triển khai Kubernetes trong tất cả các không gian tên.
- kubectl get nodes --all-namespaces: Lấy danh sách tất cả các nút Kubernetes trong tất cả các không gian tên.
- kubectl get nodes --all-namespaces -o wide: Lấy danh sách tất cả các nút Kubernetes trong tất cả các không gian tên với thông tin chi tiết.
- kubectl get cluster-autoscaler --all-namespaces: Lấy trạng thái của trình tự mở rộng cụm Kubernetes trong tất cả các không gian tên.

Trường hợp cần thông tin chi tiết, các bạn có thể sử dụng lệnh *man* để có thêm thông tin của các lệnh tương ứng, cấu trúc lệnh man <command>

    man ls

Đây chỉ là một số lệnh Linux phổ biến được sử dụng trong DevOps. Có nhiều lệnh khác có thể được sử dụng, tùy thuộc vào các nhiệm vụ cụ thể mà bạn đang thực hiện.

**Tham khảo**

- https://github.com/trinib/Linux-Bash-Commands
- https://github.com/sk3pp3r/cheat-sheet-pdf