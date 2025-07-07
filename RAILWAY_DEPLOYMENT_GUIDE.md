# Railway Deployment Guide for Claude

## Overview
This guide helps Claude diagnose and fix Railway deployment issues for any app type. The key is identifying the app type and using the correct deployment pattern.

## 🔍 Step 1: Diagnose the App Type

First, examine the project files to determine what type of app you're dealing with:

### Static HTML App
**Files present:**
- `index.html` (main file)
- Maybe `style.css`, `script.js`
- No server code

**Deploy pattern:** Static file server

### Node.js App  
**Files present:**
- `package.json`
- `app.js` or `server.js` or `index.js`
- `node_modules/` (maybe)

**Deploy pattern:** Node.js server

### Python Web App
**Files present:**
- `main.py` or `app.py`  
- `requirements.txt` or `pyproject.toml`
- Uses Flask/FastAPI/Django

**Deploy pattern:** Python web server

### React/Vue/Angular App
**Files present:**
- `package.json` with build scripts
- `src/` folder
- Framework-specific config files

**Deploy pattern:** Build then serve

## 🚀 Step 2: Apply the Correct Pattern

### Pattern A: Static HTML App

**Required files:**
```
project/
├── index.html
├── main.py
├── Procfile  
└── requirements.txt (empty)
```

**main.py:**
```python
import http.server
import socketserver
import os

PORT = int(os.environ.get("PORT", 8080))

class MyHTTPRequestHandler(http.server.SimpleHTTPRequestHandler):
    def end_headers(self):
        self.send_header('Cache-Control', 'no-cache, no-store, must-revalidate')
        self.send_header('Pragma', 'no-cache')
        self.send_header('Expires', '0')
        super().end_headers()

with socketserver.TCPServer(("0.0.0.0", PORT), MyHTTPRequestHandler) as httpd:
    print(f"Server running on port {PORT}")
    httpd.serve_forever()
```

**Procfile:**
```
web: python main.py
```

**requirements.txt:**
```
(empty file)
```

### Pattern B: Node.js App

**Required files:**
```
project/
├── package.json
├── server.js (or app.js)
└── Procfile
```

**package.json:**
```json
{
  "name": "my-app",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.0"
  }
}
```

**Procfile:**
```
web: npm start
```

**server.js example:**
```javascript
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

app.use(express.static('.'));
app.get('/', (req, res) => res.sendFile(__dirname + '/index.html'));

app.listen(PORT, '0.0.0.0', () => {
    console.log(`Server running on port ${PORT}`);
});
```

### Pattern C: Python Web App (FastAPI/Flask)

**Required files:**
```
project/
├── main.py
├── requirements.txt
└── Procfile
```

**FastAPI main.py:**
```python
from fastapi import FastAPI
import os

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}

if __name__ == "__main__":
    import uvicorn
    port = int(os.environ.get("PORT", 8080))
    uvicorn.run(app, host="0.0.0.0", port=port)
```

**requirements.txt:**
```
fastapi
uvicorn
```

**Procfile:**
```
web: uvicorn main:app --host 0.0.0.0 --port $PORT
```

### Pattern D: React/Vue Build App

**Required files:**
```
project/
├── package.json (with build script)
├── src/
├── Procfile
└── server.js (for serving)
```

**package.json:**
```json
{
  "name": "react-app",
  "scripts": {
    "build": "react-scripts build",
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.0",
    "react-scripts": "^5.0.0"
  }
}
```

**server.js:**
```javascript
const express = require('express');
const path = require('path');
const app = express();

app.use(express.static(path.join(__dirname, 'build')));

app.get('*', (req, res) => {
    res.sendFile(path.join(__dirname, 'build', 'index.html'));
});

const PORT = process.env.PORT || 5000;
app.listen(PORT, '0.0.0.0');
```

**Procfile:**
```
web: npm run build && npm start
```

## 🔧 Step 3: Common Issues & Fixes

### Issue: "Service Unavailable" / Health Check Failing

**Cause:** Server not binding to correct host/port

