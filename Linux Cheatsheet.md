# 🐧 Linux Command Cheat Sheet

## 📁 File and Directory Operations

| Command                   | Description                                          |
| ------------------------- | ---------------------------------------------------- |
| `ls`                      | 📄 List files and directories                        |
| `ls -R`                   | 📂 Recursively list sub-directories                  |
| `ls -a`                   | 👻 Show hidden files                                 |
| `ls -al`                  | 📋 Detailed listing (permissions, size, owner, etc.) |
| `cd` or `cd ~`            | 🏠 Go to home directory                              |
| `cd ..`                   | ⬆️ Move up one directory level                       |
| `cd /path`                | 📌 Navigate to specific directory                    |
| `cd /`                    | 🌳 Go to root directory                              |
| `mkdir directoryname`     | 📁 Create a new directory                            |
| `rmdir directoryname`     | 🗑️ Remove an empty directory                        |
| `mv file new_path`        | 🚚 Move file to new location                         |
| `mv old_name new_name`    | ✏️ Rename a file or directory                        |
| `rm filename`             | ❌ Delete a file                                      |
| `cat > filename`          | 📝 Create a new file                                 |
| `cat filename`            | 📖 Display file content                              |
| `cat file1 file2 > file3` | 📎 Combine files into one                            |

---

## 🔧 Package Management

| Command                     | Description                                |
| --------------------------- | ------------------------------------------ |
| `apt-get`                   | 📦 Install/update packages (Debian/Ubuntu) |
| `dpkg --print-architecture` | 🧱 Show system architecture (Debian)       |
| `rpm --eval '%{_arch}'`     | 🧱 Show system architecture (RPM-based)    |

---

## 👮‍♂️ Permissions & Ownership

| Command                     | Description                        |
| --------------------------- | ---------------------------------- |
| `ls -l`                     | 🔐 Show file permissions           |
| `chown user file`           | 👤 Change file ownership           |
| `chown user:group filename` | 👥 Change file and group ownership |

> 🔑 Permissions:
>
> * `r` = Read 📖
> * `w` = Write ✍️
> * `x` = Execute 🚀
> * `-` = No permission ❌

---

## 👨‍💻 Environment Variables

| Command            | Description                   |
| ------------------ | ----------------------------- |
| `echo $VARIABLE`   | 🗣️ Print variable value      |
| `env`              | 🌍 Show environment variables |
| `VARIABLE=value`   | ✏️ Set a variable             |
| `export VAR=value` | 📤 Set environment variable   |
| `unset VAR`        | 🚫 Remove variable            |

---

## 👥 User Management

| Command                         | Description                 |
| ------------------------------- | --------------------------- |
| `sudo adduser username`         | 👤 Add a new user           |
| `sudo passwd -l username`       | 🔑 Change user's password   |
| `sudo userdel -r username`      | ❌ Remove user               |
| `sudo usermod -a -G group user` | ➕ Add user to group         |
| `sudo deluser user group`       | ➖ Remove user from group    |
| `finger`                        | 🔍 Show all logged-in users |
| `finger username`               | 🔍 Info about specific user |

---

## 🌐 Networking

| Command                   | Description               |
| ------------------------- | ------------------------- |
| `ssh user@ip`             | 🔐 SSH into remote server |
| `ping hostname`           | 📡 Test connectivity      |
| `scp file user@host:path` | 🚀 Secure file copy       |
| `wget url`                | ⬇️ Download files via URL |

---

## 🔄 File Transfer via FTP (basic commands)

| Command      | Description                  |
| ------------ | ---------------------------- |
| `dir`        | 📁 List files on remote      |
| `cd dirname` | 📂 Change directory remotely |
| `put file`   | ⬆️ Upload file               |
| `get file`   | ⬇️ Download file             |
| `quit`       | 🚪 Logout                    |

---

## 🔄 Process Management

| Command         | Description                   |
| --------------- | ----------------------------- |
| `top`           | 📊 Show active processes      |
| `ps`            | 👀 View running processes     |
| `ps PID`        | 🔎 Info on a specific process |
| `pidof process` | 🆔 Get PID of process         |
| `kill PID`      | 🔫 Kill a process             |
| `bg`            | 💤 Send job to background     |
| `fg`            | 🔦 Bring job to foreground    |
| `nice`          | 🎩 Start with priority        |
| `renice`        | 🔧 Change priority of process |

---

## 🧠 System Info

| Command   | Description         |
| --------- | ------------------- |
| `df`      | 💽 Disk space usage |
| `free`    | 🧠 Memory usage     |
| `history` | 🕘 Command history  |
| `clear`   | 🧼 Clear terminal   |

---

## 🔎 Help & Info

| Command       | Description           |
| ------------- | --------------------- |
| `man command` | 📘 Manual for command |

---

# 📝 VIM Editor Cheat Sheet

## 🚪 Exit & Save

| Command | Description            |
| ------- | ---------------------- |
| `:q`    | ❌ Quit                 |
| `:w`    | 💾 Save                |
| `:wq`   | 💾❌ Save and quit      |
| `:q!`   | 🚫 Quit without saving |
| `:wq!`  | 💾❗Force save and quit |

---

## 🖋️ Insert Text

| Command | Description               |
| ------- | ------------------------- |
| `i`     | ✍️ Insert at cursor       |
| `A`     | 🖊️ Insert at end of line |
| `o`     | ➕ Open new line below     |
| `ESC`   | ⛔ Exit insert mode        |

---

## 🧹 Editing

| Command | Description                       |
| ------- | --------------------------------- |
| `u`     | ↩️ Undo last change               |
| `U`     | 🔁 Undo entire line               |
| `dd`    | ✂️ Delete line                    |
| `D`     | ✂️ Delete from cursor to line end |
| `C`     | 🔄 Change line after cursor       |
| `dw`    | 🧽 Delete word                    |
| `4dw`   | 🧽 Delete 4 words                 |
| `cw`    | ✏️ Change word                    |
| `~`     | 🔁 Toggle character case          |

---

## 🧭 Navigation

| Command | Description            |
| ------- | ---------------------- |
| `gg`    | 🔝 Go to start of file |
| `G`     | 🔚 Go to end of file   |
| `^`     | ⬅️ Start of line       |
| `$`     | ➡️ End of line         |
