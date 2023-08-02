# Chương 13: Thiết kế hệ thống gợi ý tìm kiếm
![translation-status](https://img.shields.io/badge/Status-dropping-red)
Khi sử dụng công cụ tìm kiếm của Google hay Amazon, mỗi lần bạn gõ từ gì đó vào thanh tìm kiếm thì nó sẽ hiển thị cho bạn một hoặc nhiều kết quả khớp với cụm từ mà bạn tìm kiếm. Tính năng này được gọi bằng rất nhiều cái tên như: autocomplete (tự động hoàn thiện), typehead, search-as-you-type (tìm kiếm nóng) hoặc incremental search (tìm kiếm gia tăng).

Ta sẽ lấy một ví dụ gần gũi hơn đó chính là hình ảnh thanh tìm kiếm của Google đang hiển thị một danh sách các kết quả autocomplete được trả về khi từ “dinner” được điền vào thanh tìm kiếm.

Search autocomplete đóng một vai trò vô cùng quan trọng trong các sản phẩm ứng dụng và hệ thống. Chính vì thế nên nó được liệt kê vào danh sách các câu hỏi phỏng vấn về thiết kế hệ thống, cụ thể là: Yêu cầu thiết kế một hệ thống Gợi ý tìm kiếm (hay còn có tên gọi là top k heavy hittlers).

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/33656c11-3c1b-4fd0-99ab-6291b7b8bcb9/Untitled.png)

## Bước 1 - Hiểu rõ vấn đề và xác định phạm vi thiết kế

Bước đầu tiên để giải quyết bất kì một câu hỏi phỏng vấn nào về thiết kế hệ thống đó là đặt ra các câu hỏi để làm rõ các yêu cầu của hệ thống đó. Dưới đây sẽ là một ví dụ cụ thể về cuộc hội thoại giữa thí sinh và người phỏng vấn:

**Thí Sinh**: Kết quả trả về chỉ khớp phần đầu của câu truy vấn tìm kiếm hay phải khớp cả phần giữa?

**Người Phỏng Vấn**: Chỉ cần khớp phần đầu.

**Thí Sinh**: Hệ thống sẽ trả về tổng cộng bao nhiêu kết quả?

**Người Phỏng Vấn**: 5.

**Thí Sinh**: Vậy hệ thống đánh giá dựa theo tiêu chí nào để trả về 5 kết quả đó?

**Người Phỏng Vấn**: Dựa theo mức độ phổ biến, thường là tần số tìm kiếm trước đó.

**Thí Sinh**: Hệ thống có hỗ trợ kiểm tra chính ta không?

**Người Phỏng Vấn**: Hệ thống không hỗ trợ kiểm tra hay tự động sửa lỗi chính tả.

**Thí Sinh**: Các câu truy vấn tìm kiếm phải bằng tiếng Anh đúng không?

**Người Phỏng Vấn**: Đúng vậy. Nếu còn thời gian ở cuối buổi phỏng vấn thì ta sẽ nói thêm về việc hỗ trợ đa ngôn ngữ.

**Thí Sinh**: Vậy hệ thống có cho phép các kí tự viết hoa và các kí tự đặc biệt không?

**Người Phỏng Vấn**: Không, chúng ta sẽ giả sử tất cả câu truy vấn tìm kiếm đều chứa các kí tự viết thường thuộc bảng chữ cái.

**Thí Sinh**: Có khoảng bao nhiêu người sử dụng công cụ tìm kiếm này?

**Người Phỏng Vấn**: Khoảng 10 triệu người dùng hàng ngày (DAU).

Ta sẽ tóm tắt lại các yêu cầu của hệ thống như sau:

- **Thời gian phản hồi nhanh**: Ngay khi người dùng nhập vào thanh tìm kiếm, các kết quả gợi ý autocomplete phải được hiển thị kịp thời. Có một bài báo từng tiết lộ rằng hệ thống autocomplete của Facebook trả về các kết quả đó chỉ trong khoảng thời gian là 100 mili giây.
- **Độ liên quan**: Các kết quả gợi ý autocomplete nên liên quan tới cụm từ tìm kiếm.
- **Sắp xếp**: Các kết quả trả về bởi hệ thống phải được sắp xếp dựa theo mức độ phổ biến hoặc theo các mô hình sắp xếp khác.
- **Khả năng mở rộng**: Hệ thống có thể xử lý được lượng truy cập lớn.
- **Tính khả dụng cao**: Hệ thống nên duy trì tính khả dụng và có thể truy cập bình thường khi một phần của hệ thống ngoại tuyến, tốc độ bị chậm hoặc gặp các lỗi mạng ngoài dự đoán.

