## 📦 Inventory Management

### List hosts and groups from your inventory:

```bash
ansible-inventory --list
```

Shows the full structure of your inventory (in JSON).

### Show inventory as a tree:

```bash
ansible-inventory --graph
```

Displays a tree of groups and hosts for better visual understanding.

---

## 🖥️ Gathering Host Information

### Collect system facts from hosts:

```bash
ansible all -m setup
```

- Returns detailed JSON data: IP, OS, hostname, architecture, memory, etc.
    

---

## 🚀 General Command Structure

All commands below assume:

```bash
ansible all -m <module> -a "<args>" [options]
```

| Option              | Description                                                |
| ------------------- | ---------------------------------------------------------- |
| `-m`                | Specifies the module to use (e.g. `ping`, `shell`, `copy`) |
| `-a`                | Provides arguments for the selected module                 |
| `-b`                | Runs the task with `sudo` (become) privileges              |
| `--ask-become-pass` | Prompts for sudo password if required on remote hosts      |
| `-v` / `-vvv`       | Enables verbose output to see task results or debug issues |

---

## 🔧 Modules Overview

### 📂 `copy` — Copy a file to remote host

```bash
-m copy -a "src=./Privet.txt dest=/home mode=0777" -b --ask-become-pass
```

- `src`: local path on the control machine
    
- `dest`: destination path on the remote host
    
- `mode`: file permission (e.g. `0644`, `0777`)
    

---

### 🐚 `shell` — Run shell commands

```bash
-m shell -a "uptime"
```

- Executes command in a shell (supports pipes, redirects, environment vars).
    

### 🧾 `command` — Run basic commands

```bash
-m command -a "uptime"
```

- Runs commands **without shell**. Symbols like `$HOME`, `|`, `&&` will **not** work.
    

---

### 📦 `apt` — Install packages (Debian/Ubuntu)

```bash
-m apt -a "name=stress state=present update_cache=yes" -b --ask-become-pass
```

- `name`: package name
    
- `state=present`: ensure the package is installed
    
- `update_cache=yes`: run `apt update` before installing
    

> For RHEL/CentOS use `yum` or `dnf` module instead.

---

### 🌐 `uri` — Send HTTP requests (like curl)

```bash
-m uri -a "url=https://example.com return_content=yes"
```

- By default performs a **GET** request
    
- Use `method=POST|PUT|DELETE` for other types
    
- Add `body`, `headers`, `status_code`, `body_format=json` for API work
    
- Add `return_content=yes` to get response body
    

---

## 🧠 Notes

- Prefer `shell` only when you need shell features.
    
- `command` is safer and faster when simple commands are enough.
    
- Always test `apt`, `copy`, and `service` modules with `-b` if they require root.
    
- Use `--check` with `ansible-playbook` for dry-run behavior (not supported in `ansible` CLI).
    

---

Хочешь, я помогу сделать аналогичное summary для `ansible-playbook`?