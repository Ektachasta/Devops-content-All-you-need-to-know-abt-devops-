# 📜 Git & GitHub actions for Absolute Beginners  

## 🚀 What is Git?  
Git is a **tool** that helps track changes in your code. It allows multiple people to work on the same project **without messing up each other's code**.  

## 🌐 What is GitHub?  
GitHub is a **website** where you can store your Git projects online. Think of it as **Google Drive** but for code!  

---

## ✅ Why Use Git?  
- **Keeps track** of all your project versions.  
- **Allows collaboration** with multiple developers.  
- **Prevents mistakes** by letting you go back to older versions.  
- **Works both offline (Git) and online (GitHub).**  

---

## 🔥 Setting Up Git (Only Once)  
First, install Git from [git-scm.com](https://git-scm.com/).  
Then, **open the terminal (Command Prompt, Git Bash, or PowerShell)** and set up your name and email:  

```sh
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```
Check if Git is installed by running:  
```sh
git --version
```

---

## 📂 How to Start Using Git  

### 📌 Step 1: Create a New Project (Repository)  
A **repository (repo)** is like a folder where your project is stored.  

```sh
git init
```
This creates a hidden `.git` folder to track your project.  

### 📌 Step 2: Add a File  
Create a file (`example.txt`) and add some text inside it.  

Now, **tell Git to track the file**:  
```sh
git add example.txt
```

### 📌 Step 3: Save Changes (Commit)  
To **save your progress**, commit the changes:  
```sh
git commit -m "Added example.txt"
```

---

## 🔑 Common Git Commands  

| Command | What It Does |
|---------|-------------|
| `git init` | Start a new Git project |
| `git add <file>` | Track a file |
| `git commit -m "message"` | Save changes |
| `git status` | Check which files are changed |
| `git log` | Show commit history |
| `git clone <repo-url>` | Copy an existing GitHub repo |
| `git push origin <branch>` | Upload files to GitHub |
| `git pull origin <branch>` | Download latest changes from GitHub |

---

## 🔗 Connecting Git with GitHub  

### 📌 Step 1: Create a Repository on GitHub  
Go to [GitHub](https://github.com/), create an account, and click **"New Repository"**.  

### 📌 Step 2: Link Your Git Project to GitHub  
In the terminal, **add the GitHub repo link**:  

```sh
git remote add origin https://github.com/yourusername/repository.git
```

### 📌 Step 3: Upload (Push) Your Code to GitHub  
```sh
git branch -M main
git push -u origin main
```
🎉 **Your code is now on GitHub!**  

---

## 🌿 Working with Branches  

A **branch** lets you create a separate version of your project without affecting the main code.  

| Command | What It Does |
|---------|-------------|
| `git branch new-feature` | Create a new branch |
| `git switch new-feature` | Switch to the branch |
| `git merge new-feature` | Merge the branch with `main` |
| `git branch -d new-feature` | Delete the branch |

---

## ⏪ Undoing Mistakes  

### ❌ Undo Last Commit (But Keep Changes)  
```sh
git reset --soft HEAD~1
```

### ❌ Undo Everything (Hard Reset)  
```sh
git reset --hard HEAD~1
```

### 🔄 Remove Uncommitted Changes  
```sh
git checkout -- <file>
```

---

## 🏆 Summary  
- **Git** is a tool that tracks changes in your code.  
- **GitHub** is a website where you store your Git projects online.  
- You **add, commit, and push** your code to save it.  
- **Branches** help you work on new features without breaking the main code.  

🚀 **Now you're ready to use Git & GitHub!** 🎉  
  
