# Slough - Dev Container - NodeJS

A Docker-based development container for Node.js development, part of the Slough project.

## About the Slough Project

The Slough project is a project by Daryl Stark to deliver consistent development tooling like dev containers. It provides pre-configured, reproducible development environments that ensure all team members work with the same tools and configurations, eliminating "works on my machine" problems.

## Table of Contents

- [About the Slough Project](#about-the-slough-project)
- [Quick Start](#quick-start)
- [Using This Container as a Dev Container](#using-this-container-as-a-dev-container)
- [Prerequisites](#prerequisites)
- [Working with Dev Containers on Windows](#working-with-dev-containers-on-windows)
- [Container Configuration](#container-configuration)
- [Installed Tools](#installed-tools)
- [Tool Usage](#tool-usage)
- [Troubleshooting](#troubleshooting)
- [License](#license)

## Quick Start

Pull the latest version of this container:

```bash
docker pull dast1986/slough-dev-dc-nodejs:1.0.0
```

Run the container interactively:

```bash
docker run -it dast1986/slough-dev-dc-nodejs:1.0.0
```

## Using This Container as a Dev Container

This container is designed to be used with Visual Studio Code's Dev Containers extension. To use it in your project:

### Step 1: Install Prerequisites

1. Install [Visual Studio Code](https://code.visualstudio.com/)
2. Install the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
3. Install [Docker Desktop](https://www.docker.com/products/docker-desktop)

### Step 2: Configure Your Project

Create a `.devcontainer/devcontainer.json` file in your project root:

```json
{
  "name": "Node.js Development Environment",
  "image": "dast1986/slough-dev-dc-nodejs:1.0.0",
  "customizations": {
    "vscode": {
      "extensions": [
        "dbaeumer.vscode-eslint",
        "esbenp.prettier-vscode"
      ]
    }
  },
  "forwardPorts": [3000, 8080],
  "postCreateCommand": "npm install",
  "remoteUser": "developer"
}
```

### Step 3: Open in Dev Container

1. Open your project in VS Code
2. Press `F1` or `Ctrl+Shift+P` (Windows/Linux) / `Cmd+Shift+P` (Mac)
3. Type "Dev Containers: Reopen in Container" and select it
4. Wait for the container to build and start
5. Your project is now running inside the dev container!

## Prerequisites

- **Docker Desktop**: Version 4.0 or higher
  - Windows: Docker Desktop for Windows with WSL 2 backend enabled
  - macOS: Docker Desktop for Mac
  - Linux: Docker Engine 20.10 or higher
- **Visual Studio Code**: Latest version recommended
- **Dev Containers Extension**: Latest version from the VS Code marketplace

## Working with Dev Containers on Windows

### Important Windows-Specific Tips

#### 1. Enable WSL 2

For the best performance on Windows, use WSL 2 as the backend for Docker Desktop:

1. Open PowerShell as Administrator and run:
   ```powershell
   wsl --install
   ```
2. Restart your computer
3. Open Docker Desktop settings
4. Go to **Settings** → **General**
5. Check "Use the WSL 2 based engine"
6. Go to **Settings** → **Resources** → **WSL Integration**
7. Enable integration with your default WSL distro

#### 2. Store Your Code in WSL 2

For optimal performance, clone your repositories inside WSL 2 rather than on the Windows filesystem:

```bash
# In WSL terminal
cd ~
mkdir projects
cd projects
git clone <your-repository-url>
```

Then open the project in VS Code from WSL:
```bash
code .
```

#### 3. Line Endings

Configure Git to handle line endings correctly:

```bash
git config --global core.autocrlf input
```

#### 4. File Permissions

If you encounter file permission issues, you can configure WSL to mount drives with proper permissions. Edit or create `/etc/wsl.conf` in your WSL distribution:

```ini
[automount]
options = "metadata,umask=22,fmask=11"
```

#### 5. Performance Considerations

- **Don't** store code on `/mnt/c/` (Windows filesystem) - this is significantly slower
- **Do** store code in WSL's native filesystem (e.g., `~/projects`)
- Avoid mixing Windows and Linux tools on the same files
- Use Docker Desktop's resource settings to allocate sufficient CPU and memory

#### 6. Docker Desktop Settings

Recommended settings for Windows:
- **Memory**: At least 4GB, 8GB recommended for larger projects
- **CPUs**: At least 2, 4 recommended
- **Disk image size**: 64GB or more for multiple containers

## Container Configuration

### User Account

- **Username**: `developer`
- **Home Directory**: `/home/developer`
- **Sudo Access**: Available **without password**
  - You can run `sudo apt-get install <package>` without entering a password
  - This allows you to install additional tools as needed during development

### Environment

- **Shell**: Bash
- **Prompt**: Starship prompt configured with Node.js indicator
- **Base Image**: `dast1986/slough-dev-dc-generic-base:1.0.0`
  - Includes common development tools from the generic base image

### Networking

The container has full network access and can:
- Install packages from npm registry
- Clone Git repositories
- Access external APIs and services
- Bind to ports for development servers

## Installed Tools

This container comes with the following tools pre-installed:

### Node.js Ecosystem

- **Node.js**: Version 26.5.0
  - JavaScript runtime built on Chrome's V8 JavaScript engine
  - Supports the latest ECMAScript features
- **npm**: Package manager (bundled with Node.js)
  - Manage project dependencies
  - Run scripts defined in `package.json`

### Additional Tools (from base image)

The container inherits tools from the generic base image, which typically includes:

- **Git**: Version control system
- **curl**: Command-line tool for transferring data
- **wget**: Network downloader
- **vim/nano**: Text editors
- **Starship**: Cross-shell prompt customization
- **Docker CLI**: Docker command-line interface (when Docker socket is mounted)

## Tool Usage

### Node.js

Check the installed Node.js version:

```bash
node --version
# Output: v26.5.0
```

Run a JavaScript file:

```bash
node script.js
```

Start a Node.js REPL (Read-Eval-Print Loop):

```bash
node
```

### npm (Node Package Manager)

Initialize a new Node.js project:

```bash
npm init
# or for defaults
npm init -y
```

Install dependencies:

```bash
# Install a package and add to dependencies
npm install express

# Install a package as a dev dependency
npm install --save-dev eslint

# Install all dependencies from package.json
npm install
```

Run scripts defined in `package.json`:

```bash
npm start
npm test
npm run build
```

Update packages:

```bash
# Update a specific package
npm update express

# Update all packages
npm update
```

Check for outdated packages:

```bash
npm outdated
```

### Git

Configure Git (first-time setup):

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Common Git commands:

```bash
# Clone a repository
git clone <repository-url>

# Check status
git status

# Stage changes
git add .

# Commit changes
git commit -m "Your commit message"

# Push changes
git push origin main
```

### Installing Additional Tools

Since you have passwordless sudo access, you can install additional packages:

```bash
# Update package list
sudo apt-get update

# Install a package
sudo apt-get install -y <package-name>

# Example: Install Python
sudo apt-get install -y python3
```

For Node.js packages, use npm:

```bash
# Install global npm packages
npm install -g typescript
npm install -g nodemon
npm install -g eslint
```

## Troubleshooting

### Container Won't Start

1. **Check Docker is running**: Ensure Docker Desktop is running and healthy
2. **Check Docker resources**: Make sure Docker has enough CPU and memory allocated
3. **Check image availability**: Verify the image is pulled: `docker images | grep slough-dev-dc-nodejs`

### Performance Issues on Windows

1. **Use WSL 2**: Make sure you're using WSL 2 backend, not Hyper-V
2. **Store code in WSL**: Don't store your code on `/mnt/c/`, use WSL's native filesystem
3. **Increase resources**: Allocate more CPU and memory in Docker Desktop settings

### Permission Errors

If you encounter permission errors:

```bash
# Fix permissions for the current directory
sudo chown -R developer:developer .

# Or for a specific file/directory
sudo chown developer:developer <path>
```

### Port Already in Use

If a port is already in use:

```bash
# Find the process using the port
sudo lsof -i :<port-number>

# Kill the process (if safe to do so)
sudo kill -9 <PID>
```

### Node Modules Issues

If you encounter issues with node_modules:

```bash
# Remove node_modules and package-lock.json
rm -rf node_modules package-lock.json

# Clear npm cache
npm cache clean --force

# Reinstall dependencies
npm install
```

### Git Line Ending Issues

If you see unexpected file changes related to line endings:

```bash
# Configure Git to handle line endings
git config core.autocrlf input

# Refresh the index
git rm --cached -r .
git reset --hard
```

## License

Copyright 2024 Daryl Stark

This project is licensed under the MIT License. See the [LICENSE.md](LICENSE.md) file for details.
