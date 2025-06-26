#ansible 

`extra-vars` are a way to **pass variables from the command line** when running a playbook. They make playbooks more flexible and reusable without editing files every time.

---

### **1. Why use `extra-vars`?**

- In the example, servers are grouped as `production` and `staging`.
    
- Normally, to run a playbook for a specific group, you would have to **edit the playbook file** and change the host group manually.
    
- With `extra-vars`, you can pass the target group directly **from the terminal**, no need to edit the playbook.
    

---

### **2. Using a host variable in the playbook**

- Instead of writing `hosts: all`, use a variable like:
    
    ```yaml
    hosts: "{{ my_host }}"
    ```
    
- This means Ansible expects a value for `my_host` when you run the playbook.
    

---

### **3. How to pass `extra-vars`**

- Use `-e` when running your playbook:
    
    ```bash
    ansible-playbook playbook.yml -e "my_host=staging"
    ```
    
- You can pass **multiple variables** like this:
    
    ```bash
    -e "var1=value1 var2=value2"
    ```
    

---

### **4. `extra-vars` have the highest priority**

- If a variable like `owner` is defined both in the inventory and with `-e`, the `-e` value will **override** the others.
    
- Example:
    
    ```bash
    ansible-playbook playbook.yml -e "my_host=prod owner=Denis"
    ```
    
    Even if the inventory sets `owner=petia`, the website will show "Denis".
    

---

### **5. Missing variables cause errors**

- If your playbook uses a variable (like `my_host`) and you don’t pass it, Ansible will **fail with an error**.
    
- So be sure to define all required variables.
