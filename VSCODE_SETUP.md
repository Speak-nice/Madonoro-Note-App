# 🔧 VS Code Setup Guide for Madonoro Note App

Complete guide to configure VS Code for full-stack development with recommended extensions, settings, and workflows.

---

## 📋 Table of Contents

1. [Recommended Extensions](#recommended-extensions)
2. [VS Code Settings](#vs-code-settings)
3. [Project Configuration](#project-configuration)
4. [Debugging Setup](#debugging-setup)
5. [Useful Shortcuts](#useful-shortcuts)
6. [Workflow Tips](#workflow-tips)

---

## Recommended Extensions

### Installation Method
Open VS Code → Press `Ctrl+Shift+X` (or `Cmd+Shift+X` on Mac) → Search and click "Install"

### Essential Extensions

#### 1. **Python** (Microsoft)
- ID: `ms-python.python`
- For Python support (if using Flask/FastAPI backend)
- Install Python language support
- IntelliSense, linting, debugging

#### 2. **JavaScript (ES6) code snippets** (Charalampos Karypidis)
- ID: `xabikos.JavaScriptSnippets`
- React/JSX code snippets
- Faster code writing

#### 3. **ES7+ React/Redux/React-Native snippets** (dsznajder)
- ID: `dsznajder.es7-react-js-snippets`
- React hooks snippets
- Component templates

#### 4. **Prettier - Code formatter** (Prettier)
- ID: `esbenp.prettier-vscode`
- Auto-format code on save
- Keeps code consistent

#### 5. **ESLint** (Microsoft)
- ID: `dbaeumer.vscode-eslint`
- JavaScript linting
- Code quality checks

#### 6. **Thunder Client** (Ranga Vadhineni)
- ID: `rangav.vscode-thunder-client`
- API testing directly in VS Code
- Alternative to Postman

#### 7. **REST Client** (Huachao Mao)
- ID: `humao.rest-client`
- Send HTTP requests from .rest files
- Test API endpoints

#### 8. **GitLens** (Eric Amodio)
- ID: `eamodio.gitlens`
- Git integration and history
- See who changed what and when

#### 9. **Git Graph** (mhutchie)
- ID: `mhutchie.git-graph`
- Visual git branch history
- Better branch management

#### 10. **Postman** (Postman)
- ID: `postman.postman-for-vscode`
- Direct Postman integration
- API collection management

#### 11. **Supabase** (Supabase)
- ID: `supabase.supabase`
- Supabase database browser
- Query builder
- Direct table management

#### 12. **Docker** (Microsoft)
- ID: `ms-azuretools.vscode-docker`
- Docker support and management
- For local development with docker-compose

#### 13. **Thunder Client** Alternative: **Hoppscotch**
- ID: `hoppscotch.io`
- Modern API client
- Share requests easily

#### 14. **Tailwind CSS IntelliSense** (Tailwind Labs)
- ID: `bradlc.vscode-tailwindcss`
- Tailwind class autocomplete
- Color preview

#### 15. **Live Server** (Ritwick Dey)
- ID: `ritwickdey.LiveServer`
- Local development server
- Live reload

---

## VS Code Settings

### Create `.vscode/settings.json`

```json
{
  // Editor
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.fontSize": 14,
  "editor.fontFamily": "Fira Code, monospace",
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "editor.wordWrap": "on",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },

  // JavaScript/TypeScript
  "javascript.updateImportsOnFileMove.enabled": "always",
  "typescript.updateImportsOnFileMove.enabled": "always",
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },

  // JSON
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },

  // Prettier
  "prettier.semi": true,
  "prettier.singleQuote": true,
  "prettier.trailingComma": "es5",
  "prettier.bracketSpacing": true,
  "prettier.arrowParens": "always",
  "prettier.printWidth": 100,

  // ESLint
  "eslint.enable": true,
  "eslint.run": "onSave",
  "eslint.format.enable": true,

  // Files
  "files.exclude": {
    "**/.git": true,
    "**/.DS_Store": true,
    "**/node_modules": true,
    "**/.env": true,
    "**/.env.local": true
  },
  "files.watcherExclude": {
    "**/.git/objects/**": true,
    "**/.git/subtree-cache/**": true,
    "**/node_modules/*/**": true,
    "**/.vscode/**": true
  },

  // Git
  "git.ignoreLimitWarning": true,
  "git.autofetch": true,

  // Tailwind CSS
  "tailwindCSS.experimental.classRegex": [
    ["cva\\(([^)]*)\\)", "(?:'|\"|`)([^']*)(?:'|\"|`)"]
  ],

  // Search exclude
  "search.exclude": {
    "**/node_modules": true,
    "**/.git": true,
    "**/.venv": true
  },

  // Auto save
  "files.autoSave": "onFocusChange",

  // Emmet
  "emmet.includeLanguages": {
    "javascript": "javascriptreact",
    "typescript": "typescriptreact"
  }
}
```

### Create `.vscode/extensions.json`

```json
{
  "recommendations": [
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "dsznajder.es7-react-js-snippets",
    "xabikos.JavaScriptSnippets",
    "rangav.vscode-thunder-client",
    "humao.rest-client",
    "eamodio.gitlens",
    "mhutchie.git-graph",
    "supabase.supabase",
    "ms-azuretools.vscode-docker",
    "bradlc.vscode-tailwindcss",
    "ritwickdey.LiveServer",
    "ms-python.python"
  ]
}
```

---

## Project Configuration

### Create `.prettierrc.json`

```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "bracketSpacing": true,
  "arrowParens": "always",
  "printWidth": 100,
  "endOfLine": "lf"
}
```

### Create `.prettierignore`

```
node_modules
dist
build
.next
.env
.env.*
*.md
package.json
package-lock.json
```

### Create `.eslintrc.json`

```json
{
  "env": {
    "browser": true,
    "es2021": true,
    "node": true
  },
  "extends": ["eslint:recommended", "plugin:react/recommended"],
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "rules": {
    "react/react-in-jsx-scope": "off",
    "no-unused-vars": ["warn", { "argsIgnorePattern": "^_" }],
    "no-console": ["warn", { "allow": ["warn", "error"] }]
  }
}
```

### Create `.editorconfig`

```ini
# EditorConfig helps maintain consistent coding styles
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{js,jsx,ts,tsx,json}]
indent_style = space
indent_size = 2

[*.md]
max_line_length = off
trim_trailing_whitespace = false

[*.yml,*.yaml]
indent_size = 2

[Makefile]
indent_style = tab
```

---

## Debugging Setup

### Create `.vscode/launch.json`

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug Backend",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/backend/src/server.js",
      "restart": true,
      "console": "integratedTerminal",
      "cwd": "${workspaceFolder}/backend",
      "env": {
        "NODE_ENV": "development"
      }
    },
    {
      "name": "Debug Frontend",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:5173",
      "webRoot": "${workspaceFolder}/frontend",
      "sourceMaps": true
    }
  ]
}
```

### Debugging Instructions

#### Debug Node.js Backend
1. Set breakpoint (click on line number)
2. Go to **Run → Start Debugging** (or press `F5`)
3. Select "Debug Backend"
4. Use **Debug Console** to inspect variables

#### Debug React Frontend
1. Install **Debugger for Chrome** extension
2. Set breakpoint in React component
3. Go to **Run → Start Debugging**
4. Select "Debug Frontend"
5. Open browser DevTools

---

## Useful Shortcuts

### General Shortcuts
```
Ctrl+` (backtick)      - Toggle terminal
Ctrl+B                 - Toggle sidebar
Ctrl+Shift+P           - Command palette
Ctrl+L                 - Select current line
Ctrl+Shift+K           - Delete line
Alt+Up/Down            - Move line up/down
Ctrl+/                 - Toggle comment
Ctrl+H                 - Find and replace
Ctrl+Shift+F           - Find in files
Ctrl+G                 - Go to line
```

### Code Navigation
```
Ctrl+Click             - Go to definition
F12                    - Go to definition (alternative)
Ctrl+Shift+O           - Go to symbol in file
Ctrl+T                 - Go to symbol in workspace
Ctrl+.                 - Quick fix suggestions
```

### Git Shortcuts
```
Ctrl+Shift+G           - Open Git view
Ctrl+Shift+G, P        - Git: Pull
Ctrl+Shift+G, U        - Git: Undo Last Commit
```

### Debugging Shortcuts
```
F5                     - Start debugging
F6                     - Pause
F10                    - Step over
F11                    - Step into
Shift+F11              - Step out
Ctrl+Shift+D           - Toggle debug console
```

---

## Workflow Tips

### 1. Multi-Root Workspace

Create `.code-workspace` file:

```json
{
  "folders": [
    {
      "path": "frontend",
      "name": "Frontend"
    },
    {
      "path": "backend",
      "name": "Backend"
    },
    {
      "path": "database",
      "name": "Database"
    }
  ],
  "settings": {}
}
```

Open with: `File → Open Workspace from File`

### 2. Split Terminals

```bash
# Terminal 1 - Backend
cd backend && npm run dev

# Terminal 2 - Frontend (Click + to add new terminal)
cd frontend && npm run dev

# Terminal 3 - Git (optional)
git status
```

### 3. REST Client for API Testing

Create `backend/requests.http`:

```http
### Register User
POST http://localhost:3000/api/auth/register HTTP/1.1
content-type: application/json

{
  "email": "test@example.com",
  "password": "Password123!",
  "username": "testuser"
}

### Login
POST http://localhost:3000/api/auth/login HTTP/1.1
content-type: application/json

{
  "email": "test@example.com",
  "password": "Password123!"
}

### Get Notes
GET http://localhost:3000/api/notes HTTP/1.1
Authorization: Bearer YOUR_TOKEN_HERE
```

Click "Send Request" above each block to test.

### 4. Code Snippets

Create `.vscode/snippets.json`:

```json
{
  "React Function Component": {
    "prefix": "rfc",
    "body": [
      "import React from 'react';",
      "",
      "function ${1:ComponentName}() {",
      "  return (",
      "    <div className='${2:classname}'>",
      "      ${3:content}",
      "    </div>",
      "  );",
      "}",
      "",
      "export default ${1:ComponentName};"
    ],
    "description": "React Function Component"
  },
  "Node.js Express Route": {
    "prefix": "route",
    "body": [
      "router.${1|get,post,put,delete|}('${2:/path}', async (req, res) => {",
      "  try {",
      "    ${3:// Your code here}",
      "    res.json({ success: true });",
      "  } catch (error) {",
      "    res.status(500).json({ error: error.message });",
      "  }",
      "});"
    ],
    "description": "Express Route"
  }
}
```

### 5. Task Automation

Create `.vscode/tasks.json`:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Run All Services",
      "dependsOn": ["Start Backend", "Start Frontend"],
      "problemMatcher": []
    },
    {
      "label": "Start Backend",
      "type": "shell",
      "command": "npm",
      "args": ["run", "dev"],
      "options": {
        "cwd": "${workspaceFolder}/backend"
      },
      "isBackground": true,
      "problemMatcher": {
        "pattern": {
          "regexp": "^.*$",
          "file": 1,
          "location": 2,
          "message": 3
        },
        "background": {
          "activeOnStart": true,
          "beginsPattern": "^.*Server running.*",
          "endsPattern": "^.*listening on.*"
        }
      }
    },
    {
      "label": "Start Frontend",
      "type": "shell",
      "command": "npm",
      "args": ["run", "dev"],
      "options": {
        "cwd": "${workspaceFolder}/frontend"
      },
      "isBackground": true,
      "problemMatcher": {
        "pattern": {
          "regexp": "^.*$"
        },
        "background": {
          "activeOnStart": true,
          "beginsPattern": "^.*VITE.*",
          "endsPattern": "^.*ready in.*"
        }
      }
    }
  ]
}
```

Run with: `Ctrl+Shift+P` → "Run All Services"

### 6. Source Control Best Practices

**Commit Message Convention:**
```
feat: Add new feature description
fix: Fix bug description
docs: Update documentation
style: Code style changes
refactor: Refactor code
test: Add tests
chore: Update dependencies
```

Use: `Ctrl+Shift+G` to open Git view

---

## Performance Tips

1. **Disable Unnecessary Extensions**
   - Go to **Extensions**
   - Disable extension for workspace if not needed

2. **Increase Font Size for Readability**
   - `"editor.fontSize": 14`

3. **Enable Code Folding**
   - `"editor.showFoldingControls": "always"`

4. **Auto-Save Settings**
   - `"files.autoSave": "onFocusChange"`

---

## Keyboard Shortcuts Cheat Sheet

### Most Used Commands
```
Ctrl+P                - Quick open file
Ctrl+Shift+P          - Command palette
Ctrl+F                - Find
Ctrl+H                - Find/Replace
Ctrl+/                - Comment/uncomment
Ctrl+Space            - IntelliSense
Alt+Shift+Up/Down     - Duplicate line
Ctrl+Shift+L          - Select all occurrences
F2                    - Rename symbol
```

---

## Browser DevTools Setup

### Chrome DevTools for React
1. Install **React Developer Tools** extension
2. Inspect React components directly
3. View props and state changes in real-time

### Browser Console Debugging
```javascript
// Test API in console
fetch('http://localhost:3000/api/health')
  .then(r => r.json())
  .then(console.log)
  .catch(console.error);
```

---

## Collaboration Setup

### Git Configuration
```bash
git config user.name "Your Name"
git config user.email "your@email.com"
```

### Live Share (optional)
1. Install **Live Share** extension
2. Press `Ctrl+Shift+P`
3. Select "Live Share: Start Collaboration Session"
4. Share link with teammates

---

## Resources

- **VS Code Docs**: https://code.visualstudio.com/docs
- **Extensions Marketplace**: https://marketplace.visualstudio.com
- **Keyboard Shortcuts PDF**: https://code.visualstudio.com/docs/getstarted/keybindings

---

Last Updated: 2026-05-17
Ready for development!
