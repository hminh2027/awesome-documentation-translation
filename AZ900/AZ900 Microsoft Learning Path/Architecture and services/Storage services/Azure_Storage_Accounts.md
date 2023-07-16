# Azure Storage Accounts
![translation-status](https://img.shields.io/badge/Status-in_progress-blue)

1. What?
- Là tài khoản lưu trữ, nó cung cấp 1 namespace cho dữ liệu của Azure Storage
2. Why?
- Truy cập ở bất cứ đâu qua HTTP/HTTPS
- Dữ liệu được bảo mật, khả dụng cao, bền bỉ và có thể mở rộng tốt
3. How?
- Khi tạo account, phải chọn loại account theo tiêu chí
  - Dùng để lưu trữ cho service nào
  - Redundance option

## Redundancy options
  - Locally redundant storage (LRS)
  - Geo-redundant storage (GRS)
  - Read-access geo-redundant storage (RA-GRS)
  - Zone-redundant storage (ZRS)
  - Geo-zone-redundant storage (GZRS)
  - Read-access geo-zone-redundant storage (RA-GZRS)

| Type                          | Service hỗ trợ                                            | Phương án dự phòng                   | tác dụng                                                                                                             |
| ----------------------------- | --------------------------------------------------------- | ------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| Standard general - purpose v2 | Blob Storage, Queue Storage, Table Storage và Azure Files | LRS, GRS, RA-GRS, ZRS, GZRS, RA-GZRS | Tài khoarnn standard dùng cho blob, file share, quêu và table. Thường được recommend cho các case dùng Azure Storage | kv 