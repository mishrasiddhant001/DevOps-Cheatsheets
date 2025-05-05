## 🧑‍💻 **Git Cheat Sheet** 🚀

### 1. **Set up User Info 📝**

* **Configure user name**:

  ```bash
  git config --global user.name "[firstname lastname]"
  ```
* **Set email for commits**:

  ```bash
  git config --global user.email "[valid-email]"
  ```
* **Enable auto-coloring in command line** for easier review:

  ```bash
  git config --global color.ui auto
  ```

### 2. **Initializing Repositories ⚙️**

* **Initialize a new Git repository**:

  ```bash
  git init
  ```
* **Clone an existing repository**:

  ```bash
  git clone [url]
  ```

### 3. **Check Status and Staging 📊**

* **Check the status of modified files**:

  ```bash
  git status
  ```
* **Stage a file for commit** (add it to the next commit):

  ```bash
  git add [file]
  ```
* **Unstage a file but keep changes**:

  ```bash
  git reset [file]
  ```
* **Show diff of unstaged changes**:

  ```bash
  git diff
  ```
* **Show diff of staged changes**:

  ```bash
  git diff --staged
  ```

### 4. **Committing Changes 💥**

* **Commit changes with a message**:

  ```bash
  git commit -m "[descriptive message]"
  ```

### 5. **Branching & Switching 📂**

* **List all branches** (current branch is marked with `*`):

  ```bash
  git branch
  ```
* **Create a new branch**:

  ```bash
  git branch [branch-name]
  ```
* **Switch to another branch**:

  ```bash
  git checkout [branch-name]
  ```

### 6. **Merging & History 🔄**

* **Merge a branch into the current one**:

  ```bash
  git merge [branch]
  ```
* **Show all commits in the current branch**:

  ```bash
  git log
  ```
* **Show commits on branchA that are not on branchB**:

  ```bash
  git log branchB..branchA
  ```
* **Track file changes across renames**:

  ```bash
  git log --follow [file]
  ```
* **Show diff between branchA and branchB**:

  ```bash
  git diff branchB...branchA
  ```
* **Show details of a specific commit**:

  ```bash
  git show [SHA]
  ```

### 7. **File Operations 🗂️**

* **Remove a file from the project and stage the removal**:

  ```bash
  git rm [file]
  ```
* **Move/rename a file and stage the move**:

  ```bash
  git mv [existing-path] [new-path]
  ```
* **Show commit logs with file move details**:

  ```bash
  git log --stat -M
  ```

### 8. **Remote Repositories 🌍**

* **Add a remote repository alias**:

  ```bash
  git remote add [alias] [url]
  ```
* **Fetch branches from the remote repository**:

  ```bash
  git fetch [alias]
  ```
* **Merge a remote branch into the current branch**:

  ```bash
  git merge [alias]/[branch]
  ```
* **Push local commits to the remote branch**:

  ```bash
  git push [alias] [branch]
  ```
* **Pull changes from the remote repository**:

  ```bash
  git pull
  ```

### 9. **Rebase and Reset 🔄**

* **Rebase your current branch onto another branch**:

  ```bash
  git rebase [branch]
  ```
* **Hard reset your branch to a specific commit** (wipe local changes):

  ```bash
  git reset --hard [commit]
  ```

### 10. **Stashing 🛠️**

* **Save uncommitted changes temporarily**:

  ```bash
  git stash
  ```
* **List all stashed changes**:

  ```bash
  git stash list
  ```
* **Apply the most recent stash**:

  ```bash
  git stash pop
  ```
* **Drop the most recent stash**:

  ```bash
  git stash drop
  ```
