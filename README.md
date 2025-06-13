# 🚀 Ansible Cluster Setup using Docker 🐳 & Kubernetes ☸️ on RHEL 9

This project demonstrates how to manually set up an **Ansible Cluster** environment using **custom RHEL 9 Docker images** and replicate the same setup using **Kubernetes Pods**, without using any pre-built images.

---

## 📌 Project Goals

- Build Ansible Master and Nodes manually on **RHEL 9**
- Connect Master to Nodes via SSH
- Configure and test Ansible using a manual inventory
- Repeat the same cluster setup using **Kubernetes Pods**

---

## 🛠️ Tools & Technologies

- RHEL 9 (Red Hat Enterprise Linux)
- Docker
- Kubernetes (via Minikube)
- Ansible
- SSH

---

## 🐳 Phase 1: Ansible Cluster Using Docker

### ✅ Steps:

1. **Create Custom Dockerfiles**
   - `Dockerfile.master` → Installs Ansible + SSH
   - `Dockerfile.slave` → Installs SSH only

2. **Build Docker Images**
   ```bash
   docker build -t rhel9-ansible-master -f Dockerfile.master .
   docker build -t rhel9-ansible-node -f Dockerfile.slave .
   ```

3. **Launch Containers and Setup Network**
    ```bash
   docker network create ansible-net
   docker run -dit --name master --network ansible-net rhel9-ansible-master
   docker run -dit --name slave-node-01 --network ansible-net rhel9-ansible-slave
   docker run -dit --name slave-node-02 --network ansible-net rhel9-ansible-slave
   ```
    
4. **Configure SSH from Master to Nodes**
   - Generate and copy SSH keys using `ssh-copy-id`

5. **Create Ansible Inventory File**
    ```bash
    [nodes]
    ansible-slave-node-01 ansible_user=ansible ansible_password=ansible
    ansible-slave-node-02 ansible_user=ansible ansible_password=ansible
    ```

6. **Test Ansible**
    ```bash
    ansible -i inventory nodes -m ping
    ```

## ☸️ Phase 2: Ansible Cluster Using Kubernetes

### ✅ Steps:

1. **Push Docker Images to Docker Hub**
   ```bash
   docker push himanshu8090/rhel9-ansible-master
   docker push himanshu8090/rhel9-ansible-slave
   ```
   
2. **Install & Start Minikube (Docker Driver)**
   ```bash
   minikube start --driver=docker
   ```
   
3. **Create Kubernetes Pod YAMLs**
   - `ansible-master.yaml`
   - `ansible-slave-node-01.yaml`
   - `ansible-slave-node-02.yaml`

4. **Apply Pod Configurations**
   ```bash
   kubectl apply -f ansible-master.yaml
   kubectl apply -f ansible-slave-node-01.yaml
   kubectl apply -f ansible-slave-node-02.yaml
   ```

5. **Repeat SSH and Ansible Configuration Inside Pods**
   - SSH setup
   - Inventory file
   - Ansible ping test
     
### ✅ Output
Successful ping from Ansible Master to Nodes using:
    ```bash
    ansible -i inventory nodes -m ping
    ```

### 📚 Learning Outcomes
    - Built a manual Ansible environment on Docker and Kubernetes
    - Learned container networking, image creation, and YAML-based orchestration
    - Understood the fundamentals of Ansible SSH-based automation

### 🔗 Author
    Himanshu Kumar Singh
    B.Tech CSE | DevOps & Cloud Enthusiast
    GitHub: himanshukr8090
    
