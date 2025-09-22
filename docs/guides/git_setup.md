# Git and GitHub Setup Guide for Research Teams

This comprehensive guide will help you set up Git and GitHub for collaborative research work. The guide is designed for team members with varying levels of Git experience and can accommodate both fresh installations and existing setups.

## Table of Contents

1. [Installing Git](#1-installing-git)
2. [Configuring Git](#2-configuring-git)
3. [Connecting to GitHub with SSH](#3-connecting-to-github-with-ssh)
4. [Verification and Testing](#4-verification-and-testing)

---

## 1. Installing Git

### Check Your Current Installation

Before proceeding, determine if you already have Git installed and whether it needs updating.

```bash
git --version
```

> [!NOTE]
> If you get a "command not found" error, you don't have Git installed. Skip to [Section 1.1](#11-fresh-git-installation).

Compare your version with the latest release at [git-scm.com](https://git-scm.com). If your version is more than 6 months old, consider upgrading.

### 1.1 Fresh Git Installation

#### Windows

##### Option A: Package manager (recommended)

The best way to install most software is with a package manager. Windows has two main options: [Chocolatey](https://chocolatey.org/install) (third party) or [Winget](https://learn.microsoft.com/en-gb/windows/package-manager/) (native/Microsoft).

> [!WARNING]
> You will be prompted for admin authorisation (i.e. UAC).

```powershell
# with chocolatey
choco install git.install

# with winget
winget install --id Git.Git -e --source winget

```

##### Option B: Git for Windows

1. Download from [gitforwindows.org](https://gitforwindows.org)
2. Run the installer with these recommended settings:
   - **Editor**: Use Visual Studio Code (or your preferred editor)
   - **PATH environment**: Git from the command line and also from 3rd-party software
   - **HTTPS transport backend**: Use the OpenSSL library
   - **Line ending conversions**: Checkout Windows-style, commit Unix-style line endings
   - **Terminal emulator**: Use Windows' default console window

#### macOS

Git is packaged with macOS Xcode, but it can't be updated and the version is pinned to whatever version number that Xcode depends on. Installing seperately is recommended.

```zsh
# Install Homebrew if you don't have it
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Git
brew install git
```

### 1.2 Upgrading Existing Git Installation

#### Windows with CLI

```powershell
# with chocolatey
choco upgrade git.install

# with winget
winget upgrade Git.Git

```

#### Windows with installer

Download and run the latest installer from [gitforwindows.org](https://gitforwindows.org).

#### macOS with Homebrew

```zsh
brew upgrade git
```

### 1.3 Removing Git for Clean Installation

> [!WARNING]
> This will remove your current Git installation and configuration. Back up important configurations first.

#### Windows

1. Use "Add or Remove Programs" in Windows Settings
2. Find "Git" and uninstall
3. Manually delete any remaining folders in `C:\Program Files\Git`

#### macOS

```bash
# If installed via Homebrew
brew uninstall git

```

After removal, proceed to [Section 1.1](#11-fresh-git-installation) for a fresh installation.

---

## 2. Configuring Git

### Check Your Current Configuration

First, examine your existing Git configuration:

```bash
git config --list
```

Look for these essential settings:

- `user.name`
- `user.email`
- `init.defaultBranch`
- `core.editor`

### 2.1 Fresh Installation Configuration

> [!IMPORTANT]
> Replace the example values with your actual information.

Set up your basic Git identity:

```bash
# Set your name (use your real name for research collaboration)
git config --global user.name "Jane Smith"

# Set your email (use the same email as your GitHub account)
git config --global user.email "jane.smith@university.edu"

# Set default branch name to 'main'
git config --global init.defaultBranch main

# Set your preferred editor (choose one)
git config --global core.editor "code --wait"  # VS Code
```

#### Additional settings

```bash
# Enable colored output
git config --global color.ui auto

# Set up credential caching
# macOS
git config --global credential.helper osxkeychain

# Windows
git config --global credential.helper manager-core

```

### 2.2 Aligning Existing Configuration

If you already have Git configured but need to align with team standards:

#### Check Critical Settings

```bash
# Check current settings
git config user.name
git config user.email
git config init.defaultBranch
```

#### Update Specific Settings

```bash
# Update only the settings that need changing
git config --global user.email "your-new-email@university.edu"
git config --global init.defaultBranch main
```

> [!NOTE]
> Common gotchas with existing installations:
>
> - Different email addresses between Git and GitHub
> - Default branch set to 'master' instead of 'main'
> - Missing or incompatible editor configuration
> - Cached credentials for old accounts

### 2.3 Resetting Configuration

> [!WARNING]
> This will remove ALL your Git configuration. You'll need to reconfigure everything.

```bash
# Remove global configuration file
rm ~/.gitconfig

# Or on Windows
del %USERPROFILE%\.gitconfig

# Verify removal
git config --list --global
```

After resetting, proceed to [Section 2.1](#21-fresh-installation-configuration) to reconfigure Git.

---

## 3. Connecting to GitHub with SSH

SSH keys provide secure, password-free authentication with GitHub. This section will help you set up or verify your SSH configuration.

### Check Existing SSH Configuration

First, check if you already have SSH keys configured:

```bash
# List existing SSH keys
ls -la ~/.ssh

# Test GitHub SSH connection (if keys exist)
ssh -T git@github.com
```

> [!NOTE]
> If you see a successful authentication message like "Hi username! You've successfully authenticated", you can skip to [Section 4](#4-verification-and-testing).

### 3.1 Fresh SSH Configuration

#### Step 1: Generate SSH Key

```bash
# Generate new SSH key (replace with your GitHub email)
ssh-keygen -t ed25519 -C "your-email@university.edu"

# When prompted for file location, press Enter for default
# When prompted for passphrase, choose a secure passphrase (recommended)
```

#### Step 2: Start SSH Agent and Add Key

```bash
# Start the SSH agent
eval "$(ssh-agent -s)"

# Add your SSH key to the agent
ssh-add ~/.ssh/id_ed25519

# If using RSA key
ssh-add ~/.ssh/id_rsa
```

**For macOS users**, also add this configuration to `~/.ssh/config`:

```bash
# Create or edit SSH config
touch ~/.ssh/config
open ~/.ssh/config

# Add these lines to the file
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

#### Step 3: Add SSH Key to GitHub

1. Copy your public key to clipboard:

```bash
# macOS
pbcopy < ~/.ssh/id_ed25519.pub

# Linux
cat ~/.ssh/id_ed25519.pub | xclip -selection clipboard

# Windows (Git Bash)
cat ~/.ssh/id_ed25519.pub | clip

# If above don't work, display and copy manually
cat ~/.ssh/id_ed25519.pub
```

2. Add the key to GitHub:
   - Go to [GitHub Settings → SSH and GPG Keys](https://github.com/settings/keys)
   - Click "New SSH key"
   - Give it a descriptive title (e.g., "Research Lab Laptop")
   - Paste your public key
   - Click "Add SSH key"

#### Step 4: Test SSH Connection

```bash
ssh -T git@github.com
```

> [!NOTE]
> You may see a warning about host authenticity. Type "yes" to continue. You should see a message like "Hi username! You've successfully authenticated, but GitHub does not provide shell access."

### 3.2 Resetting SSH Configuration

> [!WARNING]
> This will remove all existing SSH keys and configurations.

```bash
# Remove existing SSH directory
rm -rf ~/.ssh

# Create new SSH directory
mkdir ~/.ssh
chmod 700 ~/.ssh
```

After resetting, proceed to [Section 3.1](#31-fresh-ssh-configuration) to create new SSH keys.

---

## 4. Verification and Testing

### Final Configuration Check

Verify your complete setup:

```bash
# Check Git version
git --version

# Check Git configuration
git config --list --global

# Test GitHub SSH connection
ssh -T git@github.com

# Test repository operations (optional)
git clone git@github.com:octocat/Hello-World.git test-repo
cd test-repo
ls
cd ..
rm -rf test-repo
```

### Expected Output

You should see:

- ✅ Git version 2.30 or higher
- ✅ Proper `user.name` and `user.email` configuration
- ✅ Successful SSH authentication with GitHub
- ✅ Ability to clone repositories

### Troubleshooting Common Issues

#### "Permission denied (publickey)" Error

- Verify your SSH key is added to GitHub: [GitHub Settings → SSH Keys](https://github.com/settings/keys)
- Check if SSH agent is running: `ssh-add -l`
- Re-add your key: `ssh-add ~/.ssh/id_ed25519`

#### "Author identity unknown" Error

- Check your Git configuration: `git config --list --global`
- Set missing values: Refer to [Section 2.1](#21-fresh-installation-configuration)

#### Credential Issues on Windows

- Use Git Credential Manager: `git config --global credential.helper manager-core`
- Clear old credentials: Go to Windows Credential Manager → Generic Credentials

> [!IMPORTANT]
> If you encounter issues not covered here, please share the error message with the team for assistance.

### Next Steps

Once your setup is complete:

1. Clone the team's repositories
2. Familiarize yourself with the team's branching strategy
3. Review any project-specific Git workflows or conventions

---

## Quick Reference

### Essential Commands

```bash
# Check status
git --version
git config --list
ssh -T git@github.com

# Basic operations
git clone <repository-url>
git status
git add .
git commit -m "Your message"
git push origin main
git pull origin main
```

### Configuration Files

- Git global config: `~/.gitconfig`
- SSH config: `~/.ssh/config`
- SSH keys: `~/.ssh/id_ed25519` and `~/.ssh/id_ed25519.pub`

This completes your Git and GitHub setup. You're now ready to collaborate effectively with your research team!
