# StoxAI Setup Guide - Missing Components

This document outlines what's missing to run the StoxAI (TradingAgents) project.

## 🔍 Project Overview

StoxAI is a multi-agent LLM financial trading framework with:
- **Backend**: FastAPI server with WebSocket support
- **Frontend**: React web application
- **CLI**: Command-line interface for trading analysis
- **Core**: TradingAgents framework with multiple AI agents

## ❌ Missing Components

### 1. Backend Dependencies Missing from `backend/requirements.txt`

The following packages are **required** but **NOT listed** in `backend/requirements.txt`:

```txt
fastapi
uvicorn
python-dotenv
pydantic
```

These packages are imported in:
- `backend/main.py` - Uses FastAPI, WebSocket, Pydantic, python-dotenv
- `backend/run.py` - Uses uvicorn, python-dotenv
- `tradingagents/graph/trading_graph.py` - Uses python-dotenv

**Note**: While these might be in the root `requirements.txt`, the backend has its own `requirements.txt` that should be complete for standalone backend usage.

### 2. Environment Variables (.env file)

The project requires a `.env` file at the root directory with:

```bash
# Required API Keys
GOOGLE_API_KEY=your_google_api_key_here
FINNHUB_API_KEY=your_finnhub_api_key_here

# Optional: Alpaca Trading (for live trading)
ALPACA_API_KEY=your_alpaca_key_here
ALPACA_SECRET_KEY=your_alpaca_secret_here
ALPACA_BASE_URL=https://paper-api.alpaca.markets

# Optional: TradingAgents Configuration
TRADINGAGENTS_RESULTS_DIR=./results

# Optional: Server Configuration
HOST=0.0.0.0
PORT=8000
WORKERS=1
RELOAD=false
```

**Note**: There's an `backend/env.example` file, but no actual `.env` file at the root.

### 3. Frontend Dependencies

The frontend directory exists but may need:
- `node_modules` installed (run `npm install` in `frontend/` directory)
- `.env` file in `frontend/` directory (optional):
  ```bash
  REACT_APP_API_URL=http://localhost:8000
  REACT_APP_WS_URL=ws://localhost:8000
  ```

### 4. Package Installation

The `tradingagents` package needs to be installed as a package. You can either:
- Install it: `pip install -e .` (from project root)
- Or ensure the Python path includes the project root

### 5. Hardcoded Path Issue

In `tradingagents/default_config.py` line 6, there's a hardcoded path:
```python
"data_dir": "/Users/yluo/Documents/Code/ScAI/FR1-data",
```
This path may not exist on your system. It should be configurable or removed if not needed.

## ✅ Setup Steps

### Step 1: Install Root Dependencies
```bash
cd /Users/vrukshalpatel/StoxAI
pip install -r requirements.txt
```

### Step 2: Install Backend Dependencies (including missing ones)
```bash
cd backend
pip install -r requirements.txt
# Install missing dependencies
pip install fastapi uvicorn python-dotenv pydantic

# OR fix the requirements.txt file (see Quick Fix below)
```

### Step 3: Create .env File
```bash
# From project root
cp backend/env.example .env
# Then edit .env with your actual API keys
```

### Step 4: Install TradingAgents Package
```bash
# From project root
pip install -e .
```

### Step 5: Install Frontend Dependencies
```bash
cd frontend
npm install
```

### Step 6: Get API Keys

1. **Google API Key** (for Gemini LLM):
   - Visit: https://makersuite.google.com/app/apikey
   - Create an API key for Google Gemini

2. **FinnHub API Key**:
   - Visit: https://finnhub.io/
   - Sign up and get your free API key

3. **Alpaca API Keys** (optional, for live trading):
   - Visit: https://alpaca.markets/
   - Create a paper trading account

### Step 7: Fix Hardcoded Path (Optional)
Edit `tradingagents/default_config.py` and either:
- Update the path to a valid directory
- Set it to `None` or an empty string if not needed
- Make it configurable via environment variable

## 🚀 Running the Project

### Option 1: Backend Only
```bash
cd backend
python main.py
# or
python run.py
```

### Option 2: Frontend + Backend
```bash
# Terminal 1: Start backend
cd backend
python main.py

# Terminal 2: Start frontend
cd frontend
npm start
```

### Option 3: CLI Only
```bash
# After installing the package
tradingagents analyze
# or
python -m cli.main analyze
```

## 📋 Summary Checklist

- [ ] Install root dependencies: `pip install -r requirements.txt`
- [ ] Install missing backend dependencies: `fastapi`, `uvicorn`, `python-dotenv`, `pydantic`
- [ ] Create `.env` file with API keys
- [ ] Install tradingagents package: `pip install -e .`
- [ ] Install frontend dependencies: `npm install` in `frontend/`
- [ ] Get Google API key for Gemini
- [ ] Get FinnHub API key
- [ ] (Optional) Fix hardcoded path in `default_config.py`
- [ ] (Optional) Get Alpaca keys for live trading

## 🔗 Key Files Location

- Backend API: `backend/main.py`
- Backend Config: `backend/env.example` → should be `.env` at root
- Frontend: `frontend/`
- CLI: `cli/main.py`
- Core Framework: `tradingagents/`
- Default Config: `tradingagents/default_config.py`

## ⚠️ Known Issues

1. **Backend requirements.txt incomplete** - Missing FastAPI, uvicorn, python-dotenv, pydantic
2. **No .env file** - Must be created from env.example
3. **Hardcoded path** - `/Users/yluo/Documents/Code/ScAI/FR1-data` may not exist
4. **Package installation** - tradingagents needs to be installed as a package

## 🔧 Quick Fix

### Fix Backend requirements.txt

Add these lines to `backend/requirements.txt`:
```txt
fastapi
uvicorn[standard]
python-dotenv
pydantic
```

Or run:
```bash
echo -e "fastapi\nuvicorn[standard]\npython-dotenv\npydantic" >> backend/requirements.txt
```

### Create .env File

```bash
# From project root
cat > .env << EOF
GOOGLE_API_KEY=your_google_api_key_here
FINNHUB_API_KEY=your_finnhub_api_key_here
TRADINGAGENTS_RESULTS_DIR=./results
EOF
```
