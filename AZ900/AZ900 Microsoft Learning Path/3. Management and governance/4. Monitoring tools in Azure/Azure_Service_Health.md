# Azure Service Health
![translation-status](https://img.shields.io/badge/Status-done-green)

1. What?
- Được tạo bởi sự kết hợp của 3 services, mục đích để quản lý các Azure resource và trạng thái tổng thể của chúng

2. Why?
- Cung cấp cái nhìn tổng thể của Azure nói chung và các service của Azure nói riêng (cụ thể tới từng region và resource)

3. How?
- Gồm 3 services sau:
  - Azure Status
  - Service Health
  - Resource Health

## Azure Status
1. What?
- Là service cung cấp bức tranh tổng quát về trạng thái hiện tại của Azure (global), gồm:
  - Trạng thái, tình hình của toàn bộ service khắp các region

2. Why?
- Dùng để tham khảo tầm cỡ vĩ mô

## Service Health
1. What?
- Là service cung cấp tầm nhìn hẹp hơn, phạm vi thu gọn trong các service và region mà bạn đang dùng

2. Why?
- Có thể tìm thông tin về các hoạt động bảo trì, các sự cố hoặc vấn đề làm service không hoạt động

3. How?
- Có thể setup thông báo khi các service của bạn bị tác động

## Resource Health
1. What?
- Là service cung cấp tầm nhìn hẹp hơn, phạm vi thu gọn trong các resource mà bạn đang dùng
- Vd: trạng thái từng instance của VM

2. How?
- Có thể setup thông báo khi các resource của bạn bị tác động