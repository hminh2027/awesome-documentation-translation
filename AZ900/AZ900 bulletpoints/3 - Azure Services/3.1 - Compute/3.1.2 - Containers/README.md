# Containers
![translation-status](https://img.shields.io/badge/Status-done-green)

1. **What? - Nó là gì**
    - Là một môi trường ảo để chạy các app
    - Tương tự với VM
2. **So sánh Container với VM**
    
    ![Kiến trúc của Container](../3.1.2%20-%20Containers/images/container.png)
    
    Kiến trúc của Container
    
    ![Kiến trúc của VM](../3.1.2%20-%20Containers/images/vms.png)
    
    Kiến trúc của VM
    
    |  | Container | VM |
    | --- | --- | --- |
    | Architecture | Không chứa OS và Kernel (Dùng chung OS của thằng host) | Có OS và Kernel riêng (tốn tài nguyên hơn) |
    | Size | Nhẹ hơn nhiều so với máy ảo |_|
    Phản hồi nhanh hơn khi nhu cầu sử dụng tăng giảm hoặc khi gặp lỗi |_ |Tốn tài nguyên hơn (CPU, bộ nhớ, lưu trữ) |
    | Compatibility | Dùng chung OS version với thằng host | Dùng bất cứ OS nào trong mỗi VM |
    - **Container Orchestrator*** có thể tự động start, stop và scale các instance của app nếu cần thiết
        - Nhanh hơn VM (tính theo giây thay vì phút)
    - Bạn có thể dùng chung 1 server để host nhiều app container
        - Hoàn toàn bảo mật và độc lập

⇒ Chốt: Container hiệu quả hơn VM vì:
- Chạy các container cạnh nhau mà vẫn giữ chúng độc lập
- Kích thước bé hơn
- Quá trình dev đơn giản hơn (Dev env = prod env)

## Containers trong Azure

### Azure Container Instances

- PaaS: Cách nhanh và dễ nhất để chạy container
- Không cần cấu hình cho VM hay bất cứ service nào khác
- Cứ upload container lên và chạy với khả năng tự động scale

### **Azure Kubernetes Service**

- **Orchestration** = Lo việc tự động hoá, quản lý và điều khiển một số lượng lớn các container
- **Azure Kubernetes Service** (AKS) là một service orchestration
- Bạn có thể migrate các app lên AKS bằng cách:
    1. Chuyển app hiện tại lên một (hoặc hơn) container, sau đó publish một (hoặc hơn) **image** lên **Azure Container Registry**
    2. Dùng Azure portal hoặc CLI để deploy các container lên AKS cluster 
    3. Azure AD kiểm soát quyền truy cập vào tài nguyên AKS
    4. Bạn truy cập vào các Azure service được SLA hỗ trợ thông qua OSBA chẳng hạn như Azure Database cho MySQL 
    5. You access SLA-backed Azure services, such as Azure Database for MySQL, via OSBA.
    6. AKS được deploy bằng mạng ảo (không bắt buộc)

**Kubernetes**

1. **What? - Nó là gì**
    - Là lựa chọn phổ thông cho việc quản lý các workload trên container
    - Kết hợp API với việc tự động hoá quản lý container
    - Thuần cloud: có thể chạy trên các nền tảng cloud khác nhau
    - Quản lý **Pod***:
        - Quản lý vị trí của các pod
        - 1 pod = 1 (hoặc nhiều) container trên một node
            - Nếu node bị xoá = Kubernetes chuyển các workload bị ảnh hưởng sang node khác
        - Nếu 1 pod bị crash = Kubernetes tạo 1 instance khác
        - Các pod có thể scale thủ công hoặc tự động (theo chiều ngang)
    - Phân chia việc deploy để giảm thiểu downtime
        - Nếu bản cập nhật có vấn đề thì nó có thể roll-back
    - Có thể quản lý khối lượng lưu trữ
        - **Persistent volumes*** đại diện lưu trữ dữ liệu cho một hoặc nhiều container
        - Dữ liệu có thể được bảo tồn xuyên suốt nhiều instance của pod
        - Có thể sử dụng hệ thống lưu trữ và dữ liệu trên cloud (vd: Azure storage + Cosmos DB)
    - Có thể quản lý mạng
        - Có thể expose pod lên internet
        - Cân bằng tải lượng traffic đều cho các bản sao của 1 pod
        - Mạng tách biệt độc lập
        - An ninh mạng dựa theo chính sách
        - Quản lý giao tiếp + **name resolution*** giữa các pod
    - Có thể mở rộng chức năng
        - Ví dụ: các sự kiện cloud trên pod creation, custom pod scheduling logic, on-demand provisioning of managed cloud services.

## Micro-services

1. **What? - Nó là gì**
    - Là một dạng kiến trúc mà bạn tách nhỏ các giải pháp ra thành các phần độc lập2
2. Why? - Lợi ích
    - Dễ bảo trì, mở rộng và cập nhật riêng lẻ
    - Không cần phải dùng chung tech stack, thư việc hoặc framework
        - Mỗi team có thể tự chọn tool hợp lý để làm
        - Mỗi team dev có thể tự test, build và deploy một service
    - Kết quả là luôn có sự đổi mới liên tục và tần suất release sẽ nhanh hơn
    - Phạm vi sẽ nhỏ hơn
        - Dễ quản lý code-base
        - Dễ bắt đầu cho thành viên mới vào
    - Mỗi micro-service đều hoàn toàn độc lập, không phụ thuộc  lẫn nhau
        - Cung cấp **fault isolation***: nếu một service bị sập thì nó sẽ không ảnh hưởng phần khác
3. **How? - Như thế nào**
    - Mỗi service có một đoạn code-base nhỏ được quản lý bởi một team dev nhỏ
    - Giao tiếp với service khác dùng APIs
        - APIs đóng gói các chức năng nội bộ
            - Chi tiết việc cài đặt nội bộ của mỗi service đều được đóng gói
        - Good practices:
            - Giảm sự phụ thuộc lẫn nhau
            - Có một lớp quản lý ở tầng cao hơn để điều phối các calls và kết hợp các kết quả
    1. **When? - Dùng cho**
        - Tốc độ release nhanh
        - Các chương trình có khả năng mở rộng cao
        - Các chương trình có nhiều số lượng domain/subdomain
        - Các tổ chức có nhiều team dev nhỏ

### Micro-services deployment

- Cho phép deploy từng microservice một cách độc lập
- Một team có thể update một service mà ko cần build lại hoặc deploy lại cả ứng dụng
- Dễ dàng roll back và roll forward & cập nhật nếu có lỗi
- Giúp việc fix bug và việc release feature trở nên dễ quản lý hơn và ít rủi ro hơn
- Cho phép mỗi microservice
    - scale độc lập
    - bảo tồn dữ liệu của chính nó và các state bên ngoài mà không cần một lớp repository chung
- Ví dụ: một service cho front-end, một cho back-end và một cái để lưu trữ
    - Nếu back-end đạt giới hạn sử dụng nhưng các cái khác vẫn hoạt động thì ta chỉ cần scale riêng back-end
    - Cho phép bạn thay thế microservice lưu trữ mà không ảnh hưởng tới phần còn lại của ứng dụng

### Azure Service Fabric

1. What?
    - Là nền tảng hệ thống phân tán
    - Chạy trên Azure hoặc tại on-premise*

---

1. Kernel: nhân hệ điều hành
2. Container orchestrator: công cụ giúp tự động hoá các container trong việc quản lý, mở rộng,…
3. Pod: đơn vị nhỏ nhất để đo hoạt động trong Kubernetes, tạo bởi 1 hoặc nhiều container
4. Persistent Volume (PV): một khái niệm trong Kubernetes
5. Name resolution: hành động chuyển dổi tên thành các địa chỉ mạng
6. Fault isolation: giới hạn sự ảnh hưởng của một component nào đó khi nó gặp lỗi (cô lập lỗi)
7. On-premise: tại chỗ (ở đây nói tại chính doanh nghiệp, không qua đám mây)