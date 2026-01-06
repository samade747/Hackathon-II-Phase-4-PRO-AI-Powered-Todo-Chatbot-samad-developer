# Phase IV Installation Guide: Kubernetes Deployment

This guide provides step-by-step instructions for installing all required tools for Phase IV Kubernetes deployment of the AI-Powered Todo Chatbot.

## Profile Information

**Developer**: Samad Developer AI Agents Dev
**GitHub**: [samad-developer](https://github.com/samad-developer)
**LinkedIn**: [Web Developer & MERN Stack Specialist](https://www.linkedin.com/in/webdeveloper-mern-stack-specialist-samaddeveloper-aiagentsdev/)
**WhatsApp**: +923328222026
**Views**: This installation guide has been viewed by developers worldwide working on AI-powered applications and Kubernetes deployments.

## Prerequisites

Before starting the installation, ensure your system meets the following requirements:
- Windows 10/11, macOS, or Linux
- At least 8GB RAM (16GB recommended)
- At least 20GB free disk space
- Administrator/root privileges for installation

## Required Components

Phase IV requires the following tools for Kubernetes deployment:

1. **Docker** - Containerization platform
2. **Minikube** - Local Kubernetes cluster
3. **Helm** - Kubernetes package manager
4. **kubectl-ai** - AI-enhanced kubectl
5. **kagent** - AI agent for Kubernetes

**Note**: Standard `kubectl` is included as part of the Kubernetes CLI tools installation, which is required for kubectl-ai to function properly.

## About the AI Tools

### kubectl-ai
kubectl-ai is an AI-enhanced version of kubectl that provides intelligent suggestions and automation for Kubernetes operations. It can help with:
- Explaining Kubernetes resources and configurations
- Generating kubectl commands based on natural language
- Providing best practices and troubleshooting suggestions
- Automating repetitive tasks

### kagent
kagent is an AI-powered agent for Kubernetes operations that can automate deployment and management tasks. It may provide features such as:
- Automated deployment management
- Intelligent monitoring and alerting
- Predictive scaling based on usage patterns
- AI-assisted troubleshooting and remediation

## Windows-Specific Prerequisites

Before installing the required tools on Windows, ensure the following:

### 1. Windows Version Requirements
- Windows 10 Pro, Enterprise, or Education (64-bit) version 1909 or later
- OR Windows 11 Pro, Enterprise, or Education (64-bit)
- Home editions may work but are not recommended

### 2. Enable Virtualization in BIOS
- Restart your computer and enter BIOS/UEFI settings
- Enable Intel VT-x or AMD-V virtualization technology
- Save settings and restart

### 3. Enable Windows Features
- **WSL 2 (Windows Subsystem for Linux)**: Required for Docker Desktop
- **Hyper-V**: Alternative container runtime (optional but recommended)
- **Virtual Machine Platform**: Required for WSL 2

To enable these features:
1. Open "Turn Windows features on or off" from Start menu
2. Check the following boxes:
   - Windows Subsystem for Linux
   - Virtual Machine Platform
   - Hyper-V (if available)
3. Click OK and restart your computer

### 4. PowerShell Execution Policy
- Some installations require changing the PowerShell execution policy
- This will be handled during Chocolatey installation

## Installation Steps

### 1. Install Docker Desktop for Windows

#### Step-by-step Installation:
1. **Download Docker Desktop**:
   - Visit [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
   - Click "Download for Windows"
   - Wait for the download to complete

2. **Run the Installer**:
   - Right-click on the downloaded file and select "Run as administrator"
   - The Docker Desktop Installer will start
   - Click "OK" on the UAC prompt

3. **Follow the Installation Wizard**:
   - Accept the terms and conditions
   - Choose installation location (default is recommended)
   - Select optional components (keep defaults unless you have specific requirements)
   - Check "Use WSL 2 instead of Hyper-V" (recommended for better performance)
   - Click "Install"

4. **Complete Installation**:
   - Docker Desktop will automatically start after installation
   - You may be prompted to restart your computer
   - After restart, Docker Desktop should start automatically

5. **Verify Installation**:
   - Open Command Prompt or PowerShell
   - Run: `docker --version`
   - You should see output similar to: `Docker version 24.x.x, build xxxxx`
   - Run: `docker run hello-world`
   - This should pull and run a test container successfully

#### Troubleshooting Docker Installation:
- If Docker fails to start, check Windows Event Viewer for errors
- Ensure WSL 2 is properly installed and configured
- Try running Docker Desktop as Administrator
- Check that Windows features are properly enabled

#### Configure Docker for Optimal Performance:
- Right-click Docker Desktop icon in system tray
- Select "Settings"
- Go to "Resources"
- Allocate at least 4GB RAM and 2 CPU cores (more if available)
- Set disk space limit to at least 20GB

#### Windows-Specific Docker Settings:
- In Docker Desktop Settings > General, check "Use Docker Compose V2"
- In Docker Desktop Settings > Resources > WSL Integration, enable integration with your WSL distributions if you use WSL

### 2. Install Chocolatey (Windows Package Manager)

Chocolatey simplifies the installation of other tools on Windows:

1. **Open PowerShell as Administrator**:
   - Press Windows key + X
   - Select "Windows PowerShell (Admin)" or "Terminal (Admin)"
   - If prompted by UAC, click "Yes"

2. **Install Chocolatey**:
   - Run the following command:
   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
   ```

3. **Verify Installation**:
   - Close and reopen PowerShell as Administrator
   - Run: `choco --version`
   - You should see the Chocolatey version number

4. **Update Chocolatey (recommended)**:
   ```powershell
   choco upgrade chocolatey
   ```

### 3. Install Minikube using Chocolatey

#### Installation Steps:
1. **Open PowerShell as Administrator**
2. **Install Minikube**:
   ```powershell
   choco install minikube
   ```

3. **Verify Installation**:
   ```bash
   minikube version
   ```
   You should see the Minikube version information.

#### Minikube Configuration for Windows:
- Minikube requires a hypervisor to run Kubernetes locally
- By default, it will try to use Hyper-V if available
- If Hyper-V is not available, it will use VirtualBox or Docker as alternatives

### 4. Install Helm using Chocolatey

#### Installation Steps:
1. **Open PowerShell as Administrator**
2. **Install Helm**:
   ```powershell
   choco install kubernetes-helm
   ```

3. **Verify Installation**:
   ```bash
   helm version
   ```
   You should see the Helm version information.

#### Helm Configuration for Windows:
- Helm will be installed to a standard location and added to your PATH
- Run `helm repo update` after installation to initialize Helm repositories

### 5. Install kubectl (Kubernetes CLI) using Chocolatey

#### Installation Steps:
1. **Open PowerShell as Administrator**
2. **Install kubectl**:
   ```powershell
   choco install kubernetes-cli
   ```

3. **Verify Installation**:
   ```bash
   kubectl version --client
   ```
   You should see the kubectl client version information.

#### Alternative Installation Methods for Windows:
- **Direct Download**: Download from [Kubernetes releases page](https://kubernetes.io/releases/download/)
- **Using curl**:
  ```powershell
  curl -LO "https://dl.k8s.io/release/v1.28.0/bin/windows/amd64/kubectl.exe"
  # Move kubectl.exe to a directory in your PATH
  ```

### 6. Configure Minikube for Windows

#### Choose the Right Driver:
Minikube supports several drivers on Windows. Choose based on your system:

1. **Hyper-V** (Recommended if available):
   - Requires Windows 10 Pro/Enterprise with Hyper-V enabled
   - Run: `minikube start --driver=hyperv`

2. **Docker** (If Docker Desktop is installed):
   - Run: `minikube start --driver=docker`

3. **VirtualBox** (Alternative option):
   - Install VirtualBox separately
   - Run: `minikube start --driver=virtualbox`

#### Recommended Initial Setup:
```bash
# Start Minikube with recommended resources for this project
minikube start --cpus=4 --memory=8192 --disk-size=20g --driver=docker
```

#### Verify Minikube is Running:
```bash
minikube status
```
You should see status information showing Minikube is running.

#### Enable Required Addons:
```bash
minikube addons enable ingress
minikube addons enable metrics-server
```

### 7. Install kubectl-ai (AI-enhanced kubectl)

kubectl-ai is an AI-enhanced version of kubectl that provides intelligent suggestions and automation for Kubernetes operations. Installation method may vary depending on availability:

#### Option 1: Install via kubectl krew plugin manager
```powershell
# First install krew if not already installed
kubectl krew version

# If krew is not installed, install it first:
(Invoke-WebRequest -UseBasicParsing "https://github.com/kubernetes-sigs/krew/releases/latest/download/krew.exe") -OutFile krew.exe
.\krew.exe install krew

# Install the kubectl-ai plugin
kubectl krew install ai
```

#### Option 2: Install from official releases (if available)
```powershell
# Check for official releases on GitHub or other package managers
# This is a placeholder - replace with actual installation command when available
# Download the binary and add it to your PATH
```

#### Option 3: Build from source (if available)
```powershell
# Clone the kubectl-ai repository (if source is available)
git clone https://github.com/your-kubectl-ai-repo/kubectl-ai.git
cd kubectl-ai

# Build the binary (commands may vary based on project)
# This is an example - actual build process may differ
go build -o kubectl-ai

# Move to a directory in your PATH
move kubectl-ai.exe "$env:USERPROFILE\.krew\bin\kubectl-ai.exe"
```

#### Verification:
```cmd
kubectl ai version
```

### 8. Install kagent (AI Agent for Kubernetes)

kagent is an AI-powered agent for Kubernetes operations that can automate deployment and management tasks. Installation method may vary:

#### Option 1: Install via package manager (if available)
```powershell
# Check if available through Chocolatey or other Windows package managers
choco search kagent
# or
choco install kagent  # if available
```

#### Option 2: Install from official releases (if available)
```powershell
# Download from official source (placeholder - replace with actual source)
# This is a placeholder - replace with actual installation command when available
```

#### Option 3: Build from source (if available)
```powershell
# Clone the kagent repository (if source is available)
git clone https://github.com/your-kagent-repo/kagent.git
cd kagent

# Build the binary (commands may vary based on project)
# This is an example - actual build process may differ
go build -o kagent

# Move to a directory in your PATH
move kagent.exe "$env:USERPROFILE\.krew\bin\kagent.exe"
```

#### Verification:
```cmd
kagent version
```

#### Alternative: Using kubectl plugin approach
If kagent is available as a kubectl plugin:
```powershell
kubectl krew install kagent  # if available
```

## Verification Steps

After installing all required components, verify the installations:

### Windows Command Prompt or PowerShell:
```cmd
# Verify Docker
docker --version

# Verify kubectl
kubectl version --client

# Verify Helm
helm version

# Verify Minikube
minikube version

# Verify kubectl-ai (if installed)
kubectl ai version

# Verify kagent (if installed)
kagent version
```

### Expected Output Examples (Windows):
- `docker --version` should return something like: `Docker version 24.x.x, build xxxxxxx`
- `kubectl version --client` should return client version information
- `helm version` should return Helm version information
- `minikube version` should return Minikube version information
- `kubectl ai version` should return kubectl-ai version information (if installed)
- `kagent version` should return kagent version information (if installed)

### Windows-Specific Verification:
If any command returns "'command' is not recognized", ensure that:
1. The installation was completed successfully
2. The installation directory was added to your PATH environment variable
3. You've restarted your command prompt/PowerShell after installation
4. You're running as Administrator if required

### Troubleshooting PATH Issues on Windows:
1. Check current PATH: `echo $env:PATH` (PowerShell) or `echo %PATH%` (Command Prompt)
2. Add to PATH manually if needed through System Properties > Environment Variables
3. Restart your command prompt/PowerShell after making PATH changes

## Starting Minikube on Windows

Once all tools are installed, start your local Kubernetes cluster:

### Windows Command Prompt or PowerShell:
```cmd
# Start Minikube with recommended resources for Windows
minikube start --cpus=4 --memory=8192 --disk-size=20g --driver=docker

# Enable required addons
minikube addons enable ingress
minikube addons enable metrics-server

# Verify Minikube is running
minikube status
```

### Windows-Specific Driver Options:
Minikube supports multiple drivers on Windows. Choose the one that works best for your system:

1. **Docker Driver** (Recommended if Docker Desktop is installed):
   ```cmd
   minikube start --driver=docker --cpus=4 --memory=8192 --disk-size=20g
   ```

2. **Hyper-V Driver** (If Hyper-V is enabled):
   ```cmd
   minikube start --driver=hyperv --cpus=4 --memory=8192 --disk-size=20g
   ```

3. **VirtualBox Driver** (If VirtualBox is installed separately):
   ```cmd
   minikube start --driver=virtualbox --cpus=4 --memory=8192 --disk-size=20g
   ```

### Windows Troubleshooting for Minikube:
- If you get permission errors, run Command Prompt or PowerShell as Administrator
- If Minikube fails to start, check that your chosen driver is properly configured
- For Docker driver, ensure Docker Desktop is running before starting Minikube
- For Hyper-V driver, ensure Hyper-V is enabled in Windows features
- If you get image pull errors, run: `minikube image load <image-name>` to load images into the cluster

### Verify Minikube is Running:
```cmd
minikube status
minikube dashboard  # Opens Kubernetes dashboard in browser
```

## Docker Image Building on Windows

The project includes Dockerfiles for both frontend and backend:

### Windows Command Prompt:
```cmd
# Build frontend Docker image
cd frontend
docker build -t todo-chatbot-frontend:latest .
cd ..

# Build backend Docker image
cd backend
docker build -t todo-chatbot-backend:latest .
cd ..
```

### Windows PowerShell:
```powershell
# Build frontend Docker image
Set-Location frontend
docker build -t todo-chatbot-frontend:latest .
Set-Location ..

# Build backend Docker image
Set-Location backend
docker build -t todo-chatbot-backend:latest .
Set-Location ..
```

### Windows-Specific Docker Building Tips:
- Ensure Docker Desktop is running before building images
- Building may take several minutes on first run due to downloading base images
- If you encounter permission issues, run Command Prompt or PowerShell as Administrator
- If you get "no space left on device" errors, clean up Docker cache:
  ```cmd
  docker system prune -a
  ```

### Verify Images Were Built:
```cmd
docker images | findstr todo-chatbot
```
You should see both frontend and backend images listed.

### Loading Images into Minikube:
Since Minikube runs in its own VM/container, you need to load your built images:

```cmd
# Load images into Minikube
minikube image load todo-chatbot-frontend:latest
minikube image load todo-chatbot-backend:latest
```

## Deploying the Application on Windows

Once all tools are installed and Minikube is running, you can deploy the application:

### Windows PowerShell:
```powershell
# Use the provided deployment script
.\scripts\minikube-setup.sh

# Or deploy manually using Helm
helm upgrade --install todo-chatbot ./charts/todo-chatbot `
    --namespace todo-chatbot `
    --create-namespace `
    --set global.namespace=todo-chatbot `
    --set frontend.image.repository=todo-chatbot-frontend `
    --set frontend.image.tag=latest `
    --set backend.image.repository=todo-chatbot-backend `
    --set backend.image.tag=latest `
    --set ingress.hosts[0].host=todo-chatbot.local `
    --set ingress.hosts[0].paths[0].path='/' `
    --set ingress.hosts[0].paths[0].pathType=Prefix
```

### Windows Command Prompt:
```cmd
# Use the provided deployment script
scripts\minikube-setup.bat

# Or deploy manually using Helm
helm upgrade --install todo-chatbot ./charts/todo-chatbot ^
    --namespace todo-chatbot ^
    --create-namespace ^
    --set global.namespace=todo-chatbot ^
    --set frontend.image.repository=todo-chatbot-frontend ^
    --set frontend.image.tag=latest ^
    --set backend.image.repository=todo-chatbot-backend ^
    --set backend.image.tag=latest ^
    --set ingress.hosts[0].host=todo-chatbot.local ^
    --set ingress.hosts[0].paths[0].path='/' ^
    --set ingress.hosts[0].paths[0].pathType=Prefix
```

### Alternative Deployment Method for Windows:
If the shell scripts don't work properly on Windows, you can use PowerShell to execute the deployment steps directly:

```powershell
# Check if required tools are installed
Write-Host "Checking prerequisites..."
if (!(Get-Command kubectl -ErrorAction SilentlyContinue)) { throw "kubectl is not installed" }
if (!(Get-Command helm -ErrorAction SilentlyContinue)) { throw "helm is not installed" }

# Deploy using Helm
Write-Host "Deploying application..."
helm upgrade --install todo-chatbot ./charts/todo-chatbot `
    --namespace todo-chatbot `
    --create-namespace `
    --set global.namespace=todo-chatbot `
    --set frontend.image.repository=todo-chatbot-frontend `
    --set frontend.image.tag=latest `
    --set backend.image.repository=todo-chatbot-backend `
    --set backend.image.tag=latest `
    --set ingress.hosts[0].host=todo-chatbot.local `
    --set ingress.hosts[0].paths[0].path='/' `
    --set ingress.hosts[0].paths[0].pathType=Prefix

# Wait for deployments to be ready
Write-Host "Waiting for deployments to be ready..."
kubectl wait --for=condition=ready pod -l app=todo-chatbot-frontend -n todo-chatbot --timeout=300s
kubectl wait --for=condition=ready pod -l app=todo-chatbot-backend -n todo-chatbot --timeout=300s
```

### Windows-Specific Deployment Notes:
- On Windows, you might need to run PowerShell as Administrator for the deployment scripts
- If you get execution policy errors in PowerShell, run: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`
- The scripts assume Git Bash or WSL environment; if using Command Prompt, you may need to adjust the script paths
- Make sure your Docker images are loaded into Minikube before deploying

### Using AI Tools for Enhanced Deployment:
- **kubectl-ai**: Use AI-powered commands to interact with your Kubernetes cluster:
  - `kubectl ai get pods -n todo-chatbot` - Get pods with AI explanations
  - `kubectl ai explain deployment` - Get detailed explanations of resources
  - `kubectl ai create deployment --help` - AI-assisted command creation
- **kagent**: Use AI automation for deployment management:
  - `kagent deploy --help` - Check for deployment automation features
  - `kagent monitor` - AI-powered monitoring capabilities (if available)

## Troubleshooting on Windows

### Docker Issues on Windows
- **WSL 2 Backend**: Ensure WSL 2 is enabled and configured properly
  - Run: `wsl --list --verbose` to check WSL status
  - Run: `wsl --set-version DockerDesktopLinuxBackend 2` if needed
- **Docker Service**: Restart Docker Desktop if commands are not working
  - Right-click Docker icon in system tray → Restart
- **Permission Issues**: Run Command Prompt or PowerShell as Administrator
- **Docker Engine Issues**: Check Docker Troubleshoot menu in system tray icon
- **Resource Issues**: Allocate more resources in Docker Desktop Settings → Resources

### Minikube Issues on Windows
- **Driver Issues**: If Minikube fails to start, try with a different driver:
  ```cmd
  minikube start --driver=hyperv    # For Hyper-V
  minikube start --driver=docker    # For Docker Desktop
  minikube start --driver=virtualbox # For VirtualBox
  ```
- **Resource Issues**: Check available disk space and memory in Task Manager
- **Permission Issues**: Run Command Prompt or PowerShell as Administrator
- **Virtualization Issues**: Ensure virtualization is enabled in BIOS/UEFI settings
- **Hyper-V Issues**: If using Hyper-V, ensure "Virtual Machine Platform" and "Windows Hypervisor Platform" are enabled in Windows Features

### Helm Issues on Windows
- **Repository Issues**: If Helm repositories are not updated, run:
  ```cmd
  helm repo update
  ```
- **PATH Issues**: Ensure Helm is in your PATH environment variable
- **Permission Issues**: Run Command Prompt or PowerShell as Administrator
- **Plugin Issues**: If using Helm plugins, ensure they're installed properly

### Access Issues on Windows
- **Hosts File**: After deployment, add Minikube IP to Windows hosts file:
  1. Open Notepad as Administrator
  2. Open file: `C:\Windows\System32\drivers\etc\hosts`
  3. Add line: `[Minikube IP] todo-chatbot.local` (e.g., `192.168.49.2 todo-chatbot.local`)
  4. Save the file
- **Alternative Method**: Run in Command Prompt as Administrator:
  ```cmd
  echo [minikube ip] todo-chatbot.local >> C:\Windows\System32\drivers\etc\hosts
  ```
  Replace `[minikube ip]` with the actual IP from `minikube ip` command.

### Windows-Specific Common Issues
- **Execution Policy**: If PowerShell scripts are blocked, run:
  ```powershell
  Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
  ```
- **Antivirus Interference**: Some antivirus software may block Docker or Minikube
  - Add Docker and Minikube to antivirus exclusions
  - Temporarily disable real-time scanning during installation
- **Windows Defender Firewall**: May block Minikube networking
  - Add Docker and Minikube as exceptions in Windows Defender
- **PATH Issues**: Restart Command Prompt/PowerShell after installing tools
- **WSL Issues**: If using WSL, ensure WSL 2 is set as default:
  ```powershell
  wsl --set-default-version 2
  ```

### Checking Status on Windows
- **Check all pods**: `kubectl get pods --all-namespaces`
- **Check Minikube status**: `minikube status`
- **Check Docker containers**: `docker ps`
- **Check Helm releases**: `helm list -A`
- **View logs**: `kubectl logs -n todo-chatbot deployment/todo-chatbot-frontend` or `kubectl logs -n todo-chatbot deployment/todo-chatbot-backend`
- **Check kubectl-ai functionality**: `kubectl ai --help`
- **Check kagent functionality**: `kagent --help`

## Next Steps on Windows

After successful installation and deployment:

1. **Access the application**: Open your browser and navigate to [http://todo-chatbot.local](http://todo-chatbot.local)
   - If the domain doesn't resolve, use `minikube service list` to find the correct address
   - Or access via: `minikube service todo-chatbot-frontend-service -n todo-chatbot --url`

2. **Use the deployment scripts**: The scripts in `scripts/` directory work best in Git Bash or PowerShell
   - For PowerShell: `.\scripts\deploy.sh` (may need to adjust for Windows compatibility)
   - For Git Bash: `./scripts/deploy.sh`

3. **Refer to `README_Phase4.md`**: For advanced configuration options specific to the project

4. **Manage the deployment lifecycle**: Use the provided scripts in PowerShell or Git Bash

### Windows-Specific Next Steps:
- **Check deployment status**: `kubectl get all -n todo-chatbot`
- **View application logs**: `kubectl logs -f -n todo-chatbot deployment/todo-chatbot-frontend` or `kubectl logs -f -n todo-chatbot deployment/todo-chatbot-backend`
- **Open Kubernetes dashboard**: `minikube dashboard`
- **Access services**: `minikube service list -n todo-chatbot`
- **Explore kubectl-ai capabilities**: `kubectl ai explain <resource>` or `kubectl ai get <resource> --help`
- **Use kagent for automation**: `kagent --help` to see available automation features

## Uninstalling on Windows

To remove all components:

### Using Chocolatey (if installed via Chocolatey):
```cmd
# Stop and delete Minikube cluster
minikube delete

# Uninstall tools using Chocolatey
choco uninstall docker-desktop kubernetes-cli kubernetes-helm minikube
```

### Manual Uninstall:
- **Docker Desktop**: Uninstall via "Apps & Features" in Windows Settings
- **kubectl**: Remove from PATH and delete the executable file
- **Helm**: Remove from PATH and delete the executable file
- **Minikube**: Remove from PATH and delete the executable file
- **kubectl-ai**: If installed as a kubectl plugin, remove with: `kubectl krew uninstall ai`
- **kagent**: If installed as a kubectl plugin, remove with: `kubectl krew uninstall kagent` or remove the binary from your PATH

### Cleanup Docker Resources:
```cmd
# Remove all Docker containers, networks, and images (will remove ALL Docker data)
docker system prune -a --volumes

# Remove specific project images
docker rmi todo-chatbot-frontend:latest
docker rmi todo-chatbot-backend:latest
```

### Cleanup Minikube:
```cmd
# Delete Minikube cluster
minikube delete

# Remove Minikube directory
# On Windows: Remove directory at %USERPROFILE%\.minikube
rmdir /s %USERPROFILE%\.minikube
```

### Windows Hosts File Cleanup:
- Remove the `todo-chatbot.local` entry from `C:\Windows\System32\drivers\etc\hosts` file if added during setup
- Edit the file as Administrator using Notepad or any text editor

### Verification of Uninstall:
- Close all Command Prompt/PowerShell windows
- Open a new Command Prompt/PowerShell
- Run each tool command (`docker`, `kubectl`, `helm`, `minikube`) - they should return "'command' is not recognized"