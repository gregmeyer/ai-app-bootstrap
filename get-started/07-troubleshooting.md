# 07: Troubleshooting

## Overview
This guide covers common problems and their solutions when bootstrapping your application.

## 🔧 Common Setup Issues

### 7.1 CLI Not Working

**Check if dependencies are installed:**
```bash
# Check if dependencies are installed
pip list | grep click

# Verify Python path
which python
python --version

# Check if CLI file is executable
chmod +x cli/cli.py

# Test basic functionality
python cli/cli.py --help
```

**Common CLI Issues:**
```bash
# Import errors
python -c "import click; print('Click version:', click.__version__)"

# Path issues
export PYTHONPATH="${PYTHONPATH}:$(pwd)"

# Permission issues
chmod +x cli/cli.py
```

### 7.2 Directory Structure Issues

**Verify all directories exist:**
```bash
# Check directory structure
ls -la agentContext/
ls -la tickets/
ls -la stories/

# Create missing directories
mkdir -p agentContext/{architecture,decisions,notes}
mkdir -p tickets stories
```

**Fix directory permissions:**
```bash
# Check permissions
ls -la agentContext/

# Fix permissions if needed
chmod 755 agentContext/
chmod 755 tickets/
chmod 755 stories/
```

### 7.3 File Permission Issues

**Fix file permissions:**
```bash
# Fix file permissions
chmod 644 agentContext/*/*.md
chmod 644 tickets/*.md
chmod 644 stories/*.md

# Fix script permissions
chmod +x setup-env.sh
```

**Check file ownership:**
```bash
# Check ownership
ls -la agentContext/

# Fix ownership if needed
sudo chown -R $(whoami):$(whoami) agentContext/
```

### 7.4 Environment Issues

**Check virtual environment:**
```bash
# Check virtual environment
echo $VIRTUAL_ENV

# Reactivate if needed
source venv/bin/activate

# Reinstall dependencies
pip install --upgrade click python-dateutil
```

**Environment variable issues:**
```bash
# Check if .env.local exists
ls -la .env.local

# Verify file permissions
chmod 600 .env.local

# Test with Python
python -c "import os; print(os.getenv('SERVICE_NAME'))"
```

## 🐛 Debugging Commands

### 7.5 Check System Status

**Verify project structure:**
```bash
# Verify project structure
tree -I 'venv|__pycache__|*.pyc' .

# Check git status
git status

# Verify environment
python -c "import click; print('Click version:', click.__version__)"
```

**Check file system:**
```bash
# Check disk space
df -h .

# Check file sizes
du -sh agentContext/* tickets/* stories/*

# Optimize if needed
find . -name "*.md" -exec wc -l {} + | sort -n
```

### 7.6 Validate CLI Functionality

**Test each command:**
```bash
# Test each command
python cli/cli.py start-session
python cli/cli.py status
python cli/cli.py pick-ticket TEST-001
python cli/cli.py update-context --type note --msg "Test message"
python cli/cli.py finish-ticket TEST-001 --summary "Test completion"
```

**Check file creation:**
```bash
# Verify files are created
ls -la agentContext/notes/
ls -la agentContext/decisions/
ls -la agentContext/architecture/

# Check file content
cat agentContext/notes/$(date +%d%m%Y)_note.md
```

### 7.7 Check File Creation

**Verify file operations:**
```bash
# Check file creation
ls -la agentContext/notes/
ls -la agentContext/decisions/
ls -la agentContext/architecture/

# Check file content
cat agentContext/notes/$(date +%d%m%Y)_note.md

# Check file permissions
ls -la agentContext/notes/*.md
```

## 🔄 Reset & Recovery

### 7.8 Reset Development Session

**Remove today's files:**
```bash
# Remove today's files
rm -f agentContext/notes/$(date +%d%m%Y)_note.md
rm -f agentContext/decisions/$(date +%d%m%Y)_decision.md
rm -f agentContext/architecture/$(date +%d%m%Y)_architecture.md

# Start fresh
python cli/cli.py start-session
```

**Reset specific context:**
```bash
# Reset notes
rm -f agentContext/notes/$(date +%d%m%Y)_note.md

# Reset decisions
rm -f agentContext/decisions/$(date +%d%m%Y)_decision.md

# Reset architecture
rm -f agentContext/architecture/$(date +%d%m%Y)_architecture.md
```

### 7.9 Reset Entire Project

**Backup and reset:**
```bash
# Backup important files
cp -r agentContext agentContext.backup
cp -r tickets tickets.backup
cp -r stories stories.backup

# Remove generated files
find . -name "*.md" -not -path "./bootstrap/*" -not -path "./README.md" -delete

# Recreate structure
mkdir -p agentContext/{architecture,decisions,notes} tickets stories

# Restart
python cli/cli.py start-session
```

**Complete reset:**
```bash
# Remove all generated content
rm -rf agentContext tickets stories

# Recreate from scratch
mkdir -p agentContext/{architecture,decisions,notes} tickets stories

# Reinstall dependencies
pip install --upgrade click python-dateutil

# Start fresh
python cli/cli.py start-session
```

