---
applyTo: '**/*.sql'
---
Cung cấp ngữ cảnh dự án và hướng dẫn viết mã mà AI nên tuân theo khi tạo code, trả lời câu hỏi, hoặc xem xét thay đổi.

# Quy tắc viết DB2 SQL cho dự án

## 1. **Ngôn ngữ và Naming Conventions**

**Giao tiếp:**
- Tất cả phản hồi, tài liệu: **tiếng Việt**
- Từ khóa SQL, tên bảng, cột: **tiếng Anh**

**Quy tắc đặt tên (UPPER_SNAKE_CASE):**
```sql
-- Tables: SCHEMA.TABLE_NAME
RM_RDM_SHR.PD_USERS, RM_RDM_SHR.MODEL_RESULTS

-- Columns: COLUMN_NAME  
USER_ID, MODEL_SCORE, CREATED_TS

-- Objects với prefixes
SP_GET_USER_DATA        -- Stored procedures
FN_CALC_RISK           -- Functions  
VW_USER_SUMMARY        -- Views
TR_AUDIT_CHANGES       -- Triggers
IX_USERS_EMAIL         -- Indexes
```

**Working Default Schema Configuration:**
- **Primary Working Schema**: `RM_RDM_SHR` - Default schema cho các tables và working data
- **Read-Only Schema**: `TPBCSO` - Schema chỉ có quyền SELECT, không có quyền INSERT/UPDATE/DELETE/CREATE
- **Temporary Schema**: `SESSION` - Schema cho temporary tables

```sql
-- Ví dụ sử dụng schemas
-- Working data trong RM_RDM_SHR (có thể không cần prefix)
SELECT * FROM CIC_DB2;  -- Equivalent to RM_RDM_SHR.CIC_DB2

-- Read-only data từ TPBCSO (chỉ SELECT)
SELECT * FROM TPBCSO.PD_USERS;  -- ✅ Allowed
SELECT * FROM TPBCSO.CUSTOMER_DATA WHERE DATE_COL >= '2025-01-01';  -- ✅ Allowed

-- KHÔNG được phép với TPBCSO schema
-- INSERT INTO TPBCSO.PD_USERS (...);  -- ❌ No permission
-- UPDATE TPBCSO.PD_USERS SET ...;     -- ❌ No permission  
-- DELETE FROM TPBCSO.PD_USERS;        -- ❌ No permission
-- CREATE TABLE TPBCSO.NEW_TABLE ...;  -- ❌ No permission

-- Temporary tables trong SESSION
CREATE GLOBAL TEMPORARY TABLE SESSION.TMP_WORK_DATA (...);
```

**File Naming và Extensions:**
- **DB2 SQL Files**: Sử dụng extension `.sql` (không phải `.db2`)
- **Stored Procedures**: `SP_[FUNCTION_NAME].sql`
- **Functions**: `FN_[FUNCTION_NAME].sql`  
- **Views**: `VW_[VIEW_NAME].sql`
- **Table Definitions**: `TBL_[TABLE_NAME].sql`
- **Installation Scripts**: `INSTALL_[COMPONENT].sql`

```sql
-- Ví dụ tên files
SP_CALCULATE_PD_SCORE.sql
FN_GET_MONTH_DIFF.sql
VW_CUSTOMER_SUMMARY.sql
TBL_EWS_CORP_RESULTS.sql
INSTALL_EWS_CORP_PROCEDURES.sql
```

## 2. **SQL Formatting Standards**

```sql
-- Từ khóa SQL: HOA, thụt lề 4 spaces, alias ngắn gọn
SELECT 
    U.USER_ID,
    U.USER_NAME, 
    M.MODEL_SCORE,
    CASE 
        WHEN M.MODEL_SCORE > 0.8 THEN 'HIGH_RISK'
        WHEN M.MODEL_SCORE > 0.5 THEN 'MEDIUM_RISK'
        ELSE 'LOW_RISK'
    END AS RISK_CATEGORY
FROM TPBCSO.PD_USERS U
INNER JOIN TPBCSO.MODEL_RESULTS M ON U.USER_ID = M.USER_ID
WHERE U.STATUS = 'ACTIVE'
    AND M.CREATED_TS >= CURRENT_DATE - 30 DAYS
ORDER BY M.MODEL_SCORE DESC
FETCH FIRST 100 ROWS ONLY;
```

## 3. **DB2 Specific Features**

