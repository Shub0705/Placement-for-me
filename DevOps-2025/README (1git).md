# Introduction to SCM (Source Code Management) Tool

## What is SCM?
SCM (Source Code Management) is the practice of tracking and managing changes to software code. It helps developers collaborate, maintain code versions, and ensure consistency across a project.

## Difference Between Centralized VCS and Distributed VCS

| Feature              | Centralized VCS (CVCS) | Distributed VCS (DVCS) |
|---------------------|---------------------|---------------------|
| Repository         | Single central repository | Multiple repositories (local + remote) |
| Example Tools     | SVN, Perforce        | Git, Mercurial      |
| Offline Work      | Not possible        | Possible (local commits) |
| Speed            | Slower (relies on central server) | Faster (local operations) |
| Collaboration     | Requires network access | Can work offline and push later |

# Introduction to Git
Git is a **Distributed Version Control System (DVCS)** that enables efficient tracking of changes, branching, and collaboration among developers.

## Git Lifecycle
1. **Working Directory**: The local project files.
2. **Staging Area (Index)**: Files marked for the next commit.
3. **Local Repository**: Commits stored in the local `.git` folder.
4. **Remote Repository**: GitHub/GitLab/Bitbucket where the code is shared.

```
Working Directory → Staging Area → Local Repository → Remote Repository
```

