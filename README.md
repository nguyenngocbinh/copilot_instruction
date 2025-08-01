# Instructions Files - Hướng dẫn sử dụng

Thư mục này chứa các file instruction định nghĩa các quy tắc và standards cho việc viết code trong dự án. Các file này được sử dụng bởi AI assistants để đảm bảo consistency trong coding style và best practices.

## 🔗 Quick Navigation

| Convention | Language/Platform | Naming Style | Link |
|------------|-------------------|--------------|------|
| Python | Python | [snake_case](https://en.wikipedia.org/wiki/Snake_case)/[PascalCase](https://en.wikipedia.org/wiki/Camel_case#Pascal_case) | [`python_convention.instructions.md`](./python_convention.instructions.md) |
| Data Science Structure | All files/folders | [snake_case](https://en.wikipedia.org/wiki/Snake_case) | [`data_science_structure_convention.instructions.md`](./data_science_structure_convention.instructions.md) |
| MSSQL | SQL Server | [UPPER_SNAKE_CASE](https://en.wikipedia.org/wiki/Snake_case#Variations) | [`mssql_convention.instructions.md`](./mssql_convention.instructions.md) |
| DB2 | IBM DB2 | [UPPER_SNAKE_CASE](https://en.wikipedia.org/wiki/Snake_case#Variations) | [`db2_convention.instructions.md`](./db2_convention.instructions.md) |
| JavaScript/TypeScript | JS/TS | [camelCase](https://en.wikipedia.org/wiki/Camel_case)/[PascalCase](https://en.wikipedia.org/wiki/Camel_case#Pascal_case) | [`javascript_typescript_convention.instructions.md`](./javascript_typescript_convention.instructions.md) |
| Docker/Infrastructure | Docker/K8s/Terraform | [kebab-case](https://en.wikipedia.org/wiki/Letter_case#Kebab_case)/[snake_case](https://en.wikipedia.org/wiki/Snake_case) | [`docker_infrastructure_convention.instructions.md`](./docker_infrastructure_convention.instructions.md) |

## Cấu trúc Files

### 1. [`python_convention.instructions.md`](./python_convention.instructions.md)
- **Áp dụng cho**: Tất cả files Python (`**/*.py`)
- **Nội dung**: 
  - Quy tắc đặt tên ([snake_case](https://en.wikipedia.org/wiki/Snake_case) cho variables/functions, [PascalCase](https://en.wikipedia.org/wiki/Camel_case#Pascal_case) cho classes)
  - [PEP 8](https://peps.python.org/pep-0008/) compliance với 88 character line limit
  - Type hints requirements
  - [Google-style docstrings](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings)
  - Error handling và logging patterns
  - Testing với pytest
  - Data Science specific conventions

### 2. [`data_science_structure_convention.instructions.md`](./data_science_structure_convention.instructions.md)
- **Áp dụng cho**: Tất cả files và folders (`**/*`)
- **Nội dung**:
  - Cấu trúc folder chuẩn cho dự án Data Science
  - Quy tắc đặt tên files và folders ([snake_case](https://en.wikipedia.org/wiki/Snake_case))
  - Organization theo customer segments và model types
  - Environment management và version control
  - Documentation standards cho từng folder
  - Automation với Makefile và scripts

### 3. [`mssql_convention.instructions.md`](./mssql_convention.instructions.md)
- **Áp dụng cho**: Tất cả files SQL (`**/*.sql`)
- **Nội dung**:
  - Naming conventions cho tables, columns, stored procedures ([UPPER_SNAKE_CASE](https://en.wikipedia.org/wiki/Snake_case#Variations))
  - SQL formatting rules (uppercase keywords, proper indentation)
  - Security best practices (parameterized queries)
  - Performance optimization guidelines
  - Error handling với TRY-CATCH blocks
  - Documentation standards
  - **Convention**: [UPPER_SNAKE_CASE](https://en.wikipedia.org/wiki/Snake_case#Variations) (tương tự Oracle/DB2)
- **See also**: [`db2_convention.instructions.md`](./db2_convention.instructions.md) for similar naming style

### 4. [`db2_convention.instructions.md`](./db2_convention.instructions.md)
- **Áp dụng cho**: Tất cả files DB2 (`**/*.db2`)
- **Nội dung**:
  - DB2-specific naming conventions ([UPPER_SNAKE_CASE](https://en.wikipedia.org/wiki/Snake_case#Variations) với underscores)
  - DB2 SQL syntax và features (FETCH FIRST, WITH clauses)
  - Performance optimization với DB2 tools
  - Security với LBAC và RCAC
  - Backup và recovery strategies
  - **Convention**: [UPPER_SNAKE_CASE](https://en.wikipedia.org/wiki/Snake_case#Variations) (giống MSSQL convention)
- **See also**: [`mssql_convention.instructions.md`](./mssql_convention.instructions.md) for similar naming style

### 5. [`javascript_typescript_convention.instructions.md`](./javascript_typescript_convention.instructions.md)
- **Áp dụng cho**: JavaScript/TypeScript files (`**/*.{js,ts,jsx,tsx}`)
- **Nội dung**:
  - Naming conventions ([camelCase](https://en.wikipedia.org/wiki/Camel_case), [PascalCase](https://en.wikipedia.org/wiki/Camel_case#Pascal_case), [kebab-case](https://en.wikipedia.org/wiki/Letter_case#Kebab_case))
  - TypeScript best practices
  - Async/await patterns
  - Error handling strategies
  - Testing với Jest
  - Performance optimization

### 6. [`docker_infrastructure_convention.instructions.md`](./docker_infrastructure_convention.instructions.md)
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

### SQL Stored Procedure (UPPER_SNAKE_CASE)
```sql
/*
Mô tả: Tính toán điểm PD cho khách hàng
Tác giả: [Tên người tạo]
Ngày tạo: [Ngày tạo]
Tham số đầu vào:
    @CUSTOMER_ID INT - ID của khách hàng
    @MODEL_VERSION VARCHAR(10) - Phiên bản model
Kết quả trả về:
    Bảng chứa điểm PD và các thông tin liên quan
*/
CREATE PROCEDURE SP_CALCULATE_PD_SCORE
    @CUSTOMER_ID INT,
    @MODEL_VERSION VARCHAR(10) = 'V1.0'
AS
BEGIN
    -- Implementation logic
    SELECT 
        CUSTOMER_ID,
        PD_SCORE,
        RATING
    FROM TBL_PD_RESULTS
    WHERE CUSTOMER_ID = @CUSTOMER_ID;
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

## Cross-References

Các convention này bổ sung cho nhau:

- **Database Layer**: [`mssql_convention.instructions.md`](./mssql_convention.instructions.md) ↔️ [`db2_convention.instructions.md`](./db2_convention.instructions.md) (cùng UPPER_SNAKE_CASE)
- **Application Layer**: [`python_convention.instructions.md`](./python_convention.instructions.md) ↔️ [`javascript_typescript_convention.instructions.md`](./javascript_typescript_convention.instructions.md)
- **Project Structure**: [`data_science_structure_convention.instructions.md`](./data_science_structure_convention.instructions.md) ↔️ [`python_convention.instructions.md`](./python_convention.instructions.md)
- **Deployment**: [`docker_infrastructure_convention.instructions.md`](./docker_infrastructure_convention.instructions.md) với tất cả application conventions

---

## References & Standards

### Naming Conventions
- **[snake_case](https://en.wikipedia.org/wiki/Snake_case)**: lowercase_with_underscores (Python variables, functions, files)
- **[UPPER_SNAKE_CASE](https://en.wikipedia.org/wiki/Snake_case#Variations)**: UPPERCASE_WITH_UNDERSCORES (SQL tables, columns, constants)
- **[camelCase](https://en.wikipedia.org/wiki/Camel_case)**: firstWordLowercaseThenCapitalize (JavaScript variables, functions)
- **[PascalCase](https://en.wikipedia.org/wiki/Camel_case#Pascal_case)**: FirstWordAndOthersCapitalized (Classes, Components)
- **[kebab-case](https://en.wikipedia.org/wiki/Letter_case#Kebab_case)**: lowercase-with-hyphens (Docker containers, CSS classes)

### Python Standards
- **[PEP 8](https://peps.python.org/pep-0008/)**: Style Guide for Python Code
- **[Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)**: Google's Python coding standards
- **[Google-style docstrings](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings)**: Documentation format

### JavaScript/TypeScript Standards
- **[TypeScript Handbook](https://www.typescriptlang.org/docs/)**: Official TypeScript documentation
- **[Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)**: Popular JS/TS style guide
- **[ESLint](https://eslint.org/)**: JavaScript linting utility

### Database Standards
- **[SQL Style Guide](https://www.sqlstyle.guide/)**: General SQL formatting guidelines
- **[Microsoft T-SQL Guidelines](https://docs.microsoft.com/en-us/sql/t-sql/)**: SQL Server specific documentation
- **[IBM DB2 Documentation](https://www.ibm.com/docs/en/db2/)**: DB2 specific guidelines

### DevOps & Infrastructure
- **[Docker Best Practices](https://docs.docker.com/develop/best-practices/)**: Official Docker guidelines
- **[Kubernetes Documentation](https://kubernetes.io/docs/)**: K8s standards and practices
- **[Terraform Style Guide](https://www.terraform.io/docs/language/style.html)**: HashiCorp's Terraform conventions

**Note**: Tất cả instruction files được thiết kế để work together tạo thành một comprehensive coding standard cho PD Models project.
