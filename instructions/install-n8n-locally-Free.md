# Installing n8n on Mac - Step by Step Guide

## Step 1: Remove Any Existing Node.js (Optional)

If you have an existing Node.js installation that might conflict:

```bash
# Check current version
node --version

# If you need to remove existing Node.js installed via Homebrew
brew uninstall node
brew uninstall node@18
brew uninstall node@22
```

## Step 2: Install Node.js 20 LTS

### **Option A: Using NVM (Recommended - Easier Version Management)**

```bash
# Install NVM (Node Version Manager)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Reload your terminal configuration
source ~/.zshrc
# OR if using bash
source ~/.bashrc

# Install Node.js 20 LTS
nvm install 20
nvm use 20
nvm alias default 20

# Verify installation
node --version  # Should show v20.x.x
npm --version   # Should show 10.x.x or higher
```

### **Option B: Using Homebrew**

```bash
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Node.js 20
brew install node@20

# Link Node.js 20 as default
brew link --force --overwrite node@20

# Add to PATH (add this to ~/.zshrc or ~/.bash_profile)
echo 'export PATH="/opt/homebrew/opt/node@20/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# Verify installation
node --version  # Should show v20.x.x
npm --version
```

## Step 3: Install n8n Globally

```bash
# Install n8n using npm
npm install n8n -g

# If you get permission errors, try:
sudo npm install n8n -g
```

## Step 4: Verify n8n Installation

```bash
# Check n8n is installed
n8n -v

# Check installation location
which n8n
```

## Step 5: Start n8n and Access Dashboard

Mac users can use `export` in Command Prompt:

```bash
export N8N_SECURE_COOKIE=false
n8n
```

If the above doesn't work, use the following:

```bash
export N8N_HOST=localhost
export N8N_PROTOCOL=http
export N8N_SECURE_COOKIE=false
n8n
```

After running these commands:
- n8n will start and display: `Editor is now accessible via: http://localhost:5678/`
- **Press 'o' to open the browser** to the dashboard
- Or manually navigate to `http://localhost:5678`
- Create your owner account on first access
- You'll see the n8n dashboard where you can start building workflows!

## Optional Configurations

### Run n8n with Different Settings:

```bash
# Different port
n8n start --port=8080

# With tunnel for webhooks (useful for testing)
n8n start --tunnel

# Specify custom data folder
export N8N_USER_FOLDER=/Users/$(whoami)/n8n-data
n8n start
```

## Troubleshooting Common Issues

### Issue 1: Command not found
```bash
# If n8n command not found after installation
export PATH="$PATH:$(npm config get prefix)/bin"
echo 'export PATH="$PATH:$(npm config get prefix)/bin"' >> ~/.zshrc
source ~/.zshrc
```

### Issue 2: Permission Errors
```bash
# Fix npm permissions
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
npm install n8n -g
```

### Issue 3: Port Already in Use
```bash
# Check what's using port 5678
lsof -i :5678

# Kill the process or use different port
n8n start --port=5679
```

## Keeping Everything Updated

```bash
# Update n8n to latest version
npm update -g n8n

# Check for updates
npm outdated -g

# If using nvm, update Node.js within v20
nvm install 20 --reinstall-packages-from=20
```

## Quick Verification Checklist

Run these commands to verify everything is set up correctly:

```bash
node --version      # Should show v20.x.x
npm --version       # Should show 10.x.x or higher
n8n -v             # Should show n8n version
which n8n          # Should show n8n installation path
```

# Installing n8n on Windows - Step by Step Guide

## Step 1: Remove Any Existing Node.js (Optional)

If you have an existing Node.js installation that might conflict:

```cmd
# Check current version
node --version

# If you need to uninstall existing Node.js:
# Go to Control Panel > Programs > Uninstall a program
# Find Node.js and uninstall it
```

## Step 2: Install Node.js 20 LTS on Windows

### **Option A: Direct Download (Simplest)**

1. Go to https://nodejs.org/en/download/
2. Click on "Previous Releases" or go to https://nodejs.org/en/download/releases/
3. Find Node.js 20 LTS (v20.x.x)
4. Download the Windows Installer (.msi) for your system (64-bit recommended)
5. Run the installer:
   - Check "Automatically install the necessary tools" during installation
   - Click Next through the installation wizard
   - Restart your command prompt after installation

### **Option B: Using NVM for Windows (Better Version Management)**

