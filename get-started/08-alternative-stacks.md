# 08: Alternative Technology Stacks

## Overview
This guide covers alternative technology stacks and migration paths for your application.

## 🔄 Node.js/Express Alternative

### 8.1 Backend Structure

**If you prefer Node.js over Python:**

```
backend/
├── src/
│   ├── app.js              # Express app setup
│   ├── routes/             # API route handlers
│   ├── middleware/         # Custom middleware
│   ├── services/           # Business logic
│   ├── models/             # Data models
│   └── utils/              # Utility functions
├── tests/                  # Test suite
├── package.json            # Dependencies
├── Dockerfile              # Containerization
└── docker-compose.yml      # Local development
```

**Create `backend/package.json`:**
```json
{
  "name": "my-awesome-service",
  "version": "0.1.0",
  "description": "My awesome service description",
  "main": "src/app.js",
  "scripts": {
    "start": "node src/app.js",
    "dev": "nodemon src/app.js",
    "test": "jest",
    "test:watch": "jest --watch",
    "lint": "eslint src/",
    "format": "prettier --write src/"
  },
  "dependencies": {
    "express": "^4.18.0",
    "cors": "^2.8.5",
    "helmet": "^7.0.0",
    "morgan": "^1.10.0",
    "dotenv": "^16.0.0",
    "ws": "^8.13.0",
    "joi": "^17.9.0",
    "jsonwebtoken": "^9.0.0",
    "bcryptjs": "^2.4.3"
  },
  "devDependencies": {
    "nodemon": "^3.0.0",
    "jest": "^29.0.0",
    "supertest": "^6.3.0",
    "eslint": "^8.45.0",
    "prettier": "^3.0.0"
  },
  "engines": {
    "node": ">=18.0.0"
  }
}
```

### 8.2 CLI Alternative (Commander.js)

**Create `cli/cli.js`:**
```javascript
#!/usr/bin/env node
const { Command } = require('commander');
const fs = require('fs');
const path = require('path');

const program = new Command();

program
  .name('dev')
  .description('Development CLI for project workflow management');

program
  .command('start-session')
  .description('Start or update today\'s development session')
  .action(() => {
    const today = new Date().toISOString().slice(0, 10).replace(/-/g, '');
    const notesDir = path.join(process.cwd(), 'agentContext', 'notes');
    const notesFile = path.join(notesDir, `${today}_note.md`);
    
    // Ensure directory exists
    if (!fs.existsSync(notesDir)) {
      fs.mkdirSync(notesDir, { recursive: true });
    }
    
    // Create or update notes file
    const timestamp = new Date().toISOString();
    const content = `# Development Session - ${new Date().toLocaleDateString()}\n\n## ${timestamp}\nSession started. Ready to pick tickets and log progress.\n`;
    
    if (fs.existsSync(notesFile)) {
      fs.appendFileSync(notesFile, `\n## ${timestamp}\nSession resumed.\n`);
      console.log(`📝 Updating existing session: ${path.basename(notesFile)}`);
    } else {
      fs.writeFileSync(notesFile, content);
      console.log(`✨ Starting new development session: ${path.basename(notesFile)}`);
    }
    
    console.log(`📁 Session file: ${notesFile}`);
  });

program
  .command('pick-ticket <ticket_id>')
  .description('Set active ticket for current session')
  .action((ticket_id) => {
    const today = new Date().toISOString().slice(0, 10).replace(/-/g, '');
    const notesFile = path.join(process.cwd(), 'agentContext', 'notes', `${today}_note.md`);
    
    if (fs.existsSync(notesFile)) {
      const timestamp = new Date().toISOString();
      const content = `\n## ${timestamp}\n🎯 **Active Ticket:** ${ticket_id}\n`;
      fs.appendFileSync(notesFile, content);
      console.log(`✅ Picked ticket ${ticket_id} - logged to ${path.basename(notesFile)}`);
    } else {
      console.log('❌ No session started. Run "dev start-session" first.');
    }
  });

program.parse();
```

### 8.3 Express Application

**Create `backend/src/app.js`:**
```javascript
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
const morgan = require('morgan');
const WebSocket = require('ws');
const http = require('http');