### Ước tính nhanh Back of the Envelope (BotE)

- Giả sử có 10 triệu người dùng hàng ngày
- Trung bình một người thực hiện tìm kiếm 10 lần mỗi ngày
- Mỗi chuỗi truy vấn tốn 20 bytes dữ liệu:
    - Giả sử ta dùng mã hóa ASCII. 1 kí tự = 1 byte
    - Giả sử một câu truy vấn gồm 4 từ, mỗi từ trung bình chứa 5 kí tự
    - Suy ra mỗi câu truy vấn tốn: 4 * 5 = 20 bytes
- Với mỗi một kí tự được nhập vào thanh tìm kiếm, client sẽ gửi một request tới backend để yêu cầu các gợi ý autocomplete. Trung bình sẽ có 20 request được gửi đi với mỗi câu truy vấn tìm kiếm (20 kí tự/câu). Ví dụ, câu truy vấn sau sẽ gửi tổng cộng 6 request tới backend khi ta gõ chữ “dinner”:
    - **search?q=d
    search?q=di
    search?q=din
    search?q=dinn
    search?q=dinne
    search?q=dinner**
- ~24.000 truy vấn/giây (QPS) = 10.000.000 người dùng * 10 truy vấn / ngày * 20 kí tự / 24 giờ / 3600 giây
- Vậy số truy vấn/giây đỉnh điểm sẽ là ~ 48.000 = QPS * 2
- Giả sử có 20% các câu truy vấn hàng ngày là truy vấn. 10 triệu * 10 truy vấn / ngày * 20 byte / truy vấn * 20% = 0.4GB. Có nghĩa là sẽ có 0.4GB dữ liệu mới được thêm hàng ngày.

## Bước 2 - Đề xuất thiết kế cấp cao

Tại đây, hệ thống sẽ được chia nhỏ ra thành 2 phần:

- Dịch vụ thu thập dữ liệu: nó thu thập các câu truy vấn được nhập từ người dùng và tổng hợp lại theo thời gian thực. Việc xử lý lượng dữ liệu lớn trong thời gian thực là điều không thực tế. Tuy nhiên đó vẫn là một ý tưởng khởi đầu tốt. Chúng ta sẽ cùng nhau đào sâu vào một giải pháp thực tế hơn.
- Dịch dụ truy vấn: Cho một câu truy vấn tìm kiếm hoặc tiền tố, trả về 5 kết quả được tìm nhiều nhất.

### Dịch vụ Thu thập dữ liệu

Ta sẽ lấy một ví dụ đơn giản để hiểu cách hoạt động của dịch vụ thu thập dữ liệu. Giả sử ta có một bảng tần số dùng để lưu chuỗi truy vấn và tần số của nó. Ban đầu bảng sẽ trống rỗng, khi người dùng lần lượt nhập các câu truy vấn như “twitch”, “twitter”, “twitter” và “twillo” thì bảng tần số sẽ được cập nhật lại như sau:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/49273070-2261-463d-8b12-239c361593a2/Untitled.png)

### Dịch vụ Truy vấn

Giả sử ta có một bảng tần số gồm 2 cột: 

- Query: dùng lưu trữ chuỗi truy vấn
- Frequency: dùng để lưu trữ số lần mà câu truy vấn đó được tìm kiếm

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3a23f231-2698-4d16-b30d-9c31c4533271/Untitled.png)

Khi người dùng nhập “tw” vào thanh tìm kiếm, sẽ có 5 truy vấn được tìm nhiều nhất được hiển thị lên.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d45dc0f4-5ba5-465d-8cf5-178baf83a08c/Untitled.png)