```cmd
# Download nvm-windows from:
# https://github.com/coreybutler/nvm-windows/releases

# After installing nvm-windows, open a NEW Command Prompt as Administrator

# Install Node.js 20
nvm install 20
nvm use 20

# Verify installation
node --version
npm --version
```

### **Option C: Using Chocolatey**

```cmd
# First, install Chocolatey if not installed
# Run PowerShell as Administrator and execute:
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Then install Node.js 20
choco install nodejs --version=20.18.0

# Verify installation
node --version
npm --version
```

## Step 3: Verify Node.js Installation

Open a **new** Command Prompt or PowerShell window:

```cmd
node --version
# Should show v20.x.x

npm --version  
# Should show 10.x.x or higher
```

## Step 4: Install n8n Globally

Open Command Prompt or PowerShell **as Administrator**:

```cmd
# Install n8n using npm
npm install n8n -g

# Wait for installation to complete (may take a few minutes)
```

If you get errors, try:
```cmd
# Clear npm cache first
npm cache clean --force

# Then reinstall
npm install n8n -g
```

## Step 5: Verify n8n Installation

```cmd
# Check n8n is installed
n8n -v

# Check installation location  
where n8n
```

## Step 6: Start n8n and Access Dashboard

Windows users can use `set` in Command Prompt:

```cmd
set N8N_SECURE_COOKIE=false
n8n
```

If the above doesn't work, use the following:

```cmd
set N8N_HOST=localhost
set N8N_PROTOCOL=http
set N8N_SECURE_COOKIE=false
n8n
```

**For PowerShell users**, use:

```powershell
$env:N8N_SECURE_COOKIE="false"
n8n
```

Or if that doesn't work:

```powershell
$env:N8N_HOST="localhost"
$env:N8N_PROTOCOL="http"
$env:N8N_SECURE_COOKIE="false"
n8n
```

After running these commands:
- n8n will start and display: `Editor is now accessible via: http://localhost:5678/`
- **Press 'o' to open the browser** to the dashboard
- Or manually navigate to `http://localhost:5678`
- Create your owner account on first access
- You'll see the n8n dashboard where you can start building workflows!

## Optional Configurations

### Run n8n with Different Settings:

```cmd
# Different port
n8n start --port=8080

# With tunnel for webhooks (useful for testing)
n8n start --tunnel

# Specify custom data folder
set N8N_USER_FOLDER=C:\Users\%USERNAME%\n8n-data
n8n start
```

## Troubleshooting Common Windows Issues

### Issue 1: 'n8n' is not recognized as a command

```cmd
# Add npm global packages to PATH
# First, find npm global prefix
npm config get prefix

# Add this path + \bin to your System PATH:
# 1. Right-click "This PC" > Properties
# 2. Click "Advanced system settings"
# 3. Click "Environment Variables"
# 4. Under System Variables, find PATH
# 5. Add: C:\Users\%USERNAME%\AppData\Roaming\npm
# 6. Restart Command Prompt
```

### Issue 2: Permission Errors

```cmd
# Always run Command Prompt as Administrator when installing
# Right-click Command Prompt > Run as Administrator

# Or change npm default directory:
mkdir C:\npm-global
npm config set prefix C:\npm-global
# Add C:\npm-global to your PATH environment variable
```

### Issue 3: Port Already in Use

```cmd
# Check what's using port 5678
netstat -ano | findstr :5678

# Kill the process or use different port
n8n start --port=5679
```

### Issue 4: Windows Firewall Blocking

If n8n starts but you can't access it:
1. Windows Security > Firewall & network protection
2. Allow an app through firewall
3. Add Node.js to the allowed list

### Issue 5: Execution Policy (PowerShell)

If you get execution policy errors in PowerShell:

```powershell
# Run PowerShell as Administrator
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

## Keeping Everything Updated

```cmd
# Update n8n to latest version
npm update -g n8n

# Check for updates
npm outdated -g

# If using nvm-windows, update Node.js within v20
nvm install 20.18.0
nvm use 20.18.0
```

## Quick Verification Checklist

Run these commands to verify everything is set up correctly:

```cmd
node --version      # Should show v20.x.x
npm --version       # Should show 10.x.x or higher
n8n -v             # Should show n8n version
where n8n          # Should show n8n installation path
```

## Windows-Specific Tips

1. **Always use a new Command Prompt** after installing Node.js or n8n
2. **Run as Administrator** when installing global npm packages
3. **Windows Defender** might scan n8n files on first run, causing slight delays
4. **Use PowerShell or Windows Terminal** for better experience than Command Prompt
