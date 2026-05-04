# Git & GitHub

## Intro

Now you have your Git configured, you need to put your code somwewhere.

## Connecting

### Via SSH

The old fashioned way.

> [!WARNING] Elevated terminal required (admin/sudo)

This section summarises the [GitHub Docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent), which contain much more detail.

1. Generate a new key

    ```cli
    ssh-keygen -t ed25519 -C "your_email@example.com"
    # note: the -C flag is a comment for you to identify the key. It does not have to be your email and can be something more descriptive such as:
    ssh-keygen -t ed25519 -C "computername:github"
    ```

2. Write the key to a file.
    Use something descriptive.

    ```cli
    "$HOME/.ssh/github-key
    ```

3. Enter a passphrase if you want one.
    > Press enter to skip
4. Set up ssh-agent

### Via GitHub CLI

The modern but much simpler way. Also favoured by AI agents everywhere.

#### Install `gh`

Install with your OS package manager:

```cli
# Windows (winget)
winget install --id GitHub.cli
choco install gh

# macOS (Homebrew)
brew install gh

# Linux (apt)
sudo apt install gh
```

#### Authorise `gh`

Run the interactive login flow and follow prompts:

```cli
gh auth login
```

Recommended options when prompted:

1. `GitHub.com`
2. `HTTPS`
3. `Login with a web browser`

Confirm authentication:

```cli
gh auth status
```
