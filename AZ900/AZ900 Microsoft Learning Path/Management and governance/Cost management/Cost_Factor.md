# Cost Factor
![translation-status](https://img.shields.io/badge/Status-need_review-orange)

- Chi phí OpEx (dùng đâu trả đó) được tính dựa tren rất nhiều yếu tố:
  - Resource type
  - Consumption
  - Maintenance
  - Geography
  - Subscription type
  - Azure Marketplace

## Resource type
- Những thứ ảnh hưởng tới chi phí của resource
  - Loại resource
  - Setting của resource
  - Region

## Consumption
- Phương thức tính tiền pay-as-you-go phổ biến
- Ngoài ra Azure còn cung cấp phương thức "đặt trước" một số lượng resource nhất định và nhận discount (up to 72%)(có hạn sử dụng 1-3 năm)
- Vẫn có thể dùng thêm nếu lượng "đặt trước" không đủ

## Maintainence
- Luôn để mắt tới resource và ghé thăm trang cloud để quản lý chi phí tốt hơn
- Do tính flexibility của resource nên có thể gây phát sinh chi phí ngoài ý muốn
- Vd: 
  - tạo VM thì tự động tạo storage và networking
  - tắt VM thì 2 resource kia có thể sẽ ko tắt cùng lúc -> thêm tiền

## Geography
- Mỗi resource đều có một vị trí (region) cụ thể
- Vị trí khác nhau -> giá tiền khác nhau

## Subscription type
- Một số loại subscription được trợ cấp/khuyến mãi nên có thể ảnh hưởng tới chi phí
- Vd: Azure free trial sẽ cho dùng 1 vài service 12 tháng free và cho credit để dùng thử trong 30 ngày kể từ lúc đăng ký

## Azure Marketplace
- Là nơi cung cấp các giải pháp và service được xây dựa trên Azure (khác ở chỗ chúng được config sẵn và cài sẵn các phần mềm cần thiêt)
- Vì là chung gian bên thứ 3 cung cấp do vậy sẽ tính thêm phí