# Project Context Rules

These rules help Cursor understand the AI App Bootstrap project structure, architecture, and development patterns.

## 🏗️ Project Architecture

### **Core Structure**
```
ai-app-bootstrap/
├── get-started/           # Development guides and tutorials
├── examples/              # Project templates and examples
├── config/                # AI provider configurations
│   ├── cursor/           # Cursor-specific rules
│   ├── claude/           # Claude-specific rules
│   ├── gemini/           # Gemini-specific rules
│   └── openai/           # OpenAI-specific rules
└── README.md              # Project overview and documentation
```

### **Development Philosophy**
- **AI-First Design**: Built specifically for AI applications
- **Rapid Development**: Get from idea to production quickly
- **Scalable Architecture**: Start simple, scale as you grow
- **Production Ready**: Includes testing, validation, security
- **AI Agent Compatible**: Works with all major AI development assistants

## 🎯 Technology Stack

### **Backend Options**
- **Python**: FastAPI, Django, Flask (recommended for AI apps)
- **Node.js**: Express, NestJS, Fastify
- **Go**: Gin, Echo, Fiber
- **Rust**: Actix, Axum, Rocket

### **Frontend Options**
- **React**: Next.js, Create React App
- **Vue**: Nuxt.js, Vue CLI
- **Angular**: Angular CLI
- **Svelte**: SvelteKit

### **Database Options**
- **SQL**: PostgreSQL, MySQL, SQLite
- **NoSQL**: MongoDB, Redis, Cassandra
- **Vector**: Pinecone, Weaviate, Qdrant (for AI features)

### **AI/ML Integration**
- **OpenAI**: GPT models, embeddings, fine-tuning
- **Hugging Face**: Transformers, datasets, model hosting
- **LangChain**: LLM orchestration and chains
- **Custom Models**: TensorFlow, PyTorch, scikit-learn

## 📚 Development Flow

### **1. Project Setup**
- Use project template to define requirements
- Set up development environment
- Initialize project structure
- Configure dependencies

### **2. CLI Implementation**
- Create command-line interface
- Implement project management commands
- Set up development workflows
- Add utility functions

### **3. Architecture Planning**
- Design system architecture
- Plan data models and relationships
- Define API structure
- Consider scalability requirements

### **4. Environment Configuration**
- Set up environment variables
- Configure secrets management
- Set up different environments (dev, staging, prod)
- Implement security best practices

### **5. Backend Foundation**
- Implement core services
- Set up database connections
- Create API endpoints
- Add authentication and authorization

### **6. Validation Framework**
- Implement data validation
- Add input sanitization
- Set up error handling
- Include security measures

## 🔧 Code Organization Patterns

### **Backend Structure**
```
backend/
├── app/
│   ├── api/              # API endpoints
│   ├── core/             # Core business logic
│   ├── models/           # Data models
│   ├── services/         # Business services
│   └── utils/            # Utility functions
├── tests/                # Test files
├── config/               # Configuration files
└── requirements.txt      # Dependencies
```

### **Frontend Structure**
```
frontend/
├── src/
│   ├── components/       # Reusable components
│   ├── pages/            # Page components
│   ├── services/         # API services
│   ├── hooks/            # Custom React hooks
│   └── utils/            # Utility functions
├── public/               # Static assets
└── package.json          # Dependencies
```

### **Shared Patterns**
- **Clean Architecture**: Separation of concerns
- **Dependency Injection**: Loose coupling between components
- **Repository Pattern**: Data access abstraction
- **Service Layer**: Business logic encapsulation
- **DTO Pattern**: Data transfer objects for APIs

## 🚀 Deployment Targets

### **Cloud Platforms**
- **DigitalOcean**: App Platform, Droplets, Managed Databases
- **AWS**: EC2, Lambda, RDS, S3
- **Google Cloud**: Compute Engine, Cloud Run, Cloud SQL
- **Azure**: Virtual Machines, App Service, Azure SQL
- **Heroku**: Platform as a Service
- **Railway**: Modern deployment platform
- **Render**: Simple cloud platform

### **Deployment Considerations**
- **Environment Variables**: Secure configuration management
- **Database Setup**: Production database configuration
- **SSL/TLS**: HTTPS enforcement
- **Monitoring**: Health checks and logging
- **Scaling**: Auto-scaling and load balancing

## 🔒 Security Patterns

### **Authentication & Authorization**
- **JWT Tokens**: Stateless authentication
- **OAuth 2.0**: Third-party authentication
- **Role-Based Access Control**: Permission management
- **Multi-Factor Authentication**: Enhanced security

### **Data Protection**
- **Input Validation**: Prevent injection attacks
- **Rate Limiting**: Prevent abuse
- **CORS Configuration**: Cross-origin resource sharing
- **Data Encryption**: At rest and in transit

## 📊 Testing Strategy

### **Test Types**
- **Unit Tests**: Individual component testing
- **Integration Tests**: Component interaction testing
- **End-to-End Tests**: Full user journey testing
- **Security Tests**: Vulnerability assessment

### **Testing Tools**
- **Python**: pytest, unittest, coverage
- **Node.js**: Jest, Mocha, Chai
- **Frontend**: React Testing Library, Cypress
- **API**: Postman, Insomnia, pytest-asyncio

## 💡 Best Practices

### **Code Quality**
- **Type Safety**: Use type hints and interfaces
- **Documentation**: Clear docstrings and comments
- **Error Handling**: Graceful failure and logging
- **Performance**: Optimize critical paths

### **AI Integration**
- **Prompt Engineering**: Clear, specific prompts
- **Context Management**: Maintain conversation context
- **Fallback Handling**: Graceful degradation
- **Rate Limiting**: Respect API limits

---

*These rules provide the foundation for understanding and working with AI App Bootstrap projects. Reference them when asking Cursor to help with development tasks.*
