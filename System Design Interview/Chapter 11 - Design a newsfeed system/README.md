# Chương 11: Thiết kế hệ thống newsfeed

![translation-status](https://img.shields.io/badge/Status-in_progress-blue)

Trong chương này, yêu cầu của bạn sẽ là thiết kế một hệ thống bảng newsfeed. Trước hết ta cần hiểu news feed là gì? Định nghĩa của nó trên trang Facebook như sau: “News feed là một danh sách được liên tục cập nhật các mẩu chuyện nhỏ nằm ở chính giữa màn hình trang chủ của bạn. News Feed bao gồm các dòng trạng thái, hình ảnh, video, đường dẫn, hoạt động của app và lượt like từ những người, những trang, những hội nhóm mà bạn theo dõi trên Facebook”. Đây là một bài toán khá phổ biến trong các buổi phỏng vấn và chúng có nhiều biến thể như: thiết kế Facebook news feed, Instagram feed, Twitter (hay X) timeline, …

## Bước 1 - ****Hiểu rõ vấn đề và xác định phạm vi thiết kế****

Giờ thì bạn sẽ cần đặt ra các câu hỏi để thực sự hiểu được nhà tuyển dụng đang nghĩ gì trong đầu khi họ yêu cầu bạn thiết kế một hệ thống news feed. Tối thiểu bạn cũng phải biết được hệ thống này hỗ trợ những tính năng nào. Phía dưới đây sẽ là một ví dụ cho cuộc trò chuyện giữa 2 người:

**Thí sinh (TS)**: Đây là mobile app hay web app? Hay cả 2?

**Nhà tuyển dụng (NTD)**: Cả hai

**TS**: Những tính năng nào được coi là quan trọng nhất của sản phẩm này?

**NTD**: Cho phép tạo bài viết và xem những bài viết của bạn bè

**TS**: Thứ tự sắp xếp các bài viết theo thời gian hay theo một yếu tố cụ thể? Nghĩa là mỗi bài viết sẽ có một số “cân nặng” nhất định (ví dụ các bài viết từ bạn bè thân thiết của bạn sẽ “nặng” hơn các bài viết trong group)

**NTD**: Để đơn giản thì ta sẽ sắp xếp theo thời gian

**TS**: Một user được phép có tối đa bao nhiêu bạn bè?

**NTD**: 5000

**TS**: Lượng truy cập là bao nhiêu?

**NTD**: 10 triệu người dùng hàng ngày (Daily Active Users)

**TS**: Bài viết có chứa hình ảnh, video hay chỉ chứa text thôi?

**NTD**: Nó có thể chứa các file media, tức là cả ảnh và video

Giờ bạn đã có được những yêu cầu cụ thể, chúng ta sẽ tập trung vào việc thiết kế hệ thống

## Bước 2 – Đề xuất thiết kế cấp cao

The design is divided into two flows: feed publishing and news feed building.
• Feed publishing: when a user publishes a post, corresponding data is written into cache
and database. A post is populated to her friends’ news feed.
• Newsfeed building: for simplicity, let us assume the news feed is built by aggregating
friends’ posts in reverse chronological order.
Newsfeed APIs
The news feed APIs are the primary ways for clients to communicate with servers. Those
APIs are HTTP based that allow clients to perform actions, which include posting a status,
retrieving news feed, adding friends, etc. We discuss two most important APIs: feed
publishing API and news feed retrieval API.
Feed publishing API
To publish a post, a HTTP POST request will be sent to the server. The API is shown below:
POST /v1/me/feed
Params:
• content: content is the text of the post.
• auth_token: it is used to authenticate API requests.
Newsfeed retrieval API
The API to retrieve news feed is shown below:
GET /v1/me/feed
Params:
• auth_token: it is used to authenticate API requests.
Feed publishing
Figure 11-2 shows the high-level design of the feed publishing flow.

## Bước 3 – Thiết kế chi tiết

## Bước 4 – Tổng kết