const app = express();
const server = http.createServer(app);

// WebSocket server
const wss = new WebSocket.Server({ server });

// Middleware
app.use(helmet());
app.use(cors());
app.use(morgan('combined'));
app.use(express.json());

// Health check endpoint
app.get('/healthz', (req, res) => {
  res.json({
    status: 'healthy',
    service: 'my-awesome-service',
    version: '0.1.0',
    connections: wss.clients.size
  });
});

// Root endpoint
app.get('/', (req, res) => {
  res.json({ message: 'Welcome to My Awesome Service' });
});

// API status endpoint
app.get('/api/v1/status', (req, res) => {
  res.json({
    status: 'operational',
    timestamp: new Date().toISOString(),
    version: '0.1.0'
  });
});

// WebSocket connection handling
wss.on('connection', (ws) => {
  console.log('WebSocket connected. Total connections:', wss.clients.size);
  
  ws.on('message', (message) => {
    const data = message.toString();
    
    // Echo back to sender
    ws.send(`Message: ${data}`);
    
    // Broadcast to all connections
    wss.clients.forEach((client) => {
      if (client !== ws && client.readyState === WebSocket.OPEN) {
        client.send(`Broadcast: ${data}`);
      }
    });
  });
  
  ws.on('close', () => {
    console.log('WebSocket disconnected. Total connections:', wss.clients.size);
  });
});

// Error handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Something went wrong!' });
});

// 404 handler
app.use((req, res) => {
  res.status(404).json({ error: 'Not found' });
});

const PORT = process.env.PORT || 8000;

server.listen(PORT, () => {
  console.log(`🚀 Server running on port ${PORT}`);
});

module.exports = app;
```

## 🐹 Go/Gin Alternative

### 8.4 Backend Structure

**If you prefer Go over Python:**

```
backend/
├── cmd/
│   └── server/
│       └── main.go         # Application entry point
├── internal/
│   ├── handlers/           # HTTP handlers
│   ├── services/           # Business logic
│   ├── models/             # Data structures
│   └── middleware/         # Custom middleware
├── pkg/                    # Public packages
├── tests/                  # Test files
├── go.mod                  # Go modules
├── go.sum                  # Go checksums
├── Dockerfile              # Containerization
└── docker-compose.yml      # Local development
```

**Create `backend/go.mod`:**
```go
module myproject

go 1.21

require (
    github.com/gin-gonic/gin v1.9.1
    github.com/gorilla/websocket v1.5.0
    github.com/spf13/cobra v1.7.0
    github.com/spf13/viper v1.16.0
)

require (
    github.com/bytedance/sonic v1.9.1 // indirect
    github.com/chenzhuoyu/base64x v0.0.0-20221115062448-fe3a3abad311 // indirect
    github.com/fsnotify/fsnotify v1.6.0 // indirect
    github.com/gin-contrib/sse v0.1.0 // indirect
    github.com/go-playground/locales v0.14.1 // indirect
    github.com/go-playground/universal-translator v0.18.1 // indirect
    github.com/go-playground/validator/v10 v10.14.0 // indirect
    github.com/goccy/go-json v0.10.2 // indirect
    github.com/hashicorp/hcl v1.0.0 // indirect
    github.com/inconshreveable/mousetrap v1.1.0 // indirect
    github.com/json-iterator/go v1.1.12 // indirect
    github.com/klauspost/cpuid/v2 v2.2.4 // indirect
    github.com/leodido/go-urn v1.2.4 // indirect
    github.com/magiconair/properties v1.8.7 // indirect
    github.com/mattn/go-isatty v0.0.19 // indirect
    github.com/mitchellh/mapstructure v1.5.0 // indirect
    github.com/modern-go/concurrent v0.0.0-20180306012644-bacd9c7ef1dd // indirect
    github.com/modern-go/reflect2 v1.0.2 // indirect
    github.com/pelletier/go-toml/v2 v2.0.8 // indirect
    github.com/spf13/afero v1.9.5 // indirect
    github.com/spf13/cast v1.5.1 // indirect
    github.com/spf13/jwalterweatherman v1.1.0 // indirect
    github.com/spf13/pflag v1.0.5 // indirect
    github.com/subosito/gotenv v1.4.2 // indirect
    github.com/twitchyliquid64/golang-asm v0.15.1 // indirect
    github.com/ugorji/go/codec v1.2.11 // indirect
    golang.org/x/arch v0.3.0 // indirect
    golang.org/x/crypto v0.9.0 // indirect
    golang.org/x/net v0.10.0 // indirect
    golang.org/x/sys v0.8.0 // indirect
    golang.org/x/text v0.9.0 // indirect
    gopkg.in/ini.v1 v1.67.0 // indirect
    gopkg.in/yaml.v3 v3.0.1 // indirect
)
```

### 8.5 CLI Alternative (Cobra)

**Create `cli/main.go`:**
```go
package main

