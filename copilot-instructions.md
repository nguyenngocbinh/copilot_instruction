---
applyTo: '**/*'
---
Instruction chung cho tất cả các dự án - Cung cấp nguyên tắc và quy tắc cơ bản mà AI phải tuân theo trong mọi tình huống làm việc với code và dự án.

# Quy tắc và Nguyên tắc chung cho Tất cả Dự án

## 1. **Ngôn ngữ và Giao tiếp**:
   - **Ngôn ngữ phản hồi**: Tất cả phản hồi, giải thích, tài liệu phải được viết bằng **tiếng Việt**.
   - **Ngôn ngữ code**: Tên variables, functions, classes, comments trong code phải sử dụng **tiếng Anh**.
   - **Tính nhất quán**: Luôn duy trì tính nhất quán trong cách đặt tên và viết code trong cùng một dự án.

## 2. **Nguyên tắc Thiết kế Code**:

### **SOLID Principles**:
   - **S**ingle Responsibility: Mỗi class/function chỉ có một trách nhiệm duy nhất
   - **O**pen/Closed: Mở cho extension, đóng cho modification
   - **L**iskov Substitution: Subclasses phải có thể thay thế base classes
   - **I**nterface Segregation: Tách biệt interfaces thành các phần nhỏ cụ thể
   - **D**ependency Inversion: Phụ thuộc vào abstractions, không phải concrete implementations

### **Clean Code Principles**:
   - **Readability**: Code phải dễ đọc và hiểu
   - **Simplicity**: Giữ code đơn giản, tránh over-engineering
   - **DRY** (Don't Repeat Yourself): Tránh duplicate code
   - **KISS** (Keep It Simple, Stupid): Giữ mọi thứ đơn giản
   - **YAGNI** (You Aren't Gonna Need It): Không implement tính năng chưa cần thiết

## 3. **Cấu trúc và Tổ chức Dự án**:

### **Nguyên tắc Tổ chức**:
- **Separation of Concerns**: Tách biệt rõ ràng code, config, tests, documentation
- **Consistent Structure**: Duy trì cấu trúc folder nhất quán trong dự án
- **Clear Naming**: Tên files/folders phải mô tả rõ chức năng

### **File Naming Principles**:
- **Consistency**: Duy trì một naming style trong cùng dự án (snake_case hoặc kebab-case)
- **Clarity**: Tên file phải mô tả rõ nội dung và chức năng
- **No Special Characters**: Tránh spaces, ký tự đặc biệt, ký tự Unicode

## 4. **Documentation Principles**:

### **Core Documentation Rules**:
- **Vietnamese Responses**: Tất cả phản hồi và tài liệu phải bằng tiếng Việt
- **English Code**: Code comments và technical terms bằng tiếng Anh
- **Clear Purpose**: Mỗi module/function phải có mô tả rõ ràng về purpose
- **Up-to-date**: Documentation phải được cập nhật khi code thay đổi

## 5. **Version Control Principles**:

### **Git Commit Convention**:
```
[type]: mô tả thay đổi bằng tiếng Việt

Types:
- feat: thêm tính năng mới
- fix: sửa bug
- docs: cập nhật documentation
- style: thay đổi formatting
- refactor: refactor code
- test: thêm/sửa tests
- chore: cập nhật build tools, dependencies
```

### **Branch Naming**:
```
feature/ten-tinh-nang-moi
bugfix/sua-loi-xac-dinh
hotfix/sua-loi-khan-cap
release/v1.0.0
```

## 6. **Security Principles**:

### **Bảo mật Cơ bản**:
- **Không commit**: Passwords, API keys, sensitive data
- **Environment variables**: Sử dụng .env files cho config
- **Input validation**: Validate tất cả user inputs
- **Parameterized queries**: Tránh SQL injection
- **HTTPS**: Luôn sử dụng HTTPS trong production

## 7. **Error Handling Principles**:
- **Fail fast**: Phát hiện lỗi sớm nhất có thể
- **Graceful degradation**: Xử lý lỗi một cách nhẹ nhàng
- **User-friendly messages**: Thông báo lỗi dễ hiểu cho user
- **Developer-friendly logs**: Chi tiết cho debugging

## 8. **Testing Principles**:
- **Testing Pyramid**: Unit (70%) > Integration (20%) > E2E (10%)
- **Test Naming**: Descriptive test names theo pattern nhất quán
- **Test Coverage**: Maintain reasonable test coverage

## 9. **Performance Principles**:
- **Measure First**: Profile trước khi optimize
- **Appropriate Algorithms**: Chọn algorithms phù hợp với use case
- **Resource Management**: Quản lý memory và connections efficiently
- **Caching Strategy**: Cache data phù hợp

## 10. **Code Review Standards**:

### **Review Checklist**:
- [ ] Code tuân thủ naming conventions
- [ ] Logic rõ ràng và dễ hiểu
- [ ] Error handling đầy đủ
- [ ] Tests coverage đạt yêu cầu
- [ ] Documentation cập nhật
- [ ] Security considerations được xem xét
- [ ] Performance impact được đánh giá

### **Review Comments**:
- Sử dụng tiếng Việt khi comment
- Constructive feedback
- Explain "why" không chỉ "what"
- Suggest alternatives khi possible

---

## **Instruction Hierarchy và Cross-References**:

### **Cách sử dụng Instructions**:
- **File này**: Foundation principles cho tất cả projects
- **Specific instructions**: Extend và override theo từng technology
- **Prompts**: Practical templates cho complex tasks

### **Technology-Specific Instructions**:
- `instructions/python_convention.instructions.md` - Python coding standards
- `instructions/db2_convention.instructions.md` - DB2 SQL standards  
- `instructions/mssql_convention.instructions.md` - MSSQL standards
- `instructions/docker_infrastructure_convention.instructions.md` - Infrastructure as Code
- `instructions/javascript_typescript_convention.instructions.md` - JS/TS standards
- `instructions/data_science_structure_convention.instructions.md` - Data science project structure
- `instructions/python_project_skeleton.instructions.md` - Python project templates

### **Prompt Templates**:
- `prompts/mssql.prompt.md` - SQL refactoring prompt templates
- `prompts/mssql_incremental_refactor.prompt.md` - Incremental refactor workflows

---

## **Lưu ý quan trọng**:
- Các quy tắc này là **foundation** cho tất cả dự án
- Specific instructions sẽ **extend** và **override** các quy tắc này khi cần
- **Consistency** là key - luôn áp dụng nhất quán trong cùng một dự án  
- **Team agreement** quan trọng hơn perfect adherence to rules
