# Cursor Rules Configuration

This directory contains rules and configurations specifically designed for Cursor, the AI-powered code editor. These rules help Cursor understand your project structure and provide better AI assistance.

## 🎯 What These Rules Do

- **Project Understanding**: Help Cursor understand your AI App Bootstrap project structure
- **Code Generation**: Guide Cursor to generate code that follows your patterns
- **File Organization**: Ensure new files are created in the right locations
- **Best Practices**: Enforce coding standards and architectural patterns
- **AI Integration**: Optimize how Cursor's AI features work with your toolkit

## 📁 File Structure

```
config/cursor/rules/
├── README.md                 # This file
├── project-context.md        # Project overview and context
├── coding-standards.md       # Code style and standards
├── file-templates.md         # Templates for new files
├── ai-prompting.md          # How to prompt Cursor effectively
└── examples/                 # Example configurations and usage
    ├── new-feature.md        # Example: Adding a new feature
    ├── api-endpoint.md       # Example: Creating an API endpoint
    └── database-model.md     # Example: Adding a database model
```

## 🚀 Quick Start

1. **Use our .rules template** - Copy `config/cursor/rules/.rules-template` to your project root
2. **Customize the template** for your specific AI service and technology stack
3. **Reference rules** when asking Cursor to help with development
4. **Share rules** with your team for consistent development
5. **Update rules** as your project evolves and new patterns emerge

## 🔧 How to Use

### **1. Install Rules in Cursor**

#### **Option A: Create .rules file in your project root**
Create a `.rules` file in your project root directory:

```bash
# In your project root
touch .rules
```

Then copy the relevant rules from this configuration into your `.rules` file. Cursor will automatically read and apply these rules.

#### **Option B: Use Cursor's Rules Panel**
1. Open Cursor
2. Go to **Settings** → **AI** → **Rules**
3. Add your custom rules directly in the rules panel

#### **Option C: Project-specific .rules file**
For team projects, commit the `.rules` file to version control so all team members use the same rules.

### **2. Reference Rules in Cursor Chat**
```
@cursor-rules
I want to add a new AI feature to my application. Can you help me implement it following our project patterns?
```

### **3. Use Rules in Code Comments**
```typescript
// @cursor-rules: Follow our API endpoint pattern
// @cursor-rules: Use our validation framework
```

### **4. Apply Rules in File Headers**
```typescript
/**
 * @cursor-rules: Follow our service layer pattern
 * @cursor-rules: Include proper error handling
 */
```

## 📋 Rule Categories

### 1. **Project Context Rules**
- Project structure and organization
- Technology stack preferences
- Architecture patterns to follow
- Dependencies and constraints

### 2. **Coding Standards**
- Code style and formatting
- Naming conventions
- Documentation requirements
- Testing patterns

### 3. **File Templates**
- Standard file structures
- Import/export patterns
- Component organization
- API endpoint patterns

### 4. **AI Integration Rules**
- How to prompt effectively
- Context management
- Code generation preferences
- Quality assurance steps

## 🎨 Customization

These rules are designed to be customized for your specific project:

1. **Copy the base rules** to your project
2. **Modify them** to match your preferences
3. **Add project-specific rules** as needed
4. **Test and refine** based on results
5. **Share with your team** for consistency

## 🔗 Related Documentation

- [AI App Bootstrap Main Guide](../../../get-started/README.md)
- [Project Template](../../../examples/project-template.md)
- [AI Agent Prompts](../../../get-started/prompts/)

## 📋 .rules File Format

### **What is a .rules file?**
A `.rules` file is a plain text file that Cursor reads to understand your project's coding standards, architecture patterns, and development preferences. It's placed in your project root and automatically applied to all AI interactions.

### **Ready-to-Use Template**
We provide a comprehensive `.rules-template` file that you can copy directly to your project:

```bash
# Copy the template to your project root
cp config/cursor/rules/.rules-template .rules

# Or copy the content and create your own
cat config/cursor/rules/.rules-template > .rules
```

The template includes:
- **AI App Bootstrap patterns** and architecture
- **Security requirements** and best practices
- **Testing strategies** for AI applications
- **Performance guidelines** and scalability patterns
- **Code examples** and integration patterns

### **Basic .rules File Example**
```bash
# AI App Bootstrap Project Rules

## Project Structure
- Follow clean architecture principles
- Separate concerns: API, services, models, utilities
- Use snake_case for Python files and directories
- Use kebab-case for JavaScript/TypeScript files

## Coding Standards
- Always include type hints in Python
- Use comprehensive docstrings for functions and classes
- Follow PEP 8 for Python code formatting
- Implement proper error handling with try/catch blocks

## AI Integration Patterns
- Use Pydantic for data validation
- Implement JWT authentication for protected endpoints
- Follow RESTful API design principles
- Include comprehensive testing for all new features

## Security Requirements
- Never store plain-text passwords
- Validate all user input
- Use environment variables for secrets
- Implement rate limiting for API endpoints
```

### **Advanced .rules File Example**
```bash
# AI App Bootstrap - Advanced Rules

## Architecture Patterns
- Use dependency injection for services
- Implement repository pattern for data access
- Follow CQRS for complex operations
- Use event-driven architecture for async processing

## Database Patterns
- Use migrations for schema changes
- Implement soft deletes for data integrity
- Use connection pooling for database connections
- Implement proper indexing strategies

## Testing Requirements
- Minimum 80% test coverage for business logic
- Use pytest for Python testing
- Mock external dependencies in tests
- Test both success and failure scenarios

## Performance Guidelines
- Implement caching for expensive operations
- Use async/await for I/O operations
- Optimize database queries with proper indexing
- Implement pagination for large datasets
```

### **How Cursor Uses .rules Files**
1. **Automatic Application**: Rules are applied to all AI interactions
2. **Context Awareness**: AI understands your project structure and patterns
3. **Code Generation**: Generated code follows your established standards
4. **Suggestions**: AI provides suggestions that align with your rules
5. **Refactoring**: AI helps refactor code according to your patterns

## 💡 Tips for Best Results

- **Be specific** when referencing rules
- **Update rules** as your project evolves
- **Test rules** with different types of requests
- **Combine rules** for complex tasks
- **Iterate and improve** based on results
- **Keep rules concise** and focused on key patterns
- **Use clear language** that AI can easily understand

---

*These rules are designed to make Cursor work seamlessly with your AI App Bootstrap project. Customize them to fit your specific needs and development workflow.*