import (
    "fmt"
    "os"
    "time"
    "path/filepath"
    "github.com/spf13/cobra"
)

var startSessionCmd = &cobra.Command{
    Use:   "start-session",
    Short: "Start or update today's development session",
    Run: func(cmd *cobra.Command, args []string) {
        today := time.Now().Format("02012006")
        notesDir := filepath.Join(".", "agentContext", "notes")
        notesFile := filepath.Join(notesDir, today+"_note.md")
        
        // Ensure directory exists
        if err := os.MkdirAll(notesDir, 0755); err != nil {
            fmt.Printf("❌ Error creating directory: %v\n", err)
            return
        }
        
        // Create or update notes file
        timestamp := time.Now().Format("15:04:05")
        content := fmt.Sprintf("# Development Session - %s\n\n## %s\nSession started. Ready to pick tickets and log progress.\n",
            time.Now().Format("January 2, 2006"), timestamp)
        
        if _, err := os.Stat(notesFile); err == nil {
            // File exists, append to it
            f, err := os.OpenFile(notesFile, os.O_APPEND|os.O_CREATE|os.O_WRONLY, 0644)
            if err != nil {
                fmt.Printf("❌ Error opening file: %v\n", err)
                return
            }
            defer f.Close()
            
            appendContent := fmt.Sprintf("\n## %s\nSession resumed.\n", timestamp)
            if _, err := f.WriteString(appendContent); err != nil {
                fmt.Printf("❌ Error writing to file: %v\n", err)
                return
            }
            
            fmt.Printf("📝 Updating existing session: %s\n", filepath.Base(notesFile))
        } else {
            // File doesn't exist, create it
            if err := os.WriteFile(notesFile, []byte(content), 0644); err != nil {
                fmt.Printf("❌ Error creating file: %v\n", err)
                return
            }
            
            fmt.Printf("✨ Starting new development session: %s\n", filepath.Base(notesFile))
        }
        
        fmt.Printf("📁 Session file: %s\n", notesFile)
    },
}

var pickTicketCmd = &cobra.Command{
    Use:   "pick-ticket [ticket_id]",
    Short: "Set active ticket for current session",
    Args:  cobra.ExactArgs(1),
    Run: func(cmd *cobra.Command, args []string) {
        ticketID := args[0]
        today := time.Now().Format("02012006")
        notesFile := filepath.Join(".", "agentContext", "notes", today+"_note.md")
        
        if _, err := os.Stat(notesFile); err == nil {
            timestamp := time.Now().Format("15:04:05")
            content := fmt.Sprintf("\n## %s\n🎯 **Active Ticket:** %s\n", timestamp, ticketID)
            
            f, err := os.OpenFile(notesFile, os.O_APPEND|os.O_CREATE|os.O_WRONLY, 0644)
            if err != nil {
                fmt.Printf("❌ Error opening file: %v\n", err)
                return
            }
            defer f.Close()
            
            if _, err := f.WriteString(content); err != nil {
                fmt.Printf("❌ Error writing to file: %v\n", err)
                return
            }
            
            fmt.Printf("✅ Picked ticket %s - logged to %s\n", ticketID, filepath.Base(notesFile))
        } else {
            fmt.Println("❌ No session started. Run \"dev start-session\" first.")
        }
    },
}

