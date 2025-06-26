#ansible 
### **1. Creating a Role Structure**

- First, create a folder called `roles`.
    
- Then use this command inside it:
    
    ```bash
    ansible-galaxy init <role_name>
    ```
    
- Example: `ansible-galaxy init deploy_apache_web`
    
- This creates many subfolders inside `roles/deploy_apache_web`.
    

---

### **2. Role Folder Structure**

Each folder has a purpose:

- `tasks/` – main tasks (put them in `main.yml`)
    
- `handlers/` – handlers (like restarting services)
    
- `defaults/` – default variables
    
- `vars/` – other variables (similar to `defaults`)
    
- `files/` – regular files to copy to servers
    
- `templates/` – Jinja2 templates (like `.j2` files)
    
- `meta/` – role metadata (not used in the lesson)
    
- `tests/` – for testing (can be deleted if not used)
    

---

### **3. Moving Old Playbook Content into the Role**

- Move tasks to `tasks/main.yml`.
    
- Move variables to `defaults/main.yml`.
    
- Put image files (e.g. `.png`) into `files/`.
    
- Put template files (e.g. `index.j2`) into `templates/`.
    
- Move handlers into `handlers/main.yml`.
    

Note: You don’t need full paths anymore when using `copy` or `template`. Just use the file name.

---

### **4. Using the Role in a Playbook**

- The main playbook becomes short and simple.
    
- It just includes the role like this:
    
    ```yaml
    roles:
      - deploy_apache_web
    ```
    
- You can also add `when` conditions or pass variables.
    

---

### **5. Running the Playbook**

- Run your playbook:
    
    ```bash
    ansible-playbook our_small_playbook.yml
    ```
    
- Apache is installed, files are copied, the website is generated, and everything works!
