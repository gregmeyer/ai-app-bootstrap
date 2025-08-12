# 04: Environment & Configuration

## Overview
This guide covers setting up your development environment, configuration files, and validation.

## 🔧 Environment Setup

### 4.1 Environment Template

**Create `env.template`:**
```bash
# Environment Configuration Template
# Copy this file to .env.local and fill in your actual values

# Service Configuration
SERVICE_NAME=my-awesome-service
SERVICE_VERSION=0.1.0
DEBUG=true
ENVIRONMENT=development

# Backend Configuration
BACKEND_HOST=0.0.0.0
BACKEND_PORT=8000
BACKEND_WORKERS=1
BACKEND_BASE_URL=http://localhost:8000

# Database Configuration
DATABASE_URL=postgresql://user:password@localhost:5432/mydb
DATABASE_POOL_SIZE=10
DATABASE_MAX_OVERFLOW=20

# External Services
EXTERNAL_API_URL=https://api.example.com
EXTERNAL_API_KEY=your_api_key_here

# Feature Flags
ENABLE_AUTH=true
ENABLE_LOGGING=true
ENABLE_METRICS=true

# Security
SECRET_KEY=your_secret_key_here
JWT_SECRET=your_jwt_secret_here
ENCRYPTION_KEY=your_encryption_key_here

# Logging
LOG_LEVEL=INFO
LOG_FORMAT=json
LOG_FILE=logs/app.log

# Rate Limiting
RATE_LIMIT_REQUESTS=100
RATE_LIMIT_WINDOW=3600

# CORS Configuration
CORS_ORIGINS=http://localhost:3000,https://www.example.com
ALLOWED_HOSTS=localhost,127.0.0.1,example.com
```

### 4.2 Setup Scripts

**Create `setup-env.sh`:**
```bash
#!/bin/bash

# Environment Setup Script
echo "🚀 Setting up development environment..."

# Check if .env.local already exists
if [ -f ".env.local" ]; then
    echo "⚠️  .env.local already exists. Backing up to .env.local.backup"
    cp .env.local .env.local.backup
fi

# Copy template to .env.local
if [ -f "env.template" ]; then
    echo "📋 Creating .env.local from template..."
    cp env.template .env.local
    echo "✅ .env.local created successfully!"
    echo ""
    echo "📝 Next steps:"
    echo "1. Edit .env.local with your actual values"
    echo "2. Set up your virtual environment: python3 -m venv venv"
    echo "3. Activate it: source venv/bin/activate"
    echo "4. Install dependencies: pip install -r requirements.txt"
    echo ""
    echo "🔐 Important: Never commit .env.local to version control!"
else
    echo "❌ env.template not found!"
    exit 1
fi

# Check if virtual environment exists
if [ ! -d "venv" ]; then
    echo ""
    echo "🐍 Virtual environment not found. Creating one..."
    python3 -m venv venv
    echo "✅ Virtual environment created!"
    echo "   Activate it with: source venv/bin/activate"
fi

echo ""
echo "🎉 Environment setup complete!"
echo "   Don't forget to activate your virtual environment: source venv/bin/activate"
```

### 4.3 Environment Validation

**Test that everything is working:**
```bash
# Test CLI functionality
python cli/cli.py --help
python cli/cli.py start-session
python cli/cli.py status

# Verify file creation
ls -la agentContext/notes/
ls -la agentContext/decisions/
ls -la agentContext/architecture/

# Test backend (if created)
cd backend
python -c "import fastapi; print('FastAPI version:', fastapi.__version__)"
uvicorn app.main:app --reload --port 8001 &
curl http://localhost:8001/healthz
kill %1
```

## 📁 Configuration Files

### 4.4 Backend Configuration

**Create `backend/pyproject.toml`:**
```toml
[project]
name = "my-awesome-service"
version = "0.1.0"
description = "My awesome service description"
authors = [
    {name = "Your Name", email = "your.email@example.com"}
]
readme = "README.md"
requires-python = ">=3.9"
classifiers = [
    "Development Status :: 3 - Alpha",
    "Intended Audience :: Developers",
    "License :: OSI Approved :: MIT License",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
]

dependencies = [
    "fastapi>=0.104.0",
    "uvicorn[standard]>=0.24.0",
    "pydantic>=2.0.0",
    "python-multipart>=0.0.6",
    "python-jose[cryptography]>=3.3.0",
    "passlib[bcrypt]>=1.7.4",
    "sqlalchemy>=2.0.0",
    "alembic>=1.12.0",
    "psycopg2-binary>=2.9.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0.0",
    "pytest-asyncio>=0.21.0",
    "httpx>=0.25.0",
    "black>=23.0.0",
    "flake8>=6.0.0",
    "mypy>=1.7.0",
    "pre-commit>=3.5.0",
]
test = [
    "pytest>=7.0.0",
    "pytest-asyncio>=0.21.0",
    "httpx>=0.25.0",
    "coverage>=7.3.0",
]

[project.scripts]
my-service = "app.main:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.black]
line-length = 88
target-version = ['py39']
include = '\.pyi?$'
extend-exclude = '''
/(
  # directories
  \.eggs
  | \.git
  | \.hg
  | \.mypy_cache
  | \.tox
  | \.venv
  | build
  | dist
)/
'''

[tool.mypy]
python_version = "3.9"
warn_return_any = true
warn_unused_configs = true
disallow_untyped_defs = true
disallow_incomplete_defs = true
check_untyped_defs = true
disallow_untyped_decorators = true
no_implicit_optional = true
warn_redundant_casts = true
warn_unused_ignores = true
warn_no_return = true
warn_unreachable = true
strict_equality = true

[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = ["test_*.py", "*_test.py"]
python_classes = ["Test*"]
python_functions = ["test_*"]
addopts = [
    "--strict-markers",
    "--strict-config",
    "--verbose",
    "--tb=short",
    "--cov=app",
    "--cov-report=term-missing",
    "--cov-report=html",
]
markers = [
    "slow: marks tests as slow (deselect with '-m \"not slow\"')",
    "integration: marks tests as integration tests",
    "unit: marks tests as unit tests",
]
```

