Since you have already completed the previous experiment, this question is the same experiment with the procedure written as a task statement. You can use your existing work directly.

Ex. No. 8 — Install Ansible and Configure Ansible Roles and Write Playbooks
Aim:
To install Ansible on a Linux system, create three Ansible roles named prerequisites, PostgreSQL, and apache to install Git, PostgreSQL, and Apache respectively, configure the tasks, execute them using a stack.yml playbook on localhost, and verify successful installation.

Procedure
Update the package index:

sudo apt update
Install the required packages and add the Ansible repository:

sudo apt upgrade
sudo apt install -y software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install -y ansible
Verify Ansible installation:

ansible --version
Navigate to the Ansible directory and check the configuration files:

cd /etc/ansible
ls -la
Create the three roles:

sudo ansible-galaxy init prerequisites
sudo ansible-galaxy init PostgreSQL
sudo ansible-galaxy init apache
Configure the role tasks.

prerequisites/tasks/main.yml

---
- name: Install Git
  apt:
    name: git
    state: present
    update_cache: yes
PostgreSQL/tasks/main.yml

---
- name: Install PostgreSQL
  apt:
    name: postgresql
    state: present
    update_cache: yes

- name: Start PostgreSQL service
  systemd:
    name: postgresql
    state: started
    enabled: yes
apache/tasks/main.yml

---
- name: Install Apache web server
  apt:
    name: apache2
    state: present
    update_cache: yes
Create the stack.yml playbook:

---
- hosts: localhost
  connection: local
  become: true

  roles:
    - prerequisites
    - PostgreSQL
    - apache
Execute the playbook:

sudo ansible-playbook stack.yml
Verify the installation:

git --version
apache2 -v
psql --version
sudo systemctl status postgresql
sudo systemctl status apache2
Result
Thus, Ansible was successfully installed, three roles (prerequisites, PostgreSQL, and apache) were created and configured, and the stack.yml playbook was executed on localhost to install and verify Git, PostgreSQL, and Apache web server successfully.

Since you have already completed the installation and Apache web application deployment experiment, you can use the same playbook with a slightly modified welcome message for this question. No need to create a new project or reinstall Ansible.

Ex. No: 8 – Create an Ansible Playbook to Install Apache Web Server
Aim:

To create an Ansible playbook to install the Apache web server on a Linux system, start and enable the service, deploy a simple HTML web page, and verify its availability using an HTTP request.

Procedure
Step 1: Navigate to the Ansible project directory

cd ~/ansible-webapp
Step 2: Create the Ansible playbook

nano webapp.yml
Step 3: Add the following playbook content


---
- name: Deploy a Simple Web Application
  hosts: localhost
  connection: local
  become: true

  tasks:
    - name: Install Apache web server
      ansible.builtin.apt:
        name: apache2
        state: present
        update_cache: true

    - name: Start and enable Apache service
      ansible.builtin.service:
        name: apache2
        state: started
        enabled: true

    - name: Deploy welcome HTML page
      ansible.builtin.copy:
        dest: /var/www/html/index.html
        content: |
          <!DOCTYPE html>
          <html>
          <head>
              <title>Welcome Page</title>
          </head>
          <body>
              <h1>Welcome to My Web Application!</h1>
              <p>Apache server deployed successfully using Ansible.</p>
          </body>
          </html>
        mode: "0644"

    - name: Verify application using HTTP request
      ansible.builtin.uri:
        url: http://127.0.0.1/
        status_code: 200
      register: web_result

    - name: Display deployment status
      ansible.builtin.debug:
        msg: "Apache web application is running successfully!"

Save the file using Ctrl + O, press Enter, and exit using Ctrl + X.

Step 4: Execute the Ansible playbook

ansible-playbook webapp.yml --ask-become-pass
Enter your Linux password when prompted.

Step 5: Verify the Apache service

sudo systemctl status apache2
The service should show active (running).

Step 6: Verify the deployed web page

curl http://localhost
Expected output:

<!DOCTYPE html>
<html>
<head>
    <title>Welcome Page</title>
</head>
<body>
    <h1>Welcome to My Web Application!</h1>
    <p>Apache server deployed successfully using Ansible.</p>
</body>
</html>
You can also open the following URL in your browser:

http://localhost

Result
Thus, an Ansible playbook was successfully created and executed to install the Apache web server, start and enable the service, deploy a welcome HTML web page, and verify the application's availability using an HTTP request.

Important for your lab

You have already performed this experiment. The main change here is the HTML welcome message. You can reuse your existing webapp.yml and execute it again.