func main() {
    var rootCmd = &cobra.Command{Use: "dev"}
    rootCmd.AddCommand(startSessionCmd)
    rootCmd.AddCommand(pickTicketCmd)
    
    if err := rootCmd.Execute(); err != nil {
        fmt.Println(err)
        os.Exit(1)
    }
}
```

### 8.6 Gin Application

**Create `backend/cmd/server/main.go`:**
```go
package main

import (
    "log"
    "net/http"
    "time"
    
    "github.com/gin-gonic/gin"
    "github.com/gorilla/websocket"
)

var upgrader = websocket.Upgrader{
    CheckOrigin: func(r *http.Request) bool {
        return true // Allow all origins in development
    },
}

type Connection struct {
    ws   *websocket.Conn
    send chan []byte
}

type Hub struct {
    connections map[*Connection]bool
    broadcast   chan []byte
    register    chan *Connection
    unregister  chan *Connection
}

func newHub() *Hub {
    return &Hub{
        connections: make(map[*Connection]bool),
        broadcast:   make(chan []byte),
        register:    make(chan *Connection),
        unregister:  make(chan *Connection),
    }
}

func (h *Hub) run() {
    for {
        select {
        case c := <-h.register:
            h.connections[c] = true
            log.Printf("WebSocket connected. Total connections: %d", len(h.connections))
        case c := <-h.unregister:
            if _, ok := h.connections[c]; ok {
                delete(h.connections, c)
                close(c.send)
                log.Printf("WebSocket disconnected. Total connections: %d", len(h.connections))
            }
        case message := <-h.broadcast:
            for c := range h.connections {
                select {
                case c.send <- message:
                default:
                    close(c.send)
                    delete(h.connections, c)
                }
            }
        }
    }
}

func (c *Connection) readPump(hub *Hub) {
    defer func() {
        hub.unregister <- c
        c.ws.Close()
    }()
    
    for {
        _, message, err := c.ws.ReadMessage()
        if err != nil {
            break
        }
        
        // Echo back to sender
        c.send <- []byte("Message: " + string(message))
        
        // Broadcast to all connections
        hub.broadcast <- []byte("Broadcast: " + string(message))
    }
}

func (c *Connection) writePump() {
    defer func() {
        c.ws.Close()
    }()
    
    for {
        select {
        case message, ok := <-c.send:
            if !ok {
                c.ws.WriteMessage(websocket.CloseMessage, []byte{})
                return
            }
            w, err := c.ws.NextWriter(websocket.TextMessage)
            if err != nil {
                return
            }
            w.Write(message)
            
            if err := w.Close(); err != nil {
                return
            }
        }
    }
}

func serveWs(hub *Hub, c *gin.Context) {
    ws, err := upgrader.Upgrade(c.Writer, c.Request, nil)
    if err != nil {
        log.Println(err)
        return
    }
    
    conn := &Connection{ws: ws, send: make(chan []byte, 256)}
    hub.register <- conn
    
    go conn.writePump()
    go conn.readPump(hub)
}

func main() {
    gin.SetMode(gin.ReleaseMode)
    r := gin.Default()
    
    // CORS middleware
    r.Use(func(c *gin.Context) {
        c.Header("Access-Control-Allow-Origin", "*")
        c.Header("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS")
        c.Header("Access-Control-Allow-Headers", "Origin, Content-Type, Content-Length, Accept-Encoding, X-CSRF-Token, Authorization")
        
        if c.Request.Method == "OPTIONS" {
            c.AbortWithStatus(204)
            return
        }
        
        c.Next()
    })
    
    hub := newHub()
    go hub.run()
    
    // Health check endpoint
    r.GET("/healthz", func(c *gin.Context) {
        c.JSON(http.StatusOK, gin.H{
            "status":      "healthy",
            "service":     "my-awesome-service",
            "version":     "0.1.0",
            "connections": len(hub.connections),
        })
    })
    
    // Root endpoint
    r.GET("/", func(c *gin.Context) {
        c.JSON(http.StatusOK, gin.H{
            "message": "Welcome to My Awesome Service",
        })
    })
    
    // API status endpoint
    r.GET("/api/v1/status", func(c *gin.Context) {
        c.JSON(http.StatusOK, gin.H{
            "status":    "operational",
            "timestamp": time.Now().Format(time.RFC3339),
            "version":   "0.1.0",
        })
    })
    
    // WebSocket endpoint
    r.GET("/ws", func(c *gin.Context) {
        serveWs(hub, c)
    })
    
    log.Println("🚀 Server starting on port 8000")
    if err := r.Run(":8000"); err != nil {
        log.Fatal("Failed to start server:", err)
    }
}
```

## 🔧 Different CLI Frameworks

### 8.7 Python Alternatives

**Typer (Modern alternative to Click):**
```python
#!/usr/bin/env python3
import typer
from datetime import datetime
from pathlib import Path

