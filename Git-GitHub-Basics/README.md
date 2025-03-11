# 📝 Git Basics  

Git is a distributed version control system that tracks changes in source code and enables collaboration among multiple developers.

## 🔹 Why Use Git?
- Tracks code changes efficiently.
- Supports collaboration with multiple developers.
- Allows rollback to previous versions.
- Works both online (GitHub, GitLab) and offline.

## 🔹 Key Git Commands
| Command | Description |
|---------|-------------|
| `git init` | Initialize a new repository |
| `git clone <repo-url>` | Clone an existing repository |
| `git status` | Check the status of files |
| `git add <file>` | Stage a file |
| `git commit -m "message"` | Commit staged files |
| `git push origin <branch>` | Push changes to GitHub |
| `git pull origin <branch>` | Pull latest changes |
| `git log` | Show commit history |

## 🌿 Working with Branches  

### Create a New Branch  
```sh
git branch new-feature
```

### Switch to a Branch  
```sh
git checkout new-feature
```
or  
```sh
git switch new-feature
```

### Create and Switch to a New Branch  
```sh
git checkout -b new-feature
```
or  
```sh
git switch -c new-feature
```

### Merge Branches  
```sh
git checkout main
git merge new-feature
```

---

## 🌍 Working with Remote Repositories  

### Add a Remote Repository  
```sh
git remote add origin https://github.com/user/repository.git
```

### View Remote Repositories  
```sh
git remote -v
```

### Remove a Remote Repository  
```sh
git remote remove origin
```

---

## 🔄 Undoing Changes in Git  

### Discard Unstaged Changes  
```sh
git checkout -- <file>
```

### Unstage a Staged File  
```sh
git reset HEAD <file>
```

### Reset the Last Commit (Soft Reset)  
```sh
git reset --soft HEAD~1
```

### Reset the Last Commit (Hard Reset)  
```sh
git reset --hard HEAD~1
```

---

## 📦 Git Stash (Temporary Storage)  

If you need to save uncommitted changes temporarily:  

### Stash Current Changes  
```sh
git stash
```

### View Stashed Changes  
```sh
git stash list
```

### Apply Stashed Changes  
```sh
git stash apply
```

### Remove Stash  
```sh
git stash drop
```

---

## 🏆 Conclusion  
Git is a powerful version control system that helps developers track changes, collaborate, and manage project versions efficiently. By mastering these commands, you can streamline your development workflow and make collaboration seamless. 🚀  
