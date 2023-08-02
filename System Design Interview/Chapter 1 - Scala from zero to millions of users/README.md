# Chương 1: Mở rộng cho hàng triệu người dùng từ con số 0
![translation-status](https://img.shields.io/badge/Status-dropping-red)
Việc thiết kế một hệ thống hỗ trợ hàng triệu người dùng là một thử thách lớn, nó đòi hỏi ta luôn phải liên tục học hỏi và cải thiện. Trong chương này, chúng ta sẽ cùng nhau xây dựng một hệ thống cho từ một người dùng duy nhất và dần dần mở rộng nó để có thể phục vụ hàng triệu người về sau. Kết thúc chương, bạn sẽ thành thạo một số kĩ thuật hữu ích giúp giải quyết được các câu hỏi phỏng vấn liên quan tới việc thiết kế hệ thống.

### Setup một server

Hành trình vạn dặm bắt đầu từ một bước chân và việc xây dựng một hệ thống phức tạp cũng vậy. Để đơn giản hóa bước đầu tiên thì ta sẽ coi mọi thứ đều đang chạy trên một server duy nhất. Hình 1-1 bên dưới minh hoạ setup của một server chứa của tất cả mọi thứ như: web app, database, cache, …

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2798d4d2-e280-45a1-b0f5-fb1c6c4508ed/Untitled.png)

Để hiểu được setup này, ta cần phải tìm hiểu về luồng request cũng như nguồn tài nguyên truy cập. Đầu tiên hãy nhìn vào luồng request dưới đây (Hình 1-2).

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/61acb3bb-947d-41f3-8c2b-aa63fceffbd9/Untitled.png)

1. Người dùng truy cập vào các website thông qua tên miền api.mysite.com. Thường thì DNS sẽ là một dịch vụ mất phí được cung cấp bởi bên thứ 3 và sẽ không được host tại server của chúng ta.
2. Địa chỉ IP được trả về cho trình duyệt hoặc ứng dụng di động. Trong ví dụ này địa chỉ IP sẽ là 15.125.23.214
3. Khi nhận được địa chỉ IP, HTTP request sẽ được gửi trực tiếp từ web server của bạn
4. Web server trả về các trang HTML và JSON dùng để hiển thị

Tiếp theo là về nguồn tài nguyên truy cập. Lượng truy cập tới web server của bạn sẽ đến từ 2 nguồn: ứng dụng web và ứng dụng mobile.

- Ứng dụng web: nó dùng các ngôn ngữ server-side (Java, Python, …) để xử lý các logic nghiệp vụ, lưu trữ, … và các ngôn ngữ client-side (HTML và JavaScript) để hiển thị
- Ứng dụng mobile: nó sẽ dùng giao thức HTTP để giao tiếp với web server. JavaScript Object Notation (JSON) là một định dạng đơn giản thường xuyên được sử dụng cho API response với mục đích là để truyền dữ liệu. Bên dưới là một ví dụ cụ thể của API response theo định dạng JSON:
    
    ```json
    //GET /users/12 – Retrieve user object for id = 12
    {
    	"id": 12,
    	"firstName": "John",
    	"lastName": "Smith",
    	"address": {
    		"streetAddress": "21 2nd Street",
    		"city": "New York",
    		"state": "NY",
    		"postalCode": 10021
    	},
    	"phoneNumbers": [
    		"212 555-1234",
    		"646 555-4567"
    	]
    }
    ```
    

### Cơ sở dữ liệu

Với sự tăng trưởng người dùng thì một server là không đủ, bởi vậy chúng ta sẽ cần phải có nhiều server: một cái cho web/mobile và một cái cho cơ sở dữ liệu (Hình 1- 3). Việc tách riêng server của web/mobile và cơ sở dữ liệu sẽ cho phép ta mở rộng chúng một cách riêng biệt.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/da744744-3cd9-4d14-bf04-020a685400bb/Untitled.png)

### Nên chọn cơ sở dữ liệu nào?

Bạn có thể chọn giữa cơ sở dữ liệu có quan hệ (loại truyền thống) hoặc cơ sở dữ liệu phi quan hệ. Hãy cùng nhau phân tích điểm khác biệt giữa chúng.

Các cơ sở dữ liệu có quan hệ có cái tên gọi khác là hệ thống quản lý cơ sở dữ liệu có quan hệ (RDBMS) hay còn được gọi với cái tên quen thuộc hơn là cơ sở dữ liệu SQL. Một vài loại cơ sở dữ liệu SQL nổi tiếng như MySQL, Oracle, PostgreSQL, … 

## Bước 1 - Hiểu rõ vấn đề và xác định phạm vi thiết kế

## Bước 2 - Đề xuất thiết kế cấp cao

## Bước 3 - Thiết kế chi tiết

## Bước 4 - Tổng kết