app = typer.Typer()

@app.command()
def start_session():
    """Start or update today's development session."""
    today = datetime.now().strftime("%d%m%Y")
    notes_file = Path(f"agentContext/notes/{today}_note.md")
    
    notes_file.parent.mkdir(parents=True, exist_ok=True)
    
    if not notes_file.exists():
        typer.echo(f"✨ Starting new development session: {notes_file.name}")
        notes_file.write_text(f"# Development Session - {datetime.now().strftime('%B %d, %Y')}\n\n")
    
    timestamp = datetime.now().strftime("%H:%M:%S")
    with open(notes_file, 'a') as f:
        f.write(f"\n## {timestamp}\nSession started. Ready to pick tickets and log progress.\n")
    
    typer.echo(f"📁 Session file: {notes_file}")

@app.command()
def pick_ticket(ticket_id: str):
    """Set active ticket for current session."""
    today = datetime.now().strftime("%d%m%Y")
    notes_file = Path(f"agentContext/notes/{today}_note.md")
    
    if notes_file.exists():
        timestamp = datetime.now().strftime("%H:%M:%S")
        with open(notes_file, 'a') as f:
            f.write(f"\n## {timestamp}\n🎯 **Active Ticket:** {ticket_id}\n")
        typer.echo(f"✅ Picked ticket {ticket_id}")
    else:
        typer.echo("❌ No session started. Run 'dev start-session' first.")

if __name__ == "__main__":
    app()
```

**Fire (Automatically generates CLI from Python functions):**
```python
#!/usr/bin/env python3
import fire
from datetime import datetime
from pathlib import Path

class DevCLI:
    def start_session(self):
        """Start or update today's development session."""
        today = datetime.now().strftime("%d%m%Y")
        notes_file = Path(f"agentContext/notes/{today}_note.md")
        
        notes_file.parent.mkdir(parents=True, exist_ok=True)
        
        if not notes_file.exists():
            print(f"✨ Starting new development session: {notes_file.name}")
            notes_file.write_text(f"# Development Session - {datetime.now().strftime('%B %d, %Y')}\n\n")
        
        timestamp = datetime.now().strftime("%H:%M:%S")
        with open(notes_file, 'a') as f:
            f.write(f"\n## {timestamp}\nSession started. Ready to pick tickets and log progress.\n")
        
        print(f"📁 Session file: {notes_file}")
    
    def pick_ticket(self, ticket_id: str):
        """Set active ticket for current session."""
        today = datetime.now().strftime("%d%m%Y")
        notes_file = Path(f"agentContext/notes/{today}_note.md")
        
        if notes_file.exists():
            timestamp = datetime.now().strftime("%H:%M:%S")
            with open(notes_file, 'a') as f:
                f.write(f"\n## {timestamp}\n🎯 **Active Ticket:** {ticket_id}\n")
            print(f"✅ Picked ticket {ticket_id}")
        else:
            print("❌ No session started. Run 'dev start-session' first.")

if __name__ == "__main__":
    fire.Fire(DevCLI)
```

### 8.8 Node.js Alternatives

**Commander.js (Most popular):**
```javascript
#!/usr/bin/env node
const { Command } = require('commander');
const fs = require('fs');
const path = require('path');

