# Setup
The instructions support:

* 🍎 macOS
* 🪟 Windows

---

# 1. Requirements

Before starting, make sure you have:

* Git
* Python 3.12.x
* PostgreSQL 16
* Odoo 19.0 source code
* Internet connection

> **Important:** Odoo 19 requires a compatible Python environment. This guide uses **Python 3.12.11**.

---

# 2. Get the Odoo Source Code

If you have not cloned the project yet:

```bash
git clone https://github.com/odoo/odoo.git
cd odoo
```

Switch to Odoo 19:

```bash
git checkout 19.0
```

Verify:

```bash
git branch --show-current
```

Expected:

```text
19.0
```

---

# 🍎 macOS Setup

## 3. Install Homebrew

If Homebrew is not installed, install it first.

Check whether Homebrew is already installed:

```bash
brew --version
```

If the command is not found, install Homebrew from the official website.

---

## 4. Install pyenv

Install pyenv:

```bash
brew install pyenv
```

Configure pyenv:

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```

Reload:

```bash
source ~/.zshrc
```

Verify:

```bash
pyenv --version
```

---

## 5. Install XZ

Install XZ because Python needs it for the `lzma` module:

```bash
brew install xz
```

---

## 6. Install Python 3.12.11

```bash
pyenv install 3.12.11
```

Set it for the Odoo project:

```bash
pyenv local 3.12.11
```

Verify:

```bash
python --version
```

Expected:

```text
Python 3.12.11
```

Check LZMA:

```bash
python -c "import lzma; print('LZMA OK')"
```

Expected:

```text
LZMA OK
```

---

## 7. Create the Python Virtual Environment

From the Odoo directory:

```bash
python -m venv .venv
```

Activate:

```bash
source .venv/bin/activate
```

Verify:

```bash
python --version
```

Expected:

```text
Python 3.12.11
```

---

## 8. Install PostgreSQL 16

Install:

```bash
brew install postgresql@16
```

Start PostgreSQL:

```bash
brew services start postgresql@16
```

Add PostgreSQL to PATH:

```bash
export PATH="/usr/local/opt/postgresql@16/bin:$PATH"
```

Make the PATH permanent:

```bash
echo 'export PATH="/usr/local/opt/postgresql@16/bin:$PATH"' >> ~/.zshrc
```

Reload:

```bash
source ~/.zshrc
```

Check:

```bash
pg_config --version
```

and:

```bash
psql --version
```

---

## 9. Initialize PostgreSQL

If PostgreSQL reports that the data directory does not exist:

```bash
initdb -D /usr/local/var/postgresql@16
```

Then:

```bash
brew services restart postgresql@16
```

Verify:

```bash
brew services list | grep postgresql
```

Expected:

```text
postgresql@16 started
```

Check:

```bash
pg_isready
```

Expected:

```text
/tmp:5432 - accepting connections
```

Test:

```bash
psql postgres
```

You should see:

```text
postgres=#
```

Exit:

```sql
\q
```

---

# 🪟 Windows Setup

## 3. Install Git

Install Git for Windows.

After installation, open **Git Bash** or PowerShell and check:

```powershell
git --version
```

---

## 4. Install Python 3.12.11

Download and install **Python 3.12.11** for Windows.

During installation:

### IMPORTANT

Enable:

```text
Add python.exe to PATH
```

Then select the option to install Python.

After installation, open a **new terminal**.

Check:

```powershell
python --version
```

Expected:

```text
Python 3.12.11
```

Also check:

```powershell
python -m pip --version
```

> If Windows opens the Microsoft Store when you type `python`, disable the Windows **App execution aliases** for Python or use the Python installation directly.

---

# 5. Install PostgreSQL 16 on Windows

Install **PostgreSQL 16** using the official Windows installer.

During installation, remember the password you create for the PostgreSQL `postgres` user.

Recommended settings:

```text
Port: 5432
Username: postgres
```

Keep the default PostgreSQL installation directory unless your organization has another standard.

After installation, PostgreSQL normally runs automatically as a Windows service.

---

# 6. Verify PostgreSQL on Windows

Open a new PowerShell window:

```powershell
psql --version
```

Expected:

```text
psql (PostgreSQL) 16.x
```

If Windows says:

```text
'psql' is not recognized
```

add PostgreSQL's `bin` directory to the Windows PATH.

Typical location:

```text
C:\Program Files\PostgreSQL\16\bin
```

You can temporarily test it with:

```powershell
& "C:\Program Files\PostgreSQL\16\bin\psql.exe" --version
```

---

# 7. Test PostgreSQL

Try:

```powershell
psql -U postgres -h localhost
```

Enter the PostgreSQL password created during installation.

If successful, you should see:

```text
postgres=#
```

Exit:

```sql
\q
```

---

# 8. Clone the Odoo Project on Windows

Open **PowerShell** or **Git Bash**.

Example:

```powershell
cd Desktop
git clone https://github.com/odoo/odoo.git
cd odoo
```

Switch to Odoo 19:

```powershell
git checkout 19.0
```

Verify:

```powershell
git branch --show-current
```

Expected:

```text
19.0
```

---

# 9. Create the Python Virtual Environment

From the Odoo directory:

```powershell
python -m venv .venv
```

Activate it in PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

You should see:

```text
(.venv)
```

at the beginning of the terminal.

### If PowerShell blocks activation

If you receive an execution-policy error, run:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Then activate again:

```powershell
.\.venv\Scripts\Activate.ps1
```

---

# 10. Upgrade pip

With `.venv` activated:

```powershell
python -m pip install --upgrade pip setuptools wheel
```

Always prefer:

```text
python -m pip
```

instead of:

```text
pip
```

This ensures the correct virtual environment is being used.

---

# 11. Install Odoo Dependencies

From the Odoo root directory:

### macOS

```bash
python -m pip install -r requirements.txt
```

### Windows

```powershell
python -m pip install -r requirements.txt
```

This can take some time because Odoo has many Python dependencies.

---

# 12. Configure PostgreSQL for Odoo

Odoo needs a PostgreSQL database user.

On both macOS and Windows, you can create an Odoo PostgreSQL user.

Connect as the PostgreSQL administrator.

### macOS

```bash
psql postgres
```

### Windows

```powershell
psql -U postgres -h localhost
```

Create an Odoo database user:

```sql
CREATE USER odoo WITH CREATEDB LOGIN PASSWORD 'odoo';
```

Then exit:

```sql
\q
```

> For a local development environment, the password above is only an example. Do not use this password for production.

---

# 13. Run Odoo

Make sure:

1. PostgreSQL is running
2. `.venv` is activated
3. You are inside the Odoo root directory

### macOS

```bash
cd ~/Desktop/projects/odoo
source .venv/bin/activate
python odoo-bin
```

### Windows PowerShell

```powershell
cd "$HOME\Desktop\odoo"
.\.venv\Scripts\Activate.ps1
python odoo-bin
```

A successful startup should show:

```text
Odoo version 19.0
```

and:

```text
HTTP service (werkzeug) running on ...:8069
```

---

# 14. Open Odoo

Open your browser:

```text
http://localhost:8069
```

Odoo uses port:

```text
8069
```

by default.

---

# 15. Normal Startup

After the initial installation is complete, staff do not need to reinstall everything.

## macOS

```bash
cd ~/Desktop/projects/odoo
source .venv/bin/activate
python odoo-bin
```

## Windows

```powershell
cd "$HOME\Desktop\odoo"
.\.venv\Scripts\Activate.ps1
python odoo-bin
```

Then open:

```text
http://localhost:8069
```

---

# 16. Stop Odoo

To stop the Odoo development server:

```text
Ctrl + C
```

PostgreSQL can remain running.

---

# 17. PostgreSQL Troubleshooting

## macOS

Check:

```bash
brew services list | grep postgresql
```

Then:

```bash
pg_isready
```

Expected:

```text
/tmp:5432 - accepting connections
```

If PostgreSQL is stopped:

```bash
brew services start postgresql@16
```

---

## Windows

Check whether PostgreSQL is running:

1. Press `Win + R`
2. Type:

```text
services.msc
```

3. Find the PostgreSQL service.
4. Make sure the PostgreSQL 16 service is **Running**.

You can also test:

```powershell
pg_isready -h localhost -p 5432
```

Expected:

```text
localhost:5432 - accepting connections
```

---

# 18. Common Problems

## `pip: command not found`

Use:

```bash
python -m pip
```

instead of:

```bash
pip
```

Example:

```bash
python -m pip install -r requirements.txt
```

---

## Python version is wrong

Check:

```bash
python --version
```

Odoo development should use:

```text
Python 3.12.11
```

### macOS

```bash
pyenv local 3.12.11
```

### Windows

Make sure Python 3.12 is installed and appears first in PATH.

---

## `No module named '_lzma'`

This is primarily relevant to the pyenv-based macOS setup.

Install:

```bash
brew install xz
```

Then reinstall Python:

```bash
pyenv uninstall -f 3.12.11
pyenv install 3.12.11
pyenv local 3.12.11
```

Verify:

```bash
python -c "import lzma; print('LZMA OK')"
```

---

## `pg_config executable not found`

This usually means PostgreSQL's development tools are not available in PATH.

### macOS

```bash
export PATH="/usr/local/opt/postgresql@16/bin:$PATH"
```

Then:

```bash
pg_config --version
```

### Windows

Add:

```text
C:\Program Files\PostgreSQL\16\bin
```

to the Windows PATH.

Then open a new terminal and run:

```powershell
pg_config --version
```

---

# 19. Verify Complete Installation

Run:

### macOS

```bash
python --version
pg_config --version
psql --version
pg_isready
```

### Windows

```powershell
python --version
pg_config --version
psql --version
pg_isready -h localhost -p 5432
```

Then:

```text
python odoo-bin
```

A successful setup should have:

```text
Python 3.12.11
PostgreSQL 16.x
PostgreSQL accepting connections
Odoo version 19.0
HTTP service running on port 8069
```

Open:

```text
http://localhost:8069
```

---

# 20. Git / Project Rules

Do not commit the Python virtual environment.

Add this to `.gitignore`:

```gitignore
.venv/
```

Other local files containing passwords or machine-specific configuration should also not be committed.

---

# 21. Quick Start After Installation

Once a developer has completed the initial setup:

### 🍎 macOS

```bash
cd ~/Desktop/projects/odoo
source .venv/bin/activate
python odoo-bin
```

### 🪟 Windows

```powershell
cd "$HOME\Desktop\odoo"
.\.venv\Scripts\Activate.ps1
python odoo-bin
```

Open:

```text
http://localhost:8069
```

---

# Development Environment Summary

| Component              | Version                      |
| ---------------------- | ---------------------------- |
| Odoo                   | 19.0                         |
| Python                 | 3.12.11                      |
| PostgreSQL             | 16.x                         |
| Virtual Environment    | `.venv`                      |
| Odoo Port              | 8069                         |
| PostgreSQL Port        | 5432                         |
| macOS Python Manager   | pyenv                        |
| Windows Python Manager | python.org installer         |
| macOS PostgreSQL       | Homebrew                     |
| Windows PostgreSQL     | PostgreSQL Windows Installer |

---

# Important

This README is intended for **local development**.

It is not a production deployment guide.

For production, Odoo should be configured with appropriate:

* PostgreSQL security
* Odoo configuration
* Passwords/secrets
* Reverse proxy
* HTTPS
* Firewall
* Backups
* Workers
* Logging
* File storage
* Access control
