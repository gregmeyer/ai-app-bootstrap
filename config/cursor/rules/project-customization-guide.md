# Project Customization Guide

This guide shows you how to transform the generic `.rules-template` into a project-specific `.rules` file for your AI application.

## 🎯 **Why Customize?**

The generic template provides excellent AI App Bootstrap patterns, but your `.rules` file should reflect:
- **Your specific project** and domain
- **Your chosen technology stack**
- **Your AI services** and integration patterns
- **Your deployment environment**
- **Your team's preferences** and conventions

## 🔧 **Customization Process**

### **Step 1: Copy the Template**
```bash
# Copy the template to your project root
cp config/cursor/rules/.rules-template .rules
```

### **Step 2: Identify What to Change**
Look for these sections in the template and customize them:

#### **Project Posture Section**
```bash
# BEFORE (Generic)
- **Purpose**: Building AI-powered applications with rapid development and production readiness
- **Philosophy**: AI-First design, scalable architecture, security by default
- **Stack**: Flexible - Python (FastAPI/Django/Flask), Node.js, Go, or Rust based on project needs

# AFTER (Your Project)
- **Purpose**: Building an AI-powered customer support chatbot for e-commerce
- **Philosophy**: AI-First design, scalable architecture, security by default
- **Stack**: Python with FastAPI, PostgreSQL, Redis, OpenAI GPT-4, deployed on DigitalOcean
```

#### **AI Service Integration Section**
```bash
# BEFORE (Generic)
- **OpenAI**: GPT models, embeddings, fine-tuning with proper error handling
- **Hugging Face**: Transformers, datasets, model hosting
- **LangChain**: LLM orchestration, chains, and memory

# AFTER (Your Project)
- **OpenAI**: GPT-4 for customer support responses, text-embedding-ada-002 for semantic search
- **Custom Integration**: Zendesk API for ticket management, Stripe for payment processing
- **Vector Database**: Pinecone for storing customer conversation embeddings
```

#### **Architecture Section**
```bash
# BEFORE (Generic)
- **Core Pattern**: API → Services → Models → Database
- **Structure**: Clean architecture with separation of concerns, dependency injection, and modular design

# AFTER (Your Project)
- **Core Pattern**: FastAPI → AI Services → Customer Models → PostgreSQL + Pinecone
- **Structure**: 
  ```
  app/
    ├── api/
    │   ├── chat.py          # Chat endpoints
    │   ├── tickets.py       # Support ticket endpoints
    │   └── analytics.py     # Customer analytics
    ├── services/
    │   ├── ai_service.py    # OpenAI integration
    │   ├── chat_service.py  # Chat logic
    │   └── zendesk_service.py # Zendesk integration
    ├── models/
    │   ├── customer.py      # Customer data
    │   ├── conversation.py  # Chat history
    │   └── ticket.py        # Support tickets
    └── utils/
        ├── embeddings.py    # Vector operations
        └── validators.py    # Input validation
  ```
```

### **Step 3: Add Project-Specific Rules**

#### **Domain-Specific Patterns**
```bash
# Add rules specific to your domain
## E-commerce Customer Support Patterns
- **Customer Context**: Always include customer order history and preferences
- **Response Tone**: Professional but friendly, match customer's communication style
- **Escalation Rules**: Automatically escalate complex technical issues to human agents
- **Multi-language Support**: Detect customer language and respond appropriately
- **Integration Points**: Connect with inventory, shipping, and payment systems
```

#### **Technology-Specific Rules**
```bash
# Add rules for your specific tech choices
## FastAPI Specific Patterns
- **Dependencies**: Use FastAPI's dependency injection for services
- **Response Models**: Always define Pydantic response models
- **Background Tasks**: Use FastAPI background tasks for AI processing
- **WebSocket Support**: Implement real-time chat with WebSockets
- **API Versioning**: Use /api/v1/ prefix for all endpoints

## PostgreSQL Specific Patterns
- **Migrations**: Use Alembic for database schema changes
- **Indexing**: Create indexes on customer_id, conversation_date, and embedding vectors
- **Connection Pooling**: Use asyncpg with proper connection limits
- **Full-text Search**: Implement PostgreSQL full-text search for customer queries
```

