# Giao thức TCP và UDP là gì ?

**TCP** và **UDP** là hai giao thức truyền tải được sử dụng trong mô hình **TCP/IP**. **TCP** là giao thức hướng kết nối, trong khi **UDP** là giao thức không hướng kết nối.

![TCP vs UDP](/Image/TCP-UDP.png)

## Giao thức TCP là gì?

**Transmission Control Protocol (TCP)** là giao thức tiêu chuẩn trên **Internet** đảm bảo trao đổi thành công các gói dữ liệu giữa các thiết bị qua mạng. **TCP** là giao thức truyền tải cơ bản cho nhiều loại ứng dụng, bao gồm máy chủ web và trang **web**, ứng dụng **email**, **FTP** và các ứng dụng ngang hàng.

**TCP** hoạt động với giao thức **Internet** (**IP**) để chỉ định cách dữ liệu được trao đổi trực tuyến. **IP** chịu trách nhiệm gửi từng gói đến đích của nó, trong khi **TCP** đảm bảo rằng các **byte** được truyền theo thứ tự mà chúng được gửi mà không có lỗi hoặc thiếu sót nào. Hai giao thức kết hợp với nhau được gọi là **TCP/IP**.

### TCP hoạt động như thế nào?

**TCP** hoạt động theo tiến trình bắt tay 3 bước (**3 way handshake**). Tiến trình này hoạt động như sau:

![TCP 3 way handshake](/Image/what-is-tcp-3-way-handshake.png)

- Máy khách gửi cho máy chủ một gói SYN — một yêu cầu kết nối từ port nguồn của nó đến port đích đến của máy chủ.
- Máy chủ phản hồi bằng gói SYN/ACK, xác nhận việc nhận được yêu cầu kết nối.
- Máy khách nhận gói SYN/ACK và trả lời bằng gói ACK của chính nó.

Sau khi kết nối được thiết lập, **TCP** hoạt động bằng cách chia nhỏ dữ liệu đã truyền thành các **segment** (phân đoạn), mỗi **segment** được đóng gói thành một gói dữ liệu và được gửi đến đích của nó.

### Cấu trúc của TCP Header

![TCP Header](/Image/TCP-Header.png)

- Source port (16 bit): Số cổng của thiết bị gửi.
- Destination port (16 bit): Số cổng của thiết bị nhận.
- Sequence number (32 bit): Dùng để đánh số thứ tự gói tin ( từ số sequense nó sẽ tính ra được số byte đã được truyền)
- Acknowledgment number (32 bit): Dùng để báo đã nhận được gói tin nào và mong nhận được byte mang số thứ tự nào tiếp theo.
- DO (4 bit): Cho biết toàn bộ header dài bao nhiêu tính theo đơn vị word (1 Word = 4 byte).
- RSV (4 bit): Đều được thiết lập bằng 0.
- Flags (9 bit): Được sử dụng để thiết lập kết nối, gửi dữ liệu và chấm dứt kêt nối.
- - URG: Ưu tiên dữ liệu này hơn các dữ liệu khác.
- - ACK: Được sử dụng để xác nhận.
- - PSH: Segment yêu cầu chức năng push.
- - RST: Thiết lập lại kết nối.
- - SYN: Được sử dụng để đặt số thứ tự ban đầu.
- - FIN: Kết thúc kết nối TCP.

- Windows (16 bit): Số lượng byte được thiết bị sẵn sàng tiếp nhận.
- Checksum (16 bit): Kiểm tra lỗi của toàn bộ TCP segment.
- Urgent pointer (16 bit): Sử dụng trong trường hợp cần ưu tiên dữ liệu.
- Options (tối đa 32 bit): Cho phép thêm vào TCP các tính năng khác.

Để xem các trường này hoạt động, các bạn có thể sử dụng công cụ **Wireshard**.

![TCP 3 way handshake](/Image/giao-thuc-tcp-la-gi-3.png)

## Giao thức UDP là gì?

**UDP (User Datagram Protocol)** là một trong những giao thức cốt lõi của giao thức **TCP/IP**. Dùng **UDP**, chương trình trên mạng máy tính có thể gửi những dữ liệu ngắn được gọi là **datagram** tới máy khác. **UDP** không cung cấp sự tin cậy và thứ tự truyền nhận mà **TCP** làm; các gói dữ liệu có thể đến không đúng thứ tự hoặc bị mất mà không có thông báo. Tuy nhiên **UDP** nhanh và hiệu quả hơn đối với các mục tiêu như kích thước nhỏ và yêu cầu khắt khe về thời gian. Do bản chất không trạng thái của nó nên nó hữu dụng đối với việc trả lời các truy vấn nhỏ với số lượng lớn người yêu cầu.

Những ứng dụng phổ biến sử dụng **UDP** như **DNS** (*Domain Name System*), ứng dụng **streaming media**, **Voice over IP**, **Trivial File Transfer Protocol (TFTP)**, và **game** trực tuyến.

### UDP hoạt động như thế nào?

Giao thức **UDP** hoạt động tương tự như **TCP**, nhưng nó bỏ qua quá trình kiểm tra lỗi. Khi một ứng dụng sử dụng giao thức **UDP**, các gói tin được gửi cho bên nhận và bên gửi không phải chờ để đảm bảo bên nhận đã nhận được gói tin, do đó nó lại tiếp tục gửi gói tin tiếp theo. Nếu bên nhận bỏ lỡ một vài gói tin **UDP**, họ sẽ mất vì bên gửi không gửi lại chúng. Do đó thiết bị có thể giao tiếp nhanh hơn.

![UDP](/Image/file-transfer-using-udp.png)

### Cấu trúc UDP Header

![UDP Header](/Image/UDP-Header.png)

- Source port: Số cổng của thiết bị gửi. Trường này có thể đặt là 0 nếu máy tính đích đến không cần trả lời người gửi.
- Destination port: Số cổng của thiết bị nhận.
- Length: Xác định chiều dài của toàn bộ datagram: phần header và dữ liệu. Chiều dài tối thiểu là 8 byte khi gói tin không có dữ liệu, chỉ có header.
- Checksum: Kiểm tra lỗi của phần header và dữ liệu. Việc sử dụng checks.

## Bảng tóm tắt sự khác biệt chính giữa TCP và UDP

|Tính năng|	TCP|	UDP|
|---------|----|-------|
|Hướng kết nối|	Có|	Không|
|Tính đáng tin cậy|	Có|	Không|
|Tính toàn vẹn|	Có|	Không|
|Tính có thể tái tạo|	Có|	Không|
|Kiểm soát luồng|	Có|	Không|
|Tốc độ|	Chậm hơn|	Nhanh hơn|
|Sử dụng|	Các ứng dụng yêu cầu tính đáng tin cậy cao, chẳng hạn như FTP, SSH và HTTP|	Các ứng dụng không yêu cầu tính đáng tin cậy cao, chẳng hạn như phát trực tiếp video và âm thanh|

## Tham khảo

- https://blog.vinahost.vn/giao-thuc-tcp-la-gi/
- https://blog.vinahost.vn/giao-thuc-udp-la-gi/