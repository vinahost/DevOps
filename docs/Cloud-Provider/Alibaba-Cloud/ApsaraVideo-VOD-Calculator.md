# ApsaraVideo VOD là gì?

- **Alibaba Cloud ApsaraVideo VOD (Video on Demand)** là một dịch vụ đám mây cho phép người dùng lưu trữ, quản lý và phân phối video trực tuyến trên nền tảng điện toán đám mây của **Alibaba Cloud**.

- **ApsaraVideo VOD** cung cấp các tính năng như lưu trữ video, mã hóa video, phân phối, phát trực tiếp và phát lại định dạng video, cho phép người dùng tùy chỉnh, chi tiết và quản lý các nội dung video trên đám mây.

### Phương thức thanh toán của ApsaraVideo VOD:

Chi phí sử dụng **ApsaraVideo VOD** được là tổng các chi phí **sử dụng bộ nhớ**, **transcoding**, **phân phối nội dung CDN** và chi phí **Short Video SDK** (nếu có).

**1) Chi phí sử dụng bộ nhớ (Storage Service) sẽ bao gồm:**

Chi phí bộ nhớ (chi phí hiện hành như bảng bên dưới, đơn vị tính: **USD/GB/Tháng**)

![ApsaraVideo VOD Calculator](/Image/ApsaraVideo-VOD-Calculator01.png)

Chi phí lưu lượng bộ nhớ phát sinh dùng trên **OSS Downstream** (**Upstream Trafic** là miễn phí) – chi phí hiện hành như bảng bên dưới (đơn vị tính **USD/GB**)

![ApsaraVideo VOD Calculator](/Image/ApsaraVideo-VOD-Calculator02.png)

**2) Chi phí transcoding**

Chi phí **transcoding** được tính theo phút, làm tròn đến hai chữ số thập phân. Video dưới 1 giây được tính là **0,02 phút**. Chi phí **transcoding** phụ thuộc vào loại **codec**, **độ phân giải** và **khu vực**.

![ApsaraVideo VOD Calculator](/Image/ApsaraVideo-VOD-Calculator03.png)

**3) Chi phí phân phối nội dung CDN**

![ApsaraVideo VOD Calculator](/Image/ApsaraVideo-VOD-Calculator04.png)

**4) Chi phí Short Video SDK**

![ApsaraVideo VOD Calculator](/Image/ApsaraVideo-VOD-Calculator05.png)

**5) Chi phí truyền dữ liệu ra bên ngoài (Outbound Data Transfer)**

![ApsaraVideo VOD Calculator](/Image/ApsaraVideo-VOD-Calculator06.png)

#### Sử dụng Alibaba VOD Calculator để tính toán chi phí giải pháp ApsaraVideo VOD của bạn:

Việc tính toán chi phí trong **Alibaba Cloud ApsaraVideo VOD** là tương đối phức tạp và liên quan đến nhiều yếu tố, bạn có thể sử dụng công cụ tính toán có sẵn của **Alibaba Cloud** dành cho **ApsaraVideo VOD**.
Để sử dụng công cụ này, truy cập vào trang **Pricing** của **ApsaraVideo VOD**, chọn **Pricing Calculator** hoặc truy cập vào [link sau](https://yida.alibaba-inc.com/o/vodcalculator#/)

![ApsaraVideo VOD Calculator](/Image/ApsaraVideo-VOD-Calculator07.png)

Bạn sẽ vào được giao diện chính của **VOD Calculator**. Nếu ngôn ngữ đang là tiếng *Trung Quốc*, ở góc phải phía trên màn hình bạn chọn **EN** để chuyển về ngôn ngữ *Tiếng Anh*. 

**ApsaraVideo VOD Calculator** hỗ trợ tính toán chi phí các mục **Transcoding**, **Storage** và **CDN Service**.

![ApsaraVideo VOD Calculator](/Image/ApsaraVideo-VOD-Calculator08.png)

Tại trang chủ, nhập vào các thông số các video của bạn:

- **No of Video**: số lượng video.
- **Average video in mins**: số phút trung bình (thời lượng trung bình của video), thông số này để tính **Transcoding Mins (Start)**.
- **Average viewer**: lượt xem trung bình của video.
- **Average view duration in mins**: thời lượng xem video trung bình của người xem.
- **New video in mins**: thời lượng video mới. Trường này là trường không bắt buộc nhập.
- **Media Center**: chọn vị trí phát trung tâm của bạn. Cước phí cho một số dịch vụ sẽ được tính dựa trên vị trí này.
- **Quality**: Độ phân giải mong muốn khi phát video. Độ phân giải thấp nhất là 480p và cao nhất là 4K với những mức giá tương ứng.

**Cách chương trình tính toán**: **ApsaraVideo VOD** sẽ thực hiện tính toán **trafic** (số GB) sử dụng; **Storage GB** và **Transcoding Minutes** (để nhân với đơn giá ở bước sau) dựa trên các nguyên tắc như sau:

- **Transcoding Minute** = No.Video * Average Video in mins
- **Transcoding Start** = Transcoding Minute * Price (Price xem tại mục 2.2).
- **Storage GB (Start)** = Transcoding Minute * số GB/phút (xem ở bảng 3.1)
- - **Storage Fee** miễn phí với số GB từ 50 trở xuống; số GB > 50 được tính theo giá ở phần 2.1 (tính cả phần <50).
- **Traffic GB** (xấp xỉ) = No. Video Average Viewer Average View Duration in minute* số GB/phút (xem ở bảng 3.1)
- Phí **Traffic** được tính theo **CDN Service** theo số GB và khu vực (theo giá ở mục 2.3)
- Ngoài ra bạn có thể thiết lập **Region Split** để chia lượng **Trafic** GB về từng khu vực (nếu có nhu cầu).
 
![ApsaraVideo VOD Calculator](/Image/ApsaraVideo-VOD-Calculator09.png)

Sau khi các thông số về lượng sử dụng trên được tính, hệ thống sẽ nhân với giá **list** để ra hóa đơn tổng thể ứng với lượng sử dụng của bạn.

Ví dụ, nhập các thông số sau:

![ApsaraVideo VOD Calculator](/Image/ApsaraVideo-VOD-Calculator010.png)

![ApsaraVideo VOD Calculator](/Image/ApsaraVideo-VOD-Calculator011.png)

Lúc này, các thông số sẽ được tính toán như sau:

- **Transcoding Mins (Start)** = No of Video Average video in mins = 1 15 = 15
- **Traffic GB** = No of Video Average viewer Average view durrtion in mins số GB/phút = 1 70 10 0.042915 = 30.04
- **Storage GB** = No of Video Average video in mins số GB/phút
= 1 15 0.042915 = 0.64
- **Asia Pacific – AP1 (GB)** do ta đã nhập 100%: = 100% * Traffic GB

Từ đó giá tiền sẽ được tính:

- **Chi phí Transcoding (Transcoding (Start))** = Transcoding Mins * Price
= 15 * 0.0375 = 0.5625
- **Chi phí Traffic** = Traffic by Region Price = 30.04 0.081 = 2.43324

=> **Tổng chi phí = 0.5625 + 2.43324 = 2.99574 (đơn vị USD)**

![ApsaraVideo VOD Calculator](/Image/ApsaraVideo-VOD-Calculator012.png)
    
*Author*: **Vo Thi Phuong Anh**

*Solution Architect* - **Alibaba Cloud Intelligence**

### Tham khảo

- [Alibaba Cloud Partner](https://vinahost.vn/alibaba-cloud-partner/)
- [Apsara Video Cloud | CDN Calculator](https://yida.alibaba-inc.com/o/vodcalculator#/)