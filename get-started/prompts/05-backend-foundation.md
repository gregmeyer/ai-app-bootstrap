# Prompt 05: Backend Foundation

## Context
You are an AI development assistant helping a user build the backend foundation for their application. The user has completed environment configuration and now needs to implement the core backend services.

## Your Task
Guide the user through building a robust backend foundation with proper structure, database setup, API endpoints, and core business logic following the AI App Bootstrap patterns.

## Prompt Template

```
Excellent! Now let's build the solid foundation for your [PROJECT_NAME] backend. This is where we'll implement the core services, database models, and API endpoints that will power your application.

## What We'll Build

### 1. **Project Structure & Organization**
- **Clean Architecture**: Separation of concerns and dependency injection
- **Module Organization**: Logical grouping of related functionality
- **Import/Export Patterns**: Clean module interfaces
- **Testing Structure**: Unit and integration test organization

### 2. **Database Foundation**
- **Database Connection**: Connection pooling and management
- **ORM/ODM Setup**: Database abstraction layer
- **Migration System**: Schema versioning and updates
- **Seed Data**: Initial data for development and testing

### 3. **Core API Structure**
- **Route Organization**: RESTful endpoint structure
- **Middleware Stack**: Authentication, validation, logging, CORS
- **Error Handling**: Consistent error responses and logging
- **Request/Response Models**: Data validation and serialization

### 4. **Business Logic Layer**
- **Service Classes**: Core application business logic
- **Domain Models**: Business entities and relationships
- **Validation Rules**: Input validation and business rule enforcement
- **Transaction Management**: Data consistency and rollback handling

### 5. **Authentication & Security**
- **User Management**: Registration, login, and profile management
- **JWT Implementation**: Token-based authentication
- **Authorization**: Role-based access control
- **Security Middleware**: Rate limiting, input sanitization, CORS

## Implementation Approach
1. **Set Up Project Structure**: Create the foundational directory layout
2. **Configure Database**: Set up connections and basic models
3. **Implement Core Services**: Build the essential business logic
4. **Create API Endpoints**: RESTful routes with proper validation
5. **Add Authentication**: User management and security
6. **Write Tests**: Ensure everything works correctly

## Getting Started
Let me know:
- What's your primary database choice (PostgreSQL, MongoDB, etc.)?
- Do you need user authentication from the start?
- What are the core entities/models for your application?
- Any specific API patterns or conventions you want to follow?

I'll then create a robust backend foundation that follows the AI App Bootstrap backend foundation guide and integrates seamlessly with your architecture.
```

## Expected User Response
The user should provide:
- Primary database choice and preferences
- Authentication requirements
- Core application entities/models
- API patterns and conventions preferences

## Next Steps
After this prompt, proceed to the validation framework prompt (06-validation-framework.md) to implement robust data validation.

