# 02: CLI Implementation

## Overview
This guide covers implementing the development CLI that manages your development workflow, tracks progress, and maintains context.

## 🚀 CLI Setup

### 2.1 Install Dependencies
```bash
# Ensure virtual environment is activated
source venv/bin/activate

# Install required packages
pip install click python-dateutil
```

### 2.2 Create CLI Package Structure
```bash
# Create __init__.py for CLI package
echo "# CLI package for development tools" > cli/__init__.py
```

### 2.3 Implement Core CLI

**Create `cli/cli.py` with complete implementation:**

```python
#!/usr/bin/env python3
"""
Development CLI for project workflow management.
"""

import click
from datetime import datetime
from pathlib import Path
from typing import Optional


class DevContext:
    """Manages development context and file operations."""
    
    def __init__(self, base_path: Path):
        self.base_path = base_path
        self.agent_context_path = base_path / "agentContext"
        self.architecture_path = self.agent_context_path / "architecture"
        self.decisions_path = self.agent_context_path / "decisions"
        self.notes_path = self.agent_context_path / "notes"
        
        # Ensure directories exist
        for path in [self.architecture_path, self.decisions_path, self.notes_path]:
            path.mkdir(parents=True, exist_ok=True)
    
    def get_today_filename(self, prefix: str) -> str:
        """Generate filename with today's date."""
        today = datetime.now().strftime("%d%m%Y")
        return f"{today}_{prefix}.md"
    
    def get_or_create_file(self, file_path: Path, title: str) -> Path:
        """Get existing file or create with header if new."""
        if not file_path.exists():
            file_path.write_text(f"# {title} - {datetime.now().strftime('%B %d, %Y')}\n\n")
        return file_path
    
    def append_to_file(self, file_path: Path, content: str):
        """Append content to file with timestamp."""
        timestamp = datetime.now().strftime("%H:%M:%S")
        with open(file_path, 'a') as f:
            f.write(f"\n## {timestamp}\n{content}\n")


@click.group()
@click.pass_context
def cli(ctx):
    """Development CLI for project workflow management"""
    ctx.ensure_object(dict)
    base_path = Path.cwd()
    ctx.obj['context'] = DevContext(base_path)


@cli.command()
@click.pass_context
def start_session(ctx):
    """Start or update today's development session."""
    context = ctx.obj['context']
    notes_file = context.notes_path / context.get_today_filename("note")
    
    if not notes_file.exists():
        click.echo(f"✨ Starting new development session: {notes_file.name}")
        context.get_or_create_file(notes_file, "Development Session")
        context.append_to_file(notes_file, "Session started. Ready to pick tickets and log progress.")
    else:
        click.echo(f"📝 Updating existing session: {notes_file.name}")
        context.append_to_file(notes_file, "Session resumed.")
    
    click.echo(f"📁 Session file: {notes_file}")


@cli.command()
@click.argument('ticket_id')
@click.pass_context
def pick_ticket(ctx, ticket_id):
    """Set active ticket for current session."""
    context = ctx.obj['context']
    notes_file = context.notes_path / context.get_today_filename("note")
    
    context.get_or_create_file(notes_file, "Development Session")
    context.append_to_file(notes_file, f"🎯 **Active Ticket:** {ticket_id}")
    click.echo(f"✅ Picked ticket {ticket_id} - logged to {notes_file.name}")


@cli.command()
@click.option('--type', 'context_type', 
              type=click.Choice(['architecture', 'decision', 'note']), 
              required=True, help='Type of context to update')
@click.option('--msg', 'message', required=True, help='Message to log')
@click.pass_context
def update_context(ctx, context_type, message):
    """Update context with new information."""
    context = ctx.obj['context']
    
    if context_type == 'architecture':
        file_path = context.architecture_path / context.get_today_filename("architecture")
        title = "Architecture Decision"
    elif context_type == 'decision':
        file_path = context.decisions_path / context.get_today_filename("decision")
        title = "Development Decision"
    else:  # note
        file_path = context.notes_path / context.get_today_filename("note")
        title = "Development Note"
    
    context.get_or_create_file(file_path, title)
    context.append_to_file(file_path, message)
    click.echo(f"📝 Updated {context_type}: {file_path.name}")


@cli.command()
@click.argument('ticket_id')
@click.option('--summary', required=True, help='Summary of what was accomplished')
@click.pass_context
def finish_ticket(ctx, ticket_id, summary):
    """Mark ticket as complete with summary."""
    context = ctx.obj['context']
    notes_file = context.notes_path / context.get_today_filename("note")
    
    context.get_or_create_file(notes_file, "Development Session")
    completion_msg = f"✅ **Completed Ticket:** {ticket_id}\n\n**Summary:** {summary}"
    context.append_to_file(notes_file, completion_msg)
    
    # If it's architectural work, also log to decisions
    if any(word in summary.lower() for word in ['architecture', 'design', 'structure', 'system']):
        decisions_file = context.decisions_path / context.get_today_filename("decision")
        context.get_or_create_file(decisions_file, "Development Decision")
        context.append_to_file(decisions_file, f"**Ticket {ticket_id} completed:** {summary}")
        click.echo(f"📋 Also logged to decisions: {decisions_file.name}")
    
    click.echo(f"🎉 Ticket {ticket_id} completed and logged to {notes_file.name}")


@cli.command()
@click.pass_context
def status(ctx):
    """Show current development status."""
    context = ctx.obj['context']
    today = datetime.now().strftime("%d%m%Y")
    
    click.echo("🔍 Development Status")
    click.echo("=" * 50)
    
    notes_file = context.notes_path / f"{today}_note.md"
    if notes_file.exists():
        click.echo(f"📝 Session: {notes_file.name}")
        with open(notes_file, 'r') as f:
            lines = f.readlines()
            if len(lines) > 5:
                click.echo("Recent activity:")
                for line in lines[-5:]:
                    if line.strip():
                        click.echo(f"  {line.strip()}")
    else:
        click.echo("📝 No session started today. Run 'dev start-session' to begin.")


if __name__ == '__main__':
    cli()
```

