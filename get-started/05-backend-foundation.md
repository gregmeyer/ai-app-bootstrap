# 05: Backend Foundation

## Overview
This guide covers creating your core FastAPI backend service with basic endpoints and WebSocket support.

## 🚀 FastAPI Application Setup

### 5.1 Basic Application Structure

**Create `backend/app/main.py`:**
```python
from fastapi import FastAPI, WebSocket, WebSocketDisconnect
from fastapi.middleware.cors import CORSMiddleware
from fastapi.staticfiles import StaticFiles
from contextlib import asynccontextmanager
import logging
from typing import List

# Configure logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

# WebSocket connection manager
class ConnectionManager:
    def __init__(self):
        self.active_connections: List[WebSocket] = []

    async def connect(self, websocket: WebSocket):
        await websocket.accept()
        self.active_connections.append(websocket)
        logger.info(f"WebSocket connected. Total connections: {len(self.active_connections)}")

    def disconnect(self, websocket: WebSocket):
        self.active_connections.remove(websocket)
        logger.info(f"WebSocket disconnected. Total connections: {len(self.active_connections)}")

    async def send_personal_message(self, message: str, websocket: WebSocket):
        await websocket.send_text(message)

    async def broadcast(self, message: str):
        for connection in self.active_connections:
            try:
                await connection.send_text(message)
            except Exception as e:
                logger.error(f"Error broadcasting message: {e}")
                # Remove broken connections
                self.active_connections.remove(connection)

manager = ConnectionManager()

# Application lifespan
@asynccontextmanager
async def lifespan(app: FastAPI):
    # Startup
    logger.info("Starting up FastAPI application...")
    yield
    # Shutdown
    logger.info("Shutting down FastAPI application...")

# Create FastAPI app
app = FastAPI(
    title="My Awesome Service",
    description="A modern, maintainable backend service",
    version="0.1.0",
    lifespan=lifespan
)

# CORS middleware
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000", "https://www.example.com"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Mount static files
app.mount("/static", StaticFiles(directory="static"), name="static")

# Health check endpoint
@app.get("/healthz")
async def health_check():
    return {
        "status": "healthy",
        "service": "my-awesome-service",
        "version": "0.1.0",
        "connections": len(manager.active_connections)
    }

# WebSocket endpoint
@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await manager.connect(websocket)
    try:
        while True:
            data = await websocket.receive_text()
            # Echo the message back
            await manager.send_personal_message(f"Message: {data}", websocket)
            # Broadcast to all connections
            await manager.broadcast(f"Broadcast: {data}")
    except WebSocketDisconnect:
        manager.disconnect(websocket)

# Basic API endpoints
@app.get("/")
async def root():
    return {"message": "Welcome to My Awesome Service"}

@app.get("/api/v1/status")
async def api_status():
    return {
        "status": "operational",
        "timestamp": "2025-01-08T12:00:00Z",
        "version": "0.1.0"
    }

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

### 5.2 Dependencies and Middleware

**Create `backend/app/deps.py`:**
```python
from fastapi import Depends, HTTPException, status
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
from typing import Optional
import logging

logger = logging.getLogger(__name__)

# Security
security = HTTPBearer()

# Authentication dependency (placeholder)
async def get_current_user(credentials: HTTPAuthorizationCredentials = Depends(security)):
    """
    Placeholder for user authentication.
    Replace with your actual authentication logic.
    """
    # This is a placeholder - implement your auth logic here
    if not credentials:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid authentication credentials",
            headers={"WWW-Authenticate": "Bearer"},
        )
    
    # Mock user for now
    user = {"id": "user-123", "username": "testuser", "role": "user"}
    return user

# Role-based access control
async def require_role(required_role: str, current_user: dict = Depends(get_current_user)):
    """
    Check if user has required role.
    """
    if current_user.get("role") != required_role:
        raise HTTPException(
            status_code=status.HTTP_403_FORBIDDEN,
            detail="Insufficient permissions"
        )
    return current_user

# Admin access
async def require_admin(current_user: dict = Depends(get_current_user)):
    return await require_role("admin", current_user)

# User access
async def require_user(current_user: dict = Depends(get_current_user)):
    return await require_role("user", current_user)
