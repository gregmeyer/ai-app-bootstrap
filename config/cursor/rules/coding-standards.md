# Coding Standards Rules

These rules define the coding standards, style guidelines, and quality requirements for AI App Bootstrap projects.

## 🎨 Code Style & Formatting

### **General Principles**
- **Readability First**: Code should be easy to read and understand
- **Consistency**: Follow established patterns throughout the project
- **Simplicity**: Prefer simple, clear solutions over clever ones
- **Documentation**: Document complex logic and important decisions

### **Naming Conventions**

#### **Files & Directories**
- **Python**: `snake_case` (e.g., `user_service.py`, `database_models.py`)
- **JavaScript/TypeScript**: `kebab-case` (e.g., `user-service.ts`, `database-models.ts`)
- **Directories**: `snake_case` for Python, `kebab-case` for JS/TS
- **Test Files**: `test_*.py` or `*.test.ts` or `*.spec.ts`

#### **Variables & Functions**
- **Python**: `snake_case` for variables and functions
- **JavaScript/TypeScript**: `camelCase` for variables and functions
- **Constants**: `UPPER_SNAKE_CASE` (Python) or `UPPER_CAMEL_CASE` (JS/TS)
- **Classes**: `PascalCase` in all languages

#### **Database & API**
- **Database Tables**: `snake_case` (e.g., `user_profiles`, `order_items`)
- **API Endpoints**: `kebab-case` (e.g., `/api/user-profiles`, `/api/order-items`)
- **JSON Fields**: `snake_case` for consistency with database

## 📝 Code Structure

### **File Organization**

#### **Python Files**
```python
"""
File docstring explaining purpose and usage.
"""

# Standard library imports
import os
import sys
from typing import List, Optional

# Third-party imports
import fastapi
from pydantic import BaseModel

# Local imports
from app.core.config import settings
from app.models.user import User

# Constants
MAX_RETRY_ATTEMPTS = 3
DEFAULT_TIMEOUT = 30

# Classes
class UserService:
    """Service class for user operations."""
    
    def __init__(self, db_session):
        self.db_session = db_session
    
    def get_user(self, user_id: int) -> Optional[User]:
        """Retrieve user by ID."""
        # Implementation here
        pass

# Functions
def validate_user_data(data: dict) -> bool:
    """Validate user input data."""
    # Implementation here
    pass

# Main execution (if applicable)
if __name__ == "__main__":
    # Main logic here
    pass
```

#### **TypeScript/JavaScript Files**
```typescript
/**
 * File description and purpose
 */

// Imports
import { useState, useEffect } from 'react';
import { UserService } from '../services/user-service';
import { User } from '../types/user';

// Constants
const MAX_RETRY_ATTEMPTS = 3;
const DEFAULT_TIMEOUT = 30000;

// Types/Interfaces
interface UserFormData {
  name: string;
  email: string;
  role: UserRole;
}

// Components/Functions
export const UserForm: React.FC<UserFormProps> = ({ onSubmit, initialData }) => {
  // Implementation here
};

// Utility functions
export const validateUserData = (data: UserFormData): boolean => {
  // Implementation here
};
```

### **Function & Method Structure**

#### **Python Functions**
```python
def create_user(
    username: str,
    email: str,
    password: str,
    role: UserRole = UserRole.USER,
    is_active: bool = True
) -> User:
    """
    Create a new user in the system.
    
    Args:
        username: Unique username for the user
        email: User's email address
        password: User's password (will be hashed)
        role: User's role in the system (default: USER)
        is_active: Whether the user account is active (default: True)
    
    Returns:
        User: The newly created user object
    
    Raises:
        ValidationError: If input data is invalid
        DuplicateUserError: If username or email already exists
    """
    # Input validation
    if not username or not email or not password:
        raise ValidationError("Username, email, and password are required")
    
    # Check for duplicates
    if user_exists(username, email):
        raise DuplicateUserError("Username or email already exists")
    
    # Create user logic
    hashed_password = hash_password(password)
    user = User(
        username=username,
        email=email,
        password_hash=hashed_password,
        role=role,
        is_active=is_active
    )
    
    # Save to database
    db.session.add(user)
    db.session.commit()
    
    return user
```

#### **TypeScript Functions**
```typescript
/**
 * Create a new user in the system
 * @param username - Unique username for the user
 * @param email - User's email address
 * @param password - User's password (will be hashed)
 * @param role - User's role in the system (default: USER)
 * @param isActive - Whether the user account is active (default: true)
 * @returns Promise<User> - The newly created user object
 * @throws ValidationError - If input data is invalid
 * @throws DuplicateUserError - If username or email already exists
 */
export const createUser = async (
  username: string,
  email: string,
  password: string,
  role: UserRole = UserRole.USER,
  isActive: boolean = true
): Promise<User> => {
  // Input validation
  if (!username || !email || !password) {
    throw new ValidationError('Username, email, and password are required');
  }
  
  // Check for duplicates
  if (await userExists(username, email)) {
    throw new DuplicateUserError('Username or email already exists');
  }
  
  // Create user logic
  const hashedPassword = await hashPassword(password);
  const user = new User({
    username,
    email,
    passwordHash: hashedPassword,
    role,
    isActive
  });
  
  // Save to database
  await user.save();
  
  return user;
};
```

