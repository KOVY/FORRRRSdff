# 📋 VS Code AI Workflow - Kompletní Instalační Průvodce

## 🎯 Cíl: Rozšíření stávajícího VS Code o pokročilý AI workflow

Tento průvodce rozšíří tvé stávající VS Code prostředí o pokročilé AI funkce, které převyšují Replit/Cursor možnosti - a to zcela zdarma.

## ✅ Předpoklady (Již máš nainstalované)
- Ubuntu Desktop 22.04 LTS ✅
- VS Code Desktop s základními rozšířeními ✅
- Docker + Docker Compose ✅
- Python 3.11 + Node.js 20.x ✅
- PostgreSQL 15, Redis, Nginx ✅
- Ollama s modely (Llama, DeepSeek V3, Qwen 3 2507) ✅

---

## 🚀 Krok 1: Upgrade AI Modelů pro Vývoj

### **Přidání Qwen2.5-Coder (Nejlepší pro coding)**
```bash
# Zastavit Ollama pokud běží
sudo systemctl stop ollama

# Aktualizovat Ollama na nejnovější verzi
curl -fsSL https://ollama.ai/install.sh | sh

# Stáhnout nejnovější coding modely
ollama pull qwen2.5-coder:70b    # Hlavní coding model
ollama pull qwen2.5:70b          # Architect model (už máš)
ollama pull deepseek-v3          # Analyst model (už máš)

# Ověřit stažené modely
ollama list
```

### **Instalace vLLM pro rychlejší inference**
```bash
# Instalace vLLM s CUDA podporou (využije tvé 6× Nvidia P106-90)
pip install vllm torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

# Test GPU dostupnosti
python -c "import torch; print(f'CUDA: {torch.cuda.is_available()}, GPUs: {torch.cuda.device_count()}')"
```

---

## 🚀 Krok 2: Instalace AI Workflow Frameworku

### **Python AI Dependencies**
```bash
# Hlavní AI framework
pip install --upgrade \
  crewai \
  langgraph \
  mem0ai \
  litellm \
  anthropic \
  openai \
  aider-chat \
  continue-dev

# Databáze a vector store
pip install --upgrade \
  chromadb \
  pinecone-client \
  weaviate-client \
  qdrant-client

# Development tools
pip install --upgrade \
  fastapi \
  uvicorn \
  websockets \
  asyncio-mqtt \
  playwright
```

### **Node.js AI Tools**
```bash
# Model Context Protocol (MCP) framework
npm install -g \
  @modelcontextprotocol/sdk \
  @modelcontextprotocol/server-memory \
  @anthropic-ai/sdk

# Development tools
npm install -g \
  @railway/cli \
  vercel \
  tsx \
  concurrently \
  nodemon
```

---

## 🚀 Krok 3: VS Code Rozšíření pro AI Workflow

### **Instalace rozšíření přes terminál**
```bash
# AI a coding rozšíření
code --install-extension continue.continue
code --install-extension github.copilot-chat
code --install-extension ms-toolsai.jupyter
code --install-extension ms-python.python
code --install-extension ms-python.debugpy

# Advanced development
code --install-extension bradlc.vscode-tailwindcss
code --install-extension formulahendry.auto-rename-tag
code --install-extension christian-kohler.path-intellisense
code --install-extension christian-kohler.npm-intellisense
code --install-extension wix.vscode-import-cost

# Git a deployment
code --install-extension eamodio.gitlens
code --install-extension ms-vscode.vscode-github-issue-notebooks
code --install-extension github.vscode-github-actions

# Monitoring a debugging
code --install-extension ms-vscode.vscode-json
code --install-extension redhat.vscode-yaml
code --install-extension humao.rest-client
```

---

## 🚀 Krok 4: Konfigurace Continue.dev pro AI Modely