```

### 5.3 Data Models

**Create `backend/app/schemas/base.py`:**
```python
from pydantic import BaseModel, Field
from typing import Optional
from datetime import datetime
from enum import Enum

class StatusEnum(str, Enum):
    ACTIVE = "active"
    INACTIVE = "inactive"
    PENDING = "pending"
    DELETED = "deleted"

class BaseSchema(BaseModel):
    """Base schema with common fields."""
    
    id: Optional[str] = Field(None, description="Unique identifier")
    created_at: Optional[datetime] = Field(None, description="Creation timestamp")
    updated_at: Optional[datetime] = Field(None, description="Last update timestamp")
    status: Optional[StatusEnum] = Field(StatusEnum.ACTIVE, description="Entity status")
    
    class Config:
        from_attributes = True
        json_encoders = {
            datetime: lambda v: v.isoformat()
        }

class PaginationParams(BaseModel):
    """Pagination parameters for list endpoints."""
    
    page: int = Field(1, ge=1, description="Page number")
    size: int = Field(20, ge=1, le=100, description="Page size")
    
class PaginatedResponse(BaseModel):
    """Paginated response wrapper."""
    
    items: list
    total: int
    page: int
    size: int
    pages: int
    
    @classmethod
    def create(cls, items: list, total: int, page: int, size: int):
        pages = (total + size - 1) // size
        return cls(
            items=items,
            total=total,
            page=page,
            size=size,
            pages=pages
        )
```

**Create `backend/app/schemas/requests.py`:**
```python
from pydantic import BaseModel, Field
from typing import Optional

class HealthCheckRequest(BaseModel):
    """Health check request."""
    
    service: str = Field(..., description="Service name to check")
    timeout: Optional[int] = Field(5, description="Timeout in seconds")

class WebSocketMessage(BaseModel):
    """WebSocket message format."""
    
    type: str = Field(..., description="Message type")
    data: dict = Field(..., description="Message data")
    timestamp: Optional[str] = Field(None, description="Message timestamp")
```

**Create `backend/app/schemas/responses.py`:**
```python
from pydantic import BaseModel, Field
from typing import Optional, Any
from datetime import datetime

class HealthCheckResponse(BaseModel):
    """Health check response."""
    
    status: str = Field(..., description="Service status")
    service: str = Field(..., description="Service name")
    version: str = Field(..., description="Service version")
    timestamp: datetime = Field(..., description="Check timestamp")
    details: Optional[dict] = Field(None, description="Additional details")

class WebSocketResponse(BaseModel):
    """WebSocket response format."""
    
    type: str = Field(..., description="Response type")
    data: Any = Field(..., description="Response data")
    timestamp: datetime = Field(..., description="Response timestamp")
    success: bool = Field(..., description="Operation success")

class ErrorResponse(BaseModel):
    """Error response format."""
    
    error: str = Field(..., description="Error message")
    code: str = Field(..., description="Error code")
    details: Optional[dict] = Field(None, description="Error details")
    timestamp: datetime = Field(..., description="Error timestamp")
```

### 5.4 API Routes

**Create `backend/app/api/__init__.py`:**
```python
# API package initialization
```

**Create `backend/app/api/health.py`:**
```python
from fastapi import APIRouter, Depends
from app.schemas.requests import HealthCheckRequest
from app.schemas.responses import HealthCheckResponse
from datetime import datetime
import logging

logger = logging.getLogger(__name__)
router = APIRouter(prefix="/health", tags=["health"])

@router.get("/", response_model=HealthCheckResponse)
async def health_check():
    """Basic health check endpoint."""
    return HealthCheckResponse(
        status="healthy",
        service="my-awesome-service",
        version="0.1.0",
        timestamp=datetime.utcnow(),
        details={
            "uptime": "0:00:00",
            "memory_usage": "0 MB",
            "active_connections": 0
        }
    )

@router.post("/check", response_model=HealthCheckResponse)
async def health_check_service(request: HealthCheckRequest):
    """Health check for specific service."""
    logger.info(f"Health check requested for service: {request.service}")
    
    # Here you would implement actual health checking logic
    # For now, return a mock response
    return HealthCheckResponse(
        status="healthy",
        service=request.service,
        version="0.1.0",
        timestamp=datetime.utcnow(),
        details={
            "timeout": request.timeout,
            "checked_at": datetime.utcnow().isoformat()
        }
    )
