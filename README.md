# 💰 Finance Goal Tracker — Web Edition

A full-stack personal finance application with a Java REST backend and a browser-based UI.

---

## 📁 Project Structure

```
finance-tracker/
├── src/
│   ├── app/
│   │   └── MainApp.java              ← Entry point (starts web server)
│   ├── auth/
│   │   ├── AuthUser.java             ← Auth user model (SHA-256 hashing)
│   │   └── AuthService.java          ← Login, register, session management
│   ├── api/
│   │   └── ApiServer.java            ← Embedded HTTP REST API server
│   ├── model/
│   │   ├── Expense.java
│   │   ├── Goal.java
│   │   └── User.java
│   ├── service/
│   │   ├── ExpenseService.java
│   │   ├── GoalService.java
│   │   ├── FinanceCalculator.java
│   │   ├── ReportService.java
│   │   └── AutomationService.java
│   ├── ai/
│   │   ├── AIExpenseCategorizer.java
│   │   ├── InsightGenerator.java
│   │   └── OCRService.java
│   └── utils/
│       ├── FileManager.java
│       ├── DateUtil.java
│       └── JsonParserUtil.java
├── webapp/
│   └── index.html                    ← Full browser UI (served at http://localhost:8080)
├── data/                             ← Auto-created: expenses.csv, goals.json, users.json, auth_users.json
├── receipts/                         ← Auto-created: uploaded receipt images
├── libs/                             ← Place optional JARs here (tess4j, gson, etc.)
├── out/                              ← Compiled .class files (auto-generated)
├── run.sh                            ← Linux/macOS build & run
├── run.bat                           ← Windows build & run
└── README.md
```

---

## ✅ Prerequisites

### 1. Java JDK 11 or higher

**Check if installed:**
```bash
java -version
javac -version
```

**Install if not present:**

- **Ubuntu/Debian:**
  ```bash
  sudo apt update
  sudo apt install openjdk-17-jdk
  ```

- **macOS (Homebrew):**
  ```bash
  brew install openjdk@17
  ```

- **Windows:**
  Download from: https://adoptium.net/
  Choose: **Temurin 17 (LTS)** → Windows x64 Installer

---

## 🚀 Running the Application

### Linux / macOS

```bash
# 1. Make the script executable
chmod +x run.sh

# 2. Build and run
./run.sh
```

### Windows

```cmd
run.bat
```

### Manual (any OS)

```bash
# 1. Create output directory
mkdir out

# 2. Compile all source files (excluding Swing DashboardUI if present)
javac -encoding UTF-8 -d out src/app/MainApp.java src/auth/*.java src/api/*.java src/model/*.java src/service/*.java src/ai/*.java src/utils/*.java

# 3. Run the server
java -Dfile.encoding=UTF-8 -cp out app.MainApp
```

> **Windows path separator:** Use `;` instead of `:` in `-cp`  
> ```
> javac -encoding UTF-8 -d out -cp out src\...
> java -Dfile.encoding=UTF-8 -cp out app.MainApp
> ```

---

## 🌐 Using the Application

1. After starting, open your browser and go to:  
   **http://localhost:8080**

2. **Default login credentials:**
   - Username: `admin`
   - Password: `admin123`

3. Or click **Register** to create a new account.

---

## 🔌 Optional Dependencies (JARs)

Place any optional JARs in the `libs/` folder. They are auto-included in the classpath.

### Tesseract OCR (for receipt scanning)

The `OCRService` uses Tess4J for receipt image OCR.

1. Download `tess4j-5.x.x-src.zip` from: https://github.com/nguyenq/tess4j/releases
2. Extract and copy `tess4j.jar` (and dependencies) to `libs/`
3. Download Tesseract language data:
   ```bash
   # Ubuntu
   sudo apt install tesseract-ocr
   # OR download tessdata from https://github.com/tesseract-ocr/tessdata
   ```
4. Set the `TESSDATA_PATH` environment variable:
   ```bash
   export TESSDATA_PATH=/usr/share/tesseract-ocr/4.00/tessdata
   ```

> OCRService gracefully degrades to filename-based fallback if Tess4J is unavailable.

### AI Categorization & Insights (Hugging Face API)

To enable AI-powered expense categorization and insights:

1. Get a free API key from: https://huggingface.co/
2. Set the environment variable:
   ```bash
   # Linux/macOS
   export HUGGINGFACE_API_KEY=hf_your_key_here

   # Windows
   set HUGGINGFACE_API_KEY=hf_your_key_here
   ```
3. Restart the application.

> Without the API key, the app uses keyword-based categorization and rule-based insights — fully functional offline.

---

## 📡 REST API Reference

All endpoints (except auth) require:  
`Authorization: Bearer <token>` header

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/login` | Login → returns token |
| POST | `/api/auth/register` | Register new account |
| POST | `/api/auth/logout` | Logout (invalidate session) |
| GET | `/api/auth/me` | Get current user info |
| GET | `/api/expenses` | List all expenses |
| POST | `/api/expenses` | Add new expense |
| DELETE | `/api/expenses/{id}` | Delete expense |
| GET | `/api/goals` | List all goals |
| POST | `/api/goals` | Add new goal |
| DELETE | `/api/goals/{id}` | Delete goal |
| POST | `/api/goals/{id}/contribute` | Add funds to a goal |
| GET | `/api/dashboard` | Dashboard summary data |
| GET | `/api/insights` | AI-generated financial insights |
| GET | `/api/report/monthly` | Monthly spending report |
| GET | `/api/report/yearly?year=2025` | Yearly overview |
| GET | `/api/categories` | Available expense categories |
| GET | `/api/user` | Get user profile |
| PUT | `/api/user` | Update user profile |

---

## 🔐 Authentication Details

- Passwords are hashed with **SHA-256 + salt** (no plain-text storage)
- Sessions use **secure random tokens** (256-bit, URL-safe Base64)
- Sessions expire after **8 hours**
- Auth data stored in `data/auth_users.json`

---

## 🛠 Changing the Port

Default port is `8080`. To use a different port:

```bash
# Linux/macOS
java -Dfile.encoding=UTF-8 -cp out app.MainApp 9090

# Windows
java "-Dfile.encoding=UTF-8" -cp out app.MainApp 9090
```

---

## 📂 Data Files

All data is stored in the `data/` directory (auto-created on first run):

| File | Contents |
|------|----------|
| `data/expenses.csv` | All expenses |
| `data/goals.json` | Financial goals |
| `data/users.json` | User profile |
| `data/auth_users.json` | Authentication credentials |
| `data/backups/` | Timestamped auto-backups |

---

## 🐛 Troubleshooting

**"javac not found"**  
→ JDK not installed or not in PATH. Install JDK 11+ and add `bin/` to PATH.

**"Address already in use"**  
→ Port 8080 is busy. Use a different port: `java -cp out app.MainApp 8081`

**"Cannot connect to server"**  
→ Server not running. Run `./run.sh` first, then open the browser.

**Blank page or 404**  
→ Make sure you're running from the project root directory (where `webapp/` exists).

**OCR not working**  
→ Optional feature. Set `TESSDATA_PATH` env variable and add tess4j.jar to `libs/`.