### **Vytvoření config souboru**
```bash
# Vytvoř adresář pro Continue.dev config
mkdir -p ~/.continue

# Vytvoř konfigurační soubor
cat > ~/.continue/config.json << 'EOF'
{
  "models": [
    {
      "title": "🎯 AI Architect (Qwen 3 2507)",
      "provider": "ollama",
      "model": "qwen2.5:70b",
      "apiBase": "http://localhost:11434",
      "contextLength": 128000,
      "systemMessage": "Jsi expert software architekt. Navrhuješ škálovatelné, udržitelné systémy s moderními best practices. Komunikuješ česky, ale kód píšeš v angličtině."
    },
    {
      "title": "💻 AI Coder (Qwen2.5-Coder)",
      "provider": "ollama", 
      "model": "qwen2.5-coder:70b",
      "apiBase": "http://localhost:11434",
      "contextLength": 32000,
      "systemMessage": "Jsi senior full-stack developer. Píšeš čistý, efektivní, produkční kód s komplexním error handlingem. Komunikuješ česky, ale kód a komentáře píšeš v angličtině."
    },
    {
      "title": "🧠 AI Analyst (DeepSeek V3)",
      "provider": "ollama",
      "model": "deepseek-v3", 
      "apiBase": "http://localhost:11434",
      "contextLength": 64000,
      "systemMessage": "Jsi code reviewer a performance analytik. Zaměřuješ se na optimalizaci, security a best practices. Komunikuješ česky, ale doporučení píšeš v angličtině."
    }
  ],
  "customCommands": [
    {
      "name": "🚀 Kompletní Full-Stack Projekt",
      "prompt": "Vytvoř kompletní full-stack aplikaci s:\n- Modern React/TypeScript frontend\n- Node.js/Express backend s PostgreSQL\n- Autentifikace a autorizace\n- Responsive design s Tailwind CSS\n- Production deployment config pro Railway + Vercel\n- Comprehensive testing\n\nPožadavky: {{{ input }}}"
    },
    {
      "name": "🔧 Optimalizace a Deploy",
      "prompt": "Optimalizuj tento kód pro produkci a vytvoř deployment konfiguraci pro Railway (backend) a Vercel (frontend). Zahrň environment variables, health checks a monitoring."
    },
    {
      "name": "🧪 Generování Testů",
      "prompt": "Vytvoř comprehensive testy pro tento kód včetně unit testů, integration testů a edge cases. Použij moderní testing frameworky a zajisti vysoké pokrytí."
    },
    {
      "name": "📊 Performance Audit",
      "prompt": "Analyzuj tento kód na performance bottlenecky a navrhni konkrétní optimalizace s měřitelnými dopady na výkon."
    },
    {
      "name": "🔒 Security Review",
      "prompt": "Proveď security audit tohoto kódu a navrhni zlepšení. Zaměř se na input validation, authentication, authorization a data protection."
    },
    {
      "name": "📝 Dokumentace",
      "prompt": "Vytvoř comprehensive dokumentaci pro tento kód včetně README, API docs a inline komentářů. Udělej to beginner-friendly."
    },
    {
      "name": "🔄 Migration z Replit",
      "prompt": "Převeď tento Replit projekt na moderní, lokálně spustitelný projekt s proper strukturou, dependencies a deployment konfigurací pro Railway/Vercel."
    }
  ],
  "tabAutocompleteModel": {
    "title": "Tab Autocomplete",
    "provider": "ollama",
    "model": "qwen2.5-coder:70b",
    "apiBase": "http://localhost:11434"
  },
  "allowAnonymousTelemetry": false,
  "enableTabAutocomplete": true
}
EOF
```

---

## 🚀 Krok 5: VS Code Settings pro AI Workflow