const program = new Command();

program
  .name('dev')
  .description('Development CLI for project workflow management');

program
  .command('start-session')
  .description('Start or update today\'s development session')
  .action(() => {
    // Implementation here
  });

program.parse();
```

**Yargs (Feature-rich alternative):**
```javascript
#!/usr/bin/env node
const yargs = require('yargs/yargs');
const { hideBin } = require('yargs/helpers');

yargs(hideBin(process.argv))
  .command('start-session', 'Start or update today\'s development session', {}, (argv) => {
    // Implementation here
  })
  .command('pick-ticket <ticket_id>', 'Set active ticket for current session', {}, (argv) => {
    // Implementation here
  })
  .demandCommand(1)
  .help()
  .argv;
```

### 8.9 Go Alternatives

**Cobra (Most popular Go CLI framework):**
```go
package main

import (
    "fmt"
    "github.com/spf13/cobra"
)

var startSessionCmd = &cobra.Command{
    Use:   "start-session",
    Short: "Start or update today's development session",
    Run: func(cmd *cobra.Command, args []string) {
        // Implementation here
    },
}

func main() {
    var rootCmd = &cobra.Command{Use: "dev"}
    rootCmd.AddCommand(startSessionCmd)
    rootCmd.Execute()
}
```

**Viper (Configuration management):**
```go
package main

import (
    "fmt"
    "github.com/spf13/viper"
)

func main() {
    viper.SetConfigName("config")
    viper.SetConfigType("yaml")
    viper.AddConfigPath(".")
    
    if err := viper.ReadInConfig(); err != nil {
        fmt.Printf("Error reading config: %v\n", err)
        return
    }
    
    fmt.Printf("Service name: %s\n", viper.GetString("service.name"))
    fmt.Printf("Service version: %s\n", viper.GetString("service.version"))
}
```

## 🏗️ Project Structure Variations

### 8.10 Monorepo Approach

**For large projects with multiple services:**

```
monorepo/
├── packages/
│   ├── backend/            # Backend service
│   ├── frontend/           # Frontend application
│   ├── shared/             # Shared utilities
│   └── cli/                # Development CLI
├── tools/                  # Build and development tools
├── docs/                   # Documentation
└── package.json            # Root package configuration
```

**Root `package.json`:**
```json
{
  "name": "my-awesome-monorepo",
  "version": "0.1.0",
  "private": true,
  "workspaces": [
    "packages/*"
  ],
  "scripts": {
    "dev": "concurrently \"npm run dev:backend\" \"npm run dev:frontend\"",
    "dev:backend": "cd packages/backend && npm run dev",
    "dev:frontend": "cd packages/frontend && npm run dev",
    "build": "npm run build:backend && npm run build:frontend",
    "test": "npm run test:backend && npm run test:frontend"
  },
  "devDependencies": {
    "concurrently": "^8.0.0",
    "lerna": "^7.0.0"
  }
}
```

### 8.11 Microservices Approach

**For distributed systems:**

```
services/
├── api-gateway/            # API gateway service
├── user-service/           # User management service
├── business-service/        # Core business logic
├── notification-service/    # Notification service
└── shared/                 # Shared libraries and utilities
```

**Service template:**
```yaml
# docker-compose.yml
version: '3.8'

services:
  api-gateway:
    build: ./api-gateway
    ports:
      - "8000:8000"
    environment:
      - SERVICE_NAME=api-gateway
    depends_on:
      - user-service
      - business-service
  
  user-service:
    build: ./user-service
    ports:
      - "8001:8000"
    environment:
      - SERVICE_NAME=user-service
  
  business-service:
    build: ./business-service
    ports:
      - "8002:8000"
    environment:
      - SERVICE_NAME=business-service
```

### 8.12 Plugin Architecture

**For extensible systems:**

```
core/
├── plugin-system/          # Plugin management
├── core-api/               # Core API endpoints
└── plugin-interface/       # Plugin interfaces

