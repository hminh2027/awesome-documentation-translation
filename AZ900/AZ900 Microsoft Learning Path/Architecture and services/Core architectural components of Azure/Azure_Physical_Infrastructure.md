# Azure physical infrastructure

## Physical infrastructure
- Chứa các datacenter gồm các trang bị như: 
## Regions
- Một region là một vùng địa lý mà trong đó nó chứa ít nhất 1 hoặc nhiều datacenter được kết nối với nhau
- Các tài nguyên sẽ được chia sẻ đều và quản lý trong mỗi region
- Khi deploy một tài nguyên lên Azure, bạn sẽ cần chọn region mình muốn deploy

### Availability Zones
- Availability Zones phân chia các datacenter ra riêng biệt trong cùng một Azure region
- Chúng được setup trang bị riêng biệt đầy đủ và kết nối với nhau với tốc độ mạng cực nhanh, với mục đích là tăng tính hồi phục và khả dụng (một cái tèo thì các cái còn lại vẫn hoạt động)


### Use availability zones in your apps
- Đôi khi bạn sẽ muốn có dữ liệu và dịch vụ dự phòng để bảo đảm sự an toàn, tuy nhiên việc setup sẽ yêu cầu bạn phải tạo ra một môi trường mới với các phần cứng trùng lặp

⇒ Azure giải quyết bằng **Availability Zones**

- Bạn có thể setup ứng dụng của bạn trong một **Availability Zone**, đồng thời setup một phiên bản trùng lặp để dự phòng ở một **Availability Zone** khác
- Availability Zones thường được sử dụng cho các máy ảo, các đĩa cứng, các bộ cân bằng tải và csdl SQL.
- Service hỗ trợ Availability Zones của Azure gồm 3 loại:
    - Zonal services: ghim tài nguyên của bạn tới một zone cụ thể (ví dụ như máy ảo, đĩa cứng, IP addess)
    - Zone-redundant services: là nền tảng tự động sao chép giữa các zones (ví dụ như csdl SQL, zone-redundant storage)
    - Non-regional services: các service luôn khả dụng trên khắp khu vực địa lý của Azure và luôn có khả năng phục hồi tốt đối với zone-wide outages và region-wide outages (đoạn này tìm hiểu thêm)
### Region pairs
- Hầu hết các region của Azure đều bắt cặp với region khác trong cùng phạm vi địa lý (ví dụ như Mỹ, Châu Âu, Châu Á, ..) với khoảng cách tối thiểu 300m.
- Việc này giúp giảm thiểu thiệt hại cho các nguồn tài nguyên dùng để dự phòng (nếu để gần nhau quá thì lũ lụt, động đất, bạo động, mất điện, … sẽ ảnh hưởng đến cả bản sao lẫn bản gốc)

> Không phải service nào của Azure cũng tự động sao lưu dữ liệu dự phòng tới region khác. Trong trường hợp này thì người dùng phải tự config cho việc phục hồi và sao lưu dữ liệu 

### Additional advantages of region pairs:
- Nếu một region ngừng hoạt động thì region còn lại (cùng cặp) sẽ được ưu tiên để phục hồi ứng dụng càng nhanh càng tốt
- Một region chỉ bắt cặp với một region duy nhất để giảm thiểu downtime và tỉ lệ ngừng hoạt động của app
> Thông thường thì 2 region sẽ backup lẫn cho nhau theo 2 chiều (West US và East US). Tuy nhiên một vài region như West India hay Brazil South thì thường chỉ backup một chiều (Chỉ region chính backup cho region phụ)

### Sovereign Regions
- Ngoài các region thông thường thì Azure cũng có các region “tối thượng” 😀. Sovereign regions là các instance độc lập với instance chính của Azure (thường dùng cho các mục đích hợp pháp hoặc thi hành)
- Ví dụ như chính phủ mỹ, miền tây và bắc Trung Quốc, ..