```sql
-- DB2 keywords (không dùng LIMIT)
FETCH FIRST n ROWS ONLY

-- CTEs cho complex queries
WITH USER_SCORES AS (
    SELECT USER_ID, AVG(SCORE) AS AVG_SCORE
    FROM TPBCSO.SCORES 
    GROUP BY USER_ID
)
SELECT * FROM USER_SCORES WHERE AVG_SCORE > 0.7;

-- DB2 Variables (sử dụng DECLARE thay vì CREATE)
DECLARE V_RPT_DT DATE DEFAULT '2025-04-30';
DECLARE V_COUNT INTEGER DEFAULT 0;
DECLARE V_SQL VARCHAR(4000);

-- DB2 functions
DAYS_BETWEEN(DATE1, DATE2)
TIMESTAMP(DATE_COLUMN)
LOCATE(SUBSTRING, STRING)
SUBSTR(STRING, START, LENGTH)

-- Upsert với MERGE
MERGE INTO TARGET_TABLE T
USING SOURCE_TABLE S ON T.ID = S.ID
WHEN MATCHED THEN UPDATE SET T.VALUE = S.VALUE
WHEN NOT MATCHED THEN INSERT (ID, VALUE) VALUES (S.ID, S.VALUE);

-- DB2 Function syntax trong BEGIN ATOMIC blocks
-- ❌ SAI: SELECT ... INTO (sẽ gây lỗi SQLSTATE 42601)
SELECT COUNT(*) INTO V_COUNT FROM TABLE_NAME;

-- ✅ ĐÚNG: SET ... = (SELECT ...)
SET V_COUNT = (SELECT COUNT(*) FROM TABLE_NAME);
SET V_MAX_DATE = (SELECT MAX(DATE_COL) FROM TABLE_NAME);
SET V_RESULT = (SELECT SUM(AMOUNT) FROM TABLE_NAME WHERE STATUS = 'ACTIVE');
```

## 4. **Stored Procedure Template**

**Header Documentation - Đầy đủ thông tin cần thiết:**
```sql
/*
================================================================================
STORED PROCEDURE: SP_FUNCTION_NAME
================================================================================
PURPOSE: Mô tả rõ ràng chức năng chính của procedure
VERSION: 1.0 | AUTHOR: [Tên Developer] | DATE: 2025-08-06

INPUT PARAMETERS:
    P_PARAM1    VARCHAR(128)  - Mô tả parameter 1 (required/optional)
    P_PARAM2    INTEGER       - Mô tả parameter 2 với business logic
    P_PARAM3    DATE         - Mô tả parameter 3 (format, constraints)

OUTPUT:
    - SESSION.RESULT_TABLE: Cấu trúc output table với columns
    - RESULT SET: Mô tả format của result set (nếu có)
    
BUSINESS LOGIC:
    1. Validate input parameters và business rules
    2. Process main logic (tóm tắt các bước chính)
    3. Generate output theo format requirements
    
DEPENDENCIES:
    - TPBCSO.SOURCE_TABLE: Table nguồn data (chỉ đọc)
    - SESSION schema: Quyền tạo temporary tables
    
CHANGES LOG:
    2025-08-06  [Author]  Initial version
    
USAGE EXAMPLE:
    CALL SP_FUNCTION_NAME('TABLE_NAME', 123, '2025-08-06');
    SELECT * FROM SESSION.RESULT_TABLE;
================================================================================
*/
```

**Header - Ngắn gọn cho simple procedures:**
```sql
/*
SP_SIMPLE_FUNCTION - Mô tả ngắn gọn chức năng
Version: 1.0 | Author: [Tên] | Date: 2025-08-06
Input: P_PARAM1 TYPE, P_PARAM2 TYPE
Output: SESSION.RESULT_TABLE
*/
```

**Structure Template:**
```sql
CREATE OR REPLACE PROCEDURE SP_FUNCTION_NAME(
    IN P_PARAM1 VARCHAR(128),
    IN P_PARAM2 INTEGER
)
LANGUAGE SQL
BEGIN
    -- Variables
    DECLARE V_SQL VARCHAR(4000);
    DECLARE V_COUNT INTEGER DEFAULT 0;
    
    -- Error handling
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        RESIGNAL; -- Không dùng ROLLBACK để tránh savepoint issues
    END;
    
    DECLARE CONTINUE HANDLER FOR SQLSTATE '42704'
    BEGIN
        -- Object not found - ignore
    END;
    
    -- Main logic
    -- 1. Input validation
    -- 2. Data processing  
    -- 3. Results output
    
END;
```

## 5. **Transaction và Error Handling**