# Installing Git CLI
### Windows:
1. Download Git from [git-scm.com](https://git-scm.com/downloads)
2. Install with default settings
3. Verify installation:
   ```sh
   git --version
   ```

### Linux:
```sh
sudo apt update
sudo apt install git -y
git --version
```

### Mac:
```sh
brew install git
git --version
```

# Git Commands (Working on Local Repo)

### Initialize a New Git Repository
```sh
git init
```
This command creates a new Git repository in the current directory.

### Check the Status of Files
```sh
git status
```
Shows modified and untracked files.

### Add Files to the Staging Area
```sh
git add <filename>
```
To add all files:
```sh
git add .
```
### commit and add 
```
git commit -a -m "update"

```



### Commit Changes
```sh
git commit -m "Initial commit"
```
Commits the changes in the staging area with a message.

### View Commit History
```sh
git log
```
Displays the commit history.

### Revert Changes (Undo a Commit)
```sh
git revert <commit-hash>
```
This creates a new commit that undoes changes.

### Restore a File (Undo Local Changes)
```sh
git restore <filename>
```
Restores the file to the last committed state.

---
This document provides a fundamental understanding of **Git** and **SCM**, covering setup, commands, and workflow. 🚀
----
# Git Branching and Remote Repositories

## Git Branching

Branching in Git allows developers to work on different versions of a repository independently. Branches help manage features, fixes, and releases without affecting the main codebase.

### Common Types of Branches
1. **Main (Master) Branch** - The primary branch where production-ready code is maintained.
2. **Feature Branch** - Used to develop new features before merging into the main branch.
3. **Bugfix Branch** - Created to fix specific bugs without disrupting ongoing development.
4. **Release Branch** - Used for final testing before deploying a new version.
5. **Hotfix Branch** - Used to quickly patch critical issues in production.

### Git Branch Commands
- `git branch` → Lists all branches
- `git branch <branch_name>` → Creates a new branch
- `git checkout <branch_name>` → Switches to the specified branch
- `git checkout -b <branch_name>` → Creates and switches to a new branch
- `git branch -d <branch_name>` → Deletes a local branch
- `git merge <branch_name>` → Merges specified branch into the current branch
- `git diff <branch1> <branch2>` → Shows differences between two branches

---

## Introduction to Remote Repositories (GitHub)
A remote repository is a version of your project stored on an online platform like GitHub, GitLab, or Bitbucket. It allows collaboration, backup, and deployment of your code.

### Essential Remote Repository Commands
- `git init` → Initializes a new Git repository in the current directory
- `git config --global user.name "Your Name"` → Sets your Git username
- `git config --global user.email "your.email@example.com"` → Sets your Git email
- `git remote add origin <repo_URL>` → Connects your local repository to a remote one
- `git clone <repo_URL>` → Clones a remote repository to your local machine
- `git pull origin <branch_name>` → Pulls the latest changes from the remote repository
- `git push origin <branch_name>` → Pushes local commits to the remote repository

### Workflow Example
```bash
# Create a new branch
$ git checkout -b feature-branch

# Work on changes
$ git add .
$ git commit -m "Added new feature"

# Push the branch to GitHub
$ git push origin feature-branch

# Merge changes into the main branch
$ git checkout main
$ git merge feature-branch

# Delete the feature branch locally
$ git branch -d feature-branch
```

---

## Summary
- Git branches help manage different versions of a repository.
- Remote repositories enable collaboration and backup.
- Commands like `git pull`, `git push`, `git merge`, and `git checkout` are essential for workflow management.
- GitHub is a popular platform for hosting Git repositories.

----

# GitHub Dashboard and Repository Management

## GitHub Dashboard
GitHub provides an intuitive dashboard that allows users to manage repositories, collaborate with teams, and track project activity. The key sections of the GitHub dashboard include:
- **Repositories**: List of personal and contributed repositories.
- **Pull Requests**: Tracks open and closed pull requests.
- **Issues**: Displays assigned and created issues.
- **Projects**: Organizes tasks using kanban-style project boards.
- **Explore**: Discover trending repositories and developers.

---
## Difference Between Public and Private Repositories
GitHub allows users to create repositories with different levels of access control.

| Feature         | Public Repo  | Private Repo  |
|----------------|-------------|--------------|
| Visibility     | Accessible by anyone | Restricted access (only invited users) |
| Contribution   | Open to community collaboration | Limited to authorized users |
| Security      | Code is publicly available | Code is private and secure |
| Use Case      | Open-source projects, sharing code | Proprietary code, personal projects |

---
## How to Search Public Repositories
1. **Go to GitHub Homepage**: [GitHub](https://github.com)
2. **Use the Search Bar**: Type keywords related to the repository.
3. **Filter by Language**: Select a programming language to refine results.
4. **Sort by Trending or Stars**: Find popular repositories.
5. **Check License & Last Update**: Ensure the repo meets your needs.

---
## Fork and Pull Requests
### **Forking a Repository**
Forking creates a personal copy of a repository under your GitHub account.
#### Steps to Fork:
1. Open the repository you want to fork.
2. Click on the **Fork** button (top-right corner).
3. The forked repo appears under your profile.

### **Creating a Pull Request**
A Pull Request (PR) is used to propose changes to a repository.
#### Steps to Create a PR:
1. Clone the forked repo and make changes.
2. Push changes to your forked repo.
3. Open the original repository and go to **Pull Requests**.
4. Click **New Pull Request**, compare changes, and submit.
5. Wait for approval and merge.

---
## Authentication Methods (HTTP & SSH)
GitHub supports authentication using **HTTPS** and **SSH**.

### **1. HTTPS Authentication**
- Uses GitHub username and personal access token instead of passwords.
- To authenticate:
  ```sh
  git clone https://github.com/your-username/repo.git
  ```
- Secure but requires re-entering credentials or storing them in a credential manager.

### **2. SSH Authentication**
- Uses an SSH key pair (public and private key) for secure access.
- To set up SSH:
  ```sh
  ssh-keygen -t rsa -b 4096 -C "your-email@example.com"
  ```
- Add the public key to GitHub under **Settings > SSH and GPG keys**.
- Use SSH to clone repositories securely:
  ```sh
  git clone git@github.com:your-username/repo.git
  ```

---

# Difference between GitHub and GitLab

| Feature        | GitHub        | GitLab        |
|---------------|--------------|--------------|
| Hosting       | Cloud-based  | Self-hosted & Cloud |
| CI/CD         | GitHub Actions | Built-in CI/CD |
| Access Control | Limited | Advanced permission controls |
| Issue Tracking | Basic | More advanced (Kanban, Epics, etc.) |
| Free Tier     | Available | Available (Community Edition) |
| Integration   | Extensive third-party integrations | Built-in DevOps tools |

---

## Difference between CE and EE GitLab

| Feature | GitLab CE (Community Edition) | GitLab EE (Enterprise Edition) |
|---------|-------------------------------|--------------------------------|
| Licensing | Open-source (Free) | Proprietary (Paid) |
| Support | Community-based | Official GitLab Support |
| Features | Basic Git repository management, CI/CD, Issues, Merge Requests | Advanced features like Audit logs, Enterprise authentication, Performance Monitoring |
| Deployment | Self-hosted | Self-hosted & Cloud |

---

## How to Merge Repository Branches in GitLab

1. **Navigate to GitLab repository**
2. **Create a new branch**
   ```sh
   git checkout -b feature-branch
   ```
3. **Make changes & commit**
   ```sh
   git add .
   git commit -m "Added new feature"
   ```
4. **Push branch to remote**
   ```sh
   git push origin feature-branch
   ```
5. **Create a Merge Request (MR) in GitLab**
6. **Review and merge the request**

---

## How to Resolve Merge Conflicts in GitLab

1. **Fetch the latest changes**
   ```sh
   git fetch origin
   ```
2. **Checkout the branch and merge**
   ```sh
   git checkout main
   git merge feature-branch
   ```
3. **Identify conflicts**
   ```sh
   git status
   ```
4. **Manually resolve conflicts in files**
5. **Add the resolved files**
   ```sh
   git add resolved-file.txt
   ```
6. **Commit and complete merge**
   ```sh
   git commit -m "Resolved merge conflicts"
   ```
7. **Push changes to remote**
   ```sh
   git push origin main
   ```

---

## Introduction to IDE and Visual Studio Code

### What is an IDE?
- An **IDE (Integrated Development Environment)** is a software tool that provides comprehensive facilities for software development.
- Examples: Visual Studio Code, IntelliJ IDEA, PyCharm, Eclipse.

### Installing Visual Studio Code
1. Download from [Visual Studio Code](https://code.visualstudio.com/)
2. Install the software following the on-screen instructions.
3. Install Git extension for VS Code.

### Auto Git in VS Code
1. **Clone a Repository**
   ```sh
   git clone https://github.com/user/repo.git
   ```
2. **Make Changes & Commit**
   - Modify files.
   - Use the **Source Control Panel** in VS Code to stage and commit changes.
3. **Push Changes to GitHub/GitLab**
   ```sh
   git push origin main
   ```
4. **Pull Changes**
   ```sh
   git pull origin main
   ```

With these steps, you can effectively manage your repositories in GitLab and GitHub! 🚀
