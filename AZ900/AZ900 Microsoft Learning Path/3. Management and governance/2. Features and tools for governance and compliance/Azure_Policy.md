# Azure Policy
![translation-status](https://img.shields.io/badge/Status-done-green)

1. What?
    - Là 1 service cho phép tạo, assign và manage các policies để kiểm soát resources.

2. Why?
    - Rà soát các resource và đánh dấu những cái không tuân thủ policy áp dụng
    - Ngăn chặn việc tạo ra các resource không tuân thủ policy
    
3. How?
    - Cho phép tạo policy riêng lẻ hoặc theo nhóm (gọi là initiatives*)
    - Set được cho từng level (resource, group resource, subscription, …)
    - Azure Policy có tính kế thừa (apply cho cha thì cho cả con)
    - Azure Policy có sẵn policy và initiative cho Storage, Networking, Compute, Security Center và Monitoring
        - Vd: Bạn tạo và apply policy quy định giới hạn size của VMs. Policy sẽ áp dụng cho cả những VMs tồn tại trước đó, đồng thời áp dụng cả khi tạo mới hoặc resize VM
    
## Azure Policy initiatives
1. What?
- Là một cách để nhóm các policies liên quan lại với nhau
- Giúp quản lý sự tuân thủ rộng hơn với mục đích lớn