### **Aktualizace settings.json**
```bash
# Otevři VS Code settings
code ~/.config/Code/User/settings.json

# Přidej nebo aktualizuj následující nastavení:
```

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.organizeImports": true
  },
  "editor.suggestSelection": "first",
  "editor.tabCompletion": "on",
  "editor.wordBasedSuggestions": "off",
  
  "continue.telemetryEnabled": false,
  "continue.enableTabAutocomplete": true,
  
  "files.associations": {
    "*.aider": "markdown",
    "*.mcp": "json",
    "*.prompt": "markdown"
  },
  
  "terminal.integrated.defaultProfile.linux": "zsh",
  "terminal.integrated.fontFamily": "MesloLGS NF",
  
  "workbench.colorCustomizations": {
    "activityBar.background": "#1a1a2e",
    "statusBar.background": "#16213e", 
    "titleBar.activeBackground": "#1a1a2e",
    "terminal.background": "#0c0c0c"
  },
  
  "typescript.preferences.includePackageJsonAutoImports": "auto",
  "javascript.preferences.includePackageJsonAutoImports": "auto",
  
  "eslint.workingDirectories": ["./"],
  "prettier.requireConfig": false,
  "prettier.useEditorConfig": false,
  
  "git.autofetch": true,
  "git.confirmSync": false,
  
  "python.defaultInterpreterPath": "/usr/bin/python3.11",
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": true,
  
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[python]": {
    "editor.defaultFormatter": "ms-python.black-formatter"
  }
}
```

---

## 🚀 Krok 6: VS Code Tasks pro AI Workflow

### **Vytvoření tasks.json**
```bash
# Vytvoř .vscode adresář v home directory pro globální tasks
mkdir -p ~/.vscode

# Vytvoř tasks.json
cat > ~/.vscode/tasks.json << 'EOF'
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "🤖 Start AI Models",
      "type": "shell",
      "command": "bash",
      "args": ["-c", "echo '🚀 Starting AI Models...' && ollama serve"],
      "group": "build",
      "presentation": {
        "echo": true,
        "reveal": "always",
        "panel": "new",
        "showReuseMessage": false
      },
      "isBackground": true,
      "problemMatcher": []
    },
    {
      "label": "📊 AI Models Status",
      "type": "shell",
      "command": "bash",
      "args": ["-c", "echo '📊 AI Models Status:' && ollama list && echo '\\n🔥 GPU Status:' && nvidia-smi --query-gpu=name,memory.used,memory.total,utilization.gpu --format=csv,noheader,nounits"],
      "group": "test",
      "presentation": {
        "echo": true,
        "reveal": "always",
        "panel": "new"
      }
    },
    {
      "label": "🚀 Full-Stack Project Setup",
      "type": "shell", 
      "command": "bash",
      "args": ["-c", "echo '🚀 Setting up full-stack project...' && npx create-next-app@latest ${input:projectName} --typescript --tailwind --eslint --app && cd ${input:projectName} && npm install @prisma/client prisma && npx prisma init"],
      "group": "build",
      "presentation": {
        "echo": true,
        "reveal": "always",
        "panel": "new"
      }
    },
    {
      "label": "🔧 Deploy to Railway + Vercel",
      "type": "shell",
      "command": "bash", 
      "args": ["-c", "echo '🔧 Deploying to production...' && railway deploy && vercel --prod"],
      "group": "build",
      "dependsOrder": "sequence"
    },
    {
      "label": "🧪 Run AI Tests",
      "type": "shell",
      "command": "bash",
      "args": ["-c", "echo '🧪 Running AI-enhanced tests...' && npm test -- --coverage"],
      "group": "test"
    },
    {
      "label": "📈 Performance Monitor",
      "type": "shell",
      "command": "watch",
      "args": ["-n", "2", "bash -c 'echo \"=== GPU Status ===\"; nvidia-smi --query-gpu=utilization.gpu,memory.used,memory.total --format=csv,noheader,nounits; echo \"\\n=== Ollama Models ===\"; ollama ps'"],
      "group": "test",
      "isBackground": true
    }
  ],
  "inputs": [
    {
      "id": "projectName",
      "description": "Název nového projektu",
      "default": "my-ai-project"
    }
  ]
}
EOF
```

---

## 🚀 Krok 7: Vytvoření AI Workflow Scriptu

### **Startup script pro AI modely**
```bash
# Vytvoř scripts adresář
mkdir -p ~/ai-scripts

