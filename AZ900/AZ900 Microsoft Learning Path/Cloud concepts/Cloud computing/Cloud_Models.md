# Cloud Models
![translation-status](https://img.shields.io/badge/Status-done-green)

- Có 3 kiểu deploy resource: public, private và hybrid model

## Private cloud
- Chuyên dùng riêng cho 1 tổ chức hoặc doanh nghiệp
- Có thể được đặt ở ngay chính datacenter
- Phải lo việc mua bán và bảo trì phần cứng lẫn phần mềm
- Thường được dùng bởi Tổ chức chính phủ hoặc phòng ban cụ thể như Kế Toán,..

### Pros
- Có thể đáp ứng một số nghiệp vụ cụ thể
- Bảo mật và kiểm soát tốt (private nên ko share với ai khác)

### Cons

- Đắt hơn vì phải trả trước phí (CapEx) mua phần cứng
- Scale thì phải tự mua và setup phần cứng mới
  - Đòi hỏi kĩ năng chuyên môn

## Public cloud
- Loại thường được dùng nhất
  - Vd: Microsoft Azure hoặc AWS là public cloud/cloud provider
- CLoud provider sẽ lo phần bảo trì và quản lý phần cứng + phần mềm

### Pros
- Giá rẻ: 
  - Không cần bỏ tiền mua phần cứng hay phần mềm, chỉ trả tiền cho service nào dùng
  - Dùng chung với các cloud user khác (chung phần cứng, phần mềm,..)
- Mở rộng dễ dàng
- Tính khả dụng cao: có nhiều server dự phòng

### Cons
- Không đáp ứng được một số nghiệp vụ cụ thể
- Không thể tự quản lý phần cứng
- Không đáp ứng hết vấn đề bảo mật

## Hybrid cloud
- Sự kết hợp của public và private, cho phép host chỗ nào tùy ý
- Cho phép chuyển đổi dữ liệu và app qua lại giữa 2 môi trường (on-premise và cloud)

### Pros
- Dữ liệu và app/service có thể linh động ở local hoặc cloud hoặc cả 2
- Giảm độ trễ

### Cons
- Đắt hơn (CapEx) vì phải trả toàn bộ
- Setup quản lý phức tạp hơn

## Multi-cloud
- Áp dụng khi bạn dùng nhiều public cloud từ nhiều provider, cho phép quản lý resource giữa nhiều môi trường

## Azure Arc
- Là tập hợp các công nghệ giúp quản lý cloud enviroment (hỗ trợ bất kì loại nào trong 4 loại trên)