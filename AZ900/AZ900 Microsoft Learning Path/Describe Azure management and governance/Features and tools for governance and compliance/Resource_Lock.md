## Resource locks

**1. What?**
- Là 1 feature của Azure

**2. Why?**
- Chặn việc xoá/cập nhật nhầm tài nguyên
- Apply đè lên cả RBAC (tức kể cả là owner vẫn bị lock)

**3. How?**
- Có thể apply cho:
  - resource
  - group resources
  - subscription
- Có tính chất kế thừa (tức apply cho 1 nhóm thì toàn bộ tài nguyên bên trong sẽ apply)

### Types

- **Delete**: read + update - delete
- **ReadOnly**: read - update - delete

### Manage

- Quản lý qua 
  - Azure portal
  - PowerShell
  - CLI
  - Azure Resource Manager template