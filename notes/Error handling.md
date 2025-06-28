#ansible 
### 🔧 **How Ansible handles errors by default:**

- When a playbook runs on multiple servers, if **a task fails on one server**, all **next tasks will be skipped** on that server.
    
- Example:
    
    - Task 1 runs everywhere → OK
        
    - Task 2 fails on Server 2
        
    - Tasks 3 and 4 will **not run on Server 2**, but will still run on Servers 1 and 3
        

---

### ✅ **Ways to control errors in Ansible:**

#### 1. `ignore_errors: yes`

- This tells Ansible to **ignore a task failure** and keep running the rest of the playbook.
    
- Even if a task fails, Ansible **won’t stop** for that server.
    
- Example:
    
    - Installing a non-existent package (Task 1) fails
        
    - But with `ignore_errors: yes`, Task 2 and 3 will still run.
        

---

#### 2. `failed_when`

- This lets you **decide what counts as a failure**, even if the command technically succeeded.
    
- You must **register the output** first using `register: result`
    
- Then you use conditions like:
    
    - `failed_when: "'fail' in result.stdout"` → Fail if the output contains "fail"
        
    - `failed_when: result.rc != 0` → Fail if return code is not zero
        

Example:

```yaml
- name: Say hello
  command: echo "hello world"
  register: result
  failed_when: "'world' in result.stdout"
```

- If output contains "world", task is marked as **failed**, and next tasks will not run on that server.
    

---

#### 3. `any_errors_fatal: yes`

- By default, when a task fails on one server, **other servers keep going**.
    
- But with this setting, **if one fails, all stop**.
    
- Add it to the play level:
    

```yaml
- hosts: all
  any_errors_fatal: yes
```

- Now, if any task fails anywhere, **the whole playbook stops for all servers**.
    

---

### 💡 Summary:

Use these options to **control how Ansible reacts to errors**, depending on your needs:

- `ignore_errors`: continue even if task fails
    
- `failed_when`: custom rules to define a failure
    
- `any_errors_fatal`: stop everything if one server fails