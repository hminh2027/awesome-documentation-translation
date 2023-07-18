# Azure Functions
![translation-status](https://img.shields.io/badge/Status-done-green)

1. What?
- Là một cách để chạy resource
- Vd: 
  - VM hoặc container cần chạy resource liên tục để app hoạt động
  - Function thì chỉ chạy resource khi có sự kiện trigger

2. Why?
- Có khả năng tự scale theo demand
- Có khả năng tự chạy code khi trigger và tự giải phóng sau khi xong
  -> giúp tiết kiệm chi phí (chỉ tính tiền theo thời gian dùng CPU)
- Có thể stateless hoặc stateful:
  - Stateless (default): restart mỗi lần chạy khi có sự kiện
  - Stateful (Durable Functions): có context truyền vào function để track lịch sử hoạt động

3. How?
- Có các tính chất như:
  - Event-driven: lắng nghe và phản hồi theo sự kiện
  - Serverless computing

## Scenarios
- Khi chỉ muốn chạy code/service mà không quan tâm cơ sở hạ tầng
- Thường dùng để phản hồi nhanh các sự kiện (qua REST req), timer, hoặc message từ Azure service khác

## Serverless computing
1. What?
- Thực chất vẫn sử dụng server nhưng nó sẽ do bên provider quản lý hết
- Người dùng chỉ lo dev và up code lên

2. Why?
- Không cần lo cơ sở hạ tầng (OS, hardware, ..)
- Khả năng scale tốt
- Pay-as-you-go (thời gian chạy code)

3. How
- Dùng 2 service là:
  - Azure Functions 
  - Azure Logic Apps