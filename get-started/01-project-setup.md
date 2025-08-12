# 01: Project Setup

## Overview
This guide covers the initial project setup including directory structure, Python environment, and basic configuration.

## 🚀 Project Initialization

### 1.1 Create Project Directory
```bash
# Create project directory
mkdir my-awesome-project
cd my-awesome-project

# Initialize git
git init

# Create directory structure
mkdir -p backend/app/{schemas,core,adapters,api} backend/tests cli agentContext/{architecture,decisions,notes} tickets stories client bootstrap
```

### 1.2 Python Environment Setup
```bash
# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install core dependencies
pip install click python-dateutil fastapi uvicorn pydantic
```

### 1.3 Verify Directory Structure
```bash
# Check that all directories were created
tree -I 'venv|__pycache__|*.pyc' .

# Expected output:
# .
# ├── agentContext
# │   ├── architecture
# │   ├── decisions
# │   └── notes
# ├── backend
# │   ├── app
# │   │   ├── adapters
# │   │   ├── api
# │   │   ├── core
# │   │   └── schemas
# │   └── tests
# ├── bootstrap
# ├── client
# ├── cli
# ├── stories
# └── tickets
```

## 📁 Directory Structure Details

### Backend Structure
```
backend/
├── app/
│   ├── schemas/        # Pydantic data models
│   ├── core/           # Business logic modules
│   ├── adapters/       # External service adapters
│   └── api/            # API endpoint handlers
├── tests/              # Unit and integration tests
├── pyproject.toml      # Dependencies and project config
├── Dockerfile          # Containerization
└── docker-compose.yml  # Local development
```

### Development Tools
```
cli/                    # Development CLI tools
├── __init__.py
└── cli.py             # Click-based commands

agentContext/           # Development context tracking
├── architecture/       # Architecture decision records
├── decisions/          # Development decision logs
└── notes/              # Daily development notes
```

### Project Management
```
tickets/                # Project tickets and progress tracking
├── README.md           # Ticket overview
└── T-XXX-*.md         # Individual ticket files

stories/                # User stories and requirements mapping
├── README.md           # Story overview
└── A-XX-*.md          # Individual story files
```

### Client/Plugin
```
client/                 # Thin client or plugin
├── manifest.json       # Client configuration
├── ui.html             # User interface
└── code.ts             # Client logic (thin)
```

## 🔧 Initial Configuration

### 1.4 Create Basic Files
```bash
# Create __init__.py files for Python packages
touch backend/__init__.py
touch backend/app/__init__.py
touch backend/app/schemas/__init__.py
touch backend/app/core/__init__.py
touch backend/app/adapters/__init__.py
touch backend/app/api/__init__.py
touch cli/__init__.py

# Create basic README files
echo "# My Awesome Project" > README.md
echo "# Backend Service" > backend/README.md
echo "# Development CLI" > cli/README.md
```

### 1.5 Git Configuration
```bash
# Create .gitignore
cat > .gitignore << 'EOF'
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
share/python-wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST

# Virtual Environment
venv/
env/
ENV/
env.bak/
venv.bak/

# Environment Variables
.env
.env.local
.env.*.local
.env.production
.env.staging

# IDE
.vscode/
.idea/
*.swp
*.swo
*~

# OS
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# Logs
*.log
logs/

# Testing
.coverage
.pytest_cache/
.tox/
htmlcov/

# Build artifacts
*.tsbuildinfo
dist/
build/

# Temporary files
*.tmp
*.temp
.cache/

# Database
*.db
*.sqlite
*.sqlite3

# Docker
.dockerignore

# Backup files
*.bak
*.backup
EOF

# Initial commit
git add .
git commit -m "Initial project structure"
```

## ✅ Verification Checklist

- [ ] Project directory created
- [ ] Git repository initialized
- [ ] Directory structure created
- [ ] Python virtual environment set up
- [ ] Core dependencies installed
- [ ] Basic files created
- [ ] Git configuration complete
- [ ] Initial commit made

## 🚀 Next Steps

After completing this setup:

1. **[CLI Implementation](02-cli-implementation.md)** - Build your development workflow
2. **[Architecture & Planning](03-architecture-planning.md)** - Define your system design
3. **[Environment & Configuration](04-environment-configuration.md)** - Set up your development environment

## 🔧 Troubleshooting

### Common Issues

#### **Directory Creation Fails**
```bash
# Check permissions
ls -la

# Create directories manually if needed
mkdir -p backend/app/schemas
mkdir -p backend/app/core
mkdir -p backend/app/adapters
mkdir -p backend/app/api
mkdir -p backend/tests
mkdir -p cli
mkdir -p agentContext/architecture
mkdir -p agentContext/decisions
mkdir -p agentContext/notes
mkdir -p tickets
mkdir -p stories
mkdir -p client
mkdir -p bootstrap
```

#### **Python Environment Issues**
```bash
# Check Python version
python3 --version

# Verify virtual environment
which python
echo $VIRTUAL_ENV

# Recreate if needed
rm -rf venv
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install click python-dateutil fastapi uvicorn pydantic
```

#### **Git Issues**
```bash
# Check git status
git status

# Initialize if needed
git init

# Configure user if needed
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

---

**Next:** [CLI Implementation](02-cli-implementation.md) →
