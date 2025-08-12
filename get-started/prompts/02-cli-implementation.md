# Prompt 02: CLI Implementation

## Context
You are an AI development assistant helping a user implement a command-line interface for their project. The user has completed the initial setup and now needs a CLI to manage their application.

## Your Task
Help the user create a robust command-line interface that provides essential project management commands and follows the AI App Bootstrap CLI patterns.

## Prompt Template

```
Great! Now that we have your project structure set up, let's create a powerful command-line interface (CLI) that will make development and deployment much easier.

## What We'll Build
I'll help you create a CLI that provides commands for:
- **Development**: Starting dev servers, running tests, linting code
- **Database**: Migrations, seeding, backup/restore
- **Deployment**: Building, testing, deploying to different environments
- **Project Management**: Creating new components, generating boilerplate code
- **Utilities**: Environment setup, dependency management, health checks

## CLI Framework Options
Based on your project type, I'll recommend the best CLI framework:
- **Python**: Click, Typer, or argparse
- **Node.js**: Commander.js, Yargs, or oclif
- **Go**: Cobra or urfave/cli
- **Rust**: Clap or structopt

## Implementation Approach
1. **Core CLI Structure**: Main command with subcommands
2. **Configuration Management**: Environment-specific settings
3. **Command Handlers**: Business logic for each operation
4. **Error Handling**: Graceful failure and helpful error messages
5. **Help System**: Comprehensive documentation and examples

## Getting Started
Let me know:
- Which CLI framework you prefer (or if you want me to recommend one)
- What specific commands are most important for your workflow
- Any existing CLI patterns you'd like to follow

I'll then create a CLI that integrates seamlessly with your project structure and follows the AI App Bootstrap CLI implementation guide.
```

## Expected User Response
The user should provide:
- CLI framework preference (or ask for recommendation)
- Priority commands for their workflow
- Any existing CLI patterns they want to maintain

## Next Steps
After this prompt, proceed to the architecture planning prompt (03-architecture-planning.md) to design the application architecture.
