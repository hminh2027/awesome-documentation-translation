# Azure Virtual Machines
![translation-status](https://img.shields.io/badge/Status-done-green)

1. What?
- Thuộc loại **IaaS**
- Là máy giả lập gồm nhân xử lý ảo, bộ nhớ, kết nối mạng, OS,...
2. When?
- Khi cần toàn quyền kiểm soát OS
- Khi cần chạy phần mềm được custom
- Khi cần dùng hosting config được custom
3. How?
- Azure lo phần cứng
- Bạn lo phần mềm (cấu hình, nâng cấp, bảo trì phần mềm)

## Use-cases
- **Giai đoạn test + dev**: dễ dàng cài đặt OS và config riêng cho ứng dụng. Khi nào không cần nữa thì xóa
- **Các task nhỏ**: khả năng tự thêm/bớt VMs để đáp ứng được nhu cầu sử dụng app
- **Mở rộng datacenter lên cloud**: dùng SharePoint
- **Khắc phục sự cố**: nếu datacenter chính bị sập thì bạn có thể tạo VM để chạy app của bạn trên Azure, sau đó tắt đi khi datacenter của bạn hồi phục xong
- **Di cư ứng dụng**: Dễ dàng chuyển datacenter vật lý lên cloud bằng cách tạo image từ server của bạn và vứt lên VM.
- Thêm một số use-case khác:  [Typical scenarios for running Azure VMs](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/overview?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) 

## Scale VMs
- Có thể nhóm VMs với nhau để tăng high availability, scalability và redundancy
- Quản lý nhóm VMs bằng Scale Sets & Availability Sets

### VM scale sets
1. What?
- Là 1 tool cho phép tạo và quản lý nhóm các VMs giống nhau và load-balanced
2. Why?
- Cho phép quản lý, cấu hình và cập nhật nhiều VMs cùng lúc 
- Số lượng instances của VM có thể tự động tăng/giảm tùy theo nhu cầu hoặc lịch trình cụ thể
3. When?
- Ứng dụng tốt trong computing, big data, container workload

### VM availability sets
1. What?
- Là 1 tool giúp build một môi trường có tính hồi phục tốt và khả dụng cao
2. Why?
- Miễn phí không mất tiền config availability set
- Chỉ tính tiền dựa theo instance của VM
3. How?
- Có 2 cách nhóm VMs:
	- Update domain
	- Fault domain

- Update domain:
	- Tập hợp các VMs có thể reboot cùng nhau
	- Cho phép apply bản update 
	- Mỗi update domain sẽ có 30 phút để hồi phục trước khi sang nhóm khác

- Fault domain:
	- Tập hợp các VMs dùng chung nguồn điện hoặc dây/ổ cắm mạng
	- 1 availability set sẽ chia các VMs ra thành tối đa
	-  3 fault domain

  -> phòng rủi ro mất điện hoặc lỗi mạng (vì VMs kết nối điện vs mạng khác nguồn nhau)

## Move to cloud with VM
- Tạo **Image** của server vật lý và vứt lên VM
- Một **Image** là 1 template gồm OS và các app bên trong nó

## VM Resources
- Khi tạo VM thì bạn được quyền chọn các resource đi kèm:
	- Size (mục đích, số lượng nhân xử lý và kích cỡ RAM)
	- Storage disks (hard disk drives, solid state drives, …)
	- Networking (virtual network, public IP address và config port)

