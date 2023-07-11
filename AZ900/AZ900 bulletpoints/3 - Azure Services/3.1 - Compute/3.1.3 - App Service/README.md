# App Service
![translation-status](https://img.shields.io/badge/Status-in_review-orange)

1. **What? - Nó là gì**
    - Là một service dựa trên  HTTP
    - Cho phép bạn build và host nhiều loại giải pháp dựa trên web mà không cần lo quản lý cơ sở hạ tầng
    - Ví dụ: bạn có thể host web app, [mobile back-ends](notion://www.notion.so/minhvu2027/4009784a4d4b412a828e01a381205ba1?v=c3b3bd5ea17a41eaa586d532802c55b4&p=121a66c39e884fbc98ca5594a660a5e9&pm=s#mobile-apps) và RESTful APIs hỗ trợ nhiều ngôn ngữ lập trình đa dạng
        - Ví dụ: .NET, .NET Core, Java, Ruby, Node.js, PHP, Python..
    - Có thể scale trên cả hai môi trường Window và Linux

## Mobile apps

- Cho phép các dev tạo **mobile backend as a service** (MBaaS)*
- Các tính năng bao gồm:
    - Tự động scale
    - Đồng bộ dữ liệu offline
    - Bắn thông báo tới thiết bị người dùng
    - Tích hợp với các nhà cung cấp danh tính (auth) như Azure Active Directory, Google, Twitter, Facebook và Microsoft

## Azure Marketplace

- Là cửa hàng online host các app được chứng nhận và tối ưu để chạy trên Azure
- Có nhiều loại app khác nhau (vd: web app)
- Deploy từ cửa hàng thông qua Azure portal dùng **UI kiểu wizard***
    - Giúp việc đánh giá các giải pháp khác nhau dễ dàng hơn

## Các mức giá

- Các loại
    
    
    | Category | Description |
    | --- | --- |
    | Dev / Test | Lý tưởng cho khối lượng công việc ít đòi hỏi hơn. Tập trung vào việc cung cấp cơ sở hạ tầng dùng chung. Các tính năng bổ sung bao gồm miền tùy chỉnh / SSL và scale thủ công. |
    | Production | Lý tưởng cho khối lượng công việc đòi hỏi khắt khe hơn. Các tính năng bổ sung bao gồm stagin slot?, sao lưu hàng ngày và trình quản lý lưu lượng. |
    | Isolated | Lý tưởng cho lượng workload đòi hỏi kết nối mạng nâng cao và mở rộng quy mô chi tiết |
- Mỗi loại có mức giá khác nhau

### Scale một App Service

1. Mở [Azure portal](https://portal.azure.com/)
2. Chọn **Dashboard** trong menu điều hướng bên trái (nhấn vào menu icon nếu cần)
3. Chọn ************App Service************ với tên bạn chọn ở bài tập trước
4. Bạn có thể thấy các setting tùy chỉnh được dưới mục ****************Settings****************
5. Chọn ****************************Scale up (App service plan)****************************