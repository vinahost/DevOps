# Snapshot và Backup

## Snapshot là gì ?

Trong hệ thống lưu trữ, một **snapshot** là một bản sao của dữ liệu tại một thời điểm nhất định. **Snapshot** được sử dụng để tạo bản sao của dữ liệu, khôi phục dữ liệu bị mất hoặc bị hỏng, và thử nghiệm các thay đổi đối với dữ liệu mà không làm ảnh hưởng đến dữ liệu gốc.

**Snapshot** được tạo bằng cách sao chép dữ liệu từ một nguồn sang một đích. Nguồn có thể là một ổ đĩa cứng, một phân vùng, một tập tin hoặc một cơ sở dữ liệu. Đích có thể là một ổ đĩa cứng khác, một phân vùng khác, một tập tin khác hoặc một cơ sở dữ liệu khác.

**Snapshot** có thể được tạo theo hai cách:

- **Theo yêu cầu**: Snapshot được tạo khi cần thiết, chẳng hạn như khi bạn muốn tạo bản sao của dữ liệu để khôi phục dữ liệu bị mất hoặc bị hỏng.

- **Tự động**: Snapshot được tạo theo lịch trình, chẳng hạn như hàng ngày, hàng tuần hoặc hàng tháng.

**Snapshot** có thể được sử dụng để:

- **Tạo bản sao của dữ liệu**: Snapshot có thể được sử dụng để tạo bản sao của dữ liệu để bảo vệ dữ liệu khỏi bị mất hoặc bị hỏng.

- **Khôi phục dữ liệu bị mất hoặc bị hỏng**: Snapshot có thể được sử dụng để khôi phục dữ liệu bị mất hoặc bị hỏng.

- **Thử nghiệm các thay đổi đối với dữ liệu**: Snapshot có thể được sử dụng để thử nghiệm các thay đổi đối với dữ liệu mà không làm ảnh hưởng đến dữ liệu gốc.

**Snapshot** là một công cụ hữu ích để bảo vệ dữ liệu và thử nghiệm các thay đổi đối với dữ liệu. **Snapshot** có thể được sử dụng trên nhiều hệ thống lưu trữ khác nhau, bao gồm:

- Hệ thống lưu trữ cục bộ
- Hệ thống lưu trữ mạng (NAS)
- Hệ thống lưu trữ đám mây (cloud storage)

Nếu bạn đang tìm kiếm một cách hiệu quả để bảo vệ hệ thống và dữ liệu của mình, thì **snapshot** là một lựa chọn tốt. **Snapshot** có thể giúp bạn khôi phục hệ thống hoặc tệp dữ liệu về trạng thái ban đầu nếu có sự cố xảy ra. Snapshot cũng có thể được sử dụng để tạo một bản sao của hệ thống hoặc tệp dữ liệu để thử nghiệm hoặc phát triển.

## Backup là gì?

**Backup** là một bản sao dữ liệu được lưu trữ ở một vị trí khác với bản gốc. **Backup** được sử dụng để khôi phục dữ liệu trong trường hợp bản gốc bị mất hoặc hư hỏng.

Có nhiều cách khác nhau để tạo **backup**, bao gồm:

- Sao chép dữ liệu thủ công sang một thiết bị lưu trữ khác
- Sử dụng phần mềm backup tự động
- Lưu trữ dữ liệu trên đám mây

**Backup** là một phần quan trọng của quy trình quản lý dữ liệu. Nó giúp bạn bảo vệ dữ liệu khỏi bị mất hoặc hư hỏng do các nguyên nhân như:

- Hệ thống máy tính bị hỏng
- Virus tấn công
- Lỗi người dùng
- Thiên tai

**Backup** thường được thực hiện theo định kỳ, chẳng hạn như hàng ngày, hàng tuần, hàng tháng hoặc hàng năm. Tần suất **backup** phụ thuộc vào mức độ quan trọng của dữ liệu và khả năng mất dữ liệu.

**Backup** là một cách hiệu quả để bảo vệ dữ liệu của bạn. Nó giúp bạn khôi phục dữ liệu trong trường hợp bản gốc bị mất hoặc hư hỏng. **Backup** cũng giúp bạn giảm thiểu chi phí và thời gian khôi phục dữ liệu.

## Sự khác nhau giữa Backup và Snapshot

![Backup và Snapshot](/Image/Snapshot-vs-Backup.jpg)

**Snapshot** và **backup** đều là các bản sao dữ liệu, nhưng chúng có một số điểm khác biệt chính. **Snapshot** là một bản sao của dữ liệu tại một thời điểm nhất định, trong khi **backup** là một bản sao toàn bộ của dữ liệu. **Snapshot** được sử dụng để khôi phục dữ liệu về trạng thái trước khi nó bị thay đổi, trong khi **backup** được sử dụng để khôi phục dữ liệu đã bị mất hoặc bị hư hỏng.

**Snapshot** được tạo nhanh hơn **backup**, vì chúng chỉ sao chép những thay đổi đối với dữ liệu kể từ lần **snapshot** trước. **Snapshot** cũng chiếm ít dung lượng hơn **backup**, vì chúng chỉ lưu trữ dữ liệu đã thay đổi.

**Snapshot** thường được sử dụng để tạo các điểm khôi phục, trong khi **backup** thường được sử dụng để lưu trữ dữ liệu lâu dài. **Snapshot** được sử dụng tốt nhất cho các ứng dụng cần khả năng khôi phục nhanh chóng, trong khi **backup** được sử dụng tốt nhất cho các ứng dụng cần lưu trữ dữ liệu lâu dài.

Dưới đây là bảng tóm tắt sự khác biệt giữa **snapshot** và **backup**:

|Feature|	Snapshot|	Backup|
|-------|-----------|---------|
|Thời gian tạo|	Tại một thời điểm nhất định|	Theo định kỳ|
|Kích thước|	Bằng kích thước của dữ liệu gốc|	Có thể lớn hơn hoặc nhỏ hơn kích thước của dữ liệu gốc|
|Mục đích sử dụng|	Khôi phục dữ liệu về trạng thái ban đầu|	Khôi phục dữ liệu trong trường hợp bản gốc bị mất hoặc hư hỏng|
|Tần suất tạo|	Có thể được tạo thường xuyên hoặc ít thường xuyên hơn|	Được tạo theo định kỳ|
|Chi phí|	Có thể tốn kém hơn backup|	Có thể rẻ hơn snapshot|

Dưới đây là một số ví dụ về cách sử dụng **snapshot** và **backup**:

- Bạn có thể sử dụng snapshot để tạo điểm khôi phục trước khi cài đặt một phần mềm mới. Nếu phần mềm mới không tương thích với hệ thống của bạn, bạn có thể khôi phục hệ thống về trạng thái trước khi cài đặt phần mềm mới bằng cách sử dụng snapshot.

- Bạn có thể sử dụng backup để lưu trữ dữ liệu của mình lâu dài. Nếu hệ thống của bạn bị hỏng hoặc bị mất, bạn có thể khôi phục dữ liệu của mình từ backup.

Nhìn chung, **snapshot** và **backup** đều là những cách hiệu quả để bảo vệ dữ liệu của bạn. Tuy nhiên, **snapshot** phù hợp hơn với việc khôi phục dữ liệu về trạng thái ban đầu, trong khi **backup** phù hợp hơn với việc khôi phục dữ liệu trong trường hợp bản gốc bị mất hoặc hư hỏng.