## 🚀 Performance Issues

### 7.10 Slow File Operations

**Check disk performance:**
```bash
# Check disk space
df -h .

# Check file sizes
du -sh agentContext/* tickets/* stories/*

# Optimize if needed
find . -name "*.md" -exec wc -l {} + | sort -n
```

**File system optimization:**
```bash
# Check file system type
df -T .

# Check inode usage
df -i .

# Optimize file operations
find . -name "*.md" -exec ls -la {} \;
```

### 7.11 Memory Issues

**Check Python memory usage:**
```bash
# Check Python memory usage
python -c "import sys; print('Memory:', sys.getsizeof(sys.modules))"

# Monitor system resources
top -p $(pgrep python)

# Check memory usage
free -h
```

**Memory optimization:**
```bash
# Restart Python processes
pkill python

# Clear Python cache
find . -name "__pycache__" -type d -exec rm -rf {} +
find . -name "*.pyc" -delete
```

## 🔧 Backend Issues

### 7.12 FastAPI Problems

**Import errors:**
```bash
# Check Python path
echo $PYTHONPATH

# Install in development mode
cd backend
pip install -e .

# Check imports
python -c "from app.main import app; print('Import successful')"
```

**Port conflicts:**
```bash
# Check what's using the port
lsof -i :8000

# Kill the process
kill -9 <PID>

# Or use a different port
uvicorn app.main:app --reload --port 8001
```

**WebSocket issues:**
```bash
# Test WebSocket endpoint
curl -i -N -H "Connection: Upgrade" -H "Upgrade: websocket" -H "Sec-WebSocket-Version: 13" -H "Sec-WebSocket-Key: test" http://localhost:8000/ws
```

### 7.13 Docker Issues

**Build failures:**
```bash
# Check Docker daemon
docker info

# Clean up Docker
docker system prune -a

# Rebuild without cache
docker build --no-cache .
```

**Container issues:**
```bash
# Check container logs
docker logs <container_id>

# Check container status
docker ps -a

# Restart container
docker restart <container_id>
```

## 🧪 Testing Issues

### 7.14 Test Failures

**Check test output:**
```bash
# Run tests with verbose output
pytest -v

# Run specific test
pytest tests/test_validation.py::TestCoreFunctionality::test_health_endpoint -v

# Check coverage
pytest --cov=app --cov-report=html
```

**Test environment issues:**
```bash
# Check test dependencies
pip list | grep pytest

# Install test dependencies
pip install pytest pytest-asyncio httpx

# Run tests in isolation
python -m pytest tests/ -v
```

### 7.15 Performance Testing Issues

**Benchmark problems:**
```bash
# Run benchmarks
python backend/tests/benchmarks.py

# Check system resources
top -p $(pgrep python)

# Monitor during testing
htop
```

## 📊 Diagnostic Tools

### 7.16 System Monitoring

**Resource monitoring:**
```bash
# Monitor CPU and memory
top

# Monitor disk I/O
iotop

# Monitor network
iftop

# Monitor processes
ps aux | grep python
```

**Log analysis:**
```bash
# Check application logs
tail -f logs/app.log

# Check system logs
tail -f /var/log/syslog

# Check error logs
grep -i error logs/*.log
```

### 7.17 Network Diagnostics

**Network connectivity:**
```bash
# Check local connectivity
curl http://localhost:8000/healthz

# Check network status
netstat -tulpn | grep :8000

# Check firewall
sudo ufw status
```

## ✅ Verification Steps

### 7.18 System Health Check

**Run comprehensive check:**
```bash
# 1. Check CLI functionality
python cli/cli.py --help

# 2. Check file creation
python cli/cli.py start-session
ls -la agentContext/notes/

# 3. Check backend (if created)
cd backend
python -c "import fastapi; print('FastAPI version:', fastapi.__version__)"

# 4. Check tests
pytest --version

# 5. Check Docker (if using)
docker --version
```

**Common verification issues:**
```bash
# Missing dependencies
pip install -r requirements.txt

# Path issues
export PYTHONPATH="${PYTHONPATH}:$(pwd)"

# Permission issues
chmod +x cli/cli.py
chmod +x setup-env.sh
```

## 🚀 Next Steps

After resolving issues:

1. **[Alternative Stacks](08-alternative-stacks.md)** - Use different technologies
2. **Continue Development** - Resume your project
3. **Document Solutions** - Add to troubleshooting guide

## 🔧 Prevention

### 7.19 Best Practices

**Avoid common issues:**
- Always use virtual environments
- Check file permissions after creation
- Test CLI commands after implementation
- Monitor system resources during development
- Keep backups of important files

**Regular maintenance:**
- Clean up temporary files
- Monitor disk space
- Update dependencies regularly
- Check for security updates
- Backup project state

---

**Previous:** [Validation Framework](06-validation-framework.md) ← | **Next:** [Alternative Stacks](08-alternative-stacks.md) →