Để lấy top 5 các câu truy vấn được tìm nhiều nhất, ta chạy đoạn mã SQL sau:

```sql
SELECT * FROM frequency_table
WHERE query Like `prefix%`
ORDER BY frequency DESC
LIMIT 5
```

Đây là một giải pháp có thể chấp nhận được nếu tập dữ liệu nhỏ. Khi tập dữ liệu lớn thì việc truy cập vào csdl là một nút thắt. Chúng ta sẽ khám phá cách tối ưu hóa trong phần thiết kế chi tiết

## Bước 3 - Thiết kế chi tiết

Trong phần thiết kế cấp cao, chúng ta đã cùng nhau thảo luận về 2 service là thu thập dữ liệu và truy vấn. Thiết kế đó tuy chưa tối ưu, nhưng vẫn là một điểm khởi đầu tốt. Trong phần này, ta sẽ đi sâu hơn vào một vài phần cụ thể và tìm ra các giải pháp tối ưu hơn như:

- Cấu trúc dữ liệu Trie
- Dịch vụ thu thập dữ liệu
- Dịch vụ truy vấn
- Mở rộng kho lưu trữ
- Hoạt động của Trie

### Cấu trúc dữ liệu Trie

Các cơ sở dữ liệu quan hệ thường được dùng cho việc lưu trữ trong thiết kế cấp cao. Tuy nhiên, việc lấy ra top 5 câu truy vấn trong csdl quan hệ là không hiệu quả. Cấu trúc dữ liệu Trie (cây tiền tố) thường được dùng để giải quyết bài toán tối ưu này. Vì cấu trúc dữ liệu Trie rất quan trọng trong hệ thống, ta sẽ tập trung vào việc thiết kế một Trie tuy chỉnh.

Việc hiểu được cấu trúc dữ liệu trie cơ bản là điều cần theies cho câu hỏi phrogn vấn này. Tuy nhiên, đây giống như là một câu hỏi về cấu trúc dữ liệu hơn là một ccaau hỏi thiết kế hệ thống. Trong chương này, ta sẽ chỉ thảo luận qua về cấu trúc dữ liệu trie và chủ yếu tập trung vào cách tối ưu trie cơ bản để cải thiện thời gian phản hồi.

Trie (đọc là try) là một cấu trúc dữ liệu dạng cây (tree) có thể lữu trữ các chuỗi. Cái tên xuất phát từ chữ retrieval, có nghĩa là nó được thiết kế để thực hiện việc truy xuất chuỗi. Ý tưởng chính của trie như sau:

- Trie là cấu trúc dữ liệu dạng cây
- Nút gốc đại diện cho chuỗi rỗng
- Mỗi nút chứa một kí tự và có 26 nút con, mỗi nút 1 kí tự
- Mỗi nút đại diejenm cho một từ hoặc một chuỗi tiền tố

Hình vẽ bên dưới biểu diễn một trie với các truy vấn tìm kiếm gồm: “tree”, “try”, “true”, “toy”, “wish”, “win”. Các câu truy vấn tìm kiếm sẽ được in viền xanh.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6b691fe0-33c5-4390-b3a1-c41ff056a73e/Untitled.png)

Cấu trúc dữ liệu trie cơ bản chứa các kí tự trong các nút. Để hỗ trợ việc sắp xếp theo tần số thì thông tin về tần số cần phải được lưu trong nút đó. Giả sử ta có bảng tần số sau:

| Query | Frequency |
| --- | --- |
| tree | 10 |
| try | 29 |
| true | 35 |
| toy | 14 |
| wish | 25 |
| win | 50 |

Sau khi bổ sung thông tin về tần số vào trong các nút, cấu trúc dữ liệu trie sẽ được cập nhật lại như hình sau.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4d98c61f-64eb-4c2e-978a-95905541b406/Untitled.png)

Vậy autocomplete sẽ hoạt động trong trie như thế nào? Trước khi đi sâu vào phần giải thuật, ta cần thống nhất với nhau về một vài định nghĩa:

- p: độ dài của một tiền tố
- n: tổng số nút trong một trie
- c: số con của một nút

Dưới đây sẽ là các bước để lấy top k câu truy vấn được tìm:

