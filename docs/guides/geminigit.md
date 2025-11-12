Of course\! Here is a quick-start guide to `git` and GitHub, written in a similar style to your `pixi.md` document and tailored for your team's environment.

# Git & GitHub for Researchers

## Introduction

**Git** is a version control system—think of it as "track changes" for your entire project, not just a single document. **GitHub** is a website that hosts your Git projects, making it easy to back them up and collaborate with others. Using them together is fundamental for reproducible and collaborative research, ensuring you never lose your work and can always trace back your steps.

This guide covers the essentials to get you up and running with Git and connecting it securely to your GitHub account.

> 🔗 [GitHub Docs: About Git](https://docs.github.com/en/get-started/using-git/about-git)

-----

## Installation (Win11)

In a managed IT environment, you often don't have admin rights to run installers. The best workaround is to use the **portable** version of Git, which doesn't require installation.

1. Navigate to the [Git for Windows](https://git-scm.com/download/win) website.

2. Download the **"64-bit Git for Windows Portable"** version. It will download as a `.zip` file.
    3\.  Create a folder for your tools, for example: `C:\Users\user.name\tools\`.

3. Extract the contents of the downloaded `.zip` file into this folder. Your Git executable will now be at a path like `C:\Users\user.name\tools\git\bin\git.exe`.

4. To make `git` commands available in your terminal, you need to add this `bin` directory to your PATH. Open a PowerShell terminal and run the following command. **Note:** This only lasts for your current terminal session.

    ```powershell
    # Replace the path with the actual path to your portable git 'bin' folder
    $env:PATH += ";C:\Users\user.name\tools\git\bin"
    ```

5. Verify the installation by checking the version:
    `git --version`

-----

## Initial Configuration

Before you start using Git, you need to tell it who you are. This information is attached to every commit you make.

1. Open a terminal (e.g., Windows Terminal, VS Code terminal).

2. Set your username and email. These should match the details on your GitHub account.

    ```powershell
    git config --global user.name "Your Name"
    git config --global user.email "your.email@example.com"
    ```

3. **Best Practice:** Set the default branch name to `main`. This is the modern standard, replacing the old `master`.
    `git config --global init.defaultBranch main`

-----

## Connecting to GitHub via SSH

Using an SSH key is more secure than using a password and saves you from typing your credentials every time. It's like giving your computer a key to securely access your GitHub account.

### 1\. Create an SSH key

1. Open a PowerShell terminal.
2. Run the following command, replacing the email with your GitHub email.
    `ssh-keygen -t ed25519 -C "your.email@example.com"`
3. When prompted to "Enter a file in which to save the key," just press **Enter** to accept the default location.
4. You'll be asked to enter a passphrase. This is an optional password for your key. It's more secure to use one, but for simplicity, you can press **Enter** twice to leave it blank.

### 2\. Add your SSH key to the ssh-agent

The `ssh-agent` is a background program that handles your SSH keys.

1. Start the agent in your current PowerShell session:
    `Start-SshAgent`
2. Add your newly created private key to the agent:
    `ssh-add ~/.ssh/id_ed25519`

### 3\. Add your Public Key to GitHub

1. Copy your **public** SSH key to the clipboard. The command below will display it in the terminal; manually copy the entire output.
    `cat ~/.ssh/id_ed25519.pub`
2. Go to your GitHub account settings.
3. In the left sidebar, click **"SSH and GPG keys"**.
4. Click **"New SSH key"** or **"Add SSH key"**.
5. Give it a descriptive **Title** (e.g., "Work Laptop").
6. Paste your public key into the **"Key"** field.
7. Click **"Add SSH key"**.

-----

## Basic Usage

### Creating a new project

Use this when you have an existing project on your computer that you want to put on GitHub.

1. Navigate to your project root in the terminal.
    `cd C:\Users\user.name\projects\my-new-project`

2. Initialise a Git repository:
    `git init`

3. Go to [GitHub](https://github.com) and create a **new, empty repository** (do not add a `README` or `.gitignore` yet).

4. Copy the SSH URL for your new repository. It will look like `git@github.com:YourUsername/my-new-project.git`.

5. Link your local repository to the remote one on GitHub:
    `git remote add origin git@github.com:YourUsername/my-new-project.git`

6. Add your files, commit them, and push them to GitHub.

    ```powershell
    # Stage all files in the current directory for the first commit
    git add .

    # Commit the staged files with a message
    git commit -m "Initial commit"

    # Push your commit to the 'main' branch on GitHub
    git push -u origin main
    ```

### Cloning an existing project

Use this when the project already exists on GitHub.

1. On the GitHub repository page, click the green **"\< \> Code"** button.
2. Make sure the **SSH** tab is selected, and copy the URL.
3. In your terminal, navigate to where you want to store the project (e.g., `cd C:\Users\user.name\projects`).
4. Run the `git clone` command with the copied URL:
    `git clone git@github.com:AnotherUser/some-existing-project.git`
5. This will create a new folder named `some-existing-project` containing all the project files.

-----

## Gotchas & Best Practices

### ⚠️ `.gitignore` is essential

A `.gitignore` file tells Git which files or folders to ignore. This is **critical** for avoiding committing large data files, temporary outputs, and environment folders like `.pixi/` or `.venv/`.

Create a file named `.gitignore` in your project root and add patterns to it.

```gitignore
# Ignore pixi environment
.pixi/
__pycache__/

# Ignore large data files
*.tif
data/raw/

# Ignore secrets
.env
```

> 🔗 [GitHub: A collection of useful .gitignore templates](https://github.com/github/gitignore)

### ‼️ Commit your lockfile

Just like with `pixi`, always commit your dependency and lock files (e.g., `pyproject.toml`, `pixi.lock`, `environment.yml`). This is the key to reproducible environments.

### ⚠️ Don't commit large data files

Git is not designed to handle large binary files (like GeoTIFFs, LAS files, or model weights). Committing them will bloat your repository and make it slow. For large data, use a dedicated tool like **Git LFS (Large File Storage)** or store the data in an object store (like S3) and access it from your code.

### ✍️ Write good commit messages

A commit message should be a short, clear summary of the changes you made. This creates a readable history of the project, which is invaluable for you and your collaborators later.
