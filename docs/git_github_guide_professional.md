# Git and GitHub Guide for Contributors

This document provides a structured introduction to installing Git, creating a GitHub account, and using a standard workflow to contribute changes to a project.

---

# 1. Installing Git

## Verify Installation

Open a terminal (Linux/macOS) or Git Bash / Command Prompt (Windows) and run:

```
git --version
```

If a version number is displayed, Git is already installed.

---

## macOS

Option 1 (recommended):
- Run `git --version` and follow the prompt to install Xcode Command Line Tools.

Option 2 (Homebrew):
```
brew install git
```

---

## Linux

### Ubuntu / Debian
```
sudo apt update
sudo apt install git
```

### Fedora
```
sudo dnf install git
```

---

## Windows

1. Download Git from:
   https://git-scm.com
2. Run the installer using default settings
3. Use **Git Bash** for command-line operations

---

# 2. Creating a GitHub Account

1. Navigate to https://github.com
2. Select **Sign up**
3. Provide a username, email address, and password
4. Complete verification

---

# 3. Contribution Workflow Overview

The standard workflow used in this project:

```
Fork → Clone → Branch → Modify → Commit → Push → Pull Request
```

---

# 4. Step-by-Step Workflow

## 4.1 Fork the Repository

1. Navigate to the project repository on GitHub
2. Select **Fork** (top-right corner)

This creates a personal copy of the repository under your account.

---

## 4.2 Clone the Repository

Replace placeholders with your GitHub username and repository name:

```
git clone https://github.com/<your-username>/<repository>.git
cd <repository>
```

---

## 4.3 Create a Branch

Create a new branch for your changes:

```
git checkout -b <branch-name>
```

Use a descriptive branch name (e.g., `add-paddle-control`).

---

## 4.4 Modify the Code

Make the required changes using your editor or IDE. Save all modified files.

---

## 4.5 Commit Changes

Stage and commit your changes:

```
git add .
git commit -m "Brief description of changes"
```

Guidelines:
- Use concise, descriptive commit messages
- Commit logically related changes together

---

## 4.6 Push Changes

Push your branch to your GitHub repository:

```
git push origin <branch-name>
```

---

## 4.7 Open a Pull Request

1. Navigate to your repository on GitHub
2. Select **Compare & pull request**
3. Review your changes
4. Submit the pull request

---

# 5. Common Commands

```
git status          # View current changes
git add .           # Stage changes
git commit -m ""    # Commit changes
git push            # Push changes
git pull            # Retrieve updates
```

---

# 6. Troubleshooting

## No changes to commit
Ensure files have been modified and saved before running `git add`.

## Permission errors
Verify you are pushing to your fork, not the original repository.

## Git not recognized
Confirm Git is installed and available in your system PATH.

---

# 7. Summary

Typical contribution sequence:

```
git clone <repo>
git checkout -b <branch>
git add .
git commit -m "message"
git push
```

After pushing, complete the process by opening a pull request on GitHub.
