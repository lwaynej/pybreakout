# Git & GitHub Beginner Guide

This guide will walk you through:
1. Installing Git
2. Creating a GitHub account
3. Using Git to contribute to this project

This is a minimal, real-world workflow used by professional developers.

---

# 1. Install Git

## Check if Git is already installed

Open a terminal (or command prompt) and run:

git --version

If you see a version number, you’re good.

---

## macOS

Option 1 (recommended):
git --version
(macOS will prompt you to install developer tools)

Option 2 (Homebrew):
brew install git

---

## Linux

Ubuntu / Debian:
sudo apt update
sudo apt install git

Fedora:
sudo dnf install git

---

## Windows

1. Download Git for Windows from:
   https://git-scm.com
2. Run the installer
3. Use Git Bash

---

# 2. Create a GitHub Account

Go to: https://github.com

Steps:
1. Click Sign up
2. Create a username and password
3. Verify your email

---

# 3. Basic Git Workflow

Fork → Clone → Branch → Edit → Commit → Push → Pull Request

---

## Step 1: Fork the Repository

1. Go to the project page on GitHub
2. Click Fork

---

## Step 2: Clone Your Fork

git clone https://github.com/<your-username>/<repo>.git
cd <repo>

---

## Step 3: Create a Branch

git checkout -b my-change

---

## Step 4: Make Changes

Edit files and save your work.

---

## Step 5: Commit Changes

git add .
git commit -m "Describe your change here"

---

## Step 6: Push to GitHub

git push origin my-change

---

## Step 7: Open a Pull Request

1. Go to your repository on GitHub
2. Click Compare & pull request
3. Submit

---

# 4. Quick Reference

git status
git add .
git commit -m "message"
git push
git pull

---

# 5. Summary

git clone
git checkout -b <branch>
git add .
git commit -m "message"
git push
