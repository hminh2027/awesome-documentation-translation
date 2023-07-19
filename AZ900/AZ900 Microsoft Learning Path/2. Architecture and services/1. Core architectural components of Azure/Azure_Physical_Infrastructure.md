# Azure Physical Infrastructure
![translation-status](https://img.shields.io/badge/Status-done-green)

- Phần này sẽ cover các khái niệm/component thông dụng như:
    - Regions
    - Availability Zones

## Regions
1. What?
- Region ám chỉ một vùng chứa ít nhất 1 datacenter gần nhau và kết nối với nhau
- Azure cân bằng tải đều resource trong region
- Mỗi lần deploy 1 resource thì phải chọn 1 region tương ứng

> ***Ghi chú: Một số service đòi hỏi region nhất định (vd: VM). Một số service thì lại không cần chọn (mang tính global)***

## Availability Zones
![av](./images/az_physic_availability.png)
1. What?
- Trong 1 region sẽ chia nhiều Availability Zones (AZ)
- Mỗi AZ có power, cooling và networking riêng biệt

2. Why?
- Các AZ trong region kết nối với nhau để cover lẫn nhau khi bị sập -> high availability

> ***Ghi chú: Để chắc cú thì cần tối thiểu 3 AZ mỗi region để tăng khả năng phục hồi***

3. How?
- Có thể chạy resource (compute, storage, networking,..) trên 1 AZ và sao chép nó thành bản sao dư phòng sang 1 AZ khác -> backup dữ liệu phòng bất trắc
- Có 3 loại service Azure hỗ trợ AZ:
    - Zonal services: ghim resource vào một zone cụ thể (vd: VM, IP addess, managed disk)
    - Zone-redundant services: tự động sao chép giữa các zone (vd: zone-redundant storage, SQL Database)
    - Non-regional services: là các service khả dụng trên khắp thế giới (không bó buộc vào region nào)

## Region pairs
![region](./images/az_physic_region.png)

1. What?
- Mỗi region được bắt cặp với region khác trong cùng 1 vùng địa lý (vd: US, Erope, Asia, ...) phạm vi 300m

2. Why?
- Giúp khi sao chép dữ liệu thì hạn chế được các thiên tai (động đất, lũ lụt, mất điện, bạo động,...) ảnh hưởng tới cả bản sao lẫn bản gốc

- Region bắt cặp sẽ giúp việc phục hồi nhanh hơn nếu cái còn lại có trục trặc

> ***Ghi chú: không phải service nào cũng tự sao lưu sang region khác (cần tự config)***

> ***Ghi chú: Thường thì 2 region backup lẫ nhau theo 2 chiều. Tuy nhiên 1 vài region đặc biệt thì chỉ được 1 chiều (India, Brazil,...)***

## Sovereign Regions
1. What? 
- Còn gọi là region "cao cấp"
- Các region instance này độc lập với instance của Azure

2. Why?
- Dùng cho mục đích liên quan tới luật pháp hoặc chính phủ
