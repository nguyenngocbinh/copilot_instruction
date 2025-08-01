---
applyTo: '**/Dockerfile*,**/*.{yml,yaml,tf,hcl}'
---
Cung cấp ngữ cảnh dự án và hướng dẫn viết mã mà AI nên tuân theo khi tạo code, trả lời câu hỏi, hoặc xem xét thay đổi.

# Quy tắc viết Docker và Infrastructure as Code

1. **Ngôn ngữ giao tiếp**:
   - Tất cả các phản hồi, tài liệu, và hướng dẫn phải được viết bằng tiếng Việt.
   - Tên resources, variables, và các thành phần infrastructure phải sử dụng tiếng Anh.

2. **Docker conventions**:
   ```dockerfile
   # Sử dụng multi-stage builds để optimize image size
   FROM python:3.11-slim as builder
   
   # Set working directory
   WORKDIR /app
   
   # Install system dependencies
   RUN apt-get update && apt-get install -y \
       gcc \
       && rm -rf /var/lib/apt/lists/*
   
   # Copy requirements first for better caching
   COPY requirements.txt .
   RUN pip install --no-cache-dir -r requirements.txt
   
   # Production stage
   FROM python:3.11-slim as production
   
   # Create non-root user cho security
   RUN groupadd -r appuser && useradd -r -g appuser appuser
   
   WORKDIR /app
   
   # Copy từ builder stage
   COPY --from=builder /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
   COPY --from=builder /usr/local/bin /usr/local/bin
   
   # Copy application code
   COPY . .
   
   # Change ownership
   RUN chown -R appuser:appuser /app
   USER appuser
   
   # Expose port
   EXPOSE 8000
   
   # Health check
   HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
       CMD curl -f http://localhost:8000/health || exit 1
   
   # Start command
   CMD ["python", "app.py"]
   ```

3. **Docker Compose conventions**:
   ```yaml
   version: '3.8'
   
   services:
     pd-model-api:
       build:
         context: .
         dockerfile: Dockerfile
         target: production
       container_name: pd-model-api
       ports:
         - "8000:8000"
       environment:
         - ENV=production
         - DB_HOST=database
         - DB_PORT=5432
       volumes:
         - ./logs:/app/logs
       depends_on:
         database:
           condition: service_healthy
       restart: unless-stopped
       networks:
         - pd-network
   
     database:
       image: postgres:14-alpine
       container_name: pd-database
       environment:
         POSTGRES_DB: pd_models
         POSTGRES_USER: pduser
         POSTGRES_PASSWORD_FILE: /run/secrets/db_password
       secrets:
         - db_password
       volumes:
         - postgres_data:/var/lib/postgresql/data
         - ./init.sql:/docker-entrypoint-initdb.d/init.sql
       healthcheck:
         test: ["CMD-SHELL", "pg_isready -U pduser -d pd_models"]
         interval: 10s
         timeout: 5s
         retries: 5
       networks:
         - pd-network
   
   volumes:
     postgres_data:
   
   networks:
     pd-network:
       driver: bridge
   
   secrets:
     db_password:
       file: ./secrets/db_password.txt
   ```

4. **Terraform conventions**:
   ```hcl
   # variables.tf - Định nghĩa các biến đầu vào
   variable "environment" {
     description = "Môi trường triển khai (dev, staging, prod)"
     type        = string
     validation {
       condition     = contains(["dev", "staging", "prod"], var.environment)
       error_message = "Environment phải là một trong: dev, staging, prod."
     }
   }
   
   variable "instance_type" {
     description = "Loại EC2 instance cho PD model service"
     type        = string
     default     = "t3.medium"
   }
   
   # main.tf - Resources chính
   terraform {
     required_version = ">= 1.0"
     required_providers {
       aws = {
         source  = "hashicorp/aws"
         version = "~> 5.0"
       }
     }
     
     backend "s3" {
       bucket = "pd-terraform-state"
       key    = "infrastructure/terraform.tfstate"
       region = "ap-southeast-1"
     }
   }
   
   provider "aws" {
     region = var.aws_region
     
     default_tags {
       tags = {
         Environment = var.environment
         Project     = "PD-Models"
         ManagedBy   = "Terraform"
       }
     }
   }
   
   # Data sources
   data "aws_availability_zones" "available" {
     state = "available"
   }
   
   # Local values
   locals {
     name_prefix = "pd-${var.environment}"
     common_tags = {
       Environment = var.environment
       Project     = "PD-Models"
       CreatedBy   = "Terraform"
     }
   }
   
   # Resources
   resource "aws_vpc" "main" {
     cidr_block           = var.vpc_cidr
     enable_dns_hostnames = true
     enable_dns_support   = true
     
     tags = merge(local.common_tags, {
       Name = "${local.name_prefix}-vpc"
     })
   }
   ```