**Tránh nested savepoints (SQLSTATE 3B002):**
```sql
-- ❌ TRÁNH: BEGIN ATOMIC trong nested calls
CREATE PROCEDURE SP_OUTER()
BEGIN
    BEGIN ATOMIC  -- Tạo savepoint
        CALL SP_INNER(); -- Nếu SP_INNER có BEGIN ATOMIC -> Lỗi!
    END;
END;

-- ✅ ĐÚNG: Simple error handling
CREATE PROCEDURE SP_SAFE()  
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        RESIGNAL; -- Re-throw error
    END;
    
    -- Logic không dùng BEGIN ATOMIC
END;
```

**Common Error Codes:**
- **SQLSTATE 3B002**: Nested savepoint → Dùng inline CTE thay vì procedure calls
- **SQLSTATE 42704**: Object not found → CONTINUE HANDLER  
- **SQLSTATE 54038**: Max recursion → Limit < 1000 trong recursive CTEs

## 6. **Recursive CTEs và String Processing**

```sql
-- Template cho string splitting
WITH RECURSIVE_SPLIT_CTE (
    BASE_DATA, DELIMITER, ROW_NUM, EXTRACTED_VALUE, REMAINING_STRING
) AS (
    -- Anchor member
    SELECT 
        BASE_DATA, 
        DELIMITER, 
        1,
        CASE WHEN LOCATE(DELIMITER, BASE_DATA) = 0 
             THEN TRIM(BASE_DATA)
             ELSE TRIM(SUBSTR(BASE_DATA, 1, LOCATE(DELIMITER, BASE_DATA) - 1)) 
        END,
        CASE WHEN LOCATE(DELIMITER, BASE_DATA) = 0 
             THEN '' 
             ELSE SUBSTR(BASE_DATA, LOCATE(DELIMITER, BASE_DATA) + 1) 
        END
    FROM SOURCE_TABLE
    WHERE BASE_DATA IS NOT NULL AND LENGTH(TRIM(BASE_DATA)) > 0
    
    UNION ALL
    
    -- Recursive member
    SELECT 
        BASE_DATA, 
        DELIMITER, 
        ROW_NUM + 1,
        CASE WHEN LOCATE(DELIMITER, REMAINING_STRING) = 0 
             THEN TRIM(REMAINING_STRING)
             ELSE TRIM(SUBSTR(REMAINING_STRING, 1, LOCATE(DELIMITER, REMAINING_STRING) - 1)) 
        END,
        CASE WHEN LOCATE(DELIMITER, REMAINING_STRING) = 0 
             THEN ''
             ELSE SUBSTR(REMAINING_STRING, LOCATE(DELIMITER, REMAINING_STRING) + 1) 
        END
    FROM RECURSIVE_SPLIT_CTE
    WHERE LENGTH(TRIM(REMAINING_STRING)) > 0 
      AND ROW_NUM < 1000 -- Prevent infinite loop
)
SELECT * FROM RECURSIVE_SPLIT_CTE 
WHERE LENGTH(TRIM(EXTRACTED_VALUE)) > 0;
```

## 7. **Temporary Tables**

```sql
-- DB2 Global Temporary Tables (sử dụng DECLARE thay vì CREATE)
-- Standard pattern với error handling
DECLARE CONTINUE HANDLER FOR SQLSTATE '42704' 
BEGIN 
    -- Table không tồn tại - ignore
END;

EXECUTE IMMEDIATE 'DROP TABLE SESSION.TMP_WORK_TABLE';

-- ✅ ĐÚNG: DECLARE GLOBAL TEMPORARY TABLE
DECLARE GLOBAL TEMPORARY TABLE SESSION.TMP_WORK_TABLE (
    ID INTEGER,
    DATA VARCHAR(255),
    CREATED_TS TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) WITH DATA ON COMMIT PRESERVE ROWS;

-- ❌ SAI: CREATE GLOBAL TEMPORARY TABLE (sẽ gây lỗi syntax)
-- CREATE GLOBAL TEMPORARY TABLE SESSION.TMP_WORK_TABLE (...);
```

## 8. **Performance Best Practices**

```sql
-- Temporary tables options
WITH DATA                     -- Create with initial data
ON COMMIT PRESERVE ROWS       -- Keep data across commits

-- Batch processing cho large data
DECLARE V_BATCH_SIZE INTEGER DEFAULT 10000;
WHILE V_OFFSET < V_TOTAL_ROWS DO
    -- Process batch
    SET V_OFFSET = V_OFFSET + V_BATCH_SIZE;
    COMMIT;
END WHILE;

-- Statistics maintenance
RUNSTATS ON TABLE LARGE_TABLE WITH DISTRIBUTION;

-- Query optimization
EXPLAIN PLAN FOR SELECT ...;  -- Analyze query plans
```

