# Cài phần mềm trên Linux

Cài đặt phần mềm trên **Linux** có thể được thực hiện theo nhiều cách khác nhau, tùy thuộc vào loại phần mềm bạn muốn cài đặt và hệ thống **Linux** bạn đang sử dụng.

![Cài phần mềm trên Linux](/Image/Cai-phan-mem-tren-Linux.png)

Một số cách phổ biến nhất để cài đặt phần mềm trên **Linux** bao gồm:

- Sử dụng trình quản lý gói: Trình quản lý gói là một công cụ được sử dụng để cài đặt, gỡ bỏ và quản lý các gói phần mềm trên **Linux**. Các trình quản lý gói phổ biến nhất bao gồm **APT** (*Advanced Package Tool*) trên các hệ thống dựa trên **Debian** và **RPM** (*Red Hat Package Manager*) trên các hệ thống dựa trên **Red Hat**.
- Chạy tệp nhị phân (binary): Nếu phần mềm bạn muốn cài đặt có sẵn dưới dạng tệp nhị phân, bạn có thể chạy tệp đó để cài đặt.
- Cài đặt từ nguồn (source code): Nếu phần mềm bạn muốn cài đặt không có sẵn dưới dạng tệp nhị phân, bạn có thể cài đặt từ nguồn. Điều này liên quan đến việc tải xuống mã nguồn của phần mềm và biên dịch nó trên hệ thống của bạn.

Dưới đây là một số ví dụ về cách cài đặt phần mềm trên Linux bằng các phương pháp khác nhau:

    # Cài đặt gói bằng APT
    sudo apt install software-name

    # Cài đặt gói bằng YUM
    sudo yum install software-name

    # Cài đặt bằng file nhị phân (binary)
    chmod +x install.bin
    ./install.bin

    # Cài đặt phần mềm từ source
    git clone https://github.com/username/software_repo.git
    cd software_repo
    make
    sudo make install

Khi cài đặt phần mềm trên **Linux**, điều quan trọng là phải đảm bảo rằng bạn cài đặt phiên bản phần mềm tương thích với hệ thống **Linux** của bạn. Bạn cũng nên đảm bảo rằng bạn đã cài đặt tất cả các phụ thuộc cần thiết cho phần mềm.

## Lệnh apt trên Debian - Ubuntu

Lệnh `apt` là một công cụ quản lý gói dòng lệnh được sử dụng để cài đặt, gỡ bỏ, cập nhật và quản lý các gói phần mềm trên các hệ thống dựa trên **Debian**, bao gồm **Ubuntu** và **Linux Mint**. Nó là một phần của hệ thống quản lý gói **APT** (**Advanced Package Tool**), cung cấp một cách thống nhất để quản lý các gói phần mềm.

Lệnh `apt` có thể được sử dụng để cài đặt các gói từ kho lưu trữ **Debian** chính thức, cũng như từ các kho lưu trữ của bên thứ ba. Nó cũng có thể được sử dụng để gỡ bỏ các gói, cập nhật các gói hiện có và quản lý các phụ thuộc của gói.

Lệnh `apt` là một công cụ mạnh mẽ và linh hoạt có thể được sử dụng để quản lý các gói phần mềm trên các hệ thống dựa trên **Debian**. Nó là một công cụ quan trọng đối với bất kỳ ai sử dụng một hệ thống dựa trên **Debian**, và nó có thể được sử dụng để dễ dàng cài đặt, gỡ bỏ, cập nhật và quản lý các gói phần mềm.

### Lệnh apt phổ biến

Một số lệnh `apt` phổ biến:

- apt install package_name: Cài đặt gói phần mềm có tên package_name.
- apt remove package_name: Gỡ bỏ gói phần mềm có tên package_name.
- apt update: Cập nhật danh sách các gói phần mềm có sẵn từ kho lưu trữ.
- apt upgrade: Cập nhật tất cả các gói phần mềm đã cài đặt lên phiên bản mới nhất.
- apt dist-upgrade: Cập nhật hệ điều hành lên phiên bản mới nhất.
- apt-cache search keyword: Tìm kiếm các gói phần mềm có chứa từ khóa keyword.
- apt-cache show package_name: Hiển thị thông tin chi tiết về gói phần mềm có tên package_name

### So sánh apt và apt-get

`apt` và `apt-get` đều là các công cụ dòng lệnh được sử dụng để quản lý các gói phần mềm trên các hệ thống dựa trên **Debian**, bao gồm **Ubuntu** và **Linux Mint**. Cả hai đều dựa trên **APT** (**Advanced Package Tool**), một hệ thống quản lý gói cung cấp một cách thống nhất để cài đặt, gỡ bỏ, cập nhật và quản lý các gói phần mềm.

Tuy nhiên, `apt` và `apt-get` có một số khác biệt chính. `Apt` là một công cụ hiện đại hơn, được thiết kế để dễ sử dụng hơn và cung cấp nhiều tính năng hơn. `Apt-get` là một công cụ cũ hơn, nhưng vẫn được sử dụng rộng rãi.

Một số điểm khác biệt chính giữa `apt` và `apt-get`:

- Apt là một công cụ hiện đại hơn, được thiết kế để dễ sử dụng hơn.
- Apt cung cấp nhiều tính năng hơn apt-get, chẳng hạn như khả năng quản lý các kho lưu trữ và các phụ thuộc của gói.
- Apt có thể được sử dụng để giao tiếp với các kho lưu trữ của bên thứ ba, trong khi apt-get không thể.
- Apt là công cụ mặc định trong Ubuntu và Linux Mint, trong khi apt-get là công cụ mặc định trong Debian.

