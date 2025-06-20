#ansible 
### 1. **Client Preparation & SSH Access**

- The instructor used **3 Linux servers on AWS**, but you can use any Linux machines (e.g., VirtualBox, VMware).
    
- **SSH key-based authentication** is recommended over passwords for reliability and security.
    
- The instructor uses **multiple SSH keys**, one per server.
    
---

### 2. **Transferring SSH Keys to Ansible Master**

- Copy your **private keys** to the Ansible control node (master) into the `~/.ssh/` directory.
    
- After copying, run `chmod 400 keyfile` to secure your key files.
    

---

### 3. **Creating the Inventory File ([[Inventory file]])**

- Create a project directory (e.g., `ansible/`), then a file called `hosts.txt` inside it.
- It's possible to add servers only by there IP or domain name.   Or even create aliases
```bash
# IP
192.168.132.27
192.168.132.222
# Domain name
webserver.google.com
# Alias
myserver ansible_host=192.168.132.43
``` 
	
    
- Organize servers into groups using square brackets (e.g., `[staging_servers]`).
    
- For each server, specify:
    
    - `ansible_host`: IP address of the target machine
        
    - `ansible_user`: SSH user
        
    - `ansible_ssh_private_key_file`: path to the corresponding private key  
        _(or use `ansible_password`, though this is not recommended)

---

### 4. **Testing the Connection**

- Use the built-in **ping module** to test access:
    `ansible <group_or_all> -i hosts.txt -m ping`
    
- On first connection, Ansible may ask to confirm the SSH fingerprint (`yes`).
    
- A successful connection will return:
    `"ping": "pong"`
---

### 5. **Creating a Config File (`ansible.cfg`)**

- To simplify your workflow:
    `[defaults] inventory = hosts.txt host_key_checking = False`
    
- This lets you run commands like:
    `ansible all -m ping`
    
    without needing to specify `-i` or deal with SSH fingerprint prompts every time.