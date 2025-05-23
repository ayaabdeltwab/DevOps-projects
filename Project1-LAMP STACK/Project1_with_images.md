# 📊 Project 1: Monitoring Stack with Prometheus & Grafana using Ansible on AWS EC2

This project demonstrates how to automate the setup of Prometheus, Node Exporter, and Grafana on an AWS EC2 instance using **Ansible**. The automation simplifies deployment and enhances consistency across environments.

---

## 🧰 Technologies Used

- AWS EC2 (Amazon Linux 2)
- Ansible
- Prometheus
- Node Exporter
- Grafana

---

## 🏗️ Architecture

```
Ansible Controller (EC2)
        |
        | SSH (using key)
        v
Target EC2 Instance
   ├── Prometheus
   ├── Node Exporter
   └── Grafana
```

---

## ✅ Prerequisites

- Two EC2 instances:
  - **Controller** (with Ansible & public IP)
  - **Target Node** (reachable via private IP)
- SSH key pair
- Open ports in security group: `9090` (Prometheus), `3000` (Grafana)

---

## 🔧 Setup Steps

### 1. Install Ansible on Controller Node

```bash
sudo dnf update -y
sudo dnf install ansible -y
ansible --version
```

### 2. Create Project Structure

```bash
mkdir prometheus_grafana_project
cd prometheus_grafana_project

ansible-galaxy init roles/prometheus
ansible-galaxy init roles/node_exporter
ansible-galaxy init roles/grafana
```

---

## 🗂️ Configuration Files

### Inventory (`inventory`)

```
[node]
<PRIVATE_IP> ansible_user=ec2-user ansible_ssh_private_key_file=/home/ec2-user/prometheus_grafana_project/your-key.pem
```

### Ansible Config (`ansible.cfg`)

```ini
[defaults]
inventory = inventory
remote_user = ec2-user
private_key_file = /path/to/your-key.pem
host_key_checking = False
roles_path = ./roles
```

### Playbook (`playbook.yml`)

```yaml
- name: Install Prometheus, Node Exporter, and Grafana
  hosts: node
  become: yes
  roles:
    - prometheus
    - node_exporter
    - grafana
```

---

## 📦 Role Overview

### 🔹 Prometheus

- Create user & directories
- Download & install binary
- Configure service
- Set up `prometheus.yml`

### 🔹 Node Exporter

- Download binary
- Create service for Node Exporter

### 🔹 Grafana

- Add Grafana repo
- Install Grafana
- Enable and start Grafana service

---

## 🚀 Run the Playbook

```bash
ansible-playbook -i inventory playbook.yml
```

---

## 🧪 Verify Services

```bash
systemctl status prometheus
systemctl status grafana-server
```

---

## 🔍 Access the Dashboards

- Prometheus: `http://<public-ip>:9090`
- Grafana: `http://<public-ip>:3000`

**Grafana default login:**

```
Username: admin
Password: admin
```

---

## 📸 Screenshots

### Prometheus
![Prometheus Dashboard](images/prometheus.png)

### Grafana Dashboard
![Grafana Dashboard](images/grafana-dashboard.png)

### Node Exporter
![Node Exporter Status](images/node-exporter-status.png)

---

## 📝 Notes

- If `curl-minimal` conflicts, remove it before installing `curl`.
- Use `mv` instead of `copy` if `remote_src` errors occur.
- Make sure your security group allows access to ports 9090 and 3000.

---

## 👤 Author

Aya Abdeltwab