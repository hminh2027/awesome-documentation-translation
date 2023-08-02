# Chương 2: Kĩ năng tính nhẩm nhanh
![translation-status](https://img.shields.io/badge/Status-done-green)
Đôi khi trong một buổi phỏng vấn về thiết kế hệ thống, người ta sẽ yêu cầu bạn phải thực hiện các phép tính nhẩm nhanh (back-of-the-envelope estimation*) để ước lượng được sức chứa của một hệ thống hoặc các yêu cầu bắt buộc về mặt hiệu năng của nó. Jeff Dean - Nhân viên Senior của Google đã từng nói: “các phép tính nhẩm nhanh này được tạo ra bằng cách kết hợp các thí nghiệm tưởng tượng của bạn với các con số liên quan tới hiệu năng của hệ thống, nhằm giúp bạn xác định được thiết kế nào đáp ứng tốt các yêu cầu của hệ thống đó”.

Tất nhiên để thực hiện được các phép tính nhẩm nhanh đó thì bạn sẽ cần phải có kiến thức cơ bản trong việc mở rộng hệ thống. Bởi vậy dưới đây sẽ là các chủ đề trọng tâm mà bạn cần phải nắm được, đó là: **power of 2***, **các con số về latency*** và **các con số về availability***.

## Power of 2

Mặc dù khối lượng dữ liệu có thể trở nên khổng lồ trong các hệ thống phân tán, tất cả đều được quy đổi về các phép tính toán cơ bản. Và để các phép tính đó trở nên chính xác thì việc xác định được đơn vị đo khối lượng dữ liệu bằng power of 2 là rất quan trọng. 

Một byte gồm 8 bit liên tục.

Một kí tự ASCII chiếm 1 byte bộ nhớ (8 bit).

Từ đó ta rút ra được bảng tính đơn vị khối lượng dữ liệu như sau:

| Số mũ | Giá trị xấp xỉ | Tên đầy đủ | Tên viết tắt |
| --- | --- | --- | --- |
| 10 | 1 Nghìn | 1 Kilobyte | 1 KB |
| 20 | 1 Triệu | 1 Megabyte | 1 MB |
| 30 | 1 Tỉ | 1 Gigabyte | 1 GB |
| 40 | 1 Nghìn tỉ | 1 Terabyte  | 1 TB |
| 50 | 1 Triệu tỉ | 1 Petabyte | 1 PB |

## Các con số về Latency mà mọi lập trình viên nên biết

Bảng phía dưới đây là thông tin chi tiết về thời gian để một chiếc máy tính thực hiện các hoạt động cơ bản của nó, được công bố bởi Tiến sĩ Dean của Google vào năm 2010. Cho tới hiện tại thì một vài con số đã bị lỗi thời bởi máy tính ngày nay đã trở nên nhanh hơn và mạnh hơn. Tuy nhiên thì những con số đó vẫn đủ để cung cấp cho ta một cái nhìn tổng quát về tốc độ nhanh chậm của các hoạt động khác nhau trong máy tính.

| Tên hoạt động | Tốc độ |
| --- | --- |
| L1 cache reference | 0.5 ns |
| Branch mispredict | 5 ns |
| L2 cache reference | 7 ns |
| Mutex lock/unlock | 100 ns |
| Main memory reference | 100 ns |
| Compress 1K bytes with Zippy | 10.000 ns = 10 μs |
| Send 2K bytes over 1 Gbps network | 20.000 ns = 20 μs |
| Read 1MB sequentially from memory | 250.000 ns = 250 μs |
| Round trip within the same datacenter | 500.000 ns = 500 μs |
| Disk seek | 10.000.000 ns = 10 ms |
| Read 1 MB sequentially from the network | 10.000.000 ns = 10 ms |
| Read 1 MB sequentially from disk | 30.000.000 ns = 30 ms |
| Send packet CA (California) → Netherlands → CA | 150.000.000 ns = 150 ms |

Ghi chú:

——————

ns = nanosecond, μs = microsecond, ms = milisecond

1 ns = 10^-9 giây

1 μs = 10^-6 giây = 1000 ns

1 ms = 10 ^-3 giây = 1000 μs = 1.000.000 ns

Để dễ hình dung hơn những con số đó thì một kĩ sư phần mềm của Google đã xây dựng một công cụ mô phỏng hình ảnh. Công cụ đó cũng sử dụng cả yếu tố về thời gian để tính toán. 

Phía dưới sẽ là hình vẽ biểu diễn chúng.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/20100184-af1d-43d6-9d48-fded72f12cf6/Untitled.png)

Bằng cách phân tích các con số ở hình trên, chúng ta rút ra được các kết luận như sau:

- Bộ nhớ nhanh hơn ổ đĩa
- Nên hạn chế việc tìm kiếm trên ổ đĩa
- Các thuật toán nén đơn giản thường có tốc độ khá nhanh
- Nên nén dữ liệu trước khi gửi qua internet
- Các trung tâm dữ liệu thường ở các vùng khác nhau, bởi vậy nó sẽ tốn thời gian để gửi dữ liệu cho nhau

