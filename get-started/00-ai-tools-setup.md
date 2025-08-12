# 00: AI Tools Setup

> **Setup your AI development environment before starting your project**

This guide helps you configure your AI development tools (Cursor, Claude, GitHub Copilot, etc.) to work optimally with AI App Bootstrap projects. Setting these up first will make your development experience much smoother.

## 🎯 What You'll Set Up

- **Cursor**: AI-powered code editor with project-specific rules
- **Claude**: Anthropic's AI assistant for development
- **GitHub Copilot**: AI pair programmer for GitHub
- **Other AI tools**: Gemini, OpenAI, etc.

## 🚀 Quick Start

### **1. Choose Your Primary AI Tool**

Most developers choose one primary tool and use others as needed:

- **Cursor**: Best for full-stack development with built-in AI
- **Claude**: Excellent for planning, architecture, and complex problem-solving
- **GitHub Copilot**: Great for inline code suggestions and pair programming
- **ChatGPT**: Good for general development questions and brainstorming

### **2. Set Up Cursor (Recommended Primary Tool)**

#### **Install Cursor**
```bash
# macOS
brew install --cask cursor

# Windows
# Download from https://cursor.sh/

# Linux
# Download from https://cursor.sh/
```

#### **Configure Project Rules**
```bash
# Navigate to your project directory
cd /path/to/your/ai-project

# Copy the AI App Bootstrap rules template
cp config/cursor/rules/.rules-template .rules

# Customize the rules for your project (see customization guide)
nano .rules
```

#### **Verify Setup**
1. **Restart Cursor** after adding the `.rules` file
2. **Open your project** in Cursor
3. **Test the rules** by asking: "What are my project rules?"
4. **Cursor should reference** your customized rules

### **3. Set Up Claude (Alternative Primary Tool)**

#### **Access Claude**
- **Claude Pro**: https://claude.ai/
- **Claude Desktop**: Download the desktop app
- **Slack Integration**: Use Claude in Slack for quick questions

#### **Configure Claude Context**
Copy relevant sections from our guides into Claude's context:

```
I'm building an AI application using the AI App Bootstrap toolkit. 
Please follow these patterns:

[Copy relevant sections from our guides]
- Project structure: API → Services → Models → Database
- Security: JWT authentication, input validation, rate limiting
- Testing: 80%+ coverage, mock AI services, comprehensive testing
- AI Integration: OpenAI, proper error handling, fallback strategies
```

### **4. Set Up GitHub Copilot**

#### **Install GitHub Copilot**
```bash
# In VS Code or Cursor
# Install GitHub Copilot extension
# Sign in with your GitHub account
```

#### **Configure Copilot Settings**
```json
// In your VS Code/Cursor settings
{
  "github.copilot.enable": {
    "*": true,
    "plaintext": false,
    "markdown": false,
    "scss": false,
    "json": false
  },
  "github.copilot.suggestions.enabled": true,
  "github.copilot.autoComplete.enabled": true
}
```

## 🔧 Detailed Setup Instructions

### **Cursor Rules Configuration**

#### **What Are .rules Files?**
- **Plain text files** that Cursor reads automatically
- **Define coding standards** and project patterns
- **Applied to all AI interactions** in your project
- **Version controlled** with your project

#### **Where to Place .rules Files**
```bash
# Project-specific (recommended)
your-project/
├── .rules                    # ← Cursor auto-detects this
└── ...other files

# Global (optional)
~/.cursor-rules              # ← Cursor uses for all projects
```

#### **Customizing Your .rules File**
1. **Copy the template**: `cp config/cursor/rules/.rules-template .rules`
2. **Edit the file**: Update with your specific project details
3. **Test the rules**: Ask Cursor about your project patterns
4. **Iterate and improve**: Refine rules based on results

#### **Example Customization**
```bash
# BEFORE (Generic Template)
- **Purpose**: Building AI-powered applications with rapid development and production readiness
- **Stack**: Flexible - Python (FastAPI/Django/Flask), Node.js, Go, or Rust based on project needs

# AFTER (Your Project)
- **Purpose**: Building an AI-powered customer support chatbot for e-commerce platform
- **Stack**: Python with FastAPI, PostgreSQL, Redis, OpenAI GPT-4, Pinecone
- **Deployment**: DigitalOcean App Platform with auto-scaling
```

