# Azure DNS
![translation-status](https://img.shields.io/badge/Status-done-green)

1. What?
- Là một service cho DNS domain với tính năng **name resolution**
- Cho phép quản lý danh sách DNS record với sự hỗ trợ của APIs, tools, billing và các service khác

2. Why?
- Dễ dùng
- Đáng tin cậy, hiệu suất và bảo mật tốt
- Virtual network có thể customize
- hỗ trợ alias record

## Reliability and performance
- DNS domain được host trên DNS name server toàn cầu của Azure -> resiliency & high availability
- **Azure DNS** dùng anycast networking -> fast performance & high availability

## Security
- **Azure DNS** dựa theo **Azure Resource Management** các tính năng sau:
  - **Azure RBAC** để kiểm soát quyền truy cập và thực hiện hành động nhất định
  - **Activity log** để theo dõi lịch sử chỉnh sửa resource và để truy vết lỗi
  - **Resource locking** để tránh xóa hoặc sửa nhầm resource quan trọng

## Ease of use
- Tích hợp sẵn trên portal -> không cần credentials, support contract hay billing riêng như các service khác
- Có thể quản lý các domain và record bằng:
  - Portal
  - Powershell
  - CLI

## Customizable virtual network with private domains
- Cho phép dùng domain name do bạn tự đặt trong VPN, không nhất thiết phải dùng của Azure

## Alias records
- **Alias record set** trỏ tới một instance của service và mỗi service gắn với 1 địa chỉ IP
- Nếu địa chỉ IP của service thay đổi thì **Alias record set** sẽ tự update trong quá trình DNS resolution

> ***Note: không dùng service này để mua domain name được mà phải mua bằng App Service hoặc mua bên ngoài***