**Fix:** Always use:
- **Host:** `0.0.0.0` (not `localhost` or `127.0.0.1`)
- **Port:** `process.env.PORT` or `os.environ.get("PORT")`

### Issue: "No package.json found" 

**Cause:** Conflicting deployment files

**Fix:** Choose ONE pattern:
- Delete conflicting files (e.g., if using Python, delete `package.json`)
- Keep only files for your chosen pattern

### Issue: "Build failed"

**Cause:** Missing dependencies or wrong build command

**Fix:** 
- Ensure all dependencies listed in `requirements.txt` or `package.json`
- Check build logs for specific error
- Verify start command in `Procfile`

### Issue: Port binding errors

**Cause:** Not using Railway's PORT environment variable

**Fix:** Always use `$PORT` in commands:
```bash
# Good
uvicorn main:app --host 0.0.0.0 --port $PORT

# Bad  
uvicorn main:app --host 0.0.0.0 --port 8080
```

## 🎯 Step 4: Deployment Checklist

Before deploying, verify:

- [ ] **One clear app type** - not mixing Node.js + Python configs
- [ ] **Procfile present** - tells Railway how to start the app
- [ ] **Correct host binding** - `0.0.0.0`, never `localhost`
- [ ] **PORT environment variable used** - `$PORT` in commands
- [ ] **Dependencies specified** - all packages listed
- [ ] **No conflicting files** - choose one deployment pattern

## 🚨 Emergency Fixes

### Quick Fix 1: Static HTML → Python Server
If you have just HTML files and deployment is failing:

1. Create `main.py` (use Pattern A code above)
2. Create `Procfile` with: `web: python main.py`
3. Create empty `requirements.txt`
4. Delete any `package.json` or `railway.toml`
5. Deploy

### Quick Fix 2: Node.js App Issues
If you have a Node.js app that's failing:

1. Ensure `package.json` has `"start"` script
2. Create `Procfile` with: `web: npm start`
3. Make sure server binds to `0.0.0.0` and `process.env.PORT`
4. Delete any Python files (`main.py`, `requirements.txt`)
5. Deploy

### Quick Fix 3: Clear All Conflicts
If nothing works and you have mixed files:

1. **Identify your main app file** (`index.html`, `app.js`, `main.py`)
2. **Choose the simplest pattern** for that file type
3. **Delete ALL other deployment files**
4. **Create ONLY the files for your chosen pattern**
5. **Deploy**

## 🎪 Railway-Specific Notes

### Domain Generation
- After successful deployment, click "Generate Domain" in Settings → Networking
- Railway creates URLs like: `https://app-production-xyz.up.railway.app`
- Don't guess the URL - always generate it

### Environment Variables
- Railway automatically provides `PORT`
- Always use `$PORT` in start commands
- Check Variables tab if app needs custom env vars

### Build Logs
- Always check "Deployments" tab for build errors
- "Deployment succeeded" but "Health check failed" = server binding issue
- Look for actual error messages, not just status

## 🧠 Diagnostic Questions

When helping users, ask:

1. **"What's your main file?"** - HTML? JavaScript? Python?
2. **"Show me your current files"** - understand the structure
3. **"What error do you see?"** - build failure vs health check failure
4. **"Do you have a working example?"** - copy successful patterns

## 📝 Templates for Common Responses

### For Static HTML Apps:
"I see you have an HTML file. Let's use the Python static server pattern that Railway handles well. You'll need to create a simple `main.py` file that serves your HTML..."

### For Mixed/Confused Setups:
"You have both `package.json` and `requirements.txt` which is confusing Railway. Let's pick one approach and delete the conflicting files..."

### For Health Check Failures:
"Your build succeeded but health check failed. This means your server isn't binding to the right host. Make sure you're using `0.0.0.0` as the host and `$PORT` for the port..."

## 🎯 Success Pattern

The most reliable pattern for any app type:

1. **One app type** - don't mix technologies
2. **Procfile** - explicit start command
3. **Correct binding** - `0.0.0.0:$PORT`
4. **Dependencies listed** - complete requirements
5. **Test locally first** - if it works locally with these settings, it'll work on Railway

This pattern works for 95% of Railway deployments.
