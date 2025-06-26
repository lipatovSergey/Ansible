#ansible 
## 🧩 **Why use `import` and `include` in Ansible?**

In large Ansible projects, it's smart to **split your logic** into smaller pieces (files, roles, task lists).  
Ansible gives you two main ways to reuse code:  
👉 `import_*` (static) and 👉 `include_*` (dynamic).

---

## 🔍 **Key Difference: Parse Time vs Run Time**

| Feature            | `import_*`                            | `include_*`                           |
| ------------------ | ------------------------------------- | ------------------------------------- |
| 🔁 Loaded          | When the playbook starts (parse time) | During task execution (run time)      |
| 📍Supports `when`  | ❌ No                                  | ✅ Yes                                 |
| 🔂 Supports `loop` | ❌ No                                  | ✅ Yes                                 |
| 📌 Flexibility     | Low (always included)                 | High (included only when needed)      |
| ✅ Use for          | Always-needed tasks, static logic     | Conditional or repeated logic         |
| 🛑 Don’t use if    | You want to control it with `when`    | You want to load tasks before running |

---

## 📦 **What to use for different situations**

| Purpose          | Use This Module                   | Example                             |
| ---------------- | --------------------------------- | ----------------------------------- |
| ✅ Whole playbook | `import_playbook`                 | `- import_playbook: base_setup.yml` |
| ✅ List of tasks  | `import_tasks` or `include_tasks` | `- include_tasks: configure.yml`    |
| ✅ Roles          | `import_role` or `include_role`   | `- include_role: name=myrole`       |
| ✅ Load variables | `include_vars`                    | `- include_vars: myvars.yml`        |

---

## ✅ **Examples**

### 1. `import_playbook`

Use when you want to always run another playbook:

```yaml
- import_playbook: monitoring.yml
```

### 2. `include_tasks`

Use when you want to include tasks **only if needed**:

```yaml
tasks:
  - include_tasks: ubuntu.yml
    when: ansible_distribution == "Ubuntu"
```

### 3. `import_tasks`

Use when you always want to include these tasks (no conditions):

```yaml
tasks:
  - import_tasks: install.yml
```

### 4. `include_role`

Use when you want to include a role conditionally or multiple times:

```yaml
tasks:
  - include_role:
      name: create_user
    vars:
      username: john
```

### 5. `import_role`

Use when the role should always run, no matter what:

```yaml
tasks:
  - import_role:
      name: common_firewall
```

---

## 💡 **When to use each method**

| If you want to...                                | Use         |
| ------------------------------------------------ | ----------- |
| Run something always, unconditionally            | `import_*`  |
| Control execution with `when`, `loop`, or `vars` | `include_*` |
| Use logic that depends on facts or conditions    | `include_*` |
| Make the playbook faster to parse or cleaner     | `import_*`  |
| Reuse tasks in different ways or with variations | `include_*` |

---

## ⚠️ Important Note

- Old `- include:` (used before Ansible 2.4) is **deprecated** and will cause errors.
    
- In modern Ansible, always use **specific modules** like `include_tasks`, `import_role`, etc.
    
- You can use either short names (`include_tasks`) or full names (`ansible.builtin.include_tasks`), but full names are safer in big projects.
    
