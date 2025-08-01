---
applyTo: '**/*.db2'
---
Cung cấp ngữ cảnh dự án và hướng dẫn viết mã mà AI nên tuân theo khi tạo code, trả lời câu hỏi, hoặc xem xét thay đổi.

# Quy tắc viết DB2 SQL cho dự án

1. **Ngôn ngữ giao tiếp**:
   - Tất cả các phản hồi, tài liệu, và hướng dẫn phải được viết bằng tiếng Việt.
   - Từ khóa SQL, tên bảng, cột, và các thành phần mã nguồn khác phải sử dụng tiếng Anh.

2. **Quy tắc đặt tên (DB2 Conventions)**:
   - Tên bảng: Sử dụng kiểu `UPPER_CASE` với tiền tố schema (ví dụ: `PD_USERS`, `PD_MODEL_RESULTS`).
   - Tên cột: Sử dụng kiểu `UPPER_CASE` với separator `_` (ví dụ: `USER_ID`, `MODEL_SCORE`, `CREATED_TS`).
   - Tên stored procedure: Sử dụng tiền tố `SP_` (ví dụ: `SP_GET_USER_DATA`, `SP_CALC_PD_SCORE`).
   - Tên function: Sử dụng tiền tố `FN_` (ví dụ: `FN_GET_CURRENT_DATE`, `FN_CALC_RISK`).
   - Tên view: Sử dụng tiền tố `VW_` (ví dụ: `VW_USER_SUMMARY`, `VW_MODEL_RESULTS`).
   - Tên trigger: Sử dụng tiền tố `TR_` (ví dụ: `TR_AUDIT_USER_CHANGES`).

3. **Quy tắc định dạng mã**:
   - Từ khóa SQL phải viết HOA (SELECT, FROM, WHERE, JOIN, etc.).
   - Sử dụng thụt lề 4 spaces cho các khối lệnh con.
   - Sử dụng dấu `;` kết thúc mỗi statement.
   - Sử dụng alias rõ ràng và ngắn gọn cho bảng.

4. **Cấu trúc query DB2**:
   ```sql
   SELECT 
       U.USER_ID,
       U.USER_NAME,
       M.MODEL_SCORE,
       CASE 
           WHEN M.MODEL_SCORE > 0.8 THEN 'HIGH_RISK'
           WHEN M.MODEL_SCORE > 0.5 THEN 'MEDIUM_RISK'
           ELSE 'LOW_RISK'
       END AS RISK_CATEGORY
   FROM PD_USERS U
   INNER JOIN PD_MODEL_RESULTS M 
       ON U.USER_ID = M.USER_ID
   WHERE U.STATUS = 'ACTIVE'
       AND M.MODEL_VERSION = 'V2.1'
       AND M.CREATED_TS >= CURRENT_DATE - 30 DAYS
   ORDER BY M.MODEL_SCORE DESC
   FETCH FIRST 100 ROWS ONLY;
   ```

5. **DB2 Specific Features**:
   - Sử dụng `FETCH FIRST n ROWS ONLY` thay vì LIMIT.
   - Sử dụng `WITH` clauses (Common Table Expressions) cho complex queries.
   - Leverage DB2's built-in functions như `DAYS_BETWEEN`, `TIMESTAMP`, etc.
   - Sử dụng `MERGE` statement cho upsert operations.

6. **Quy tắc bảo mật**:
   - Sử dụng parameter markers (?) trong prepared statements.
   - Implement proper authorization và authentication.
   - Sử dụng DB2's Label-Based Access Control (LBAC) khi cần.
   - Row-level security với Row and Column Access Control (RCAC).

7. **Performance optimization**:
   - Sử dụng `EXPLAIN` để analyze query plans.
   - Tận dụng DB2's built-in optimization features.
   - Sử dụng appropriate indexing strategies.
   - Consider partitioning cho large tables.
   - Sử dụng `RUNSTATS` để maintain table statistics.

8. **Data types và constraints**:
   ```sql
   -- Ví dụ table definition
   CREATE TABLE PD_USERS (
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

9. **Tài liệu và chú thích**:
   - Tất cả các chú thích phải được viết bằng tiếng Việt.
   - Sử dụng `--` cho single-line comments và `/* */` cho multi-line.
   - Header comment cho stored procedures:
     ```sql
     /*
     Mô tả: Stored procedure tính toán điểm PD cho khách hàng
     Tác giả: [Tên người tạo]
     Ngày tạo: [Ngày tạo]
     Tham số:
         IN p_user_id INTEGER - ID của khách hàng
         IN p_model_version VARCHAR(10) - Phiên bản model
         OUT p_pd_score DECIMAL(5,4) - Điểm PD kết quả
     Mô tả logic:
         - Lấy dữ liệu khách hàng từ các bảng liên quan
         - Áp dụng model tương ứng với version
         - Trả về điểm PD đã tính toán
     */
     ```

10. **Error handling**:
    ```sql
    -- Sử dụng DB2's exception handling
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        GET DIAGNOSTICS EXCEPTION 1 
            @error_state = RETURNED_SQLSTATE,
            @error_msg = MESSAGE_TEXT;
        -- Log error và handle appropriately
    END;
    ```

11. **Backup và Recovery**:
    - Sử dụng DB2's backup utilities.
    - Implement point-in-time recovery strategies.
    - Regular maintenance với `REORG` và `RUNSTATS`.

12. **Quy trình commit**:
    - Tin nhắn commit phải được viết bằng tiếng Việt.
    - Mô tả rõ ràng thay đổi về database schema hoặc stored procedures.
    - Include performance impact assessment khi có thay đổi lớn.
