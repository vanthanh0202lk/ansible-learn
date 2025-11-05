# 🧩 Ansible Keywords & Core Concepts Summary

A complete reference of essential Ansible components, syntax, and keywords — ideal for quick learning or interview preparation.

---

## 🧱 1️⃣ Core Building Blocks

| **Keyword / Concept** | **Purpose / Description** | **Example** |
|------------------------|----------------------------|-------------|
| **Inventory** | Lists all the managed hosts or groups of hosts. Usually in `inventory.ini` or YAML. | `[web]\nserver1 ansible_host=10.0.0.1 ansible_user=ubuntu` |
| **Playbook** | YAML file that defines what tasks to run on which hosts. | `site.yml`, `k8s-setup.yml` |
| **Play** | A section inside a playbook that maps **hosts → tasks**. | `- name: Setup web servers\n  hosts: web` |
| **Task** | A single operation (module execution). Each play runs multiple tasks. | `- name: Install nginx\n  apt: name=nginx state=present` |
| **Module** | A reusable unit of work (like “install package”, “copy file”, “run command”). | `ansible.builtin.copy`, `ansible.builtin.shell`, `ansible.builtin.apt` |
| **Handler** | A special task that runs **only when notified** (usually for restarting services). | `notify: Restart nginx` |
| **Variable (vars)** | Used to parameterize playbooks. | `vars:\n  app_port: 8080` |
| **Facts** | Automatically collected system info (`ansible_facts`). | `{{ ansible_hostname }}` |
| **Template** | Dynamic config file using Jinja2 syntax. | `/templates/nginx.conf.j2 → /etc/nginx/nginx.conf` |
| **Conditionals (when)** | Run tasks only if condition true. | `when: ansible_os_family == "Debian"` |
| **Loops (loop)** | Repeat a task multiple times. | `loop: [nginx, mysql, php]` |
| **Tags** | Label tasks to run selectively. | `tags: [install, setup]` |
| **Delegation** | Run task on another host. | `delegate_to: localhost` |
| **Become** | Run tasks as root (sudo). | `become: yes` |
| **Environment** | Set environment variables for tasks. | `environment: { PATH: "/usr/local/bin:{{ ansible_env.PATH }}" }` |

---

## 🧩 2️⃣ Structuring Large Projects

| **Keyword / Concept** | **Purpose / Description** | **Example** |
|------------------------|----------------------------|-------------|
| **Role** | A reusable collection of tasks, variables, templates, handlers, etc. | `roles/web/tasks/main.yml` |
| **Include / Import** | Load another playbook, task, or vars file. | `include_tasks: setup.yml` |
| **Defaults** | Lowest-priority vars in a role. | `roles/myrole/defaults/main.yml` |
| **Vars (in roles)** | Higher-priority vars for a role. | `roles/myrole/vars/main.yml` |
| **Files** | Directory for static files. | `roles/web/files/index.html` |
| **Templates** | Directory for Jinja2 templates. | `roles/web/templates/config.j2` |
| **Handlers** | Directory for notification-based tasks. | `roles/web/handlers/main.yml` |

---

## ⚙️ 3️⃣ Execution Flow

```
Inventory → Playbook → Play → Tasks → Modules → Handlers
```

Example mini playbook:

```yaml
- name: Install and start nginx
  hosts: web
  become: yes

  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
      notify: Restart nginx

  handlers:
    - name: Restart nginx
      service:
        name: nginx
        state: restarted
```

---

## 🧠 4️⃣ Common Built-in Modules

| Category | Modules |
|-----------|----------|
| Package management | `apt`, `yum`, `dnf`, `package` |
| File operations | `copy`, `template`, `file`, `fetch`, `unarchive` |
| Service management | `service`, `systemd` |
| Command execution | `shell`, `command`, `script` |
| User management | `user`, `group`, `authorized_key` |
| Networking | `ufw`, `firewalld`, `iptables` |
| Cloud / K8s | `k8s`, `helm`, `aws_ec2`, etc. |

---

## 🧩 5️⃣ Play Example

```yaml
- name: Example play
  hosts: all
  become: yes
  gather_facts: yes
  vars:
    app_port: 8080
  environment:
    PATH: "/usr/local/bin:{{ ansible_env.PATH }}"
  tasks:
    - name: Create directory
      file:
        path: /opt/myapp
        state: directory
        mode: '0755'
```

---

## 🧱 6️⃣ Best Practice Folder Layout

```
ansible-project/
│
├── inventory.ini
├── playbooks/
│   ├── site.yml
│   ├── db.yml
│   └── web.yml
├── roles/
│   ├── web/
│   │   ├── tasks/main.yml
│   │   ├── handlers/main.yml
│   │   ├── templates/
│   │   ├── files/
│   │   ├── vars/main.yml
│   │   └── defaults/main.yml
│   └── db/
│       └── ...
└── group_vars/
    └── all.yml
```

---

## ⚡ 7️⃣ Keywords Quick Reference

| Keyword | Meaning |
|----------|---------|
| `hosts` | Which servers this play applies to |
| `become` | Run tasks as root (sudo) |
| `vars` | Define variables |
| `tasks` | List of operations to perform |
| `roles` | Attach predefined role logic |
| `notify` | Trigger handler |
| `when` | Conditional execution |
| `loop` / `with_items` | Iterate list of items |
| `tags` | Label tasks for selective run |
| `environment` | Define environment vars |
| `delegate_to` | Run task on another host |
| `register` | Store output of a command |
| `ignore_errors` | Continue even if task fails |
| `block` | Group of tasks (with `rescue`, `always`) |

---

✅ **Use this cheat sheet** to quickly recall Ansible syntax when writing or reviewing playbooks, roles, or automation pipelines.