## 🔧 CLI Commands

### Available Commands

| Command | Description | Usage |
|---------|-------------|-------|
| `start-session` | Begin/resume daily development session | `python cli/cli.py start-session` |
| `pick-ticket <id>` | Set active ticket for tracking | `python cli/cli.py pick-ticket T-001` |
| `update-context` | Log context updates | `python cli/cli.py update-context --type note --msg "message"` |
| `finish-ticket` | Complete tickets with summaries | `python cli/cli.py finish-ticket T-001 --summary "summary"` |
| `status` | Show current development status | `python cli/cli.py status` |

### Command Options

#### `update-context` Types
- **`architecture`** - System design and architectural decisions
- **`decision`** - Development choices and trade-offs
- **`note`** - General development notes and progress

## 📁 File Management

### File Naming Convention
- **Format:** `ddmmyyyy_<type>.md`
- **Example:** `11082025_note.md`
- **Purpose:** Ensures chronological organization

### File Structure
```
agentContext/
├── architecture/
│   └── ddmmyyyy_architecture.md    # Architecture decisions
├── decisions/
│   └── ddmmyyyy_decision.md        # Development decisions
└── notes/
    └── ddmmyyyy_note.md             # Daily development notes
```

### File Content Format
Each file follows this structure:
```markdown
# [Title] - [Date]

## [Timestamp]
[Content]

## [Timestamp]
[More content]
```

## 🧪 Testing the CLI

### 2.4 Verify CLI Functionality
```bash
# Test help
python cli/cli.py --help

# Test each command
python cli/cli.py start-session
python cli/cli.py status
python cli/cli.py pick-ticket TEST-001
python cli/cli.py update-context --type note --msg "Test message"
python cli/cli.py finish-ticket TEST-001 --summary "Test completion"
```

### 2.5 Verify File Creation
```bash
# Check that files were created
ls -la agentContext/notes/
ls -la agentContext/decisions/
ls -la agentContext/architecture/

# Check file content
cat agentContext/notes/$(date +%d%m%Y)_note.md
```

## 🔧 Advanced CLI Features

### 2.6 Add Additional Commands (Optional)

You can extend the CLI with additional commands:

```python
@cli.command()
@click.pass_context
def tickets(ctx):
    """Show all available project tickets."""
    # Implementation for listing tickets
    pass

@cli.command()
@click.pass_context
def stories(ctx):
    """Show user stories and their mapping to tickets."""
    # Implementation for listing stories
    pass

@cli.command()
@click.pass_context
def context(ctx):
    """Show recent context updates."""
    # Implementation for showing recent context
    pass
```

### 2.7 Error Handling

The CLI includes basic error handling:
- Directory creation if missing
- File creation if missing
- Graceful handling of file operations

## ✅ Verification Checklist

- [ ] Dependencies installed (click, python-dateutil)
- [ ] CLI package structure created
- [ ] Core CLI implementation complete
- [ ] All commands working
- [ ] File creation verified
- [ ] File content format correct
- [ ] Error handling tested

## 🚀 Next Steps

After completing CLI implementation:

1. **[Architecture & Planning](03-architecture-planning.md)** - Define your system design
2. **[Environment & Configuration](04-environment-configuration.md)** - Set up your development environment
3. **[Backend Foundation](05-backend-foundation.md)** - Create your core service

## 🔧 Troubleshooting

### Common CLI Issues

#### **Command Not Found**
```bash
# Check if CLI file exists
ls -la cli/cli.py

# Check if it's executable
chmod +x cli/cli.py

# Test with Python directly
python cli/cli.py --help
```

#### **Import Errors**
```bash
# Check if dependencies are installed
pip list | grep click
pip list | grep python-dateutil

# Reinstall if needed
pip install --upgrade click python-dateutil
```

#### **File Permission Issues**
```bash
# Check file permissions
ls -la agentContext/*/*.md

# Fix permissions if needed
chmod 644 agentContext/*/*.md
```

#### **Directory Creation Fails**
```bash
# Check if directories exist
ls -la agentContext/

# Create manually if needed
mkdir -p agentContext/{architecture,decisions,notes}
```

---

**Previous:** [Project Setup](01-project-setup.md) ← | **Next:** [Architecture & Planning](03-architecture-planning.md) →