Nhìn chung, `apt` là công cụ quản lý gói tốt hơn `apt-get`. Nó hiện đại hơn, dễ sử dụng hơn và cung cấp nhiều tính năng hơn. Nếu bạn đang sử dụng một hệ thống dựa trên **Debian**, tôi khuyên bạn nên sử dụng `apt` thay vì `apt-get`.

## Lệnh cài đặt trên Centos - Redhat

### Lệnh yum

Lệnh `yum` là một công cụ quản lý gói dòng lệnh được sử dụng để cài đặt, gỡ bỏ, cập nhật và quản lý các gói phần mềm trên các hệ thống dựa trên **Red Hat**, bao gồm **CentOS**, **RHEL**, **Fedora** và **AlmaLinux**. Nó là một phần của hệ thống quản lý gói **YUM** (*Yellow dog Updater, Modified*), cung cấp một cách thống nhất để quản lý các gói phần mềm.

Lệnh `yum` có thể được sử dụng để cài đặt các gói từ kho lưu trữ **Red Hat** chính thức, cũng như từ các kho lưu trữ của bên thứ ba. Nó cũng có thể được sử dụng để gỡ bỏ các gói, cập nhật các gói hiện có và quản lý các phụ thuộc của gói.

Lệnh `yum` là một công cụ mạnh mẽ và linh hoạt có thể được sử dụng để quản lý các gói phần mềm trên các hệ thống dựa trên **Red Hat**. Nó là một công cụ quan trọng đối với bất kỳ ai sử dụng một hệ thống dựa trên **Red Hat**, và nó có thể được sử dụng để dễ dàng cài đặt, gỡ bỏ, cập nhật và quản lý các gói phần mềm.

Một số lệnh `yum` phổ biến:

- yum install package_name: Cài đặt gói phần mềm có tên package_name.
- yum remove package_name: Gỡ bỏ gói phần mềm có tên package_name.
- yum update: Cập nhật tất cả các gói phần mềm đã cài đặt lên phiên bản mới nhất.
- yum list: Liệt kê tất cả các gói phần mềm đã cài đặt trên hệ thống của bạn.
- yum search keyword: Tìm kiếm các gói phần mềm có chứa từ khóa keyword.
- yum info package_name: Hiển thị thông tin chi tiết về gói phần mềm có tên package_name.

### Lệnh dnf

`dnf` là một công cụ quản lý gói dòng lệnh được sử dụng để cài đặt, gỡ bỏ, cập nhật và quản lý các gói phần mềm trên các hệ thống dựa trên **Red Hat**, bao gồm **Fedora**, **CentOS**, **RHEL** và **AlmaLinux**. Nó là một phần của hệ thống quản lý gói **DNF** (*Dnf Package Manager*), là một bản thay thế mới hơn cho **YUM** (*Yellow dog Updater, Modified*).

Lệnh `dnf` có thể được sử dụng để cài đặt các gói từ kho lưu trữ **Red Hat** chính thức, cũng như từ các kho lưu trữ của bên thứ ba. Nó cũng có thể được sử dụng để gỡ bỏ các gói, cập nhật các gói hiện có và quản lý các phụ thuộc của gói.

Lệnh `dnf` là một công cụ mạnh mẽ và linh hoạt có thể được sử dụng để quản lý các gói phần mềm trên các hệ thống dựa trên **Red Hat**. Nó là một công cụ quan trọng đối với bất kỳ ai sử dụng một hệ thống dựa trên **Red Hat**, và nó có thể được sử dụng để dễ dàng cài đặt, gỡ bỏ, cập nhật và quản lý các gói phần mềm.

Lệnh `dnf` được sử dụng trên hệ thống **Fedora 30** trở lên hoặc **RHEL 8** trở lên.

Một số lệnh `dnf` phổ biến:

- dnf install package_name: Cài đặt gói phần mềm có tên package_name.
- dnf remove package_name: Gỡ bỏ gói phần mềm có tên package_name.
- dnf update: Cập nhật tất cả các gói phần mềm đã cài đặt lên phiên bản mới nhất.
- dnf list: Liệt kê tất cả các gói phần mềm đã cài đặt trên hệ thống của bạn.
- dnf search keyword: Tìm kiếm các gói phần mềm có chứa từ khóa keyword.
- dnf info package_name: Hiển thị thông tin chi tiết về gói phần mềm có tên package_name.

### So sánh khác nhau giữa yum và dnf

So sánh khác biệt chính giữa **YUM** và **DNF** bao gồm:

- DNF nhanh hơn YUM.
- DNF có nhiều tính năng hơn YUM, chẳng hạn như khả năng quản lý các kho lưu trữ và các phụ thuộc của gói.
- DNF tương thích với nhiều hệ thống hơn YUM, bao gồm CentOS, RHEL, Fedora và AlmaLinux.

Nhìn chung, `dnf` là một bản nâng cấp đáng kể so với `yum`. Nó nhanh hơn, có nhiều tính năng hơn và tương thích với nhiều hệ thống hơn. Nếu bạn đang sử dụng một hệ thống dựa trên **Red Hat**, tôi khuyên bạn nên sử dụng **DNF** thay vì **YUM**.