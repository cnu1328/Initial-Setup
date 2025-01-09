## To install Docker and Docker Compose on your Ubuntu laptop, follow these steps:

---

### **Step 1: Uninstall Old Versions**
If you already have an older version of Docker installed, remove it:
```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

---

### **Step 2: Update the Package List**
Update the package list to ensure you have the latest version of `apt`:
```bash
sudo apt-get update
```

---

### **Step 3: Install Prerequisites**
Install necessary packages for Docker:
```bash
sudo apt-get install -y ca-certificates curl gnupg lsb-release
```

---

### **Step 4: Add Docker's Official GPG Key**
Add Docker’s GPG key to verify downloads:
```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

---

### **Step 5: Set Up the Docker Repository**
Add Docker’s official stable repository:
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

---

### **Step 6: Install Docker**
1. Update the package list again:
   ```bash
   sudo apt-get update
   ```

2. Install Docker:
   ```bash
   sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

---

### **Step 7: Verify Docker Installation**
1. Check the Docker version:
   ```bash
   docker --version
   ```

2. Check Docker Compose plugin version:
   ```bash
   docker compose version
   ```

---

### **Step 8: Manage Docker as a Non-Root User**
To run Docker without using `sudo`:
1. Add your user to the Docker group:
   ```bash
   sudo usermod -aG docker $USER
   ```

2. Log out and log back in, or run:
   ```bash
   newgrp docker
   ```

3. Verify by running:
   ```bash
   docker run hello-world
   ```
