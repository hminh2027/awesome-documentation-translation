# Azure Blueprints
![translation-status](https://img.shields.io/badge/Status-in_progress-blue)

1. What?
    - Là 1 tính năng giúp lặp lại các setting/config và policy cho subscription/environment
2. Why?
    - Tiết kiệm thời gian công sức khi cần thêm subscription/environment mới
3. How?
    - ?

### Artifact là gì?

- Là đơn vị gọi cho 1 component trong blueprint
- Các artifact nhận các params truyền vào (optional)
- VD: Allowed locations Policy nhận 1 param là vị trí cụ thể

[image]

- Params của Artifact có thể được truyền vào khi:
  - tạo mới
  - assign cho 1 scope

-> 1 blueprint có thể có params khác nhau ở mỗi scope


[CONTINUE HERE...]

Azure Blueprints deploy a new environment based on all of the requirements, settings, and configurations of the associated artifacts. Artifacts can include things such as:

- Role assignments
- Policy assignments
- Azure Resource Manager templates
- Resource groups

# How do Azure Blueprints help monitor deployments?

Azure Blueprints are version-able, allowing you to create an initial configuration and then make updates later on and assign a new version to the update. With versioning, you can make small updates and keep track of which deployments used which configuration set.
With Azure Blueprints, the relationship between the blueprint definition (what should be deployed) and the blueprint assignment (what was deployed) is preserved. In other words, Azure creates a record that associates a resource with the blueprint that defines it. This connection helps you track and audit your deployments.