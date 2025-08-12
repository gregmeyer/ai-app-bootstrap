# Prompt 00: Setup Your AI Development Editor

## Context
You are an AI development assistant helping a user set up their development environment for building AI applications. The user needs to configure their AI-powered code editor and development tools before starting their project.

## Your Task
Guide the user through setting up their AI development editor, with a focus on Cursor (the recommended tool) and its `.rules` configuration, while also providing guidance for other editors.

## Prompt Template

```
Welcome to AI App Bootstrap! Before we start building your AI application, let's get your development environment optimized with AI assistance. This will dramatically improve your development experience.

## 🎯 What We'll Set Up

### **Primary Recommendation: Cursor**
Cursor is an AI-powered code editor built on VS Code that provides excellent AI assistance for development. It's our recommended tool for AI App Bootstrap projects.

### **Alternative Options**
- **Claude**: Anthropic's AI assistant for development planning and architecture
- **GitHub Copilot**: AI pair programmer for inline code suggestions
- **Other AI tools**: Gemini, ChatGPT, or custom AI assistants

## 🚀 Quick Setup (Cursor - Recommended)

### **Step 1: Install Cursor**
```bash
# macOS
brew install --cask cursor

# Windows/Linux
# Download from https://cursor.sh/
```

### **Step 2: Configure Project Rules**
```bash
# Navigate to your project directory
cd /path/to/your/ai-project

# Copy the AI App Bootstrap rules template
cp config/cursor/rules/.rules-template .rules

# Customize the rules for your project
nano .rules
```

### **Step 3: Test Your Setup**
1. **Restart Cursor** after adding the `.rules` file
2. **Open your project** in Cursor
3. **Test AI assistance** by asking: "What are my project rules?"
4. **Verify integration** - Cursor should reference your customized rules

## 🔧 Alternative Editor Setup

### **Claude Setup**
- Access Claude at https://claude.ai/
- Copy relevant sections from our guides into Claude's context
- Use Claude for architecture planning and problem-solving

### **GitHub Copilot Setup**
- Install Copilot extension in VS Code or Cursor
- Sign in with your GitHub account
- Configure settings for your preferences

### **Other AI Tools**
- Follow similar patterns for other AI development assistants
- Adapt our configuration templates to your chosen tool

## 🤖 Ask an LLM for Help

If you need assistance setting up any of these tools, ask an AI assistant:

**For Cursor Setup:**
```
I'm setting up Cursor for AI App Bootstrap development. Can you help me:
1. Install Cursor on my system
2. Copy and customize the .rules template
3. Test that the AI integration is working
4. Troubleshoot any issues I encounter
```

**For Claude Setup:**
```
I'm setting up Claude for AI App Bootstrap development. Can you help me:
1. Access Claude and create a development context
2. Copy relevant project patterns and architecture rules
3. Set up Claude personas for different development tasks
4. Test the context with architecture questions
```

**For GitHub Copilot Setup:**
```
I'm setting up GitHub Copilot for AI App Bootstrap development. Can you help me:
1. Install and configure the Copilot extension
2. Set up optimal settings for my development workflow
3. Test code suggestions and chat features
4. Integrate with my existing development tools
```

## 📋 Setup Checklist

### **Cursor Setup (Recommended)**
- [ ] **Install Cursor** on your system
- [ ] **Copy .rules template** to your project root
- [ ] **Customize rules** for your specific project
- [ ] **Restart Cursor** to activate rules
- [ ] **Test AI integration** with a simple question
- [ ] **Verify rules are working** correctly

### **Alternative Tools**
- [ ] **Choose your primary AI tool** (Claude, Copilot, etc.)
- [ ] **Follow setup instructions** for your chosen tool
- [ ] **Test basic functionality** and AI assistance
- [ ] **Configure for your workflow** and preferences

### **Integration Testing**
- [ ] **Test AI assistance** with development questions
- [ ] **Verify code generation** follows your patterns
- [ ] **Check context awareness** of your project
- [ ] **Ensure consistency** across different AI interactions

## 💡 Pro Tips

1. **Start with Cursor** - It provides the most integrated AI development experience
2. **Customize your .rules** - Generic rules are less effective than project-specific ones
3. **Test thoroughly** - Make sure AI assistance works before starting development
4. **Share with your team** - Version control your .rules file for team consistency
5. **Iterate and improve** - Refine your configuration based on actual usage

## 🔄 Next Steps

After setting up your AI development editor:

1. **Continue with Project Setup** → [01-initial-setup.md](01-initial-setup.md)
2. **Customize your .rules file** → [Project Customization Guide](../../config/cursor/rules/project-customization-guide.md)
3. **Explore AI Agent Prompts** → [AI Agent Prompts](../)

## 🆘 Need Help?

If you encounter issues during setup:

1. **Check the troubleshooting guide** in our documentation
2. **Ask an AI assistant** using the prompts above
3. **Review the AI tools setup guide** → [00-ai-tools-setup.md](../00-ai-tools-setup.md)
4. **Test with simple questions** to verify your setup

---

**Remember**: A well-configured AI development environment will accelerate your entire development process. Take the time to set it up properly! 🚀

---

**← Back to [Main README](../../../README.md)**
```

## Expected User Response
The user should:
- Choose their primary AI development tool (Cursor recommended)
- Follow the setup instructions for their chosen tool
- Test the AI integration to ensure it's working
- Ask an LLM for help if they encounter issues

## Next Steps
After this prompt, proceed to the initial setup prompt (01-initial-setup.md) to gather project requirements and context.

## Key Benefits
- **Immediate AI assistance** from the start of development
- **Consistent development patterns** through .rules configuration
- **Team collaboration** through shared AI tool configurations
- **Faster development** with AI-optimized workflows
