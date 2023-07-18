# Azure management  infrastructure
![translation-status](https://img.shields.io/badge/Status-in_progress-blue)

- Phần này sẽ cover các khái niệm/component thông dụng như:
    - Resources
    - Resource groups
    - Subscriptions
    - Accounts

## Resources 
1. What?
- Resource (hay building block) là bất cứ thứ gì bạn tạo, deploy, đưa lên cloud của Azure
- Vd: VM, DB, service, ...

## Resource groups
![group](./images/az_management_group.png)

1. What?
- Resource group là nhóm các unique resources

2. Why?
- Tiện lợi khi cần tác động/tương tác tới tất cả resource trong 1 nhóm
- Khi đó cứ apply lên group là được

3. How?
- Có các tính chất sau:
  - Group không được lồng nhau
  - Một vài resource có khả năng chuyển qua lại giữa các group

## Subscriptions
![subscriptions](./images/az_management_subscriptions.png)

1. What?
- Là một container chứa các resource và sự kết nối của chúng

2. Why?
- Thường dùng để tính tiền, mở rộng và quản lý resource

3. How?
- Cần tạo 1 subsription trong account để sử dụng (mỗi acc có thể có nhiều sub)
- Có thể định nghĩa ra các giới hạn như:
  - **Billing boundary**: có thể tạo nhiều sub để quản lý bill và hóa đơn riêng biệt
  - **Access control boundary**: có thể tạo nhiều sub cho từng mô hình tổ chức khác nhau