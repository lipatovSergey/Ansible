#ansible 
### 🧩 What are templates?

- Templates are text files that use **Jinja2** syntax.
    
- They usually end with `.j2` (like `index.j2`).
    
- You can write variables inside them using `{{ variable_name }}`.
    

---

### 📦 System and custom variables

- Ansible provides built-in variables like:
    
    - `ansible_hostname`
        
    - `ansible_distribution`
        
    - `ansible_distribution_version`
        
    - `ansible_all_ipv4_addresses`
        
- You can also create **your own variables**, like `owner = Petya` or `owner = Vasya`.
    

---

### 🖥️ Example in the video

The teacher shows how to create a simple **HTML file** using a template.

- The original file `index.html` becomes `index.j2`.
    
- Inside the file, they insert variables like:
    
    - `owner`
        
    - `ansible_hostname`
        
    - `ansible_distribution`
        
    - `ansible_all_ipv4_addresses`
        
- This makes every server get its **own version** of `index.html`.
    

---

### 📥 The `template` module

- You use the `template` module (not `copy`) in the playbook.
    
- Example:
    
    ```yaml
    - name: Generate index.html
      template:
        src: index.j2
        dest: /var/www/html/index.html
        mode: '0644'
    ```
    
- The template module reads the `.j2` file, replaces variables, and creates the final file.
    

---

### 🔄 Restart service if file changes

- If the file changes, the playbook can notify Apache (or any service) to restart.
    

---
