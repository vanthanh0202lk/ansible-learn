# 🧠 Learn Ansible — Quick Start Guide with Code Examples

This README gives you a fast and practical way to start learning **Ansible** using code examples.

---

## 📘 What is Ansible?

**Ansible** is an automation tool used for:
- Server configuration (e.g., install packages, copy files)
- Application deployment
- Infrastructure orchestration
- Multi-node automation (hundreds of servers)

✅ **Agentless** — it only needs SSH + Python on target servers.
✅ Uses **YAML** playbooks — human-readable configuration.

---

## 🧩 Core Concepts

| Concept | Description |
|----------|-------------|
| **Inventory** | List of servers to manage |
| **Playbook** | YAML file describing automation tasks |
| **Task** | A single step (install, copy, restart) |
| **Module** | Reusable action like `apt`, `copy`, `service` |
| **Handler** | Triggered only when notified (e.g., restart service) |
| **Variable** | Store values for reuse |
| **Role** | Organized folder structure for large projects |

---

## ⚙️ Install Ansible (Ubuntu)

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install ansible -y
ansible --version
```

---

## 📁 Example Project Structure

```bash
ansible-demo-basic/
├── inventory.ini
└── test.yml
```

**`inventory.ini`**
```ini
[local]
localhost ansible_connection=local


```
**`test.yml`**
- hosts: local
  tasks:
    - name: Ping Google DNS
      ansible.builtin.command: ping -c 4 8.8.8.8


---

Run it:
```bash
ansible-playbook -i inventory.ini test.yml
```


---

## 🌐 ansible-lv2— Setup Nginx Web Server

**`webserver.yml`**
```yaml
---
- name: Setup web server
  hosts: local
  become: yes   # use sudo
  tasks:
    - name: Install Nginx
      ansible.builtin.apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Ensure Nginx is running
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: yes

    - name: Create custom index page
      ansible.builtin.copy:
        dest: /var/www/html/index.html
        content: |
          <h1>Hello from Ansible 🚀</h1>
          <p>Deployed automatically at {{ ansible_date_time.iso8601 }}</p>


```
Run:
```bash
ansible-playbook -i inventory.ini webserver.yml -K
```


---

## 🧠 Level-Up Learning Path

| Level | Focus | Goal |
|--------|--------|------|
| **1️⃣ Beginner** | Inventory, Playbooks, Tasks, Modules | Automate installs, copy files |
| **2️⃣ Intermediate** | Variables, Templates, Handlers | Dynamic content, reusable configs |
| **3️⃣ Advanced** | Roles, Group Vars, Conditionals, Loops | Full automation pipelines |
| **4️⃣ Expert** | Deploy clusters, integrate CI/CD | Automate K8s, multi-node systems |

---

## 🧰 Useful Commands

```bash
ansible -i inventory.ini all -m ping         # Test connection
ansible -i inventory.ini web -a "uptime"     # Run command on web group
ansible-playbook -i inventory.ini site.yml    # Run playbook
```

---

## 💡 Tips

- Use `-K` to prompt for sudo password.
- Use `--check` for dry-run mode.
- Use `--limit web` to run on specific group.
- Combine with Git for version control.

---

## 🧩 Next Step — Setup Kubernetes with Ansible
After mastering the basics, try creating a multi-node Kubernetes cluster using Ansible (as shown in the advanced guide).

---

📘 **Summary:**  
Ansible lets you automate configuration, deployment, and management with simple YAML playbooks. Start small — ping, install packages, copy files — then move to advanced roles and cluster orchestration.

