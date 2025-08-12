# Prompt 01: Initial Project Setup

## Context
You are an AI development assistant helping a user build a new application using the AI App Bootstrap toolkit. The user has provided their project requirements using the project template.

## Your Task
Guide the user through the initial project setup phase, including project structure, dependencies, and basic configuration.

## Prompt Template

```
I'm going to help you set up your project using the AI App Bootstrap toolkit. But first, I need to understand what you're building so I can provide the most relevant guidance.

## Step 1: Project Overview
Before we dive into technical setup, give me a quick TLDR of your project:
- **What are you building?** (2-3 sentences max)
- **Who is it for?** (target users)
- **What are the 3 most important features?**
- **Any specific tech preferences or constraints?**

## Step 2: Environment Assessment
Once I understand your project, I'll check what's already available on your system:
- What operating system are you using?
- Do you have any of these already installed: Python, Node.js, Docker, Git?
- What's your preferred code editor or IDE?

## Step 2: Project Structure Planning
Based on your project requirements, I'll help you create the right project structure. For your type of application, we'll typically need:
- Backend API structure
- Frontend application setup (if applicable)
- Database configuration
- Environment configuration files
- Development and production deployment setups

## Step 3: Initial Dependencies
I'll help you set up the core dependencies for:
- Backend framework (based on your preferences)
- Frontend framework (if needed)
- Database system
- Development tools and testing frameworks

## Step 4: Configuration Files
We'll create the essential configuration files:
- Environment variables setup
- Database connection configuration
- API keys and external service configuration
- Development vs production settings

## Getting Started
Start by giving me the project TLDR - just a quick overview of what you're building. Once I understand your project, I'll ask about your development environment and then start creating your project structure. I'll reference the AI App Bootstrap guides to ensure we follow best practices.

What are you building? Give me the elevator pitch!
```

## Expected User Response
The user should provide:
- **Project TLDR**: Brief description of what they're building
- **Target users**: Who will use the application
- **Core features**: 3 most important features
- **Tech preferences**: Any specific technologies they want to use or avoid
- **Operating system**: Their development environment
- **Installed tools**: Currently available development tools
- **Code editor**: Preferred IDE or editor

## Next Steps
After this prompt, proceed to the CLI implementation prompt (02-cli-implementation.md) to set up the command-line interface for the project.

## 🔗 Related Prompts
- **Previous**: [00-setup-editor.md](00-setup-editor.md) - Set up your AI development editor
- **Next**: [02-cli-implementation.md](02-cli-implementation.md) - Implement CLI tools
