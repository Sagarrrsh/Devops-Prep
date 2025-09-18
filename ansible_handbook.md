# Complete Ansible Handbook
## From Installation to Advanced Automation

*Based on Official Ansible Documentation*

---

## Table of Contents

1. [Introduction to Ansible](#1-introduction-to-ansible)
2. [Installation Guide](#2-installation-guide)
3. [Getting Started](#3-getting-started)
4. [Configuration](#4-configuration)
5. [Inventory Management](#5-inventory-management)
6. [Ad-hoc Commands](#6-ad-hoc-commands)
7. [Playbooks](#7-playbooks)
8. [Variables](#8-variables)
9. [Templates](#9-templates)
10. [Conditionals and Loops](#10-conditionals-and-loops)
11. [Handlers](#11-handlers)
12. [Roles](#12-roles)
13. [Collections](#13-collections)
14. [Modules](#14-modules)
15. [Ansible Vault](#15-ansible-vault)
16. [Error Handling](#16-error-handling)
17. [Testing and Debugging](#17-testing-and-debugging)
18. [Best Practices](#18-best-practices)
19. [Advanced Topics](#19-advanced-topics)
20. [Troubleshooting](#20-troubleshooting)

---

## 1. Introduction to Ansible

### What is Ansible?

Ansible is an open-source automation tool that simplifies configuration management, application deployment, and task automation. It uses a simple, human-readable language (YAML) to describe automation jobs.

### Key Features

- **Agentless**: No software installation required on managed nodes
- **Simple**: Uses YAML for configuration, easy to read and write
- **Powerful**: Handles complex deployments and configurations
- **Flexible**: Works with existing infrastructure
- **Secure**: Uses SSH for secure communication

### Core Concepts

- **Control Node**: Machine where Ansible is installed
- **Managed Nodes**: Target systems managed by Ansible
- **Inventory**: List of managed nodes
- **Modules**: Units of code that Ansible executes
- **Tasks**: Units of action in Ansible
- **Playbooks**: YAML files containing automation instructions
- **Roles**: Reusable units of organization for playbooks

### Architecture Overview

```
Control Node (Ansible installed)
    │
    ├── Inventory (hosts file)
    ├── Playbooks (YAML files)
    ├── Modules (Python code)
    └── Roles (organized tasks)
        │
        └── SSH Connection
            │
            └── Managed Nodes (target systems)
```

---

## 2. Installation Guide

### System Requirements

**Control Node Requirements:**
- Python 3.8 or newer
- Linux, macOS, or WSL on Windows
- 2GB RAM minimum (4GB recommended)

**Managed Node Requirements:**
- SSH access
- Python 2.7 or Python 3.5+ (Linux/Unix)
- PowerShell 3.0+ (Windows)

### Installation Methods

#### Method 1: Using pip (Recommended)

```bash
# Install Python pip
sudo apt update                    # Ubuntu/Debian
sudo yum install python3-pip      # CentOS/RHEL
brew install python3              # macOS

# Install Ansible
pip3 install ansible

# Verify installation
ansible --version
```

#### Method 2: Using Package Manager

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

**CentOS/RHEL:**
```bash
sudo yum install epel-release
sudo yum install ansible
```

**macOS:**
```bash
brew install ansible
```

#### Method 3: From Source

```bash
git clone https://github.com/ansible/ansible.git
cd ansible
source ./hacking/env-setup
sudo pip install -r requirements.txt
```

### Post-Installation Setup

**Create directory structure:**
```bash
mkdir -p ~/ansible/{playbooks,roles,inventory,group_vars,host_vars}
cd ~/ansible
```

**Verify installation:**
```bash
ansible --version
ansible localhost -m ping
```

---

## 3. Getting Started

### Basic Directory Structure

```
~/ansible/
├── ansible.cfg           # Configuration file
├── inventory/            # Inventory files
│   ├── hosts            # Main inventory
│   └── group_vars/      # Group variables
├── playbooks/           # Playbook files
├── roles/               # Custom roles
├── templates/           # Jinja2 templates
└── files/              # Files to copy
```

### Your First Ansible Command

```bash
# Test connection to localhost
ansible localhost -m ping

# Expected output:
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

### Setting up SSH Keys

```bash
# Generate SSH key pair
ssh-keygen -t rsa -b 4096

# Copy public key to managed nodes
ssh-copy-id user@target_host

# Test SSH connection
ssh user@target_host
```

---

## 4. Configuration

### Configuration File Locations

Ansible configuration is processed in the following order:
1. `ANSIBLE_CONFIG` environment variable
2. `ansible.cfg` in current directory
3. `~/.ansible.cfg` in home directory
4. `/etc/ansible/ansible.cfg` system-wide

### Creating ansible.cfg

```bash
# Generate default configuration
ansible-config init --disabled -t all > ansible.cfg
```

### Basic Configuration

```ini
[defaults]
# Inventory file location
inventory = ./inventory/hosts

# Default remote user
remote_user = ansible

# SSH key file
private_key_file = ~/.ssh/id_rsa

# Disable host key checking (for lab environments)
host_key_checking = False

# Enable SSH pipelining for performance
pipelining = True

# Log file location
log_path = ./ansible.log

# Number of parallel processes
forks = 10

[ssh_connection]
# SSH timeout
timeout = 30

# SSH arguments
ssh_args = -o ControlMaster=auto -o ControlPersist=60s

[privilege_escalation]
# Become (sudo) settings
become = True
become_method = sudo
become_user = root
become_ask_pass = False
```

### Environment Variables

```bash
# Common Ansible environment variables
export ANSIBLE_CONFIG=~/ansible/ansible.cfg
export ANSIBLE_INVENTORY=~/ansible/inventory
export ANSIBLE_REMOTE_USER=ansible
export ANSIBLE_HOST_KEY_CHECKING=False
export ANSIBLE_LOG_PATH=~/ansible/ansible.log
```

---

## 5. Inventory Management

### Static Inventory

#### INI Format

**Basic inventory (inventory/hosts):**
```ini
# Individual hosts
web1.example.com
web2.example.com

# Groups
[webservers]
web1.example.com
web2.example.com
web3.example.com

[databases]
db1.example.com
db2.example.com

# Host variables
[webservers]
web1.example.com http_port=80 max_requests=500
web2.example.com http_port=8080

# Group of groups
[production:children]
webservers
databases

# Group variables
[production:vars]
environment=prod
backup_schedule=daily
```

#### YAML Format

```yaml
# inventory/hosts.yml
all:
  children:
    webservers:
      hosts:
        web1.example.com:
          http_port: 80
          max_requests: 500
        web2.example.com:
          http_port: 8080
    databases:
      hosts:
        db1.example.com:
        db2.example.com:
      vars:
        db_engine: mysql
    production:
      children:
        webservers:
        databases:
      vars:
        environment: prod
        backup_schedule: daily
```

### Host and Group Variables

**Group variables (group_vars/webservers.yml):**
```yaml
---
# Web server configuration
nginx_version: 1.18
document_root: /var/www/html
ssl_enabled: true
ssl_cert_path: /etc/ssl/certs/server.crt
ssl_key_path: /etc/ssl/private/server.key

# Service configuration
service_ports:
  - 80
  - 443

# Security settings
firewall_rules:
  - port: 80
    protocol: tcp
  - port: 443
    protocol: tcp
```

**Host variables (host_vars/web1.example.com.yml):**
```yaml
---
# Host-specific configuration
server_id: web01
memory_allocation: 4GB
cpu_cores: 4

# Custom application settings
app_debug: false
app_environment: production
```

### Dynamic Inventory

#### AWS EC2 Dynamic Inventory

```bash
# Install boto3
pip install boto3

# Create aws_ec2.yml
```

```yaml
# aws_ec2.yml
plugin: aws_ec2
regions:
  - us-east-1
  - us-west-2
keyed_groups:
  - key: tags
    prefix: tag
  - key: instance_type
    prefix: type
hostnames:
  - dns-name
compose:
  ansible_host: public_dns_name
```

#### Custom Dynamic Inventory Script

```python
#!/usr/bin/env python3
import json
import subprocess

def get_inventory():
    inventory = {
        'webservers': {
            'hosts': ['web1.example.com', 'web2.example.com'],
            'vars': {
                'http_port': 80,
                'https_port': 443
            }
        },
        '_meta': {
            'hostvars': {
                'web1.example.com': {
                    'server_id': 1,
                    'region': 'us-east-1'
                },
                'web2.example.com': {
                    'server_id': 2,
                    'region': 'us-west-1'
                }
            }
        }
    }
    return inventory

if __name__ == '__main__':
    print(json.dumps(get_inventory(), indent=2))
```

### Inventory Commands

```bash
# List all hosts
ansible-inventory --list

# List specific group
ansible-inventory --list -l webservers

# Show host variables
ansible-inventory --host web1.example.com

# Graph inventory structure
ansible-inventory --graph

# Test inventory
ansible all -m ping
ansible webservers -m ping
```

---

## 6. Ad-hoc Commands

### Basic Syntax

```bash
ansible <pattern> -m <module> -a "<module_arguments>"
```

### Common Ad-hoc Commands

#### System Information

```bash
# Ping all hosts
ansible all -m ping

# Get system facts
ansible all -m setup

# Check disk space
ansible all -m shell -a "df -h"

# Check memory usage
ansible all -m shell -a "free -h"

# Get uptime
ansible all -m command -a "uptime"
```

#### Package Management

```bash
# Install package (Ubuntu/Debian)
ansible webservers -m apt -a "name=nginx state=present" --become

# Install package (CentOS/RHEL)
ansible webservers -m yum -a "name=httpd state=present" --become

# Update all packages
ansible all -m apt -a "upgrade=yes update_cache=yes" --become

# Remove package
ansible webservers -m apt -a "name=apache2 state=absent" --become
```

#### Service Management

```bash
# Start service
ansible webservers -m service -a "name=nginx state=started" --become

# Stop service
ansible webservers -m service -a "name=nginx state=stopped" --become

# Restart service
ansible webservers -m service -a "name=nginx state=restarted" --become

# Enable service at boot
ansible webservers -m service -a "name=nginx enabled=yes" --become
```

#### File Operations

```bash
# Copy file
ansible all -m copy -a "src=/local/file dest=/remote/path owner=root group=root mode=0644" --become

# Create directory
ansible all -m file -a "path=/tmp/test state=directory mode=0755" --become

# Create symlink
ansible all -m file -a "src=/etc/motd dest=/tmp/motd state=link" --become

# Change file permissions
ansible all -m file -a "path=/etc/passwd mode=0644" --become
```

#### User Management

```bash
# Create user
ansible all -m user -a "name=testuser shell=/bin/bash home=/home/testuser" --become

# Add user to group
ansible all -m user -a "name=testuser groups=sudo append=yes" --become

# Remove user
ansible all -m user -a "name=testuser state=absent remove=yes" --become
```

---

## 7. Playbooks

### Playbook Structure

A playbook consists of one or more plays. Each play maps a group of hosts to well-defined tasks.

```yaml
---
- name: Web server setup
  hosts: webservers
  become: yes
  vars:
    http_port: 80
    
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
        
    - name: Start nginx
      service:
        name: nginx
        state: started
        enabled: yes
```

### Your First Playbook

Create `playbooks/webserver.yml`:

```yaml
---
- name: Configure web servers
  hosts: webservers
  become: yes
  
  tasks:
    - name: Update package cache
      apt:
        update_cache: yes
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"
    
    - name: Install nginx
      package:
        name: nginx
        state: present
    
    - name: Copy nginx configuration
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
        backup: yes
      notify: restart nginx
    
    - name: Start and enable nginx
      service:
        name: nginx
        state: started
        enabled: yes
        
  handlers:
    - name: restart nginx
      service:
        name: nginx
        state: restarted
```

### Running Playbooks

```bash
# Run playbook
ansible-playbook playbooks/webserver.yml

# Check mode (dry run)
ansible-playbook playbooks/webserver.yml --check

# Diff mode (show changes)
ansible-playbook playbooks/webserver.yml --diff

# Verbose output
ansible-playbook playbooks/webserver.yml -v
ansible-playbook playbooks/webserver.yml -vvv

# Limit to specific hosts
ansible-playbook playbooks/webserver.yml --limit web1.example.com

# Use specific inventory
ansible-playbook playbooks/webserver.yml -i inventory/production

# Start at specific task
ansible-playbook playbooks/webserver.yml --start-at-task="Install nginx"
```

### Multi-play Playbooks

```yaml
---
# Play 1: Setup web servers
- name: Configure web servers
  hosts: webservers
  become: yes
  
  tasks:
    - name: Install nginx
      package:
        name: nginx
        state: present

# Play 2: Setup database servers  
- name: Configure database servers
  hosts: databases
  become: yes
  
  tasks:
    - name: Install mysql
      package:
        name: mysql-server
        state: present

# Play 3: Configure load balancer
- name: Configure load balancer
  hosts: loadbalancers
  become: yes
  
  tasks:
    - name: Install haproxy
      package:
        name: haproxy
        state: present
```

---

## 8. Variables

### Variable Types

Variables in Ansible can be defined in multiple locations with different precedence levels.

#### Playbook Variables

```yaml
---
- name: Variable examples
  hosts: webservers
  vars:
    http_port: 80
    server_name: www.example.com
    ssl_enabled: true
    
  tasks:
    - name: Display variables
      debug:
        msg: "Server {{ server_name }} on port {{ http_port }}"
```

#### Variable Files

**vars/main.yml:**
```yaml
---
# Application variables
app_name: myapp
app_version: 1.2.3
app_port: 8080

# Database variables
database_host: db.example.com
database_port: 3306
database_name: myapp_db

# SSL configuration
ssl_certificate_path: /etc/ssl/certs/server.crt
ssl_private_key_path: /etc/ssl/private/server.key
```

**Using variable files:**
```yaml
---
- name: Use external variables
  hosts: webservers
  vars_files:
    - vars/main.yml
    
  tasks:
    - name: Show app info
      debug:
        msg: "{{ app_name }} version {{ app_version }} on port {{ app_port }}"
```

#### Variable Precedence

From lowest to highest precedence:
1. group_vars/all
2. group_vars/group_name
3. host_vars/hostname
4. inventory variables
5. playbook group_vars
6. playbook host_vars
7. play vars
8. task vars
9. extra vars (-e command line)

### Complex Variables

#### Lists

```yaml
---
packages:
  - nginx
  - mysql-server
  - php-fpm

users:
  - name: alice
    uid: 1001
    group: developers
  - name: bob
    uid: 1002
    group: admins
```

#### Dictionaries

```yaml
---
database_config:
  host: db.example.com
  port: 3306
  name: myapp
  user: appuser
  
ssl_config:
  enabled: true
  cert_file: /etc/ssl/certs/server.crt
  key_file: /etc/ssl/private/server.key
  protocols:
    - TLSv1.2
    - TLSv1.3
```

### Using Variables

#### Variable Substitution

```yaml
tasks:
  - name: Install packages
    package:
      name: "{{ item }}"
      state: present
    loop: "{{ packages }}"
    
  - name: Create user
    user:
      name: "{{ users[0].name }}"
      uid: "{{ users[0].uid }}"
      group: "{{ users[0].group }}"
```

#### Variable Filters

```yaml
tasks:
  - name: Show uppercase app name
    debug:
      msg: "{{ app_name | upper }}"
      
  - name: Show default value
    debug:
      msg: "{{ undefined_var | default('default_value') }}"
      
  - name: Generate random password
    debug:
      msg: "{{ 'password' | password_hash('sha512', 'salt') }}"
```

### Registering Variables

```yaml
tasks:
  - name: Check if file exists
    stat:
      path: /etc/nginx/nginx.conf
    register: nginx_config
    
  - name: Show file info
    debug:
      var: nginx_config
      
  - name: Create backup if file exists
    copy:
      src: /etc/nginx/nginx.conf
      dest: /etc/nginx/nginx.conf.backup
    when: nginx_config.stat.exists
```

### Facts as Variables

```yaml
tasks:
  - name: Show system facts
    debug:
      msg: |
        Hostname: {{ ansible_hostname }}
        OS: {{ ansible_os_family }}
        Distribution: {{ ansible_distribution }}
        Memory: {{ ansible_memtotal_mb }}MB
        CPU: {{ ansible_processor_vcpus }} cores
```

---

## 9. Templates

Templates use Jinja2 templating engine to create dynamic configuration files.

### Basic Template Usage

**Template file (templates/nginx.conf.j2):**
```nginx
# Nginx configuration generated by Ansible
user www-data;
worker_processes {{ ansible_processor_vcpus }};
pid /run/nginx.pid;

events {
    worker_connections {{ worker_connections | default('1024') }};
}

http {
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    
    server {
        listen {{ http_port | default('80') }};
        server_name {{ server_name | default('_') }};
        root {{ document_root | default('/var/www/html') }};
        
        {% if ssl_enabled %}
        listen 443 ssl;
        ssl_certificate {{ ssl_cert_path }};
        ssl_certificate_key {{ ssl_key_path }};
        {% endif %}
        
        location / {
            try_files $uri $uri/ =404;
        }
    }
}
```

**Using template in playbook:**
```yaml
tasks:
  - name: Generate nginx configuration
    template:
      src: nginx.conf.j2
      dest: /etc/nginx/nginx.conf
      owner: root
      group: root
      mode: '0644'
      backup: yes
    notify: restart nginx
```

### Advanced Template Features

#### Loops in Templates

**Template (templates/hosts.j2):**
```jinja2
# /etc/hosts generated by Ansible
127.0.0.1 localhost

{% for host in groups['webservers'] %}
{{ hostvars[host]['ansible_default_ipv4']['address'] }} {{ host }}
{% endfor %}

{% for db in groups['databases'] %}
{{ hostvars[db]['ansible_default_ipv4']['address'] }} {{ db }} db-{{ loop.index }}
{% endfor %}
```

#### Conditionals in Templates

**Template (templates/mysql.cnf.j2):**
```jinja2
[mysqld]
bind-address = {{ mysql_bind_address | default('127.0.0.1') }}
port = {{ mysql_port | default('3306') }}

{% if mysql_innodb_buffer_pool_size is defined %}
innodb_buffer_pool_size = {{ mysql_innodb_buffer_pool_size }}
{% endif %}

{% if environment == 'production' %}
slow_query_log = 1
slow_query_log_file = /var/log/mysql/slow.log
long_query_time = 2
{% endif %}

{% if mysql_replication %}
server-id = {{ mysql_server_id }}
log-bin = mysql-bin
{% endif %}
```

#### Filters and Tests

```jinja2
# String manipulation
server_name = {{ app_name | upper | replace('_', '-') }}

# Default values
max_connections = {{ mysql_max_connections | default(100) }}

# Math operations
memory_limit = {{ (ansible_memtotal_mb * 0.8) | int }}MB

# Lists and dictionaries
{% for port in service_ports | sort %}
listen {{ port }};
{% endfor %}

# Tests
{% if ssl_enabled is defined and ssl_enabled %}
SSL is enabled
{% endif %}
```

### Template Variables and Facts

```yaml
tasks:
  - name: Generate system info page
    template:
      src: system-info.html.j2
      dest: /var/www/html/system-info.html
    vars:
      page_title: "System Information"
      generated_date: "{{ ansible_date_time.iso8601 }}"
```

**Template (templates/system-info.html.j2):**
```html
<!DOCTYPE html>
<html>
<head>
    <title>{{ page_title }}</title>
</head>
<body>
    <h1>{{ page_title }}</h1>
    <p>Generated on: {{ generated_date }}</p>
    
    <h2>System Information</h2>
    <ul>
        <li>Hostname: {{ ansible_hostname }}</li>
        <li>OS: {{ ansible_distribution }} {{ ansible_distribution_version }}</li>
        <li>Kernel: {{ ansible_kernel }}</li>
        <li>Architecture: {{ ansible_architecture }}</li>
        <li>Memory: {{ ansible_memtotal_mb }}MB</li>
        <li>CPUs: {{ ansible_processor_vcpus }}</li>
    </ul>
    
    <h2>Network Interfaces</h2>
    <ul>
    {% for interface in ansible_interfaces %}
        {% if interface != 'lo' %}
        <li>{{ interface }}: {{ ansible_facts[interface]['ipv4']['address'] | default('No IP') }}</li>
        {% endif %}
    {% endfor %}
    </ul>
</body>
</html>
```

---

## 10. Conditionals and Loops

### Conditionals

#### Basic When Statement

```yaml
tasks:
  - name: Install apache (Ubuntu/Debian)
    apt:
      name: apache2
      state: present
    when: ansible_os_family == "Debian"
    
  - name: Install apache (CentOS/RHEL)
    yum:
      name: httpd
      state: present
    when: ansible_os_family == "RedHat"
```

#### Complex Conditions

```yaml
tasks:
  - name: Install development tools
    package:
      name: "{{ dev_packages }}"
      state: present
    when: 
      - environment == "development"
      - install_dev_tools | bool
      - ansible_memtotal_mb > 4096
      
  - name: Configure SSL
    template:
      src: ssl.conf.j2
      dest: /etc/nginx/ssl.conf
    when: ssl_enabled | default(false) and ssl_cert_path is defined
```

#### Using Variables in Conditions

```yaml
tasks:
  - name: Check disk space
    shell: df / | awk 'NR==2 {print $5}' | sed 's/%//'
    register: disk_usage
    
  - name: Warning if disk is full
    debug:
      msg: "Warning: Disk usage is {{ disk_usage.stdout }}%"
    when: disk_usage.stdout | int > 80
    
  - name: Fail if critical disk usage
    fail:
      msg: "Critical: Disk usage is {{ disk_usage.stdout }}%"
    when: disk_usage.stdout | int > 95
```

### Loops

#### Simple Loops

```yaml
tasks:
  - name: Install packages
    package:
      name: "{{ item }}"
      state: present
    loop:
      - nginx
      - mysql-server
      - php-fpm
      - git
      
  - name: Create users
    user:
      name: "{{ item }}"
      shell: /bin/bash
    loop:
      - alice
      - bob
      - charlie
```

#### Loop with Variables

```yaml
vars:
  packages:
    - nginx
    - mysql-server
    - php-fpm
  users:
    - name: alice
      uid: 1001
      groups: ['sudo', 'developers']
    - name: bob
      uid: 1002
      groups: ['developers']

tasks:
  - name: Install packages
    package:
      name: "{{ item }}"
      state: present
    loop: "{{ packages }}"
    
  - name: Create users
    user:
      name: "{{ item.name }}"
      uid: "{{ item.uid }}"
      groups: "{{ item.groups }}"
    loop: "{{ users }}"
```

#### Loop Controls

```yaml
tasks:
  - name: Process items with loop controls
    debug:
      msg: "Item {{ ansible_loop.index }}/{{ ansible_loop.length }}: {{ item }}"
    loop:
      - web1
      - web2
      - web3
    loop_control:
      label: "{{ item }}"
      
  - name: Pause between tasks
    service:
      name: "{{ item }}"
      state: restarted
    loop:
      - nginx
      - mysql
    loop_control:
      pause: 5
```

#### Dictionary Loops

```yaml
vars:
  firewall_rules:
    http:
      port: 80
      protocol: tcp
    https:
      port: 443
      protocol: tcp
    ssh:
      port: 22
      protocol: tcp

tasks:
  - name: Configure firewall rules
    iptables:
      chain: INPUT
      destination_port: "{{ item.value.port }}"
      protocol: "{{ item.value.protocol }}"
      jump: ACCEPT
    loop: "{{ firewall_rules | dict2items }}"
    loop_control:
      label: "{{ item.key }}"
```

---

## 11. Handlers

Handlers are tasks that are triggered by notifications from other tasks. They run at the end of plays and only if triggered.

### Basic Handler Usage

```yaml
---
- name: Web server configuration
  hosts: webservers
  become: yes
  
  tasks:
    - name: Install nginx
      package:
        name: nginx
        state: present
        
    - name: Copy nginx configuration
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
      notify: restart nginx
      
    - name: Copy SSL certificate
      copy:
        src: server.crt
        dest: /etc/ssl/certs/server.crt
      notify:
        - restart nginx
        - reload nginx
        
  handlers:
    - name: restart nginx
      service:
        name: nginx
        state: restarted
        
    - name: reload nginx
      service:
        name: nginx
        state: reloaded
```

### Handler Best Practices

```yaml
handlers:
  - name: restart web services
    service:
      name: "{{ item }}"
      state: restarted
    loop:
      - nginx
      - php-fpm
    listen: "restart web stack"
    
  - name: reload nginx configuration
    service:
      name: nginx
      state: reloaded
    listen: "restart web stack"

tasks:
  - name: Update nginx config
    template:
      src: nginx.conf.j2
      dest: /etc/nginx/nginx.conf
    notify: "restart web stack"
```

### Conditional Handlers

```yaml
handlers:
  - name: restart apache
    service:
      name: apache2
      state: restarted
    when: ansible_os_family == "Debian"
    
  - name: restart httpd
    service:
      name: httpd
      state: restarted
    when: ansible_os_family == "RedHat"
```

---

## 12. Roles

Roles provide a framework for fully independent, or interdependent collections of variables, tasks, files, templates, and modules.

### Role Structure

```
roles/
  webserver/
    tasks/
      main.yml          # Main tasks file
    handlers/
      main.yml          # Handlers file
    templates/          # Jinja2 templates
      nginx.conf.j2
    files/              # Static files
      index.html
    vars/
      main.yml          # Role variables
    defaults/
      main.yml          # Default variables
    meta/
      main.yml          # Role metadata
    tests/
      inventory
      test.yml
```

### Creating a Role

```bash
# Create role directory structure
ansible-galaxy init roles/webserver

# Manual creation
mkdir -p roles/webserver/{tasks,handlers,templates,files,vars,defaults,meta}
```

### Role Files

#### tasks/main.yml
```yaml
---
# Main tasks for webserver role
- name: Install nginx
  package:
    name: nginx
    state