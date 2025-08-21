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

12. **Quy tắc Refactoring và Backward Compatibility**:

    ### **12.1 Backup và Legacy Management**:
    ```python
    # LUÔN backup version cũ trước khi refactor
    # Tạo legacy folder và di chuyển files cũ
    legacy/
    ├── main_legacy.py          # Version cũ của main.py
    ├── etl_legacy.py           # Version cũ của ETL class
    ├── db_legacy.py            # Version cũ của database handlers
    └── README.md               # Ghi chú về legacy files
    ```

    ### **12.2 Legacy File Header Template**:
    ```python
    """
    LEGACY VERSION - DEPRECATED
    ============================

    ⚠️  WARNING: This is the LEGACY version of [filename] before refactoring.
        Do NOT use this file for new development.
        Use [new_filename] instead.

    This file is kept for:
    - Reference and comparison
    - Emergency rollback only
    - Understanding old business logic

    For current development, use:
    - [new_file_path] (new main script)
    - [refactored_modules] (refactored logic)

    Last Updated: [YYYY-MM-DD]
    Status: DEPRECATED - DO NOT MODIFY
    """
    ```

    ### **12.3 Backward Compatibility (Zero Breaking Changes)**:
    ```python
    # WRONG: Breaking API changes
    class DataExtractor:
        def extract_smart_data(self, config, date):  # ❌ New method name
            pass

    # CORRECT: Enhanced internal, same API
    class DataExtractor:
        def extract_data_for_date(self, config, date):  # ✅ Same method name
            """
            Extract data từ source table cho một date cụ thể.
            ENHANCED: Smart resource management bên trong - ZERO API CHANGES
            """
            # Smart enhancements internally
            smart_chunksize = self._get_smart_chunk_size(config, chunksize)
            # ... existing logic with internal improvements
    ```

    ### **12.4 API Enhancement Rules**:
    ```python
    # Rule 1: Existing methods MUST keep same signature
    def extract_data_for_date(self, table_config, date, chunksize=100000, max_workers=8):
        """
        ENHANCED: Smart resource management - KHÔNG thay đổi API
        """
        # Internal enhancements only
        pass

    # Rule 2: Add NEW optional methods, don't modify existing
    def get_performance_stats(self):  # ✅ NEW optional method
        """NEW: Get performance statistics - OPTIONAL method"""
        return self._performance_stats.copy()

    # Rule 3: Add internal methods with underscore prefix  
    def _get_smart_chunk_size(self, config, default):  # ✅ Internal method
        """Internal smart calculation"""
        pass
    ```

    ### **12.5 Deprecation Warnings**:
    ```python
    import warnings
    from functools import wraps

    def deprecated(reason):
        """Decorator để mark functions as deprecated"""
        def decorator(func):
            @wraps(func)
            def wrapper(*args, **kwargs):
                warnings.warn(
                    f"Function {func.__name__} is deprecated. {reason}",
                    DeprecationWarning,
                    stacklevel=2
                )
                return func(*args, **kwargs)
            return wrapper
        return decorator

    # Sử dụng:
    @deprecated("Use new_function() instead. Will be removed in v2.0")
    def old_function():
        """Legacy function - DEPRECATED"""
        pass
    ```

    ### **12.6 Wrapper Classes cho Compatibility**:
    ```python
    # New refactored class
    class ETLOrchestrator:
        def run_etl(self, config, min_date=None):
            # New implementation
            pass

    # Backward compatibility wrapper
    class ETL(ETLOrchestrator):
        """
        Backward compatibility class
        Inherits từ ETLOrchestrator để maintain existing code
        """
        def __init__(self, src_cnxn, tgt_cnxn, etl_type='append'):
            # Map old parameters to new structure
            super().__init__(src_cnxn, tgt_cnxn, etl_type)
    ```

    ### **12.7 Factory Functions cho Transition**:
    ```python
    # Backward compatibility functions
    def create_db2_engine(db2_config=None):
        """
        Backward compatibility function
        Wrapper around new DB2Connector class
        """
        connector = DB2Connector(db2_config)
        return connector.connect()

    # New recommended way
    def create_etl_orchestrator(src_connection, tgt_connection, etl_type='append'):
        """
        NEW: Factory function để create ETL orchestrator
        """
        return ETLOrchestrator(src_connection, tgt_connection, etl_type)
    ```

    ### **12.8 Configuration Compatibility**:
    ```python
    # Support both old and new configuration formats
    def load_table_config(config_input):
        """Support both legacy dict and new YAML config"""
        if isinstance(config_input, str):
            # New YAML path
            return yaml.safe_load(open(config_input))
        elif isinstance(config_input, dict):
            # Legacy dict format
            warnings.warn("Dict config is deprecated, use YAML files")
            return config_input
        else:
            raise ValueError("Unsupported config format")
    ```

    ### **12.9 Import Aliases cho Transition**:
    ```python
    # In __init__.py của refactored modules
    from .new_module import NewClass

    # Backward compatibility imports
    OldClass = NewClass  # Alias cho compatibility
    legacy_function = new_function  # Function alias

    # Deprecation warning khi import
    def __getattr__(name):
        if name == 'OldClass':
            warnings.warn(f"{name} is deprecated, use NewClass", DeprecationWarning)
            return NewClass
        raise AttributeError(f"module has no attribute {name}")
    ```

    ### **12.10 Testing Compatibility**:
    ```python
    # Test cả old và new APIs
    def test_backward_compatibility():
        """Test old API still works"""
        # Test legacy ETL class
        old_etl = ETL(src_conn, tgt_conn, 'append')
        old_etl.run_etl(config, min_date)
        
        # Test new orchestrator  
        new_etl = ETLOrchestrator(src_conn, tgt_conn, 'append')
        new_etl.run_etl(config, min_date)
        
        # Both should produce same results
        assert old_result == new_result
    ```

    ### **12.11 Documentation Standards cho Refactoring**:
    ```python
    def enhanced_method(self, param1, param2=None):
        """
        Method description với ENHANCED capabilities.
        
        ENHANCED FEATURES:
        - Smart resource management
        - Automatic optimization
        - Better error handling
        
        BACKWARD COMPATIBILITY:
        - API signature unchanged
        - Same return type
        - All existing functionality preserved
        
        Args:
            param1: Same as before
            param2: Same as before, now with smart defaults
            
        Returns:
            Same type as before, with enhanced internal processing
            
        Note:
            This method now includes smart optimizations but maintains
            100% backward compatibility with existing code.
        """
    ```

    ### **12.12 Refactoring Process Checklist**:
    ```python
    # Pre-refactoring checklist:
    # ✅ 1. Backup original files to legacy/ folder
    # ✅ 2. Create comprehensive tests for existing functionality  
    # ✅ 3. Document all existing APIs and behaviors
    # ✅ 4. Plan backward compatibility strategy

    # During refactoring:
    # ✅ 5. Keep existing API signatures unchanged
    # ✅ 6. Add enhancements as internal methods
    # ✅ 7. Use wrapper classes/functions for compatibility
    # ✅ 8. Add deprecation warnings for old patterns

    # Post-refactoring:
    # ✅ 9. Run all existing tests to ensure compatibility
    # ✅ 10. Add new tests for enhanced features
    # ✅ 11. Update documentation with migration path
    # ✅ 12. Plan gradual deprecation timeline
    ```

    ### **12.13 Version Planning**:
    ```python
    # Version strategy:
    # v1.x: Legacy version với backward compatibility
    # v1.x+1: Enhanced version với internal improvements, same API
    # v2.x: Major version với new APIs, deprecated old APIs
    # v3.x: Remove deprecated APIs, clean new architecture

    # File naming during transition:
    # main.py                  -> Current version
    # legacy/main_legacy.py    -> Backup of old version  
    # src/orchestrators/       -> New architecture
    # src/connectors/          -> New modules
    ```