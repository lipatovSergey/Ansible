#ansible 
#### 1. **Defining and Using Variables**

- Variables can be defined in **inventory structure** (`group_vars`, `host_vars`) to assign different values per host or group.
    
- You can also define them **inline in a playbook** under `vars` — these will be available across all tasks.
    

#### 2. **Printing Variables with `debug`**

- Use the **`debug` module** to inspect variable values.
    
- `var` parameter shows only the variable's value:  
    `debug: var: secret`
    
- `msg` allows embedding a variable into a custom message:  
    `debug: msg: "Secret word is {{ secret }}"`
    

#### 3. **Ansible Facts (Predefined Variables)**

- Ansible gathers many system details automatically, called **facts** (e.g., `ansible_distribution`, `ansible_os_family`).
    
- These facts help create adaptive playbooks (e.g., install Apache differently on Ubuntu and RedHat).
    
- Use `debug` with `var` to display them.
    

#### 4. **Creating Variables with `set_fact`**

- The **`set_fact` module** lets you define new variables at runtime or override existing ones.
    
- Useful for **combining values** (e.g., `full_message` = `message1 + message2`).
    
- You can then debug this new variable.
    

#### 5. **Storing Command Output with `register`**

- Use `register` to **store the result of a command** (e.g., `shell: uptime register: result`).
    
- The result is stored as a variable and can be inspected with `debug: var: result`.
    
- Use dot notation (e.g., `result.stdout`) to access specific parts of the output.
    
