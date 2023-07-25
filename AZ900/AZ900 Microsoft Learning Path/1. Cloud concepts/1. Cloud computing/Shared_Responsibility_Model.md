# Shared Responsibility Model
![translation-status](https://img.shields.io/badge/Status-done-green)

![table](./images/az_computing_table.svg)
1. What?
- Là mô hình để xác định xem ai sẽ là người chịu trách nhiệm cho cái gì khi sử dụng cloud service

2. How?
- Dựa theo ảnh trên ta rút ra được:
  - Người dùng luôn chịu trách nhiệm:
    - Thông tin và dữ liệu lưu trên cloud
    - Các thiết bị được truy cập
    - Tài khoản và danh tính của người truy cập

  - Cloud provider luôn chịu trách nhiệm:
    - Datacenter vật lý
    - Network vật lý
    - Host vật lý

  - Tùy mỗi service model sẽ phân chia trách nhiệm cho 1 trong 2:
    - OS
    - Kiểm soát network
    - App
    - Xác thực và cơ sở hạ tầng