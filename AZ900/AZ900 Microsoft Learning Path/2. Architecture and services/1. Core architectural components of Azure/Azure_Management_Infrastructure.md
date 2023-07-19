# Azure Management Infrastructure
![translation-status](https://img.shields.io/badge/Status-done-green)

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
- Là nhóm tập hợp các unique resources

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
- Là nhóm tập hợp các resource groups

2. Why?
- Thường dùng để tính tiền, mở rộng và quản lý resources

3. How?
- Cần tạo 1 subsription (sub) trong account (acc) để sử dụng (mỗi acc có thể có nhiều sub)
- Có thể định nghĩa ra các giới hạn như:
  - **Billing boundary**: có thể tạo nhiều sub để quản lý bill và hóa đơn riêng biệt
  - **Access control boundary**: có thể tạo nhiều sub cho từng mô hình tổ chức khác nhau

## Azure management groups
1. What?
- Là một tầng quản lý cao hơn giúp quản lý các resource group dễ dàng (policies, access, ...)

2. Why?
- Dữ dàng quản lý theo nhóm (vì có tính kế thừa)

3. How?
- Management group có thể lồng nhau
- Cấu trúc ví dụ như hình dưới:
![management](./images/az_management_management.png)

- Một vài facts về cây cối:
  - 1 dicrectory có thể có 10000 management groups
  - 1 management group tree có độ sâu lên đến 6 (không gồm root hoặc subscription level)
  - 1 management group hoặc subscription chỉ có 1 parent duy nhất