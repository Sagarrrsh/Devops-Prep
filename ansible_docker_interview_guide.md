# Ansible & Docker Interview Questions - Complete Guide
## Most Important Q&A for DevOps Interviews

---

## Table of Contents
1. [Ansible Questions](#ansible-questions)
2. [Docker Questions](#docker-questions)
3. [Ansible + Docker Integration](#ansible--docker-integration)
4. [Real-World Scenarios](#real-world-scenarios)
5. [Best Practices](#best-practices)

---

## Ansible Questions

### 🟢 Fundamental Ansible Questions

**Q1: What is Ansible and why is it popular in DevOps?**
- **Answer:** Ansible is an open-source automation tool for configuration management, application deployment, and orchestration. It's popular because:
  - **Agentless:** No software installation required on target machines
  - **Simple YAML syntax:** Human-readable and easy to learn
  - **Push-based:** Control node pushes configurations
  - **Idempotent:** Same result regardless of execution times
  - **Large community and modules**

**Q2: How does Ansible work? Explain its architecture.**
- **Answer:** 
  - **Control Node:** Machine where Ansible is installed and playbooks are executed
  - **Managed Nodes:** Target servers managed by Ansible
  - **SSH Connection:** Uses SSH for Linux/Unix, WinRM for Windows
  - **Python:** Executes modules written in Python on target nodes
  - **No agents required** on managed nodes

**Q3: What is the difference between Ansible and other CM tools (Puppet, Chef)?**
- **Answer:**
  | Feature | Ansible | Puppet | Chef |
  |---------|---------|---------|------|
  | Architecture | Agentless | Agent-based | Agent-based |
  | Language | YAML | Ruby DSL | Ruby |
  | Learning Curve | Easy | Moderate | Steep |
  | Configuration | Push | Pull | Pull |
  | Setup | Simple | Complex | Complex |

### 🟡 Core Ansible Concepts

**Q4: What is an Ansible Playbook? Provide example.**
- **Answer:** A playbook is a YAML file containing automation instructions organized in plays.
```yaml
---
- name: Install and start nginx
  hosts: webservers
  become: yes
  tasks:
    - name: Install nginx
      yum:
        name: nginx
        state: present
    
    - name: Start nginx service
      service:
        name: nginx
        state: started
        enabled: yes
```

**Q5: What is Ansible Inventory? Show different types.**
- **Answer:** Inventory defines hosts and groups that Ansible manages.

**Static Inventory (INI format):**
```ini
[webservers]
web1.example.com
web2.example.com

[databases]
db1.example.com ansible_user=admin
db2.example.com ansible_port=2222

[production:children]
webservers
databases
```

**YAML Inventory:**
```yaml
all:
  children:
    webservers:
      hosts:
        web1.example.com:
        web2.example.com:
    databases:
      hosts:
        db1.example.com:
          ansible_user: admin
```

**Q6: What are Ansible Modules? List important ones.**
- **Answer:** Modules are discrete units of code that perform specific tasks.

**Important Modules:**
- **file:** File/directory management
- **copy:** Copy files to remote hosts
- **template:** Template files with Jinja2
- **yum/apt:** Package management
- **service:** Service management
- **shell/command:** Execute commands
- **git:** Git repository management
- **user:** User account management
- **cron:** Cron job management

**Q7: What are Ansible Variables? Show different ways to define them.**
- **Answer:** Variables store values that can be used in playbooks.

**1. Playbook Variables:**
```yaml
---
- hosts: all
  vars:
    http_port: 80
    max_clients: 200
```

**2. Group Variables (group_vars/all.yml):**
```yaml
ntp_server: pool.ntp.org
database_host: db.example.com
```

**3. Host Variables (host_vars/web1.yml):**
```yaml
nginx_port: 8080
ssl_enabled: true
```

**4. Command Line:**
```bash
ansible-playbook site.yml --extra-vars "version=1.23.45"
```

**Q8: What are Ansible Roles? Why use them?**
- **Answer:** Roles are reusable units of organization for playbooks.

**Benefits:**
- Code reusability
- Better organization
- Sharing across teams
- Simplified maintenance

**Role Structure:**
```
roles/
  webserver/
    tasks/main.yml          # Main task file
    handlers/main.yml       # Handlers
    templates/              # Jinja2 templates
    files/                  # Files to copy
    vars/main.yml          # Role variables
    defaults/main.yml      # Default variables
    meta/main.yml          # Role metadata
```

**Using Roles:**
```yaml
---
- hosts: webservers
  roles:
    - webserver
    - database
```

### 🔴 Advanced Ansible Concepts

**Q9: What is Ansible Vault? How do you use it?**
- **Answer:** Ansible Vault encrypts sensitive data like passwords, API keys.

**Commands:**
```bash
# Create encrypted file
ansible-vault create secrets.yml

# Encrypt existing file
ansible-vault encrypt vars.yml

# Edit encrypted file
ansible-vault edit secrets.yml

# View encrypted file
ansible-vault view secrets.yml

# Run playbook with vault
ansible-playbook site.yml --ask-vault-pass
ansible-playbook site.yml --vault-password-file ~/.vault_pass
```

**Q10: What are Ansible Facts? How to use custom facts?**
- **Answer:** Facts are system information automatically gathered about managed nodes.

**Viewing Facts:**
```bash
ansible hostname -m setup
```

**Using Facts in Playbooks:**
```yaml
- debug:
    msg: "OS Family: {{ ansible_os_family }}"
    
- debug:
    msg: "Memory: {{ ansible_memtotal_mb }}MB"
```

**Custom Facts (/etc/ansible/facts.d/custom.fact):**
```ini
[general]
app_version=1.2.3
environment=production
```

**Q11: How do you handle errors in Ansible?**
- **Answer:** Multiple error handling mechanisms:

**1. Ignore Errors:**
```yaml
- name: This might fail
  command: /bin/false
  ignore_errors: yes
```

**2. Block/Rescue/Always:**
```yaml
- block:
    - name: Attempt task
      command: risky_command
  rescue:
    - name: Handle failure
      debug:
        msg: "Task failed, handling it"
  always:
    - name: Always run
      debug:
        msg: "This always runs"
```

**3. Failed When:**
```yaml
- name: Check service
  command: systemctl status nginx
  register: result
  failed_when: result.rc != 0 and "inactive" not in result.stdout
```

**Q12: What are Ansible Handlers?**
- **Answer:** Handlers are tasks that run when notified by other tasks.

```yaml
tasks:
  - name: Copy nginx config
    template:
      src: nginx.conf.j2
      dest: /etc/nginx/nginx.conf
    notify: restart nginx

handlers:
  - name: restart nginx
    service:
      name: nginx
      state: restarted
```

**Q13: How do you optimize Ansible performance?**
- **Answer:** Several optimization techniques:

**1. Parallel Execution:**
```yaml
- hosts: all
  strategy: free  # Don't wait for all hosts
  serial: 20%     # Process 20% at a time
```

**2. SSH Optimization:**
```ini
# ansible.cfg
[ssh_connection]
ssh_args = -o ControlMaster=auto -o ControlPersist=60s
pipelining = True
```

**3. Fact Caching:**
```ini
[defaults]
gathering = smart
fact_caching = jsonfile
fact_caching_connection = /tmp/ansible_facts_cache
```

**4. Async Tasks:**
```yaml
- name: Long running task
  command: /long/running/command
  async: 300
  poll: 10
```

---

## Docker Questions

### 🟢 Docker Fundamentals

**Q14: What is Docker? Why use it?**
- **Answer:** Docker is a containerization platform that packages applications and dependencies into lightweight, portable containers.

**Benefits:**
- **Consistency:** Same environment across dev/test/prod
- **Isolation:** Applications run in isolated environments
- **Portability:** Run anywhere Docker is installed
- **Efficiency:** Lighter than virtual machines
- **Scalability:** Easy horizontal scaling
- **Version Control:** Image versioning and rollbacks

**Q15: What's the difference between Docker Container and Virtual Machine?**
- **Answer:**
| Feature | Container | Virtual Machine |
|---------|-----------|----------------|
| OS | Shares host OS kernel | Full guest OS |
| Resource Usage | Lightweight | Heavy |
| Boot Time | Seconds | Minutes |
| Isolation | Process-level | Hardware-level |
| Performance | Near-native | Overhead |

**Q16: Explain Docker Architecture.**
- **Answer:**
  - **Docker Client:** Command-line interface
  - **Docker Daemon:** Background service managing containers
  - **Docker Images:** Read-only templates
  - **Docker Containers:** Running instances of images
  - **Docker Registry:** Repository for images (Docker Hub)
  - **Dockerfile:** Instructions to build images

**Q17: What is a Dockerfile? Provide example.**
- **Answer:** Dockerfile contains instructions to build Docker images.

```dockerfile
# Use official Python image
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Copy requirements first (for layer caching)
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Expose port
EXPOSE 5000

# Set environment variable
ENV FLASK_APP=app.py

# Define default command
CMD ["flask", "run", "--host=0.0.0.0"]
```

### 🟡 Docker Operations & Management

**Q18: What are Docker Images and Containers?**
- **Answer:**
  - **Image:** Read-only template with application code, runtime, libraries, and dependencies
  - **Container:** Running instance of an image with writable layer

**Key Commands:**
```bash
# Images
docker images                    # List images
docker pull nginx               # Download image
docker build -t myapp:v1 .      # Build image
docker rmi image_id             # Remove image

# Containers
docker ps                       # List running containers
docker ps -a                    # List all containers
docker run -d -p 80:80 nginx    # Run container
docker stop container_id        # Stop container
docker rm container_id          # Remove container
```

**Q19: What are Docker Volumes? Types and uses.**
- **Answer:** Volumes provide persistent storage for containers.

**Types:**
1. **Named Volumes:** Managed by Docker
2. **Bind Mounts:** Mount host directories
3. **tmpfs Mounts:** Store in host memory

```bash
# Named volume
docker run -v mydata:/app/data nginx

# Bind mount
docker run -v /host/path:/container/path nginx

# tmpfs mount
docker run --tmpfs /tmp nginx
```

**Q20: What is Docker Compose? Provide example.**
- **Answer:** Docker Compose defines and runs multi-container applications.

**docker-compose.yml:**
```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/myapp
    depends_on:
      - db
      - redis
    volumes:
      - .:/code

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:alpine

volumes:
  postgres_data:
```

**Commands:**
```bash
docker-compose up -d        # Start services
docker-compose down         # Stop services
docker-compose logs web     # View logs
docker-compose exec web bash   # Execute commands
```

**Q21: What are Docker Networks?**
- **Answer:** Networks enable communication between containers.

**Network Types:**
- **bridge:** Default network for containers
- **host:** Use host's network directly
- **none:** No networking
- **overlay:** Multi-host networking (Swarm)

```bash
# Create custom network
docker network create mynetwork

# Run containers on same network
docker run --network mynetwork --name web nginx
docker run --network mynetwork --name app myapp

# List networks
docker network ls

# Inspect network
docker network inspect mynetwork
```

### 🔴 Advanced Docker Concepts

**Q22: What is Multi-stage Docker build?**
- **Answer:** Multi-stage builds optimize image size and security by using multiple FROM statements.

```dockerfile
# Build stage
FROM node:16-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**Benefits:**
- Smaller final images
- Exclude build dependencies
- Better security

**Q23: What is Docker Swarm?**
- **Answer:** Docker Swarm is native orchestration solution for Docker containers.

**Key Concepts:**
- **Manager Nodes:** Control the swarm
- **Worker Nodes:** Run containers
- **Services:** Define desired state
- **Tasks:** Individual containers

**Commands:**
```bash
# Initialize swarm
docker swarm init

# Join worker
docker swarm join --token TOKEN MANAGER_IP:2377

# Create service
docker service create --name web --replicas 3 -p 80:80 nginx

# Scale service
docker service scale web=5

# List services
docker service ls
```

**Q24: How do you secure Docker containers?**
- **Answer:** Multiple security practices:

**1. Use Official Base Images:**
```dockerfile
FROM node:16-alpine  # Official, minimal image
```

**2. Run as Non-root User:**
```dockerfile
RUN adduser -D -s /bin/sh appuser
USER appuser
```

**3. Scan Images:**
```bash
docker scan myapp:latest
```

**4. Use Secrets Management:**
```yaml
# docker-compose.yml
services:
  app:
    image: myapp
    secrets:
      - db_password

secrets:
  db_password:
    file: ./db_password.txt
```

**5. Limit Resources:**
```bash
docker run --memory=512m --cpus=1 myapp
```

**Q25: What are Docker Health Checks?**
- **Answer:** Health checks monitor container health.

**In Dockerfile:**
```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=30s --retries=3 \
  CMD curl -f http://localhost:8080/health || exit 1
```

**In docker-compose.yml:**
```yaml
services:
  web:
    image: nginx
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 30s
```

**Q26: What is Docker Registry? How to set up private registry?**
- **Answer:** Registry stores and distributes Docker images.

**Public Registries:** Docker Hub, AWS ECR, Google Container Registry

**Private Registry Setup:**
```bash
# Run registry container
docker run -d -p 5000:5000 --name registry registry:2

# Tag and push image
docker tag myapp localhost:5000/myapp
docker push localhost:5000/myapp

# Pull from private registry
docker pull localhost:5000/myapp
```

---

## Ansible + Docker Integration

**Q27: How do you use Ansible with Docker?**
- **Answer:** Ansible provides Docker modules for container management.

**Docker Modules:**
- **docker_image:** Manage images
- **docker_container:** Manage containers
- **docker_compose:** Use docker-compose files
- **docker_network:** Manage networks
- **docker_volume:** Manage volumes

**Example Playbook:**
```yaml
---
- name: Deploy application with Docker
  hosts: docker_hosts
  tasks:
    - name: Pull Docker image
      docker_image:
        name: nginx:latest
        source: pull

    - name: Create Docker network
      docker_network:
        name: app_network

    - name: Run nginx container
      docker_container:
        name: web_server
        image: nginx:latest
        state: started
        restart_policy: always
        ports:
          - "80:80"
        networks:
          - name: app_network
        volumes:
          - /host/data:/var/www/html

    - name: Deploy with docker-compose
      docker_compose:
        project_src: /path/to/compose/project
        state: present
```

---

## Real-World Scenarios

**Q28: How would you deploy a web application using Ansible and Docker?**
- **Answer:** Complete deployment strategy:

**1. Ansible Playbook Structure:**
```
deploy/
├── site.yml
├── group_vars/
│   └── all.yml
├── roles/
│   ├── docker/
│   ├── nginx/
│   └── app/
└── docker-compose.yml
```

**2. Main Playbook (site.yml):**
```yaml
---
- name: Deploy Web Application
  hosts: web_servers
  become: yes
  roles:
    - docker
    - app
```

**3. Docker Role (roles/docker/tasks/main.yml):**
```yaml
---
- name: Install Docker
  yum:
    name: docker
    state: present

- name: Start Docker service
  service:
    name: docker
    state: started
    enabled: yes

- name: Install docker-compose
  pip:
    name: docker-compose
```

**4. App Deployment (roles/app/tasks/main.yml):**
```yaml
---
- name: Copy docker-compose file
  template:
    src: docker-compose.yml.j2
    dest: /opt/app/docker-compose.yml

- name: Deploy application
  docker_compose:
    project_src: /opt/app
    state: present
    pull: yes
```

**Q29: How do you handle secrets in Ansible + Docker?**
- **Answer:** Multiple approaches for secure secret management:

**1. Ansible Vault + Docker Secrets:**
```yaml
# Encrypted with ansible-vault
---
database_password: !vault |
  $ANSIBLE_VAULT;1.1;AES256
  66633...

# Playbook
- name: Create Docker secret
  docker_secret:
    name: db_password
    data: "{{ database_password }}"
    state: present
```

**2. External Secret Management:**
```yaml
- name: Get secret from HashiCorp Vault
  uri:
    url: "{{ vault_url }}/v1/secret/data/myapp"
    headers:
      X-Vault-Token: "{{ vault_token }}"
  register: secret_data

- name: Use secret in container
  docker_container:
    name: app
    image: myapp:latest
    env:
      DB_PASSWORD: "{{ secret_data.json.data.data.password }}"
```

---

## Best Practices

### Ansible Best Practices

**1. Directory Structure:**
```
project/
├── ansible.cfg
├── site.yml
├── group_vars/
├── host_vars/
├── roles/
└── inventory/
```

**2. Naming Conventions:**
- Use descriptive names for plays and tasks
- Use lowercase with underscores for variables
- Tag tasks for selective execution

**3. Idempotency:**
- Always write idempotent tasks
- Use appropriate modules (avoid shell/command when possible)
- Test playbooks multiple times

**4. Security:**
- Use Ansible Vault for sensitive data
- Implement least privilege access
- Regular security updates

### Docker Best Practices

**1. Dockerfile Optimization:**
```dockerfile
# Good practices
FROM python:3.9-alpine              # Use specific, minimal base
COPY requirements.txt .             # Copy dependencies first
RUN pip install --no-cache-dir -r requirements.txt
COPY . .                           # Copy code after dependencies
USER appuser                       # Don't run as root
EXPOSE 8000                        # Document exposed ports
HEALTHCHECK CMD curl -f http://localhost:8000/health
```

**2. Image Management:**
- Use specific image tags (not 'latest')
- Implement multi-stage builds
- Regular security scans
- Keep images small

**3. Container Security:**
- Run containers as non-root
- Use read-only filesystems when possible
- Limit container resources
- Network segmentation

**4. Production Deployment:**
- Use orchestration (Docker Swarm, Kubernetes)
- Implement proper logging
- Monitor container health
- Backup strategies

---

## Quick Reference Commands

### Ansible Commands
```bash
# Run ad-hoc commands
ansible all -m ping
ansible webservers -a "df -h"

# Run playbooks
ansible-playbook site.yml
ansible-playbook site.yml --check --diff
ansible-playbook site.yml --tags "nginx"

# Vault operations
ansible-vault create secrets.yml
ansible-vault encrypt vars.yml
ansible-vault decrypt secrets.yml
```

### Docker Commands
```bash
# Image operations
docker build -t myapp:v1 .
docker images
docker rmi image_id
docker pull nginx

# Container operations
docker run -d -p 80:80 --name web nginx
docker ps -a
docker logs container_id
docker exec -it container_id bash
docker stop container_id
docker rm container_id

# System operations
docker system prune -a
docker volume prune
docker network prune
```

---

*This comprehensive guide covers the most important Ansible and Docker interview questions. Practice hands-on scenarios and understand the concepts deeply to excel in your DevOps interviews!*