### 4.5 Docker Configuration

**Create `backend/Dockerfile`:**
```dockerfile
FROM python:3.11-slim

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV PYTHONPATH=/app

# Set work directory
WORKDIR /app

# Install system dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY pyproject.toml .
RUN pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir .

# Copy project
COPY . .

# Create non-root user
RUN adduser --disabled-password --gecos '' appuser \
    && chown -R appuser:appuser /app
USER appuser

# Expose port
EXPOSE 8000

# Run the application
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

**Create `backend/docker-compose.yml`:**
```yaml
version: '3.8'

services:
  backend:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DEBUG=true
      - BACKEND_HOST=0.0.0.0
      - BACKEND_PORT=8000
    volumes:
      - .:/app
    command: uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:15
    environment:
      - POSTGRES_DB=myapp
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

## 🔧 Environment Management

### 4.6 Environment Variables

**Loading environment variables in Python:**
```python
# backend/app/config.py
import os
from typing import Optional
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    # Service Configuration
    service_name: str = "my-awesome-service"
    service_version: str = "0.1.0"
    debug: bool = True
    environment: str = "development"
    
    # Backend Configuration
    backend_host: str = "0.0.0.0"
    backend_port: int = 8000
    backend_workers: int = 1
    backend_base_url: str = "http://localhost:8000"
    
    # Database Configuration
    database_url: Optional[str] = None
    database_pool_size: int = 10
    database_max_overflow: int = 20
    
    # External Services
    external_api_url: Optional[str] = None
    external_api_key: Optional[str] = None
    
    # Feature Flags
    enable_auth: bool = True
    enable_logging: bool = True
    enable_metrics: bool = True
    
    # Security
    secret_key: str = "your-secret-key-here"
    jwt_secret: str = "your-jwt-secret-here"
    encryption_key: str = "your-encryption-key-here"
    
    # Logging
    log_level: str = "INFO"
    log_format: str = "json"
    log_file: Optional[str] = None
    
    # Rate Limiting
    rate_limit_requests: int = 100
    rate_limit_window: int = 3600
    
    # CORS Configuration
    cors_origins: str = "http://localhost:3000,https://www.example.com"
    allowed_hosts: str = "localhost,127.0.0.1,example.com"
    
    class Config:
        env_file = ".env.local"
        env_file_encoding = "utf-8"
        case_sensitive = False

# Create settings instance
settings = Settings()
```

### 4.7 Configuration Validation

**Test configuration loading:**
```python
# backend/test_config.py
from app.config import settings

def test_config():
    print(f"Service: {settings.service_name} v{settings.service_version}")
    print(f"Environment: {settings.environment}")
    print(f"Debug: {settings.debug}")
    print(f"Backend: {settings.backend_host}:{settings.backend_port}")
    print(f"Database: {settings.database_url}")
    print(f"External API: {settings.external_api_url}")

if __name__ == "__main__":
    test_config()
```

## ✅ Verification Checklist

- [ ] Environment template created
- [ ] Setup script created and executable
- [ ] .env.local created from template
- [ ] Backend configuration files created
- [ ] Docker configuration created
- [ ] Environment variables loaded correctly
- [ ] Configuration validation working

## 🚀 Next Steps

After completing environment configuration:

1. **[Backend Foundation](05-backend-foundation.md)** - Create your core service
2. **[Validation Framework](06-validation-framework.md)** - Prove your concept works
3. **[Troubleshooting](07-troubleshooting.md)** - Solve common problems

## 🔧 Troubleshooting

### Common Environment Issues

#### **Environment Variables Not Loading**
```bash
# Check if .env.local exists
ls -la .env.local

# Verify file permissions
chmod 600 .env.local

# Test with Python
python -c "import os; print(os.getenv('SERVICE_NAME'))"
```

#### **Docker Build Fails**
```bash
# Check Docker daemon
docker info

# Clean up Docker
docker system prune -a

# Rebuild without cache
docker build --no-cache .
```

#### **Port Already in Use**
```bash
# Check what's using the port
lsof -i :8000

# Kill the process
kill -9 <PID>

# Or use a different port
export BACKEND_PORT=8001
```

---

**Previous:** [Architecture & Planning](03-architecture-planning.md) ← | **Next:** [Backend Foundation](05-backend-foundation.md) →