## 🔒 Security Standards

### **Input Validation**
- **Always validate** user input at the API boundary
- **Sanitize data** before processing or storing
- **Use type checking** and validation libraries (Pydantic, Joi, Zod)
- **Implement rate limiting** for API endpoints
- **Validate file uploads** for type, size, and content

### **Authentication & Authorization**
- **Never store** plain-text passwords
- **Use secure hashing** algorithms (bcrypt, Argon2)
- **Implement JWT** with proper expiration and refresh
- **Validate tokens** on every protected request
- **Use HTTPS** in production environments

### **Data Protection**
- **Encrypt sensitive data** at rest and in transit
- **Implement CORS** policies appropriately
- **Use environment variables** for secrets and configuration
- **Log security events** for monitoring and auditing
- **Implement proper error handling** without information leakage

## 📊 Testing Standards

### **Test Coverage Requirements**
- **Unit Tests**: Minimum 80% coverage for business logic
- **Integration Tests**: Test component interactions
- **API Tests**: Test all endpoints and error conditions
- **Security Tests**: Test authentication and authorization

### **Test Structure**
```python
# Python test example
def test_create_user_success():
    """Test successful user creation."""
    # Arrange
    user_data = {
        "username": "testuser",
        "email": "test@example.com",
        "password": "securepassword123"
    }
    
    # Act
    user = create_user(**user_data)
    
    # Assert
    assert user.username == user_data["username"]
    assert user.email == user_data["email"]
    assert user.is_active is True
    assert user.role == UserRole.USER

def test_create_user_duplicate_username():
    """Test user creation with duplicate username."""
    # Arrange
    user_data = {
        "username": "existinguser",
        "email": "test@example.com",
        "password": "securepassword123"
    }
    
    # Create first user
    create_user(**user_data)
    
    # Act & Assert
    with pytest.raises(DuplicateUserError):
        create_user(**user_data)
```

```typescript
// TypeScript test example
describe('UserService', () => {
  describe('createUser', () => {
    it('should create a user successfully', async () => {
      // Arrange
      const userData = {
        username: 'testuser',
        email: 'test@example.com',
        password: 'securepassword123'
      };
      
      // Act
      const user = await userService.createUser(userData);
      
      // Assert
      expect(user.username).toBe(userData.username);
      expect(user.email).toBe(userData.email);
      expect(user.isActive).toBe(true);
      expect(user.role).toBe(UserRole.USER);
    });
    
    it('should throw error for duplicate username', async () => {
      // Arrange
      const userData = {
        username: 'existinguser',
        email: 'test@example.com',
        password: 'securepassword123'
      };
      
      // Create first user
      await userService.createUser(userData);
      
      // Act & Assert
      await expect(userService.createUser(userData))
        .rejects.toThrow(DuplicateUserError);
    });
  });
});
```

## 📚 Documentation Standards

### **Code Comments**
- **Explain WHY**, not WHAT (the code should be self-explanatory)
- **Document complex algorithms** and business logic
- **Include examples** for non-obvious usage
- **Update comments** when code changes

### **API Documentation**
- **Use OpenAPI/Swagger** for REST APIs
- **Document all endpoints** with examples
- **Include error responses** and status codes
- **Provide request/response schemas**

### **README Files**
- **Project overview** and purpose
- **Setup instructions** with prerequisites
- **Usage examples** and common patterns
- **API documentation** links
- **Contributing guidelines**

## 🚀 Performance Standards

### **Code Optimization**
- **Profile before optimizing** - measure first
- **Use appropriate data structures** for the task
- **Implement caching** for expensive operations
- **Optimize database queries** with proper indexing
- **Use async/await** for I/O operations

### **Resource Management**
- **Close database connections** properly
- **Implement connection pooling** for databases
- **Use streaming** for large file operations
- **Implement pagination** for large datasets
- **Set appropriate timeouts** for external API calls

## 🔧 Error Handling

### **Error Types**
- **ValidationError**: Invalid input data
- **AuthenticationError**: Invalid credentials
- **AuthorizationError**: Insufficient permissions
- **NotFoundError**: Requested resource not found
- **InternalError**: Unexpected system errors

### **Error Handling Patterns**
```python
# Python error handling
try:
    user = create_user(username, email, password)
    return {"success": True, "user": user}
except ValidationError as e:
    return {"success": False, "error": str(e)}, 400
except DuplicateUserError as e:
    return {"success": False, "error": str(e)}, 409
except Exception as e:
    logger.error(f"Unexpected error creating user: {e}")
    return {"success": False, "error": "Internal server error"}, 500
```

```typescript
// TypeScript error handling
try {
  const user = await userService.createUser(userData);
  return { success: true, user };
} catch (error) {
  if (error instanceof ValidationError) {
    return { success: false, error: error.message, status: 400 };
  }
  if (error instanceof DuplicateUserError) {
    return { success: false, error: error.message, status: 409 };
  }
  
  logger.error('Unexpected error creating user:', error);
  return { success: false, error: 'Internal server error', status: 500 };
}
```

---

*These coding standards ensure consistency, quality, and maintainability across AI App Bootstrap projects. Follow them to create professional, production-ready code.*
