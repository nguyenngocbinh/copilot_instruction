---
applyTo: '**/*.sql'
---
Cung cấp ngữ cảnh dự án và hướng dẫn viết mã mà AI nên tuân theo khi tạo code, trả lời câu hỏi, hoặc xem xét thay đổi.

# Quy tắc viết SQL Server (MSSQL) cho dự án

1. **Ngôn ngữ giao tiếp**:
   - Tất cả các phản hồi, tài liệu, và hướng dẫn phải được viết bằng tiếng Việt.
   - Từ khóa SQL, tên bảng, cột, và các thành phần mã nguồn khác phải sử dụng tiếng Anh.

2. **Quy tắc đặt tên**:
   - Tên bảng: Sử dụng kiểu `UPPER_SNAKE_CASE` với tiền tố rõ ràng (ví dụ: `TBL_USERS`, `TBL_PD_MODEL`).
   - Tên cột: Sử dụng kiểu `UPPER_SNAKE_CASE` (ví dụ: `USER_ID`, `MODEL_SCORE`, `CREATED_DATE`).
   - Tên stored procedure: Sử dụng tiền tố `SP_` (ví dụ: `SP_GET_USER_DATA`).
   - Tên function: Sử dụng tiền tố `FN_` (ví dụ: `FN_GET_CURRENT_DATE`).
   - Tên view: Sử dụng tiền tố `VW_` (ví dụ: `VW_USER_SUMMARY`, `VW_MODEL_RESULTS`).
   - Tên trigger: Sử dụng tiền tố `TR_` (ví dụ: `TR_AUDIT_USER_CHANGES`).

   **Lý do chọn UPPER_SNAKE_CASE:**
   - Truyền thống trong enterprise database systems (DB2, Oracle)
   - Tách biệt rõ ràng với code variables (lowercase)
   - Dễ nhận diện database objects trong mixed environment
   - Consistent với SQL keywords (SELECT, FROM, WHERE)
   - Phổ biến trong banking và financial systems

3. **Quy tắc định dạng mã**:
   - Từ khóa SQL phải viết HOA (SELECT, FROM, WHERE, JOIN, etc.).
   - Sử dụng thụt lề 4 spaces cho các khối lệnh con.
   - Mỗi điều kiện WHERE trên một dòng riêng khi có nhiều điều kiện.
   - Sử dụng alias rõ ràng cho bảng và cột.

4. **Cấu trúc query**:
   ```sql
   SELECT 
       T1.COLUMN_NAME,
       T2.ANOTHER_COLUMN,
       CASE 
           WHEN CONDITION THEN VALUE
           ELSE OTHER_VALUE
       END AS COMPUTED_COLUMN
   FROM TBL_TABLE_NAME T1
   INNER JOIN TBL_ANOTHER_TABLE T2 
       ON T1.ID = T2.FOREIGN_ID
   WHERE T1.STATUS = 'ACTIVE'
       AND T1.CREATED_DATE >= '2024-01-01'
   ORDER BY T1.CREATED_DATE DESC;
   ```

5. **Quy tắc bảo mật**:
   - Luôn sử dụng parameterized queries để tránh SQL injection.
   - Không hard-code sensitive data trong script.
   - Sử dụng schema permissions thích hợp.
   - Luôn validate input parameters trong stored procedures.

6. **Quy tắc performance**:
   - Sử dụng index hints khi cần thiết.
   - Tránh SELECT * khi không cần tất cả columns.
   - Sử dụng EXISTS thay vì IN khi có thể.
   - Limit kết quả với TOP hoặc paging khi cần.

7. **Tài liệu và chú thích**:
   - Tất cả các chú thích phải được viết bằng tiếng Việt.
   - Mỗi stored procedure/function phải có header comment mô tả:
     ```sql
     /*
     Mô tả: Tính toán điểm PD cho khách hàng dựa trên dữ liệu đầu vào
     Tác giả: [Tên người tạo]
     Ngày tạo: [Ngày tạo]
     Tham số đầu vào:
         @CUSTOMER_ID INT - ID của khách hàng
         @MODEL_VERSION VARCHAR(10) - Phiên bản model
     Kết quả trả về:
         Bảng chứa điểm PD và các thông tin liên quan
     */
     ```

8. **Quy tắc error handling**:
   - Sử dụng TRY-CATCH blocks cho stored procedures.
   - Log errors với thông tin chi tiết.
   - Return appropriate error codes và messages.

9. **Versioning và deployment**:
   - Mỗi script thay đổi cấu trúc DB phải có rollback script.
   - Sử dụng migration scripts cho database changes.
   - Tag version trong database objects khi cần.

10. **Best Practices cho UPPER_SNAKE_CASE**:
    - Sử dụng underscores để tách các từ có ý nghĩa
    - Tránh sử dụng abbreviations không rõ ràng
    - Giữ tên table/column ngắn gọn nhưng mô tả đầy đủ
    - Sử dụng consistent prefixes cho các loại objects
    - Examples:
      ```sql
      -- Good naming
      TBL_CUSTOMER_PROFILES
      TBL_LOAN_APPLICATIONS  
      VW_MONTHLY_REPORT_SUMMARY
      SP_CALCULATE_PD_SCORE
      
      -- Avoid
      TBL_CUSTPROF (unclear abbreviation)
      CUSTOMER_DATA (missing prefix)
      tbl_customers (inconsistent case)
      ```

11. **Quy trình commit**:
    - Tin nhắn commit phải được viết bằng tiếng Việt.
    - Mô tả rõ ràng thay đổi đã thực hiện về database schema hoặc logic.
