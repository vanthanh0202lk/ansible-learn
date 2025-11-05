Let’s install Ansible on Ubuntu step by step.
🧩 Step 1: Update your system
run bash command: sudo apt update && sudo apt upgrade -y

🧰 Step 2: Install Ansible

Ubuntu 22.04+ has Ansible available via the official repository:

run bash command: sudo apt install ansible -y

🧪 Step 3: Verify installation
ansible --version

Run Ansible Script:

Example: for ansible-demo-basic
cd ansible-demo-basic
ansible-playbook -i inventory.ini test.yml

Example: for ansible-lv2
cd ansible-lv2
ansible-playbook -i inventory.ini webserver.yml

Example: for ansible-k8s, this project need 3 node: one k8s_master, two k8s_workers

🪄 Step 1: Generate a new SSH key pair
ssh-keygen -t rsa -b 4096 -C "thanh@ansible"

🧩 Step 2: Copy the public key to your remote nodes
ssh-copy-id -i ~/.ssh/id_rsa.pub thanh@172.168.8.5
ssh-copy-id -i ~/.ssh/id_rsa.pub thanh@172.168.8.12
ssh-copy-id -i ~/.ssh/id_rsa.pub thanh@172.168.8.13


🚀 Step 3: Run your playbook again
cd ansible-k8s
ansible-playbook -i inventory.ini k8s-setup.yml -K