# Vytvoř startup script
cat > ~/ai-scripts/start-ai-workflow.sh << 'EOF'
#!/bin/bash

echo "🚀 Starting AI Development Workflow..."

# Start Ollama service
sudo systemctl start ollama
sleep 3

# Check if models are available
echo "📋 Checking AI models..."
ollama list

# Start Continue.dev compatible server
echo "🔧 Starting MCP server..."
cd ~/ai-scripts
nohup python3 mcp-server.py > mcp.log 2>&1 &

# Check GPU status
echo "🔥 GPU Status:"
nvidia-smi --query-gpu=name,utilization.gpu,memory.used,memory.total --format=csv,noheader,nounits

echo "✅ AI Workflow ready!"
echo "💡 Tip: Open VS Code and press Ctrl+Shift+P → 'Continue: Chat'"
EOF

chmod +x ~/ai-scripts/start-ai-workflow.sh
```

### **Jednoduchý MCP server**
```bash
# Vytvoř základní MCP server
cat > ~/ai-scripts/mcp-server.py << 'EOF'
#!/usr/bin/env python3
import asyncio
import json
from fastapi import FastAPI, WebSocket
from fastapi.middleware.cors import CORSMiddleware
import uvicorn

app = FastAPI(title="AI Workflow MCP Server")

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

@app.get("/health")
async def health():
    return {"status": "healthy", "message": "AI Workflow MCP Server running"}

@app.get("/api/models")
async def get_models():
    return {
        "models": [
            {"name": "qwen2.5:70b", "type": "architect", "status": "available"},
            {"name": "qwen2.5-coder:70b", "type": "coder", "status": "available"}, 
            {"name": "deepseek-v3", "type": "analyst", "status": "available"}
        ]
    }

@app.websocket("/ws/ai")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()
    await websocket.send_text(json.dumps({
        "type": "connection", 
        "message": "AI Workflow connected"
    }))
    
    try:
        while True:
            data = await websocket.receive_text()
            # Echo back for now - extend with actual AI processing
            await websocket.send_text(json.dumps({
                "type": "response",
                "message": f"Received: {data}"
            }))
    except:
        pass

if __name__ == "__main__":
    print("🚀 Starting AI Workflow MCP Server on http://localhost:3001")
    uvicorn.run(app, host="0.0.0.0", port=3001)
EOF

chmod +x ~/ai-scripts/mcp-server.py
```

---

## 🚀 Krok 8: Test Installation

### **Ověření instalace**
```bash
# 1. Start AI workflow
~/ai-scripts/start-ai-workflow.sh

# 2. Test Ollama models
ollama run qwen2.5-coder:70b "Napiš hello world v Reactu"

# 3. Test Continue.dev v VS Code
code
# Stiskni Ctrl+Shift+P → "Continue: Chat"
# Zadej: "Vytvoř React komponentu pro user profile card"

# 4. Test MCP server
curl http://localhost:3001/health
curl http://localhost:3001/api/models
```

### **Test React projektu s AI**
```bash
# Vytvoř testovací projekt
mkdir ~/test-ai-project
cd ~/test-ai-project

# Iniciuj Next.js projekt
npx create-next-app@latest . --typescript --tailwind --eslint --app

# Otevři v VS Code
code .

# Test AI promptu v Continue.dev:
# "Vytvoř dashboard komponentu s PostgreSQL připojením"
```

---

## 🚀 Krok 9: Integrace s Existing Database

### **Konfigurace Prisma pro PostgreSQL**
```bash
# V jakémkoli projektu
npm install prisma @prisma/client