## Các con số về Availability

**High availability** là khả năng vận hành liên tục trong một khoảng thời gian dài của một hệ thống. **High availability** được tính theo đơn vị phần trăm, tức là 100% tương đương với việc dịch vụ đó có 0 downtime*. Hầu hết các dịch vụ nằm giữa khoảng 99% và 100%.

Một **Service Level Agreement** (SLA) là một cụm thường được dùng cho các nhà cung cấp dịch vụ. Đây là một bản hợp đồng giữa bạn (nhà cung cấp dịch vụ) và khách hàng của bạn, và bản hợp đồng này định nghĩa mức độ uptime* mà dịch vụ của bạn sẽ cung cấp. Các nhà cung cấp cloud Amazon, Google và Microsoft thường đặt SLA của họ trên mức 99.9%. 

**Uptime** thông thường được tính bằng các con số 9, nghĩa là càng nhiều số 9 thì càng tốt. 

Bảng dưới đây biểu diễn sự tương quan giữa số lượng các số 9 với thời gian downtime dự kiến của hệ thống

| Availability % | Downtime / ngày | Downtime / năm |
| --- | --- | --- |
| 99% | 14.40 phút | 3.65 ngày |
| 99.9% | 1.44 phút | 8.77 giờ |
| 99.99% | 8.64 giây | 52.6 phút |
| 99.999% | 864 mili giây | 5.26 phút |
| 99.9999% | 86.4 mili giây | 31.56 giây |

## Ví dụ: Ước tính Twitter QPS và các yêu cầu về việc lưu trữ

Lưu ý rằng các dữ liệu sau đây chỉ mang tính minh hoạ, không phải dữ liệu thật của Twitter.

Giả sử:

- 300 triệu người dùng hàng tháng
- 50% người dùng Twitter hàng ngày
- Trung bình mỗi người dùng tweet 2 bài / ngày
- 10% bài tweet chứa file phương tiện
- Dữ liệu được lưu trữ trong 5 năm

Ước tính:

Query per second (QPS)*:

- Daily active user (DAU)* = 300 triệu * 50% = 150 triệu
- Tweet QPS = 150 triệu * 2 tweet/24 giờ/3600 giây =~ 3500
- QPS đỉnh điểm: 2 * QPS =~ 7000

Ở đây chúng ta sẽ chỉ ước tính số dung lượng để lưu trữ media

- Kích cỡ trung bình của một bài tweet:
    - tweed_id: 64 bytes
    - text: 140 bytes
    - media: 1MB
- Dung lượng lưu trữ media: 150 triệu * 2 *  10% * 1MB = 30TB mỗi ngày
- Dung lượng lưu trữ media trong 5 năm: 30TB * 360 * 5 =~ 55PB

## Tips

**Các phép tính nhẩm nhanh** tập trung chủ yếu vào quá trình. Rằng việc ta xử lý một bài toán thực tế còn quan trọng hơn cái kết quả mà nó đem lại. Tất nhiên, các nhà tuyển dụng hoàn toàn có thể kiểm tra khả năng giải quyết vấn đề của bạn, do đó chúng tôi có một vài lời khuyên có thể giúp bạn:

- **Làm tròn và xấp xỉ**: Không dễ gì để ta thực hiện các phép tính toán phức tạp khi ngồi giữa một buổi phỏng vấn. Ta không cần phải tốn thời gian để tính xem 99987/9.1 = bao nhiêu, không ai đòi bạn phải chính xác đến từng chi tiết như vậy cả. Hãy luôn làm tròn số và dùng giá trị xấp xỉ để tiết kiệm thời gian. Ví dụ như phép tính phía trên có thể đổi thành 100000/10.
- **Ghi lại các giả thuyết**: Hãy làm điều đó vì bạn có thể sẽ cần chúng về sau.
- **Ghi rõ đơn vị:** khi bạn ghi “5” thì hãy ghi rõ là 5KB hay 5MB để tránh gây nhầm lẫn.
- **Các câu hỏi thuờng gặp khi ước lượng**: QPS, QPS đỉnh điểm, dung lượng lưu trữ, cache, số lượng servers, … Bạn có thể luyện tập những phép tính ước lượng này cho buổi phỏng vấn. “Có công mài sắt có ngày nên kim”.

Chúc mừng! Giờ hãy tự thưởng cho bản thân một cái vỗ lưng nào. Làm tốt lắm!

---

- Back-of-the-envelope estimation: ám chỉ các phép tính nhanh gọn có thể thực hiện ở bất cứ đâu (kể cả mẩu giấy hay phong thư), không yêu cầu sự chính xác
- power of 2:
- latency: độ trễ, thời gian để thực hiện gì đó
- availability: tính khả dụng, tính sẵn sàng
- uptime/downtime: thời gian hoạt động/ngừng hoạt động của một hệ thống hoặc dịch vụ
- query per second: số truy vấn mỗi giây
- daily active user: số ngươi dùng mỗi ngày