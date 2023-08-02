# Chương 8: Thiết kế hệ thống rút gọn đường dẫn
![translation-status](https://img.shields.io/badge/Status-dropping-red)
Ở chương này, chúng ta sẽ cùng nhau xử đẹp một câu hỏi phỏng vấn về thiết kế hệ thống cực kì thú vị và kinh điển: thiết kế một **“Dịch vụ rút gọn URL”** giống như *tinyurl*.

## Bước 1 - Hiểu rõ vấn đề và xác định phạm vi thiết kế

Các câu hỏi phỏng vấn về thiết kế hệ thống thường là các câu hỏi mở. Lý do là bởi nếu muốn thiết kế được một hệ thống tốt thì việc đặt ra các câu hỏi để làm rõ hệ thống là không thể thiếu.

**Thí Sinh**: anh có thể cho tôi một ví dụ về cách hoạt động của trình rút gọn URL được không?

**Người Phỏng Vấn**: Giả sử ta có một URL ban đầu là https://www.systeminterview.com/q=chatsystem&c=loggedin&v=v3&l=long. Dịch vụ của bạn sẽ phải tạo ra một đường dẫn khác với độ dài ngắn hơn, ví dụ như: https://tinyurl.com/y7keocwj. Nếu bạn nhấn vào đường dẫn ngắn đó thì nó sẽ chuyển hướng bạn về với đường dẫn gốc.

**Thí Sinh**: Vậy mật độ truy cập là bao nhiêu?

**Người Phỏng Vấn**: Có 100 triệu URL được tạo ra mỗi ngày

**Thí Sinh**: Vậy còn độ dài URL sau khi rút gọn là bao nhiêu?

**Người Phỏng Vấn**: Ngắn nhất có thể

**Thí Sinh**: Những kí tự nào sẽ được phép xuất hiện trong URL rút gọn?

**Người Phỏng Vấn**: URL rút gọn sẽ được tạo ra bởi hỗn hợp các số (0-9) và kí tự (a-z, A-Z)

**Thí Sinh**: Vậy URL rút gọn có thể bị xóa hay cập nhật không?

**Người Phỏng Vấn**: Để đơn giản thì ta sẽ không xóa hay cập nhật các URL rút gọn.

Dưới đây sẽ là các use-case cơ bản:

1. **Rút gọn URL**: cho URL dài ⇒ trả về một URL siêu ngắn.
2. **Chuyển hướng URL**: cho một URL ngắn ⇒ chuyển hướng về URL gốc.
3. Hệ thống cần có **tính khả dụng cao**, **tính mở rộng cao** và có **khả năng chịu lỗi tốt**.

### Ước tính nhanh Back of the Envelope (BotE)

- Hoạt động **Ghi dữ liệu**: 100 triệu URL được tạo mỗi ngày.
- Hoạt động **Ghi dữ liệu/giây**: 100 triệu / 24 giờ / 3600 giây = 1160.
- Hoạt động **Đọc dữ liệu**: Giả sử tỉ lệ **Đọc/Ghi** là 10:1, vậy thì hoạt động **Đọc dữ liệu/giây** sẽ là: 1160 * 10 = 11.600.
- Giả sử **độ dài trung bình của URL** là: 100.
- **Dung lượng lưu trữ** cần thiết trong vòng 10 năm là: 365 tỉ * 100 bytes * 10 năm = 365TB.

Việc tính nhẩm các phép toán và ước lượng cho người phỏng vấn của bạn là vô cùng quan trọng bởi nó sẽ giúp họ hiểu và nắm được điều bạn đang làm.

## Bước 2 - Đề xuất thiết kế cấp cao

Trong phần này chúng ta sẽ cùng nhau thảo luận về các **API endpoints**, **quy trình chuyển hướng** và **quy trình rút gọn URL**.

### API Endpoints

**API Endpoints** có công dụng giúp cho *client* và *server* có thể giao tiếp được với nhau. Cụ thể chúng ta sẽ thiết kế APIs theo phong cách **REST**. Sẽ có 2 API endpoints chính cho **Dịch vụ rút gọn URL**.

1. **Rút gọn URL**: Có nhiệm vụ tạo một ra một *URL rút gọn*. Client sẽ gửi một **POST request** chứa tham số là **URL gốc** như sau:
    
    *POST api/v1/data/shorten*
    
    - *request parameter: {longUrl: longURLString}*
    - *return shortURL*
2. **Chuyển hướng URL**: Có nhiệm vụ chuyển hướng từ *URL rút gọn* về *URL gốc*. Client sẽ gửi một **GET request** như sau:
    
    *GET api/v1/shortUrl*
    
    - *Return longURL for HTTP redirection*

### Chuyển hướng URL

Hình bên dưới được chụp lại khi người dùng truy cập tới một *tinyurl* trên trình duyệt của họ. Ngay khi *server* nhận được một *tinyurl reques*t, nó sẽ thay đổi *URL rút gọn* thành *URL gốc* với trạng thái là **“Chuyển hướng 301”**.

![Chi tiết request khi truy cập vào tinyurl](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c9ed41b0-dbf0-4c62-add2-682f1f788337/Untitled.png)

Chi tiết request khi truy cập vào tinyurl

Còn hình vẽ bên dưới sẽ minh họa chi tiết cách giao tiếp giữa *client* và *sever*.

![Minh họa cách giao tiếp giữa client và server](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/49372935-37b2-4f85-8600-b47bcfacf9f7/Untitled.png)

Minh họa cách giao tiếp giữa client và server

Một điều mà ta cần lưu ý đó là phân biệt sự khác nhau giữa 2 trạng thái *301* và *302*.

**Trạng thái 301:** có nghĩa là *URL rút gọn* “vĩnh viễn” chuyển thành *URL gốc*. Trình duyệt sẽ cache lại *response* và cả các *request* tiếp theo tới cùng URL đó và không cần gửi *request* tới **dịch vụ rút gọn URL** nữa.

**Trạng thái 302:** có nghĩa là *URL rút gọn“*tạm thời” được chuyển thành *URL gốc*. Các *request* tiếp theo tới cùng URL đó sẽ phải gửi qua **dịch vụ rút gọn URL** trước, sau đó chúng mới được chuyển hướng trở về *URL gốc*.

### Rút gọn URL

Cùng giả sử *URL rút gọn* của chúng ta có format sau: : www.tinyurl.com/{hashValue}. Để hỗ trợ việc rút gọn URL thì chúng ta phải tìm một hàm băm *fx để* biến một URL dài thành một *hashValue* (giá trị băm).

![Luồng hoạt động của hàm băm](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/68d9e14a-ff2a-412c-9a01-aa2585140d79/Untitled.png)

Luồng hoạt động của hàm băm

Hàm băm phải đó thỏa mãn các tiêu chí như sau:

- Mỗi một *longURL* phải được băm thành một *hashValue.*
- Mỗi một *hashValue* có thể được ánh xạ trở lại *longURL.*

Thiết kế chi tiết của hàm băm sẽ được thảo luận cụ thể hơn ở phía dưới.

## Bước 3 - Thiết kế chi tiết

Vậy là chúng ta đã thảo luận xong về thiết kế cấp cao của việc rút gọn URL và chuyển hướng URL. Ở phần này chúng ta sẽ đào sâu hơn vào các phần quan trọng như: **mô hình dữ liệu**, **hàm băm**, **rút gọn URL** và **chuyển hướng URL**.

### Mô hình dữ liệu

Trong thiết kế cấp cao, mọi thứ đều được lưu trữ trong *bảng băm*. Điều này tốt, nhưng không khả thi với các hệ thống ngoài đời bởi tài nguyên và bộ nhớ khá đắt đỏ và bị hạn chế. Một lựa chọn khác tốt hơn đó là lưu chữ *<shortURL, longURL> ở* trong cơ sở dữ liệu có quan hệ. 

Hình bên dưới là một bảng csdl với thiết kế đơn giản gồm 3 cột: **id**, **shortURL** và **longURL**.

![Bảng urlTable dùng để chứa cặp dữ liệu <shortURL, longURL>](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/06b3290a-3869-4684-9a83-a265c33b1278/Untitled.png)

Bảng urlTable dùng để chứa cặp dữ liệu <shortURL, longURL>

### Hàm băm

**Hàm băm** dùng để băm một chuỗi URL dài thành URL ngắn (còn gọi là hashValue)

### Độ dài của giá trị băm

*hashValue* bao gồm các kí tự trong tập [0-9, a-z, A-Z], tức là gồm 10 + 26 + 26 = 62 kí tự khả thi. 

Để xác định được độ dài của *hashValue*, ta cần tìm giá trị n nhỏ nhất thỏa mãn điều kiện là: **62^n ≥ 365 tỉ URL**. Lí do là bởi vì hệ thống sẽ phải hỗ trợ được lên tới con số 365 tỉ URL dựa trên tính toán BotE ở phần trên. 

Bảng phía dưới sẽ biểu diễn lần lượt độ dài của *hashValue* và số lượng URL tối đa tương ứng mà nó có thể hỗ trợ.

| N | Số URL tối đa |
| --- | --- |
| 1 | 62^1 = 62 |
| 2 | 62^2 = 3.844 |
| 3 | 62^1 = 238.328 |
| 4 | 62^1 = 14.776.336 |
| 5 | 62^1 = 916.132.832 |
| 6 | 62^1 = 56.800.235.584 |
| 7 | 62^1 = 3.521.614.606.208 = ~3.5 nghìn tỉ |

Vậy khi n = 7 thì số URL tương ứng mà nó có thể tạo ra là ~3.5 nghìn tỉ, hoàn toàn thỏa mãn việc hỗ trợ 365 tỉ URL, bởi vậy độ dài của *hashValule* sẽ là 7.

Tiếp theo chúng ta sẽ cùng khám phá 2 loại hàm băm dùng cho việc rút ngắn URL. Loại đầu tiên là **“Băm và Xử lý xung đột”** còn loại thứ hai là **“Chuyển đổi base62”**.

### Băm và Xử lý xung đột

Ta sẽ cần cài đặt một hàm băm có khả năng băm chuỗi URL dài thành một chuỗi ngắn hơn với độ dài là 7 kí tự.

Khi đó người ta sẽ nghĩ ngay đến các hàm băm có tên tuổi như **CRC32**, **MD5** hay **SHA-1.**

Bảng dưới đây sẽ so sánh cụ thể các kết quả băm sau khi áp dụng các loại hàm băm phía trên cho cùng một URL là https://en.wikipedia.org/wiki/Systems_design.

| Hàm băm | Giá trị băm |
| --- | --- |
| CRC32 | 5cb54054 |
| MD5 | 5a62509a84df9ee03fe1230b9df8b84e |
| SHA-1 | 0eeae7916c06853901d9ccbefbfcaf4de57ed85b |

Tuy nhiên quan sát ta sẽ thấy trong cả 3 giá trị băm kia, không có giá trị nào có độ dài bé hơn hoặc bằng 7 (CRC32 nhỏ nhất là 8 kí tự). Vậy chúng ta nên làm gì?

Giải pháp đầu tiên là chúng ta sẽ chỉ lấy 7 kí tự đầu tiên của giá trị băm, tuy nhiên cách này có thể dẫn tới việc xảy ra các xung đột băm. Khi đó ta sẽ cần xử lý các xung đột băm bằng cách chạy vòng lặp để nối thêm một chuỗi mới (do ta tự định nghĩa) cho giá trị băm hiện tại cho tới khi không bị xung đột nữa. 

Toàn bộ quá trình này được minh họa cụ thể trong hình vẽ bên dưới.

![Luồng hoạt động của Băm và Xử lý xung đột](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/18499ddc-0025-4ae1-bdd8-aa1d1572ec84/Untitled.png)

Luồng hoạt động của Băm và Xử lý xung đột

Vậy là ta đã xử lý xong xung đột băm, tuy nhiên việc phải truy xuất database để kiểm tra sự tồn tại của *shortURL* cho mỗi *request* là sẽ tốn khá nhiều tài nguyên. Do đó để cải thiện hiệu suất, người ta thường sử dụng một kĩ thuật gọi là **Bloom filter.** 

**Bloom filter** là một cấu trúc dữ liệu xác suất dùng để kiểm tra xem liệu một phần tử có thuộc vào một nhóm dữ liệu hay không. Thông tin chi tiết hơn thì bạn có thể tra Google nhé.

### Chuyển đổi base62

**Chuyển đổi base** là một phương pháp khác cũng hay được sử dụng trong việc rút gọn URL. 

**Chuyển đổi base** có nghĩa là ta sẽ biểu diễn một số dưới các hệ cơ sở khác nhau mà vẫn giữ nguyên giá trị của nó.

**Chuyển đổi base62** sẽ là ứng cử viên thích hợp trong trường hợp này bởi nó có tổng 62 ký tự có thể dùng cho *hashValue*, hoàn toàn khớp với yêu cầu của người phỏng vấn ban đầu. 

Dưới đây sẽ là một ví dụ giải thích cụ thể cách chuyển con số 11157 từ hệ thập phân sang hệ base62.

- Như tên gọi của nó, base62 sẽ dùng 62 ký tự để mã hóa. Các ánh xạ cụ thể như sau: 0-0, …, 9-9, 10-a, 11-b, …, 25-z, 26-A, …, 61-Z. Tức là chữ ‘a’ sẽ đại diện cho 10, chữ ‘Z’ đại diện cho 61 và v..v…
- 11157 hệ 10 = $2 * 62^2 + 55 * 62 ^1 + 59 * 62^0$ = [2, 55, 59] → [2, T, X] trong base62
    
    Hình vẽ dưới đây sẽ biểu diễn các bước làm
    
    ![Các bước tính toán từ hệ 10 sang base62](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/32a18c37-0762-4c38-996c-7771d19a1eac/Untitled.png)
    
    Các bước tính toán từ hệ 10 sang base62
    
- Do đó, URL rút gọn sẽ là https://tinyurl.com/2TX

### So sánh 2 cách tiếp cận

| Băm và Xử lý xung đột | Chuyển đổi base62 |
| --- | --- |
| Độ dài URL rút gọn luôn cố định | Độ dài URL rút gọn phụ thuộc vào ID |
| Không cần unique ID generator | Phụ thuộc vào bộ unique ID generator |
| Có thể xảy ra xung đột cần xử lý | Không thể xung đột vì ID là duy nhất |
| Khó để xác định URL rút gọn tiếp theo vì không phụ thuộc vào ID | Dễ xác định URL rút gọn tiếp theo nếu ID tự tăng |

### Đào sâu vào rút gọn URL

Việc rút gọn URL mà một phần cốt lõi của hệ thống này, do đó chúng ta sẽ cố gắng giữ nó đơn giản và hợp lý. Cụ thể, trong bản thiết kế này ta sẽ sử dụng phương pháp *Chuyển đổi base62*. 

Sơ đồ như hình bên dưới để mô tả luồng hoạt động của việc rút gọn URL.

![Luồng hoạt động của việc rút gọn URL](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d338b722-f36e-4092-a535-eb8800166406/Untitled.png)

Luồng hoạt động của việc rút gọn URL

1. Input sẽ là *longURL*
2. Hệ thống sẽ kiểm tra xem *longURL* đã có trong database chưa
3. Nếu có, tức là *longURL* đã được rút gọn trước đó rồi. Khi đó ta chỉ cần lấy *shortURL* trong database ra và trả về cho người dùng
4. Nếu chưa, tức là *longURL* này mới. Khi đó ta sẽ sinh ra một *unique ID* (khóa chính)
5. Chuyển đổi *unique ID* đó thành *shortURL* dựa theo phương pháp “*Chuyển đổi base62”*
6. Lưu dữ liệu mới vào database gồm: ID, shortURL và longURL

Để cho dễ hiểu hơn thì chúng ta sẽ cùng làm một ví dụ thực tế sau.

- Giả sử input *longURL* là: https://en.wikipedia.org/wiki/Systems_design
- *Unique ID* sinh ra là: 2009215674938
- Biến đổi *unique ID* thành *shortURL* dùng “*chuyển đổi base62”*: 2009215674938 → zn9edcu
- Lưu lại ID, shortURL và longURL vào database

| id | shortURL | longURL |
| --- | --- | --- |
| 2009215674938 | zn9edcu | https://en.wikipedia.org/wiki/Systems_design |

Chúng ta vẫn chưa nhắc đến **Unique ID Generator** - một trình tạo ID duy nhất trong phạm vi toàn bộ hệ thống và góp phần tạo ra *shortURL*.

Trong một môi trường có tính phân tán cao thì việc cài đặt *Unique ID Generator* là một thách thức lớn. May mắn là chúng ta đã từng thảo luận một vài giải pháp ở [Chương 7: “Thiết kế Unique ID Generator trong các Hệ Thống Phân Tán”](https://minhvu2027.tk/phien-dich/sdi-chuong-7-thiet-ke-unique-id-generator-trong-cac-he-thong-phan-tan/), bạn nào quên thì có thể xem lại.

### Đào sâu vào chuyển hướng URL

Bởi vì có nhiều hoạt động *Đọc* dữ liệu hơn là *Ghi* nên *<shortURL, longURL>* sẽ được lưu trong cache để cải thiện hiệu năng.

Hình vẽ phía dưới sẽ biểu diễn chi tiết thiết kế cho việc chuyển hướng URL. 

![Luồng hoạt động của chuyển hướng URL](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/37d9487b-066b-4293-aa39-894567f09dea/Untitled.png)

Luồng hoạt động của chuyển hướng URL

Quy trình của chuyển hướng URL sẽ được tóm tắt như sau:

1. Người dùng nhấn vào link URL rút gọn: https://tinyurl.com/zn9edcu
2. Bộ cân bằng tải sẽ chuyển request tới các web server
3. Nếu *shortURL* đã tồn tại trong cache rồi thì trực tiếp trả *longURL* về cho người dùng
4. Nếu *shortURL* chưa tồn tại trong cache thì ta tiếp tục tìm *longURL* trong database . Nếu *longURL* không có trong database thì có khả năng người dùng nhập *shortURL* sai
5. Nếu *longURL* trong database thì thì trả cho người dùng

## Bước 4 - Tổng kết

Trong chương này chúng ta đã cùng nhau bàn về các chủ đề như thiết kế API, mô hình dư liệu, hàm băm, rút gọn URL và chuyển hướng URL.

Nếu có dư thời gian ở cuối buổi phỏng vấn, ta có thể bàn thêm với người phỏng vấn các chủ đề mở rộng như:

- **Rate limiter**: Một lỗ hổng an ninh tiềm ẩn mà chúng ta có thể gặp phải đó là bị các người dùng xấu (hacker) liên tục gửi một lượng request khổng lồ tới dịch vụ của ta. Rate limiter có thể giúp ta lọc các request dựa theo địa chỉ IP hoặc các quy tắc lọc khác. Nếu bạn quên thì có thể xem lại [Chương 4: “Thiết kế một rate limiter”](https://minhvu2027.tk/phien-dich/sdi-chuong-4-thiet-ke-rate-limiter/).
- **Mở rộng máy chủ web**: Vì web tier là stateless nên rất dễ để mở rộng web tier bằng cách thêm bớt các web server. *(Tra google nghen)*
- **Mở rộng database**: Các kĩ thuật thông dụng database replication và sharding
- **Phân tích**: Dữ liệu đóng vào trò rất quan trọng trong sự phát triển của một doanh nghiệp. Việc tích hợp một giải pháp phân tích vào trình rút gọn URL có thể giúp ta trả lời các câu hỏi quan trọng như có bao nhiêu người bấm vào link này? Họ bấm vào khoảng thời gian nào? v..v…
- **Tính khả dụng, tính nhất quán và độ tin cậy**: Đây đều là yếu tố cốt lõi để dựng lên một hệ thống hoàn hảo. Chúng ta đã từng thảo luận về chúng tại [Chương 1: “Mở rộng từ con số 0 cho tới hàng triệu người dùng”](https://minhvu2027.tk/phien-dich/sdi-chuong-1-mo-rong-tu-con-so-0-cho-toi-hang-trieu-nguoi-dung/), bạn nên xem lại để hiểu rõ hơn.

Bản dịch tới đây là kết thúc, ở bên dưới sẽ là mục Các từ ngữ chuyên ngành và Link bài viết. Cảm ơn mọi người đã theo dõi!