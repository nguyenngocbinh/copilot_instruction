---
applyTo: '**/*'
---
Cung cấp ngữ cảnh dự án và hướng dẫn tổ chức files, folders mà AI nên tuân theo khi tạo cấu trúc dự án, đặt tên files, hoặc tổ chức code.

# Quy tắc đặt tên Files và Cấu trúc Folder cho Dự án Khoa học Dữ liệu

## 1. **Ngôn ngữ và Convention chung**:
   - Tất cả các phản hồi, tài liệu phải được viết bằng tiếng Việt.
   - Tên files, folders, variables phải sử dụng tiếng Anh.
   - Sử dụng `snake_case` cho tên files và folders.
   - Tránh sử dụng spaces, ký tự đặc biệt trong tên files/folders.

## 2. **Cấu trúc Folder chính cho Dự án Data Science**:

```
project_name/
├── README.md                          # Mô tả tổng quan dự án
├── requirements.txt                   # Python dependencies
├── environment.yml                    # Conda environment (nếu có)
├── .gitignore                        # Git ignore patterns
├── .env.example                      # Environment variables template
├── setup.py                         # Package setup (nếu cần)
├── Makefile                          # Automation commands
│
├── data/                             # Dữ liệu (không commit vào git)
│   ├── raw/                         # Dữ liệu thô, chưa xử lý
│   │   ├── external/               # Dữ liệu từ nguồn bên ngoài
│   │   ├── internal/               # Dữ liệu nội bộ
│   │   └── manual/                 # Dữ liệu nhập tay
│   ├── interim/                    # Dữ liệu đang xử lý
│   ├── processed/                  # Dữ liệu đã xử lý, sẵn sàng modeling
│   └── features/                   # Feature engineering results
│
├── notebooks/                        # Jupyter notebooks
│   ├── exploratory/               # EDA notebooks
│   │   ├── 01_initial_exploration.ipynb
│   │   ├── 02_data_quality_check.ipynb
│   │   └── 03_feature_analysis.ipynb
│   ├── modeling/                   # Model development notebooks
│   │   ├── 01_baseline_model.ipynb
│   │   ├── 02_feature_engineering.ipynb
│   │   └── 03_model_tuning.ipynb
│   ├── reporting/                  # Report generation notebooks
│   └── archive/                    # Old/experimental notebooks
│
├── src/                              # Source code chính
│   ├── __init__.py
│   ├── config/                     # Configuration files
│   │   ├── __init__.py
│   │   ├── settings.py
│   │   └── logging_config.py
│   ├── data/                       # Data processing modules
│   │   ├── __init__.py
│   │   ├── data_loader.py
│   │   ├── data_preprocessor.py
│   │   ├── feature_engineer.py
│   │   └── data_validator.py
│   ├── models/                     # ML models
│   │   ├── __init__.py
│   │   ├── base_model.py
│   │   ├── pd_model.py
│   │   ├── model_trainer.py
│   │   └── model_evaluator.py
│   ├── utils/                      # Utility functions
│   │   ├── __init__.py
│   │   ├── helpers.py
│   │   ├── database.py
│   │   └── visualization.py
│   └── api/                        # API endpoints (nếu có)
│       ├── __init__.py
│       ├── app.py
│       └── endpoints/
│
├── tests/                            # Test files
│   ├── __init__.py
│   ├── test_data/                  # Test data processing
│   ├── test_models/                # Test models
│   ├── test_utils/                 # Test utilities
│   └── fixtures/                   # Test fixtures
│
├── models/                           # Trained models (không commit vào git)
│   ├── checkpoints/               # Model checkpoints
│   ├── final/                     # Final production models
│   └── experiments/               # Experimental models
│
├── reports/                          # Generated reports
│   ├── figures/                   # Plots, charts
│   ├── tables/                    # Summary tables
│   └── final/                     # Final reports (PDF, HTML)
│
├── scripts/                          # Automation scripts
│   ├── data_pipeline/             # Data processing scripts
│   ├── training/                  # Model training scripts
│   ├── deployment/                # Deployment scripts
│   └── monitoring/                # Model monitoring scripts
│
├── docker/                           # Docker related files
│   ├── Dockerfile
│   ├── docker-compose.yml
│   └── requirements-docker.txt
│
├── docs/                            # Documentation
│   ├── api/                       # API documentation
│   ├── data/                      # Data documentation
│   ├── models/                    # Model documentation
│   └── deployment/                # Deployment guides
│
├── config/                          # Configuration files
│   ├── model_config.yaml
│   ├── data_config.yaml
│   └── deployment_config.yaml
│
└── .github/                         # GitHub specific files
    ├── workflows/                  # CI/CD workflows
    ├── instructions/               # Coding instructions
    └── templates/                  # Issue/PR templates
```

## 3. **Quy tắc đặt tên Files**:

### **Notebooks:**
```
# Format: [số thứ tự]_[mô tả ngắn gọn].ipynb
01_data_exploration.ipynb
02_feature_engineering.ipynb  
03_model_baseline.ipynb
04_model_tuning.ipynb
05_model_evaluation.ipynb
```

### **Python modules:**
```
# Sử dụng snake_case
data_loader.py
feature_engineer.py
model_trainer.py
pd_score_calculator.py
credit_risk_analyzer.py
```

### **Scripts:**
```
# Format: [action]_[object].py
train_model.py
evaluate_model.py
preprocess_data.py
deploy_model.py
generate_report.py
```

### **Config files:**
```
model_config.yaml
data_config.json
deployment_settings.ini
database_config.env
```

### **Data files:**
```
# Raw data
raw_customer_data_20240101.csv
loan_applications_202401.parquet
external_credit_scores.xlsx

# Processed data  
processed_customer_features.csv
train_dataset_v1.parquet
test_dataset_v1.parquet

# Models
pd_model_xgboost_v2_1.pkl
credit_score_model_final.joblib
feature_scaler_v1.pkl
```

## 4. **Quy tắc đặt tên Folders**:

### **Theo chức năng:**
```
data_processing/
model_development/
feature_engineering/
model_evaluation/
deployment_scripts/
```

### **Theo loại model:**
```
pd_models/          # Probability of Default models
lgd_models/         # Loss Given Default models  
ead_models/         # Exposure at Default models
credit_scoring/     # Credit scoring models
```

### **Theo segment khách hàng:**
```
corporate_large/
corporate_medium/
corporate_small/
retail_mortgage/
retail_auto_loan/
retail_credit_card/
```

## 5. **Metadata và Documentation**:

### **README.md cho mỗi folder chính:**
```markdown
# Data Folder

## Mô tả
Folder này chứa tất cả dữ liệu cho dự án PD Models.

## Cấu trúc
- `raw/`: Dữ liệu thô từ các nguồn
- `interim/`: Dữ liệu đang được xử lý
- `processed/`: Dữ liệu sẵn sàng cho modeling

## Quy tắc
- Không commit dữ liệu vào git
- Sử dụng DVC để track large files
- Đảm bảo data privacy và security
```

### **File manifest cho data:**
```yaml
# data_manifest.yaml
datasets:
  customer_data:
    source: "internal_crm"
    format: "csv"
    size: "2.1GB" 
    last_updated: "2024-07-31"
    description: "Dữ liệu khách hàng từ CRM system"
    
  loan_applications:
    source: "loan_origination_system"
    format: "parquet"
    size: "850MB"
    last_updated: "2024-07-30"
    description: "Hồ sơ vay từ LOS system"
```

## 6. **Version Control cho Data Science**:

### **Git ignore patterns:**
```gitignore
# Data files
data/raw/
data/interim/
data/processed/
*.csv
*.parquet
*.xlsx

# Models
models/
*.pkl
*.joblib
*.h5

# Jupyter checkpoints
.ipynb_checkpoints/

# Environment
.env
.venv/
```

### **DVC cho data versioning:**
```yaml
# .dvc/config
[core]
    remote = storage
['remote "storage"']
    url = s3://pd-models-data-bucket
```

## 7. **Environment Management**:

### **requirements.txt structure:**
```txt
# Core data science
pandas>=1.5.0
numpy>=1.21.0
scikit-learn>=1.1.0
xgboost>=1.6.0

# Visualization  
matplotlib>=3.5.0
seaborn>=0.11.0
plotly>=5.0.0

# Database
sqlalchemy>=1.4.0
pymssql>=2.2.0
ibm-db>=3.1.0

# Development
jupyter>=1.0.0
pytest>=7.0.0
black>=22.0.0
flake8>=4.0.0
```

## 8. **Automation và Makefile**:

```makefile
# Makefile
.PHONY: setup clean test train deploy

setup:
	pip install -r requirements.txt
	pre-commit install

clean:
	find . -type f -name "*.pyc" -delete
	find . -type d -name "__pycache__" -delete

test:
	pytest tests/ -v

train:
	python scripts/training/train_pd_model.py

deploy:
	docker build -t pd-model-api .
	docker run -p 8000:8000 pd-model-api
```

## 9. **Best Practices**:

1. **Consistency**: Luôn sử dụng naming convention nhất quán
2. **Clarity**: Tên file/folder phải mô tả rõ ràng nội dung
3. **Versioning**: Include version numbers cho models và datasets
4. **Documentation**: Mỗi folder quan trọng cần có README.md
5. **Security**: Không commit sensitive data hoặc credentials
6. **Scalability**: Cấu trúc phải dễ scale khi dự án lớn lên

## 10. **Quy trình commit**:
- Tin nhắn commit phải được viết bằng tiếng Việt
- Format: `[type]: mô tả thay đổi về structure/organization`
- Ví dụ: `structure: tổ chức lại folder data theo customer segments`