## 9. **Data Types và Table Definition**

```sql
-- Ví dụ CREATE TABLE (chỉ cho reference, TPBCSO chỉ có quyền SELECT)
CREATE TABLE RM_RDM_SHR.PD_USERS (
    USER_ID INTEGER NOT NULL GENERATED ALWAYS AS IDENTITY,
    USER_NAME VARCHAR(100) NOT NULL,
    EMAIL VARCHAR(255) NOT NULL,
    CREATED_TS TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    UPDATED_TS TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    STATUS CHAR(1) NOT NULL DEFAULT 'A' CHECK (STATUS IN ('A', 'I')),
    PRIMARY KEY (USER_ID),
    UNIQUE (EMAIL)
);
```

## 10. **Code Review Checklist**

**Documentation Review:**
- [ ] **Complex procedures**: Header đầy đủ thông tin (PURPOSE, INPUTS, OUTPUTS, BUSINESS LOGIC, DEPENDENCIES, CHANGES LOG, USAGE)
- [ ] **Simple procedures**: Header ngắn gọn đủ ý (Name, Version, Author, Date, Input, Output)
- [ ] Không có redundant explanations hoặc copy-paste boilerplate
- [ ] Examples code chính xác và up-to-date

**Quality Gates - Reject nếu:**
- 🚩 Header comments > 100 lines (quá dài)
- 🚩 Header comments < 5 lines cho complex procedures (thiếu thông tin)
- 🚩 Stored procedures > 300 lines total
- 🚩 Variable names > 30 characters  
- 🚩 Dynamic SQL strings > 100 chars per line
- 🚩 Multiple nested BEGIN-END blocks
- 🚩 Excessive parameter validation (> 20 lines)

**Must-have checklist:**
- [ ] **Header appropriate**: Complex procedures có đủ 7 sections, simple procedures có essential info
- [ ] **Error handling**: Sử dụng RESIGNAL (không ROLLBACK)
- [ ] **Temporary tables**: Với SESSION.* schema và proper cleanup
- [ ] **Recursive CTEs**: Có limit conditions (< 1000)
- [ ] **Dynamic SQL**: Clean, readable và secure
- [ ] **Variable names**: Ngắn gọn nhưng clear (V_SQL, V_COUNT, V_ERROR)
- [ ] **Business logic**: Documented với inline comments tiếng Việt

## 11. **Security và Backup**

```sql
-- Parameter markers để tránh SQL injection
PREPARE STMT FROM 'SELECT * FROM USERS WHERE ID = ?';
EXECUTE STMT USING @user_id;

-- Row-level security
CREATE ROW PERMISSION ROW_POLICY_NAME ON TABLE_NAME
FOR ROWS WHERE USER_ID = SESSION_USER;
```

## 12. **Commit và Documentation**

**Git commit messages (tiếng Việt):**
```
feat: thêm stored procedure SP_CALC_PD_SCORE
fix: sửa lỗi savepoint trong SP_SPLIT_DELIMITED_COLUMNS  
perf: tối ưu query performance cho large tables
```

**Inline comments:**
```sql
-- Tính toán điểm PD dựa trên model version
-- Xử lý batch để tránh memory issues
-- Cleanup temporary tables sau khi xử lý xong
```

---

## **Anti-patterns - Tránh:**

❌ **Verbose documentation** (70+ lines headers)  
❌ **Over-descriptive variable names** (`V_DYNAMIC_SQL_STATEMENT_FOR_SPLITTING`)  
❌ **CREATE VARIABLE** (dùng DECLARE thay vì CREATE cho variables)  
❌ **CREATE GLOBAL TEMPORARY TABLE** (dùng DECLARE GLOBAL TEMPORARY TABLE)  
❌ **Nested BEGIN ATOMIC blocks**  
❌ **ROLLBACK trong exception handlers**  
❌ **Procedure calls trong transactions** (gây savepoint conflicts)  
❌ **Missing recursion limits** trong CTEs  
❌ **Excessive string concatenation** cho dynamic SQL
❌ **SELECT ... INTO syntax** trong BEGIN ATOMIC blocks (dùng SET ... = (SELECT ...) thay vì)

✅ **Simple, clear, functional code** là mục tiêu hàng đầu!
