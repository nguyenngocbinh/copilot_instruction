# Instructions Files - Hướng dẫn sử dụng

Thư mục này chứa các file instruction định nghĩa các quy tắc và standards cho việc viết code trong dự án. Các file này được sử dụng bởi AI assistants để đảm bảo consistency trong coding style và best practices.

## Cấu trúc Files

### 1. `python_convention.instructions.md`
- **Áp dụng cho**: Tất cả files Python (`**/*.py`)
- **Nội dung**: 
  - Quy tắc đặt tên (snake_case cho variables/functions, PascalCase cho classes)
  - PEP 8 compliance với 88 character line limit
  - Type hints requirements
  - Google-style docstrings
  - Error handling và logging patterns
  - Testing với pytest
  - Data Science specific conventions

### 2. `data_science_structure_convention.instructions.md`
- **Áp dụng cho**: Tất cả files và folders (`**/*`)
- **Nội dung**:
  - Cấu trúc folder chuẩn cho dự án Data Science
  - Quy tắc đặt tên files và folders (snake_case)
  - Organization theo customer segments và model types
  - Environment management và version control
  - Documentation standards cho từng folder
  - Automation với Makefile và scripts

### 3. `mssql_convention.instructions.md`
- **Áp dụng cho**: Tất cả files SQL (`**/*.sql`)
- **Nội dung**:
  - Naming conventions cho tables, columns, stored procedures
  - SQL formatting rules (uppercase keywords, proper indentation)
  - Security best practices (parameterized queries)
  - Performance optimization guidelines
  - Error handling với TRY-CATCH blocks
  - Documentation standards

### 4. `db2_convention.instructions.md`
- **Áp dụng cho**: Tất cả files DB2 (`**/*.db2`)
- **Nội dung**:
  - DB2-specific naming conventions (UPPER_CASE với underscores)
  - DB2 SQL syntax và features (FETCH FIRST, WITH clauses)
  - Performance optimization với DB2 tools
  - Security với LBAC và RCAC
  - Backup và recovery strategies

### 5. `javascript_typescript_convention.instructions.md`
- **Áp dụng cho**: JavaScript/TypeScript files (`**/*.{js,ts,jsx,tsx}`)
- **Nội dung**:
  - Naming conventions (camelCase, PascalCase, kebab-case)
  - TypeScript best practices
  - Async/await patterns
  - Error handling strategies
  - Testing với Jest
  - Performance optimization

### 6. `docker_infrastructure_convention.instructions.md`
- **Áp dụng cho**: Docker và Infrastructure files (`**/Dockerfile*,**/*.{yml,yaml,tf,hcl}`)
- **Nội dung**:
  - Multi-stage Docker builds
  - Security best practices
  - Kubernetes YAML standards
  - Terraform conventions
  - CI/CD integration patterns

## Cách sử dụng

### Syntax của Instruction Files
Mỗi instruction file bắt đầu với YAML frontmatter:

```yaml
---
applyTo: '**/*.py'  # Glob pattern cho files áp dụng
---
```

### Tích hợp với AI Tools
Các AI assistants sẽ:
1. Đọc các instruction files khi làm việc với code
2. Áp dụng rules tương ứng dựa trên file extension
3. Đảm bảo output code tuân theo standards đã định nghĩa

### Ngôn ngữ giao tiếp
- **Tất cả documentation, comments, và giao tiếp**: Tiếng Việt
- **Code elements (variables, functions, classes)**: Tiếng Anh
- **Commit messages**: Tiếng Việt với format `[type]: description`

## Best Practices

### 1. Consistency
- Tuân theo naming conventions cho từng ngôn ngữ
- Sử dụng consistent formatting rules
- Apply proper documentation standards

### 2. Security
- Luôn sử dụng parameterized queries cho SQL
- Implement proper error handling
- Validate input data

### 3. Performance
- Follow language-specific optimization guidelines
- Use appropriate data structures
- Implement proper caching strategies

### 4. Maintainability
- Write clear, documented code
- Use meaningful variable và function names
- Implement comprehensive testing

## Cập nhật Instructions

Khi cần cập nhật quy tắc:
1. Edit file instruction tương ứng
2. Test với sample code để đảm bảo rules work properly
3. Commit với message rõ ràng về thay đổi
4. Notify team về updates

## Tools Integration

### Python
- `black` - Code formatting
- `flake8` - Linting
- `mypy` - Type checking
- `pytest` - Testing
- `isort` - Import sorting

### SQL
- SQL Server Management Studio formatting tools
- DB2 Developer Workbench tools
- Custom linting scripts

### JavaScript/TypeScript
- `prettier` - Code formatting
- `eslint` - Linting
- `jest` - Testing
- `tsc` - TypeScript compilation

## Ví dụ sử dụng

### Python Function
```python
def calculate_pd_score(user_data: Dict[str, Union[int, float]], 
                      model_version: str = "v1.0") -> float:
    """
    Tính toán điểm PD cho khách hàng dựa trên dữ liệu đầu vào.
    
    Args:
        user_data: Dictionary chứa thông tin khách hàng
        model_version: Phiên bản model sử dụng
        
    Returns:
        Điểm PD trong khoảng 0-1
    """
```

### SQL Stored Procedure
```sql
/*
Mô tả: Tính toán điểm PD cho khách hàng
Tác giả: [Tên người tạo]
Ngày tạo: [Ngày tạo]
*/
CREATE PROCEDURE sp_CalculatePdScore
    @CustomerId INT,
    @ModelVersion VARCHAR(10) = 'v1.0'
AS
BEGIN
    -- Implementation logic
END
```

### TypeScript Function
```typescript
/**
 * Tính toán điểm tín dụng cho khách hàng
 * 
 * @param userData - Thông tin khách hàng
 * @param modelVersion - Phiên bản model sử dụng
 * @returns Promise trả về điểm tín dụng
 */
async function calculateCreditScore(
  userData: IUserData, 
  modelVersion: string = 'v1.0'
): Promise<number> {
  // Implementation
}
```