1. Tìm tiền tố - Độ phức tạp O(p)
2. Duyệt qua câu con khởi đầu từ nút tiền tố để lấy ra tất các nút con hợp lệ. Một nút con được coi là hợp lệ nếu có thể hình thành một chuỗi truy vấn hợp lý - Độ phức tạp O(c)
3. Sắp xếp các con và lấy ra top k - Độ phức tạp O(clogc)

Ta sẽ lấy một ví dụ cụ thể để giải thích cho thuật toán này. Giả sử k = 2 và người dùng nhập chữ “tr” vào thanh tìm kiếm. Thuật toán sẽ chạy như sau:

- Bước 1: Tìm nút tiền tố “tr”
- Bước 2: Duyệt và các cây con để lấy các nút con hợp lệ. Trong trường hợp này, các nút [tree: 10], [true: 35], [try: 29] hộp lệ
- Bước 3: Sắp xếp các nút con và lấy ra top 2. [true: 35] and [try: 29] là 2 truy vấn được tìm nhiều nhất với tiền tố “tr”

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eecb208c-d66d-4c09-9908-0db1e16f0862/Untitled.png)

Độ phức tạp của thuật toán này là tổng phức tạp của các bước trên: O(p) + O(c) + O(clogc)

Thuật toán ở trên khá rõ ràng và đơn giản. Tuy nhiên nó chạy khá chậm, trong tường hợp xấu nhất thì ta sẽ phải duyệt qua toàn bộ trie để lấy có kết quả. Vậy nên có 2 giải pháp tối ưu sau:

1. Giới hạn độ dài của prefix
2. Cache các truy vấn tìm kiếm nhiều nhất vào mỗi nút

Giờ thì hãy cùng nhau đi vào từng giải pháp cụ thể

### Giới hạn độ dài của prefix

Người dùng hiếm khi nhập một chuỗi truy vấn dài vào trong thanh tìm kiếm. Do đó, ta có thể coi biến p có giá trị nguyên khá nhỏ, chẳng hạn như 50. Nếu ta giới hạn độ dài của prefix, độ phức tạp thời gian của thuật toán cho công việc “Tìm tiền tố” có thể giảm từ O(p) xuống O(hằng số giới hạn), hay còn gọi là O(1).

### Cache các truy vấn tìm kiếm nhiều nhất vào mỗi nút

Để tránh việc phải duyệt qua toàn bộ trie, ta sẽ lưu top k các truy vấn được người dùng tìm nhiều nhất vào bên trong mỗi nút. Vì kết quả gợi ý trả về thường là 5 - 10 nên k sẽ là một số tương đối nhỏ. Trong trường hợp của ta, 5 sẽ là số truy vấn được cache.

Bằng việc cache những câu truy vấn top tìm kiếm vào mỗi nút, ta đã giảm thiểu đáng kể độ phức tạp thời gian khi lấy ra top 5 truy vấn. Tuy nhiên, thiết kế như thế này thì sẽ tốn khá nhiều dung lượng để chứa top truy vấn vào mỗi nút. Việc đánh đổi không gian lấy thời gian là thỏa đáng bởi vì tốc độ phản hồi nhanh là rất quan trọng đối với hệ thống này.

Hình bên dưới mô tả cấu trúc dữ liệu trie sau khi được cập nhật. Top 5 truy vấn sẽ được lưu vào mỗi nút. Ví dụ, nút có tiền tố “be” sẽ lưu chữ các truy vấn gồm: [best: 35, bet: 29, bee: 20, be: 15, beer: 10].

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fb173e68-cd08-40b4-bba0-de08ea3465ea/Untitled.png)

Ta sẽ cùng điểm qua về độ phức tạp thời gian của thuật toán này sau khi áp dụng 2 phương pháp tối ưu:

1. Tìm nút tiền tố - độ phức tạp thời gian: O(1)
2. Trả về top k - độ phức tạp thời gian: O(1)

Vì độ phức tạp thời gian của mỗi bước đều được giảm xuống O(1), thuật toán của ta còn lại O(1) khi lấy ra top k truy vấn.

### Dịch vụ thu thập dữ liệu

## Bước 4 - Tổng kết