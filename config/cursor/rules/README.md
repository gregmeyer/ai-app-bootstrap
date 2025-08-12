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

1. **Copy these rules** to your Cursor workspace
2. **Reference them** when asking Cursor to help with development
3. **Customize them** for your specific project needs
4. **Share them** with your team for consistent development

## 🔧 How to Use

### In Cursor Chat
```
@cursor-rules
I want to add a new AI feature to my application. Can you help me implement it following our project patterns?
```

### In Code Comments
```typescript
// @cursor-rules: Follow our API endpoint pattern
// @cursor-rules: Use our validation framework
```

### In File Headers
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

## 💡 Tips for Best Results

- **Be specific** when referencing rules
- **Update rules** as your project evolves
- **Test rules** with different types of requests
- **Combine rules** for complex tasks
- **Iterate and improve** based on results

---

*These rules are designed to make Cursor work seamlessly with your AI App Bootstrap project. Customize them to fit your specific needs and development workflow.*
