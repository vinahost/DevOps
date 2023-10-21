# Go nhận giá trị đầu vào

Trong bài viết trước, chúng ta đã tạo chương trình **Go** đầu tiên của mình độc lập và các phần nhận thông tin đầu vào của người dùng đã được tạo dưới dạng các biến trong mã với giá trị cho sẵn. Bây giờ chúng ta muốn hỏi người dùng cho đầu vào của họ để cung cấp cho biến giá trị để có thể in ra thông điệp cuối cùng.

## Nhận thông tin đầu vào từ người dùng

Trước khi làm điều đó, chúng ta hãy xem xét lại ứng dụng của mình và xem qua các biến mà chúng ta muốn kiểm tra trước khi nhận thông tin đầu vào của người dùng.

Hôm qua, chúng ta đã hoàn thành mã giống như file [example04.go](/Scripts/GoLang/example04.go), chúng ta đã xác định thủ công các biến và hằng số `challenge, daystotal, dayscomplete`.

Bây giờ hãy thêm một biến mới có tên là `TwitterName`, bạn có thể tìm thấy mã mới này tại [Input01.go](/Scripts/GoLang/Input01.go) và đây sẽ là đầu ra nếu chạy mã này.

![Go nhận giá trị đầu vào](/Image/Go-Input01.png)

Chúng ta sẽ cần phải thay đổi số ngày đã hoàn thành mỗi ngày và biên dịch lại mã nếu nó này là mã cứng (**hard code**), điều này có vẻ không tốt cho lắm

Nhận thông tin đầu vào của người dùng, giá trị có thể là tên và số ngày đã hoàn thành. Để thực hiện việc này, chúng ta có thể sử dụng một hàm khác từ bên trong gói `fmt`.

Các chức năng khác nhau cho đầu vào và đầu ra được định dạng (**I/O**) có trong gói `fmt`

- In ra tin nhắn
- Nhận thông tin đầu vào của người dùng
- Ghi thành tệp

Thay vì chỉ định giá trị của một biến, chúng ta sẽ yêu cầu người dùng nhập thông tin đầu vào của họ.

```
fmt.Scan(&TwitterName)
```

Lưu ý việc sử dụng `&` trước biến. Đây được gọi là một con trỏ mà chúng ta sẽ đề cập trong phần tiếp theo.

Trong mã của chúng ta [Input02.go](/Scripts/GoLang/Input02.go), bạn có thể thấy rằng chúng ta yêu cầu người dùng nhập hai biến, `TwitterName` và` DaysCompleted`

Bây giờ hãy chạy chương trình và bạn thấy có đầu vào cho cả hai điều trên.

![Go nhận giá trị đầu vào](/Image/Go-Input01.png)

Được rồi, thật tuyệt, chúng ta đã nhận được thông tin người dùng nhập vào và in một tin nhắn nhưng vẫn còn việc cho biết còn bao nhiêu ngày trong thử thách của mình.

Để thực hiện điều đó, chúng ta sẽ tạo một biến có tên gọi là `remainingDays` và khai báo giá trị này trong mã của mình là `90`, sau đó, thay đổi giá trị này để in ra những ngày còn lại sau khi nhận được thông tin người dùng nhập cho `DaysCompleted`. Có thể làm điều này với thay đổi biến đơn giản này.

```
remainingDays = remainingDays - DaysCompleted
```

Bạn có thể xem chương trình đã hoàn thành tại đây [Input03.go](/Scripts/GoLang/Input03.go).

Nếu chạy chương trình này, bạn có thể thấy phép tính đơn giản được thực hiện dựa trên đầu vào của người dùng và giá trị của `remainingDays`

![Go nhận giá trị đầu vào](/Image/Go-Input03.png)

## Con trỏ là gì? (Biến đặc biệt)

Con trỏ là một biến (đặc biệt) trỏ đến địa chỉ bộ nhớ của một biến khác.

Bạn có thể đọc kỹ hơn tại [geeksforgeeks](https://www.geeksforgeeks.org/pointers-in-golang/)

Hãy đơn giản hóa mã và xem chương trình thay đổi khi có và không có `&` trước một các lệnh in của chúng ta, nó sẽ in ra địa chỉ bộ nhớ của con trỏ. Tôi đã thêm ví dụ ở đây. [Input04.go](/Scripts/GoLang/Input04.go)

Dưới đây là kết quả chạy mã này

![Go nhận giá trị đầu vào](/Image/Go-Input01.png)

## Tài liệu tham khảo

- [StackOverflow 2021 Developer Survey](https://insights.stackoverflow.com/survey/2021)
- [Why we are choosing Golang to learn](https://www.youtube.com/watch?v=7pLqIIAqZD4&t=9s)
- [Jake Wright - Learn Go in 12 minutes](https://www.youtube.com/watch?v=C8LgvuEBraI&t=312s)
- [Techworld with Nana - Golang full course - 3 hours 24 mins](https://www.youtube.com/watch?v=yyUHQIec83I)
- [**NOT FREE** Nigel Poulton Pluralsight - Go Fundamentals - 3 hours 26 mins](https://www.pluralsight.com/courses/go-fundamentals)
- [FreeCodeCamp - Learn Go Programming - Golang Tutorial for Beginners](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=1025s)
- [Hitesh Choudhary - Complete playlist](https://www.youtube.com/playlist?list=PLRAV69dS1uWSR89FRQGZ6q9BR2b44Tr9N)