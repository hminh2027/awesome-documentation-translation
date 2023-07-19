# Azure Policy
![translation-status](https://img.shields.io/badge/Status-in_progress-blue)

1. What?
    - Là 1 service cho phép tạo, assign và manage các policies để kiểm soát resources.

2. Why?
    - Rà soát các resource và đánh dấu những cái không tuân thủ policy áp dụng
    - Ngăn chặn việc tạo ra các resource không tuân thủ
    
3. How?
    - Cho phép tạo policy riêng lẻ hoặc theo nhóm (gọi là initiatives)
    - Set được cho từng level (resource, group resource, subscription, …)
    - Azure Policy có tính kế thừa (apply cho cha thì cho cả con)
    - Azure Policy có sẵn policy và initiative cho Storage, Networking, Compute, Security Center và Monitoring
        - Vd: For example, if you define a policy that allows only a certain size for the virtual machines (VMs) to be used in your environment, that policy is invoked when you create a new VM and whenever you resize existing VMs. Azure Policy also evaluates and monitors all current VMs in your environment, including VMs that were created before the policy was created.