5. **Kubernetes YAML conventions**:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: pd-model-api
     namespace: pd-models
     labels:
       app: pd-model-api
       version: v1.0
       environment: production
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: pd-model-api
     template:
       metadata:
         labels:
           app: pd-model-api
           version: v1.0
       spec:
         serviceAccountName: pd-model-service-account
         securityContext:
           runAsNonRoot: true
           runAsUser: 1000
           fsGroup: 2000
         containers:
         - name: pd-model-api
           image: your-registry/pd-model-api:v1.0
           ports:
           - containerPort: 8000
             name: http
           env:
           - name: ENV
             value: "production"
           - name: DB_HOST
             valueFrom:
               secretKeyRef:
                 name: pd-db-secret
                 key: host
           resources:
             requests:
               memory: "256Mi"
               cpu: "250m"
             limits:
               memory: "512Mi"
               cpu: "500m"
           livenessProbe:
             httpGet:
               path: /health
               port: http
             initialDelaySeconds: 30
             periodSeconds: 10
           readinessProbe:
             httpGet:
               path: /ready
               port: http
             initialDelaySeconds: 5
             periodSeconds: 5
   ```

6. **Security best practices**:
   - Sử dụng secrets management cho sensitive data
   - Chạy containers với non-root users
   - Implement proper health checks
   - Use minimal base images (alpine, distroless)
   - Regular security scanning cho images

7. **Documentation standards**:
   ```yaml
   # docker-compose.yml
   # Mô tả: Docker Compose configuration cho PD Models service
   # Tác giả: [Tên tác giả]
   # Ngày tạo: [Ngày tạo]
   # 
   # Để chạy:
   #   docker-compose up -d
   # 
   # Để stop:
   #   docker-compose down
   ```

8. **Environment management**:
   - Sử dụng `.env` files cho local development
   - Environment-specific configurations
   - Proper secrets management
   ```bash
   # .env.example
   ENV=development
   DB_HOST=localhost
   DB_PORT=5432
   DB_USER=pduser
   # DB_PASSWORD=<set trong production>
   API_PORT=8000
   LOG_LEVEL=INFO
   ```

9. **CI/CD Integration**:
   ```yaml
   # .github/workflows/docker-build.yml
   name: Build và Deploy Docker Image
   
   on:
     push:
       branches: [main, develop]
     pull_request:
       branches: [main]
   
   jobs:
     build:
       runs-on: ubuntu-latest
       steps:
       - uses: actions/checkout@v3
       
       - name: Set up Docker Buildx
         uses: docker/setup-buildx-action@v2
       
       - name: Build và test image
         run: |
           docker build -t pd-model-api:test .
           docker run --rm pd-model-api:test python -m pytest
   ```

10. **Monitoring và Logging**:
    - Configure proper logging volumes
    - Use structured logging formats
    - Implement monitoring endpoints
    - Set up log aggregation

11. **Quy trình commit**:
    - Tin nhắn commit phải được viết bằng tiếng Việt.
    - Format: `[loại]: mô tả ngắn gọn`
    - Ví dụ: `infra: thêm Docker configuration cho PD model API`
    - Loại commit: `infra`, `docker`, `k8s`, `terraform`, `fix`, `feat`
