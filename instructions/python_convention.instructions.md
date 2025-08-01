---
applyTo: '**/*.py'
---
Cung cấp ngữ cảnh dự án và hướng dẫn viết mã mà AI nên tuân theo khi tạo code, trả lời câu hỏi, hoặc xem xét thay đổi.

# Quy tắc viết mã Python cho dự án

1. **Ngôn ngữ giao tiếp**:
   - Tất cả các phản hồi, tài liệu, và hướng dẫn phải được viết bằng tiếng Việt.
   - Tên biến, hàm, class, và các thành phần mã nguồn khác phải sử dụng tiếng Anh.

2. **Quy tắc đặt tên**:
   - Tên biến: Sử dụng kiểu `snake_case` (ví dụ: `user_name`, `total_amount`, `pd_model_score`).
   - Tên hàm: Sử dụng kiểu `snake_case` (ví dụ: `calculate_total`, `get_user_data`, `train_pd_model`).
   - Tên class: Sử dụng kiểu `PascalCase` (ví dụ: `UserProfile`, `DataProcessor`, `PdModelTrainer`).
   - Tên constants: Sử dụng kiểu `UPPER_CASE` (ví dụ: `MAX_SCORE`, `DEFAULT_THRESHOLD`).
   - Tên module: Sử dụng kiểu `snake_case` (ví dụ: `data_preprocessing`, `model_training`).

3. **Quy tắc mã hóa**:
   - Tuân thủ tiêu chuẩn PEP 8.
   - Sử dụng dấu cách (spaces) thay vì tab, với độ thụt lề là 4 spaces.
   - Mỗi dòng không dài quá 88 ký tự (cập nhật từ Black formatter).
   - Sử dụng type hints cho tất cả function parameters và return values.

4. **Cấu trúc file và import**:
   ```python
   """
   Mô tả module: Module này chứa các hàm xử lý dữ liệu cho model PD
   Tác giả: [Tên tác giả]
   Ngày tạo: [Ngày tạo]
   """
   # Standard library imports
   import os
   import sys
   from typing import List, Dict, Optional, Union
   
   # Third-party imports
   import pandas as pd
   import numpy as np
   
   # Local imports
   from src.utils import data_helper
   from src.models import base_model
   ```

5. **Tài liệu và chú thích**:
   - Tất cả các chú thích và tài liệu phải được viết bằng tiếng Việt.
   - Sử dụng docstring theo format Google style:
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
           
       Raises:
           ValueError: Khi dữ liệu đầu vào không hợp lệ
           
       Example:
           >>> user_info = {"income": 50000, "age": 35}
           >>> score = calculate_pd_score(user_info)
           >>> print(f"Điểm PD: {score}")
       """
   ```

6. **Error handling và logging**:
   ```python
   import logging
   
   # Cấu hình logging
   logging.basicConfig(
       level=logging.INFO,
       format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
   )
   logger = logging.getLogger(__name__)
   
   def process_data(data: pd.DataFrame) -> pd.DataFrame:
       """Xử lý dữ liệu với error handling."""
       try:
           # Logic xử lý
           logger.info("Bắt đầu xử lý dữ liệu")
           result = data.copy()
           # ... xử lý ...
           logger.info("Hoàn thành xử lý dữ liệu")
           return result
       except Exception as e:
           logger.error(f"Lỗi khi xử lý dữ liệu: {str(e)}")
           raise
   ```

7. **Testing conventions**:
   - Sử dụng `pytest` cho unit tests.
   - Tên test files: `test_[module_name].py`
   - Tên test functions: `test_[function_name]_[scenario]`
   ```python
   def test_calculate_pd_score_valid_input():
       """Test tính toán PD score với input hợp lệ."""
       user_data = {"income": 50000, "age": 35}
       score = calculate_pd_score(user_data)
       assert 0 <= score <= 1
   ```

8. **Data Science specific conventions**:
   - Sử dụng `pandas` cho data manipulation.
   - Sử dụng `numpy` cho numerical operations.
   - Sử dụng `scikit-learn` cho machine learning models.
   - Organize notebooks với clear sections và markdown explanations.

9. **Công cụ hỗ trợ**:
   - Sử dụng `flake8` để kiểm tra mã nguồn.
   - Sử dụng `black` để định dạng mã nguồn.
   - Sử dụng `mypy` để kiểm tra type hints.
   - Sử dụng `isort` để sắp xếp imports.
   - Sử dụng `pytest` để chạy tests.

10. **Performance và best practices**:
    - Sử dụng list comprehensions thay vì loops khi có thể.
    - Sử dụng generators cho large datasets.
    - Cache expensive computations khi cần thiết.
    - Sử dụng context managers cho resource management.

11. **Quy trình commit**:
    - Tin nhắn commit phải được viết bằng tiếng Việt.
    - Format: `[loại]: mô tả ngắn gọn`
    - Ví dụ: `feat: thêm hàm tính toán PD score mới`
    - Loại commit: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`