# Vytvoř .env soubor s tvým DigitalOcean PostgreSQL
cat > .env << 'EOF'
DATABASE_URL="postgresql://<username>:<password>@<host>:<port>/<database>?sslmode=require"
EOF

# Iniciuj Prisma
npx prisma init

# Test připojení
npx prisma db pull
```

---

## 🚀 Krok 10: VS Code Launch Configuration

### **Vytvoření launch.json pro debugging**
```bash
# Vytvoř globální launch configuration
mkdir -p ~/.vscode

cat > ~/.vscode/launch.json << 'EOF'
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "🚀 Debug Next.js App",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/node_modules/.bin/next",
      "args": ["dev"],
      "console": "integratedTerminal",
      "skipFiles": ["<node_internals>/**"]
    },
    {
      "name": "🧪 Debug Node.js API",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/server.js",
      "env": {
        "NODE_ENV": "development"
      },
      "console": "integratedTerminal"
    },
    {
      "name": "🐍 Debug Python Script",
      "type": "python",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal",
      "justMyCode": true
    }
  ]
}
EOF
```

---

## ✅ Finální Ověření

### **Checklist úspěšné instalace:**
- [ ] Ollama modely běží (`ollama list`)
- [ ] Continue.dev funguje v VS Code (`Ctrl+Shift+P` → Continue: Chat)
- [ ] MCP server odpovídá (`curl localhost:3001/health`)
- [ ] GPU je dostupná pro AI (`nvidia-smi`)
- [ ] PostgreSQL připojení funguje (`psql <connection-string>`)
- [ ] React projekt se vytváří a spouští (`npm run dev`)
- [ ] AI generuje funkční kód v Continue.dev

### **Quick Test Prompt:**
```
"Vytvoř kompletní React dashboard aplikaci s:
- TypeScript a Tailwind CSS
- PostgreSQL databáze přes Prisma
- User authentication
- Data visualization charts
- Responsive design
- Production deployment config

Použij moderní best practices a zahrň error handling."
```

---

## 🎯 Co Máš Po Instalaci

### **Dostupné AI Modely:**
- 🎯 **Qwen 3 2507** - Architekt (plánování, design)
- 💻 **Qwen2.5-Coder 70B** - Hlavní programátor
- 🧠 **DeepSeek V3** - Code reviewer a analytik

### **VS Code Funkce:**
- AI chat integrovaný přímo v editoru
- Tab autocomplete s AI
- Custom commands pro běžné úkoly
- Automatické formátování a linting
- Debugging konfigurace
- Task automation

### **Workflow Capabilities:**
- Generování celých projektů jedním promptem
- Automatické připojení k PostgreSQL
- Deployment na Railway + Vercel
- Code review a optimalizace
- Security audit
- Performance analysis

---

## 📞 Troubleshooting

### **Pokud něco nefunguje:**
```bash
# Restart Ollama
sudo systemctl restart ollama

# Check GPU
nvidia-smi

# Check models
ollama list

# Restart VS Code
code --disable-extensions && code --enable-extensions

# Check logs
journalctl -u ollama -f
```

### **Časté problémy:**
1. **Ollama neodpovídá**: `sudo systemctl restart ollama`
2. **GPU nedostupná**: Zkontroluj NVIDIA ovladače
3. **Continue.dev nefunguje**: Restartuj VS Code
4. **Model příliš pomalý**: Zkontroluj GPU utilization

---

## 🚀 Ready to Go!

Po dokončení těchto kroků budeš mít nejpokročilejší AI development environment, které:

- **Překoná Replit** rychlostí a funkcemi
- **Nahradí Cursor** s vlastními modely  
- **Funguje offline** nezávisle na internetu
- **Stojí $0** měsíčně vs $20+ u konkurence
- **Dává plnou kontrolu** nad kódem a daty

**Začni s jednoduchým promptem a pozoruj, jak AI vytváří celé aplikace! 🚀**