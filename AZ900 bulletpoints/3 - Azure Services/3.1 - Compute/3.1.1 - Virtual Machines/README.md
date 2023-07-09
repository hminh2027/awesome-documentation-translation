# Virtual Machines
![translation-status](https://img.shields.io/badge/Status-in_review-orange)


1. **What? - Nó là gì**
    - Thuộc dạng **Infrastructure as a service (IaaS)**
    - Các máy ảo là các chương trình giả lập một chiếc máy tính thật
    - Một máy ảo gồm nhân xử lý ảo, bộ nhớ, lưu trữ và tài nguyên mạng
    - Bạn có thể cài đặt hệ điều hành tùy ý muốn
    - Bạn có thể kết nối tới máy ảo và điều khiển nó từ xa bằng phần mềm
2. **When? - Là lựa chọn tốt khi bạn cần**
    - Toàn quyền kiểm soát hệ điều hành
    - Chạy phần mềm được custom
    - Sử dụng cấu hình hosting được custom
3. **How? - Như thế nào**
    - Azure sẽ lo quản lý phần cứng
    - Bạn lo quản lý phần mềm (cấu hình, nâng cấp, bảo trì phần mềm trên máy ảo)
- Một **image** là một template để tạo ra máy ảo
    - Nó bao gồm hệ điều hành và các phần mềm khác như các dev tools hoặc web hosting environments.

## Ví dụ các use-case cụ thể

- **Giai đoạn test + dev**: dễ dàng cài đặt hệ điều hành và cấu hình riêng biệt cho ứng dụng. Khi nào không cần nữa thì xóa phát là xong
- **Các task nhỏ**: khả năng tự thêm bớt máy ảo để đáp ứng được nhu cầu sử dụng chương trình
- **Mở rộng datacenter sang cloud**: dùng SharePoint
- **Giai đoạn hồi phục sau sự cố**: nếu datacenter chính bị sập thì bạn có thể tạo máy ảo để chạy app của bạn trên Azure, sau đó tắt đi khi datacenter của bạn hồi phục xong
- **Di cư ứng dụng**: Dễ dàng chuyển datacenter vật lý lên cloud bằng cách tạo image từ server của bạn và vứt lên máy ảo.
- Thêm một số use-case khác: [Typical scenarios for running Azure VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/overview?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)

## Scaling và High Availability

- [Đảm bảo 99.99% uptime](https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_8/) đối với toàn bộ máy ảo nếu chúng có các instance (sl ≥ 2) được deploy ở các Availability Zone khác nhau (sl ≥ 2)

### Domains & maintenance events

**Update domains**

1. **What? - Nó là gì**
- Là tập hợp các máy ảo và phần cứng bên dưới có khả năng khởi động lại cùng một lúc

**Planned maintenance events (Bảo dưỡng định kì)**

1. **When? - Khi nào cần**
    - Khi các Azure Fabric đang host máy ảo được Microsoft cập nhật
2. **Why? - Để làm gì**
    - Mục đích để vá các lỗ hổng bảo mật, cải thiện hiệu suất và thêm hoặc cập nhật các tính năng.
3. **How? - Tính chất**
    - Thường không ảnh hưởng tới các phần khác, đôi khi cần khởi động lại
    - Khi máy ảo là một phần của một availability set thì các bản cập nhật Azure fabric sẽ diễn ra nối tiếp nhau theo thứ tự, tránh tất cả các máy ảo liên kết nhau bị khởi động lại cùng một lúc
        - Các máy ảo sẽ được đặt vào các update domain khác nhau

**Fault domains**

1. **What? - Nó là gì**
    - Gồm một nhóm các máy ảo sử dụng chung một nguồn điện và mạng… Trong một availability set thường tách các máy ảo ra thành tối đa 3 fault domain khác nhau.
    - 1 fault domain = 1 rack server.
2. **Why? - Vì sao cần**
    - Cho phép chia workload vật lý ra thành nhiều phần cứng nguồn, tản nhiệt và mạng khác nhau, nhằm hỗ trợ các server vật lý trong data center server racks
    - Trong trường hợp phần cứng hỗ trợ server rack không khả dụng thì chỉ có server rack đó bị ảnh hưởng

**Unplanned maintanance events (Bảo dưỡng bất ngờ)**

- Do lỗi của một bộ phận phần cứng trong data center (vd: mất điện hoặc lỗi ổ cứng)
- Các máy ảo mà cùng thuộc một availability set thì tự động đổi sang một server vật lý đang hoạt động để máy ảo tiếp tục chạy
- Các máy ảo có phần cứng giống nhau thì nằm trong cùng một **fault domain**

### Availability sets

1. **What?**
    - Nhóm các máy ảo một cách logic giúp cho app của bạn khả dụng trong suốt quá trình bảo trì định kì hoặc bảo trì bất chợt
2. **How?** 
    - Trong một **availability set** ta có:
        - Tối đa 3 **fault domain**
            - Mỗi **fault domain** có 1 server rack với nguồn điện và tài nguyên mạng chuyên dụng
        - Tổng 5 logical **update domain**
            - Tối đa là 20
    - Không mất xiền cho một **availability set**
        - Nhưng mất tiền tính theo các máy ảo trong set đó
    - 💡📝 Khuyên dùng nếu cần **High Availability**

### Virtual machine scale sets

- Cho phép bạn tạo và quản lý một nhóm các máy ảo giống nhau và được phân chia đều bằng load balancer
- Cho phép bạn quản lý, cấu hình và cập nhật các máy ảo theo số lượng lớn nhằm tăng tính khả dụng cho app
- Số lượng các instance của máy ảo có thể tự động tăng giảm tùy theo nhu cầu hoặc theo lịch trình cụ thể
- 💡 Giúp bạn xây dựng các service có quy mô lớn trong các lĩnh vực như điện toán, big data hoặc lưu trữ workload
- Cung cấp tính khả dụng cao thông qua phương thức deploy theo [regional hoặc availability zones](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones)

### Azure Batch

- Khi khởi chạy một job, batch sẽ giúp bạn:
    1. Khởi tạo một pool gồm các máy ảo điện toán cho bạn
    2. Cài đặt các ứng dụng và xử lý dữ liệu
    3. Chạy các **job** bằng với số lượng task của bạn
    4. Xác định các lỗi
    5. Requeue work
    6. Scale nhỏ pool lại khi hoàn thành công việc
- 💡 Ứng dụng tốt cho các trường hợp bạn cần sức mạnh tính toán thô hoặc sức mạnh tính toán của siêu máy tính

---

Technical terms:

1. Datacenter: trung tâm tập trung các server vật lý
2. Availability set: tập hợp các máy ảo
3. Workload: khối lượng công việc
4. Rack server: một dạng server đặc biệt để lắp vào giá
5. Work: đơn vị nào đó?
6. Job: là một chuỗi các hành động chạy theo thứ tự (đơn vị đo nhỏ nhất của work) (dùng trong pipeline)