### **Claude Context Management**

#### **Creating Claude Personas**
For different types of development tasks:

```
# Architecture Planning Persona
You are a senior software architect specializing in AI applications. 
Help me design scalable, secure architecture for my AI-powered application.

# Code Review Persona
You are a senior Python developer focused on code quality and security.
Review this code following our project standards and suggest improvements.

# Testing Strategy Persona
You are a QA engineer specializing in AI application testing.
Help me create comprehensive test strategies for AI features.
```

#### **Maintaining Context**
- **Save important conversations** for future reference
- **Create context files** with your project patterns
- **Use consistent terminology** across conversations
- **Reference previous decisions** when making new ones

### **GitHub Copilot Integration**

#### **Best Practices**
- **Review suggestions** before accepting
- **Use consistent naming** conventions
- **Provide clear context** in comments
- **Test generated code** thoroughly

#### **Configuration Tips**
```json
// Optimize for your workflow
{
  "github.copilot.chat.enabled": true,
  "github.copilot.terminal.enabled": true,
  "github.copilot.enableAutoCompletions": true
}
```

## 📋 Setup Checklist

### **Cursor Setup**
- [ ] **Install Cursor** on your system
- [ ] **Copy .rules template** to your project root
- [ ] **Customize rules** for your specific project
- [ ] **Test rules** with Cursor chat
- [ ] **Verify integration** with your project

### **Claude Setup**
- [ ] **Access Claude** (web, desktop, or Slack)
- [ ] **Create context** with your project patterns
- [ ] **Test context** with development questions
- [ ] **Save useful conversations** for reference

### **GitHub Copilot Setup**
- [ ] **Install extension** in your editor
- [ ] **Sign in** with GitHub account
- [ ] **Configure settings** for your preferences
- [ ] **Test suggestions** with your code

### **Integration Testing**
- [ ] **Test Cursor rules** with AI chat
- [ ] **Test Claude context** with architecture questions
- [ ] **Test Copilot** with code generation
- [ ] **Verify consistency** across all tools

## 🎨 Advanced Configuration

### **Multi-Tool Workflow**
```bash
# Use different tools for different tasks
Cursor     → Code generation and editing
Claude     → Architecture planning and problem-solving
Copilot    → Inline code suggestions and pair programming
ChatGPT    → General questions and brainstorming
```

### **Context Sharing Between Tools**
- **Export Cursor rules** to share with Claude
- **Use consistent terminology** across all tools
- **Maintain decision logs** that all tools can reference
- **Create shared patterns** that work across platforms

### **Team Configuration**
```bash
# Share .rules files with your team
git add .rules
git commit -m "Add Cursor rules for AI App Bootstrap project"
git push

# Team members can then:
git pull
# Cursor automatically uses the shared rules
```

## 🚨 Common Issues & Solutions

### **Cursor Rules Not Working**
```bash
# Check file location
ls -la .rules

# Verify file permissions
chmod 644 .rules

# Restart Cursor completely
# Check Cursor logs for errors
```

### **Claude Context Lost**
- **Save important conversations** to files
- **Create context templates** for reuse
- **Use consistent prompts** across sessions
- **Reference previous decisions** explicitly

### **Copilot Suggestions Poor**
- **Improve code comments** for better context
- **Use consistent naming** conventions
- **Provide clear function signatures**
- **Test and refine** generated code

## 💡 Pro Tips

1. **Start with Cursor rules** - They provide the foundation for all other tools
2. **Customize gradually** - Don't try to perfect everything at once
3. **Test with real tasks** - Use your tools on actual development work
4. **Iterate and improve** - Refine your setup based on results
5. **Share with your team** - Consistent tools improve collaboration
6. **Document your setup** - Help others replicate your configuration

## 🔗 Next Steps

After setting up your AI tools:

1. **Continue to Project Setup** → [01-project-setup.md](01-project-setup.md)
2. **Customize your .rules file** → [Project Customization Guide](../config/cursor/rules/project-customization-guide.md)
3. **Explore AI Agent Prompts** → [AI Agent Prompts](prompts/)

---

**Remember**: Well-configured AI tools can dramatically accelerate your development. Take the time to set them up properly, and you'll reap the benefits throughout your project! 🚀

---

**← Back to [Main README](../../README.md)**
