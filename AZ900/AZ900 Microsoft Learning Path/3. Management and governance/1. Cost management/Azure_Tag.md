# Azure Tag
![translation-status](https://img.shields.io/badge/Status-need_review-orange)

1. What?
- Là các element cung cấp thêm metadata cho resource
- Mục đích là để quản lý các resource dễ hơn
- Lưu plain text dưới dạng key-value
- Có thể apply cho resource, resource group và subscription
- Không có tính chất kế thừa

2. Why?
- Nó có các công dụng như:
  - Quản lý resource: giúp tìm vị trí resource có liên quan tới các thông tin cụ thể như workload, enviroment, business unit hoặc owner.
  - Quản lý và tối ưu chi phí: 
  - ...
3. How?
- Thêm sửa xóa tag thông qua:
  - Window Powershell
  - Azure CLI
  - Azure portal
  - Azure Resource Manager
  - REST API
- Có thể dùng kết hợp với Azure Policy
