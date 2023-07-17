# Platform as a Service
![translation-status](https://img.shields.io/badge/Status-done-green)

- Mix giữa 2 loại IaaS và SaaS

## Shared responsibility model
![model](./images/az_paas_model.svg)

- Bạn chịu trách nhiệm cho:
  - Dữ liệu đầu vào
  - Thiết bị
  - Người truy cập

- Cloud provider chịu trách nhiệm cho:
  - Bảo trì cơ sở hạ tầng vật lý
  - Kết nối mạng
  - OS
  - Database
  - Dev tools

- Tùy vào tình huống, những việc sau có thể ở 1 trong 2 bên:
  - Setting mạng và kết nối
  - Bảo mật màng và app
  - Directory infrastructure.

-> Trách nhiệm chia đôi

## Scenarios
- Các case thông dụng:
  - **Dev framework**: cho phép tạo app bằng các component cấp sẵn, bớt effort và tăng hiệu suất
  - **Phân tích dữ liệu hoặc nghiệp vụ thông minh**: Có tool phân tích và khai phá dữ liệu, tìm insight và dự đoán thông số