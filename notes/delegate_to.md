#ansible 
## 🔁 What is `delegate_to` in Ansible?

In a normal Ansible playbook, all tasks run on the **hosts listed in the `hosts` section**.

But sometimes you want **a specific task to run on a different machine** — not the one it was sent to.  
This is exactly what `delegate_to` is for.

---

### 🧩 1. Why use `delegate_to`?

Imagine this situation:

- Your playbook runs on two servers: `linux1` and `linux3`.
    
- You want to create one file on **only `linux3`**, not both.
    

If you don’t use `delegate_to`, the file will be created on **both** servers.

But if you add this to your task:

```yaml
delegate_to: linux3
```

Then even when the task is _sent_ to `linux1`, it **won’t execute there** — it will be redirected to `linux3`.

This way, the file is created **only on `linux3`**.

---

### 💻 2. Run a task on the Ansible master (localhost)

Sometimes you don’t want the task to run on **any target host** at all.

For example:

- You want to collect info from other servers and **write a log file** only on your control node (Ansible master).
    
- Or you want to call an external API, update a database, or perform some other **centralized action**.
    

In that case, you can delegate the task to your own machine (the one running the playbook):

```yaml
delegate_to: 127.0.0.1
```

or

```yaml
delegate_to: localhost
```

So even if the task is “sent” to all servers, **only your master machine will run it**.

---

## 🔄 3. Use case: Rebooting servers and waiting for them

When you reboot a server using Ansible, the SSH connection **breaks** — this causes errors.

To handle it properly, do this:

---

### ✅ Step 1: Reboot the server _in the background_

```yaml
- name: Reboot the server
  shell: reboot now
  async: 1
  poll: 0
```

- `async: 1` → the task is allowed to run for 1 second
    
- `poll: 0` → Ansible will **not wait** for the command to finish. It just sends it and moves on
    

This avoids problems from losing the SSH connection.

---

### ✅ Step 2: Wait for the server to come back (from the master)

Use the `wait_for` module, but **run it from your Ansible master**, not the target:

```yaml
- name: Wait for server to be ready
  wait_for:
    host: "{{ inventory_hostname }}"
    port: 22
    state: started
    delay: 5
    timeout: 40
  delegate_to: 127.0.0.1
```

Here:

- `host` = the server we rebooted
    
- `delegate_to` = run this task on the Ansible master
    
- `wait_for` = checks every few seconds if the SSH port (22) is available again
    

This way, your Ansible master safely waits for all servers to reboot.

---

## ☝️ What is `run_once`?

By default, Ansible runs every task on **every host in your inventory**.

But sometimes you need a task to run **only one time**.

Examples:

- Creating a shared database
    
- Backing up something once
    
- Sending a notification only once
    

Use this:

```yaml
run_once: true
```

This tells Ansible:  
"Only run this task one time, not on every server."

By default, Ansible will choose the **first host in your inventory** to run it on.

---

### 🔗 You can combine `run_once` with `delegate_to`

Let’s say you want to run a task once, but **on a specific machine**, like the master.

You can do this:

```yaml
run_once: true
delegate_to: localhost
```

Now the task will:

- Run only **one time**
    
- Run on the **Ansible master**
    

This is very useful for tasks like:

- Creating a central report
    
- Sending an API request
    
- Collecting data into a single file
    

---

## ✅ Summary

| Feature       | What it does                                   |
| ------------- | ---------------------------------------------- |
| `delegate_to` | Sends the task to a **different host**         |
| `run_once`    | Runs the task **only one time**, not per host  |
| Combined      | Run **only once**, and on **specific machine** |

---

## 🧠 When to use each:

| Situation                                 | What to use                                                    |
| ----------------------------------------- | -------------------------------------------------------------- |
| Task must run only on `linux3`            | `delegate_to: linux3`                                          |
| Task must run only on the control node    | `delegate_to: localhost`                                       |
| Task must run once for the whole playbook | `run_once: true`                                               |
| Task must run once and on a specific host | Both: `run_once` + `delegate_to`                               |
| Rebooting servers and waiting             | `reboot` with `async/poll`, then `wait_for` with `delegate_to` |