```

**Create `backend/app/api/status.py`:**
```python
from fastapi import APIRouter, Depends
from app.schemas.responses import WebSocketResponse
from app.deps import get_current_user
from datetime import datetime
import logging

logger = logging.getLogger(__name__)
router = APIRouter(prefix="/status", tags=["status"])

@router.get("/")
async def get_status():
    """Get service status."""
    return {
        "status": "operational",
        "timestamp": datetime.utcnow().isoformat(),
        "version": "0.1.0",
        "environment": "development"
    }

@router.get("/user")
async def get_user_status(current_user: dict = Depends(get_current_user)):
    """Get user-specific status."""
    return {
        "user_id": current_user["id"],
        "username": current_user["username"],
        "status": "active",
        "last_seen": datetime.utcnow().isoformat()
    }
```

### 5.5 Update Main Application

**Update `backend/app/main.py` to include routes:**
```python
# Add these imports at the top
from app.api import health, status

# Add these lines after CORS middleware
app.include_router(health.router)
app.include_router(status.router)
```

## 🧪 Testing

### 5.6 Basic Tests

**Create `backend/tests/test_main.py`:**
```python
import pytest
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)

def test_health_check():
    response = client.get("/healthz")
    assert response.status_code == 200
    data = response.json()
    assert data["status"] == "healthy"
    assert "my-awesome-service" in data["service"]

def test_root_endpoint():
    response = client.get("/")
    assert response.status_code == 200
    data = response.json()
    assert "Welcome" in data["message"]

def test_api_status():
    response = client.get("/api/v1/status")
    assert response.status_code == 200
    data = response.json()
    assert data["status"] == "operational"
    assert "version" in data

def test_health_endpoint():
    response = client.get("/health/")
    assert response.status_code == 200
    data = response.json()
    assert data["status"] == "healthy"
    assert "timestamp" in data
```

**Create `backend/tests/conftest.py`:**
```python
import pytest
from fastapi.testclient import TestClient
from app.main import app

@pytest.fixture
def client():
    return TestClient(app)

@pytest.fixture
def test_user():
    return {
        "id": "test-user-123",
        "username": "testuser",
        "role": "user"
    }
```

## 🚀 Running the Application

### 5.7 Development Server

**Start the development server:**
```bash
cd backend

# Install dependencies
pip install -e .

# Run with auto-reload
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### 5.8 Docker

**Build and run with Docker:**
```bash
cd backend

# Build image
docker build -t my-awesome-service .

# Run container
docker run -p 8000:8000 my-awesome-service

# Or use docker-compose
docker-compose up --build
```

## ✅ Verification Checklist

- [ ] FastAPI application created
- [ ] Basic endpoints working (/healthz, /, /api/v1/status)
- [ ] WebSocket endpoint functional
- [ ] CORS middleware configured
- [ ] API routes organized
- [ ] Data models defined
- [ ] Basic tests passing
- [ ] Development server running
- [ ] Docker build successful

## 🚀 Next Steps

After completing backend foundation:

1. **[Validation Framework](06-validation-framework.md)** - Prove your concept works
2. **[Troubleshooting](07-troubleshooting.md)** - Solve common problems
3. **[Alternative Stacks](08-alternative-stacks.md)** - Use different technologies

## 🔧 Troubleshooting

### Common Backend Issues

#### **Import Errors**
```bash
# Check Python path
echo $PYTHONPATH

# Install in development mode
pip install -e .

# Check imports
python -c "from app.main import app; print('Import successful')"
```

#### **Port Already in Use**
```bash
# Check what's using the port
lsof -i :8000

# Kill the process
kill -9 <PID>

# Or use a different port
uvicorn app.main:app --reload --port 8001
```

#### **WebSocket Connection Issues**
```bash
# Test WebSocket endpoint
curl -i -N -H "Connection: Upgrade" -H "Upgrade: websocket" -H "Sec-WebSocket-Version: 13" -H "Sec-WebSocket-Key: test" http://localhost:8000/ws
```

---

**Previous:** [Environment & Configuration](04-environment-configuration.md) ← | **Next:** [Validation Framework](06-validation-framework.md) →