plugins/
├── feature-a/              # Feature A plugin
├── feature-b/              # Feature B plugin
└── feature-c/              # Feature C plugin
```

**Plugin interface:**
```python
# core/plugin_interface.py
from abc import ABC, abstractmethod
from typing import Any, Dict

class PluginInterface(ABC):
    """Base interface for all plugins."""
    
    @abstractmethod
    def name(self) -> str:
        """Return plugin name."""
        pass
    
    @abstractmethod
    def version(self) -> str:
        """Return plugin version."""
        pass
    
    @abstractmethod
    def initialize(self, config: Dict[str, Any]) -> bool:
        """Initialize the plugin."""
        pass
    
    @abstractmethod
    def execute(self, data: Any) -> Any:
        """Execute plugin functionality."""
        pass
```

## 🔄 Migration Paths

### 8.13 From Existing Projects

**Gradual Migration:**
1. **Start with CLI and context management** - Add development workflow
2. **Parallel Development** - Run new workflow alongside existing
3. **Full Migration** - Complete replacement when ready

**Migration script example:**
```bash
#!/bin/bash
# migrate.sh - Migrate existing project to new structure

echo "🚀 Starting project migration..."

# Backup existing structure
echo "📦 Creating backup..."
cp -r . ../project-backup-$(date +%Y%m%d-%H%M%S)

# Create new directory structure
echo "🏗️  Creating new structure..."
mkdir -p agentContext/{architecture,decisions,notes} tickets stories cli

# Migrate existing files
echo "📁 Migrating existing files..."
if [ -d "src" ]; then
    mv src backend/
fi

if [ -d "docs" ]; then
    mv docs/* agentContext/architecture/ || true
fi

# Install new dependencies
echo "📦 Installing new dependencies..."
pip install click python-dateutil

# Create CLI
echo "🔧 Setting up CLI..."
cp bootstrap/bootstrap_v0_getstarted/cli/cli.py cli/

echo "✅ Migration complete!"
echo "   Run 'python cli/cli.py start-session' to begin"
```

### 8.14 Scaling Up

**Team Expansion:**
1. **Add collaboration features** - Shared context, team tickets
2. **Process Automation** - CI/CD integration
3. **Monitoring** - Add observability and metrics
4. **Documentation** - Expand documentation and guides

**Team workflow example:**
```yaml
# .github/workflows/development.yml
name: Development Workflow

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      - name: Install dependencies
        run: |
          pip install click python-dateutil
      - name: Validate CLI
        run: |
          python cli/cli.py --help
      - name: Check context structure
        run: |
          ls -la agentContext/
          ls -la tickets/
          ls -la stories/
```

## ✅ Verification Checklist

- [ ] Alternative technology stack chosen
- [ ] Backend structure created
- [ ] CLI implementation working
- [ ] Basic endpoints functional
- [ ] WebSocket support working
- [ ] Tests passing
- [ ] Docker configuration working
- [ ] Migration path planned
- [ ] Scaling strategy defined

## 🚀 Next Steps

After choosing alternative stack:

1. **Implement Core Functionality** - Build your service
2. **Add Tests** - Ensure quality
3. **Deploy** - Get it running
4. **Monitor** - Track performance
5. **Iterate** - Improve based on feedback

## 🔧 Troubleshooting

### Common Alternative Stack Issues

#### **Node.js Issues**
```bash
# Check Node.js version
node --version

# Check npm version
npm --version

# Clear npm cache
npm cache clean --force

# Reinstall dependencies
rm -rf node_modules package-lock.json
npm install
```

#### **Go Issues**
```bash
# Check Go version
go version

# Check Go modules
go mod tidy

# Update dependencies
go get -u ./...

# Run tests
go test ./...
```

#### **Migration Issues**
```bash
# Check backup
ls -la ../project-backup-*

# Restore if needed
cp -r ../project-backup-*/* .

# Start fresh
rm -rf agentContext tickets stories cli
mkdir -p agentContext/{architecture,decisions,notes} tickets stories cli
```

---

**Previous:** [Troubleshooting](07-troubleshooting.md) ← | **Next:** [Back to Overview](../README.md) →

---

**← Back to [Main README](../../README.md)**
