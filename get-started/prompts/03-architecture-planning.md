# Prompt 03: Architecture Planning

## Context
You are an AI development assistant helping a user design the architecture for their application. The user has completed the CLI setup and now needs to plan the overall system architecture.

## Your Task
Guide the user through designing a scalable, maintainable architecture that follows best practices and integrates with the AI App Bootstrap patterns.

## Prompt Template

```
Excellent! Now let's design the architecture for your [PROJECT_NAME] application. A well-planned architecture will make development faster and ensure your app can scale as it grows.

## Architecture Components We'll Design

### 1. **System Architecture Overview**
- **Monolithic vs Microservices**: Based on your scale and team size
- **Layered Architecture**: Separation of concerns (API, Business Logic, Data Access)
- **Service Boundaries**: Clear interfaces between components

### 2. **Backend Architecture**
- **API Design**: RESTful endpoints, GraphQL, or hybrid approach
- **Authentication & Authorization**: User management and security layers
- **Business Logic Layer**: Core application services and domain models
- **Data Access Layer**: Database interactions and caching strategies

### 3. **Frontend Architecture** (if applicable)
- **Component Structure**: Reusable UI components and state management
- **Routing**: Navigation and page organization
- **State Management**: Global state, local state, and data flow
- **API Integration**: How frontend communicates with backend

### 4. **Data Architecture**
- **Database Design**: Schema design, relationships, and indexing
- **Data Flow**: How data moves through your system
- **Caching Strategy**: Redis, in-memory, or CDN caching
- **Data Validation**: Input validation and data integrity

### 5. **Integration Points**
- **External APIs**: Third-party services and webhooks
- **Message Queues**: Asynchronous processing and event handling
- **File Storage**: Media files, documents, and asset management

## Design Principles
- **Separation of Concerns**: Each component has a single responsibility
- **Loose Coupling**: Components can evolve independently
- **High Cohesion**: Related functionality stays together
- **Scalability**: Architecture can handle growth
- **Maintainability**: Easy to understand and modify

## Getting Started
Let me know:
- What's your expected user load and growth trajectory?
- Do you have any specific architectural patterns you prefer?
- Are there any performance or scalability requirements I should consider?

I'll then create an architecture diagram and implementation plan that follows the AI App Bootstrap architecture planning guide.
```

## Expected User Response
The user should provide:
- Expected user load and growth projections
- Preferred architectural patterns
- Performance and scalability requirements
- Any specific constraints or preferences

## Next Steps
After this prompt, proceed to the environment configuration prompt (04-environment-configuration.md) to set up development and production environments.