#### **Deployment-Specific Rules**
```bash
# Add rules for your deployment environment
## DigitalOcean Deployment
- **Environment**: Use DigitalOcean App Platform for auto-scaling
- **Database**: Use DigitalOcean Managed PostgreSQL
- **Redis**: Use DigitalOcean Managed Redis for caching
- **Monitoring**: Use DigitalOcean monitoring and alerting
- **SSL**: Automatic SSL certificates with Let's Encrypt
```

### **Step 4: Remove Irrelevant Sections**

If you're not using certain technologies, remove those sections:

```bash
# REMOVE if not using Go/Rust
## Go/Rust Integration
- **Go**: Gin, Echo, Fiber patterns
- **Rust**: Actix, Axum, Rocket patterns

# REMOVE if not using Hugging Face
## Hugging Face Integration
- **Transformers**: Model hosting and inference
- **Datasets**: Data processing pipelines
```

## 📋 **Complete Customization Checklist**

- [ ] **Project Name & Purpose**: Update with your specific application
- [ ] **Technology Stack**: Specify your exact choices (FastAPI, Django, etc.)
- [ ] **AI Services**: List your specific AI providers and models
- [ ] **Database**: Specify your database choice and patterns
- [ ] **Deployment**: Update with your deployment platform
- [ ] **Domain Logic**: Add rules specific to your business domain
- [ ] **Team Preferences**: Include your team's coding conventions
- [ ] **Integration Points**: List your external service integrations

## 🎨 **Example: Customized .rules File**

Here's how a customized section might look for a customer support chatbot:

```bash
# Customer Support Chatbot - AI App Bootstrap Project

## Project Posture
- **Purpose**: Building an AI-powered customer support chatbot for e-commerce platform
- **Philosophy**: AI-First design, scalable architecture, security by default
- **Stack**: Python with FastAPI, PostgreSQL, Redis, OpenAI GPT-4, Pinecone
- **Deployment**: DigitalOcean App Platform with auto-scaling

## AI Integration Patterns
- **OpenAI GPT-4**: Primary AI model for customer responses
- **Embeddings**: text-embedding-ada-002 for semantic search
- **Context Window**: Maintain last 10 messages for conversation continuity
- **Response Style**: Professional, helpful, and solution-oriented
- **Fallback**: Escalate to human agent after 3 failed AI responses

## Customer Support Specific Rules
- **Customer Context**: Always include order history, preferences, and previous interactions
- **Response Validation**: Ensure responses don't contain sensitive information
- **Escalation Triggers**: Complex technical issues, payment problems, complaints
- **Multi-language**: Support English, Spanish, and French
- **Integration**: Connect with Zendesk, Stripe, and inventory systems
```

## 💡 **Tips for Effective Customization**

1. **Start with the template** - Don't rewrite from scratch
2. **Be specific** - Generic rules are less effective than specific ones
3. **Test your rules** - Try them with Cursor to see if they work
4. **Iterate and improve** - Update rules as you learn what works
5. **Share with your team** - Get feedback on rule clarity and effectiveness
6. **Version control** - Track changes to your .rules file
7. **Document decisions** - Explain why you chose certain patterns

## 🔄 **Maintenance**

- **Review monthly** - Update rules as your project evolves
- **Add new patterns** - Include successful approaches you discover
- **Remove outdated rules** - Keep rules relevant and focused
- **Team feedback** - Gather input on rule effectiveness
- **Performance tracking** - Monitor if rules improve AI assistance quality

---

*Remember: The goal is to make your .rules file so specific that Cursor can generate code that looks like it was written by your team, following your exact patterns and preferences.*
