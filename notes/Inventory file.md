#ansible k
## Rules of Inventory file

All hosts from `hosts.txt` file belong to two groups:
- All: It's actually all servers(hosts) that we have.
- Ungrouped: All servers that not in any group. 

We can add hosts simply by there IP address (ungrouped)
```bash
10.50.1.1
10.51.1.2
```

We can combine them into groups
```bash
[staging_DB]
192.168.1.1
192.168.1.2

[staging_WEB]
192.168.2.1
192.168.2.2

[staging_APP]
192.168.3.1
192.168.3.2
```

Combine groups into one group
```bash
[staging_ALL:children]
staging_DB
staging_WEB
staging_APP
```

One more example of grouped groups 
```bash
[prod_DB]
10.10.1.1

[prod_WEB]
10.10.2.2

[prod_APP]
10.10.3.3

[prod_ALL:children]
prod_DB
prod_WEB
prod_APP
```

<font color="#00b050">adv-it author recommends to keep Inventory file clear and write inside only IP addresses or web servers all variables better to keep in separate files.</font> 
We can put variables like `ansible_user` into vars groups
```bash
[staging_servers]
linux1 ansible_host=192.168.132.27

[prod_servers]
linux2 ansible_host=192.168.132.222
linux3 ansible_host=192.168.132.3

[all:vars]
ansible_user=vboxuser
ansible_ssh_private_key_file=/home/jovani/.ssh/id_ed25519
```

