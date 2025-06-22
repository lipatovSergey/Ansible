#ansible 
Sure! Here's your personal summary rewritten in **simple and clear English**, like you're writing it just for yourself:

---

## 🧾 My Notes on Writing Ansible Playbooks

---

### What is a playbook?

A playbook is a `.yml` file where I tell Ansible what to do on my servers.  
Unlike ad-hoc commands (one-time commands), a playbook runs **multiple steps** in one go.  
It’s repeatable and great for automation.

---

## 🔧 Basic playbook structure

```yaml
---
- name: What this playbook does
  hosts: all
  become: yes     # use sudo (root) if needed
  vars:           # optional variables
    var_name: value
  tasks:
    - name: Task description
      module_name:
        option: value
  handlers:       # optional actions triggered only if needed
    - name: restart service
      service:
        name: httpd
        state: restarted
```

---

## 🔹 Examples I’ve already used

### 1. Ping servers

```yaml
- name: Test connection
  hosts: all
  become: no
  gather_facts: false
  tasks:
    - name: Ping
      ansible.builtin.ping:
```

> Just to check if servers are online. No `sudo` needed.

---

### 2. Install and start Apache

```yaml
- name: Install Apache
  hosts: all
  become: yes
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: latest

    - name: Start and enable Apache
      service:
        name: apache2
        state: started
        enabled: yes
```

> For Ubuntu-based systems. Uses `apt` to install and `service` to run Apache

---

### 3. Install Apache + upload my custom HTML page

```yaml
- name: Install Apache and upload index.html
  hosts: all
  become: yes
  vars:
    source_file: website/index.html
    destination_file: /var/www/html/index.html

  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: latest

    - name: Copy index.html
      copy:
        src: "{{ source_file }}"
        dest: "{{ destination_file }}"
        mode: '0755'
      notify: restart apache

    - name: Make sure Apache is running
      service:
        name: apache2
        state: started
        enabled: yes

  handlers:
    - name: restart apache
      service:
        name: apache2
        state: restarted
```

> Apache only restarts **if the HTML page changed** — thanks to `notify` + `handlers`.

---

## 💡 What I’ve learned

| What it is            | Why it matters                              |
| --------------------- | ------------------------------------------- |
| `become: yes`         | Needed for sudo/root tasks                  |
| `gather_facts: false` | Faster when I don’t need system info        |
| `notify` + `handlers` | Restart service only if something changed   |
| `vars`                | Makes playbooks cleaner and easier to edit  |
| Idempotent            | Ansible doesn’t repeat things if not needed |
| `--ask-become-pass`   | Use this to enter sudo password when needed |

---

## 🧪 How I check if Apache is running

From the terminal:

```bash
ansible all -m command -a "systemctl is-active httpd" -b --ask-become-pass
```

> This shows me if Apache is running on all servers.

---

If my playbook doesn’t need sudo, I just remove `become: yes`. That’s it.

