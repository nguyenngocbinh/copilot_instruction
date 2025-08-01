---
applyTo: '**/*.sql'
---
Cung cấp ngữ cảnh dự án và hướng dẫn viết mã mà AI nên tuân theo khi tạo code, trả lời câu hỏi, hoặc xem xét thay đổi.

# Quy tắc viết SQL Server (MSSQL) cho dự án

1. **Ngôn ngữ giao tiếp**:
   - Tất cả các phản hồi, tài liệu, và hướng dẫn phải được viết bằng tiếng Việt.
   - Từ khóa SQL, tên bảng, cột, và các thành phần mã nguồn khác phải sử dụng tiếng Anh.

2. **Quy tắc đặt tên**:
   - Tên bảng: Sử dụng kiểu `PascalCase` với tiền tố rõ ràng (ví dụ: `TblUsers`, `TblPdModel`).
   - Tên cột: Sử dụng kiểu `PascalCase` (ví dụ: `UserId`, `ModelScore`, `CreatedDate`).
   - Tên stored procedure: Sử dụng tiền tố `sp_` (ví dụ: `sp_GetUserData`).
   - Tên function: Sử dụng tiền tố `fn_` (ví dụ: `fn_GetCurrentDate`).
   - Tên view: Sử dụng tiền tố `vw_` (ví dụ: `vw_UserSummary`, `vw_ModelResults`).
   - Tên trigger: Sử dụng tiền tố `tr_` (ví dụ: `tr_AuditUserChanges`).

3. **Quy tắc định dạng mã**:
   - Từ khóa SQL phải viết HOA (SELECT, FROM, WHERE, JOIN, etc.).
   - Sử dụng thụt lề 4 spaces cho các khối lệnh con.
   - Mỗi điều kiện WHERE trên một dòng riêng khi có nhiều điều kiện.
   - Sử dụng alias rõ ràng cho bảng và cột.

4. **Cấu trúc query**:
   ```sql
   SELECT 
       t1.ColumnName,
       t2.AnotherColumn,
       CASE 
           WHEN condition THEN value
           ELSE other_value
       END AS ComputedColumn
   FROM TableName t1
   INNER JOIN AnotherTable t2 
       ON t1.Id = t2.ForeignId
   WHERE t1.Status = 'Active'
       AND t1.CreatedDate >= '2024-01-01'
   ORDER BY t1.CreatedDate DESC;
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
         @CustomerId INT - ID của khách hàng
         @ModelVersion VARCHAR(10) - Phiên bản model
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

10. **Quy trình commit**:
    - Tin nhắn commit phải được viết bằng tiếng Việt.
    - Mô tả rõ ràng thay đổi đã thực hiện về database schema hoặc logic.
