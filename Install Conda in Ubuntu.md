# **Install Conda in Ubuntu**

### **Step 1: Update your system**
Before installing Conda, ensure your system is up-to-date.

```bash
sudo apt update && sudo apt upgrade -y
```

---

### **Step 2: Download the Miniconda or Anaconda installer**

- **Miniconda** is a lightweight version of Anaconda and includes only Conda and its dependencies.  
- **Anaconda** is a full-fledged data science toolkit and includes Conda plus pre-installed libraries like NumPy, pandas, and Jupyter.

#### Download Miniconda:
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

#### OR Download Anaconda:
```bash
wget https://repo.anaconda.com/archive/Anaconda3-latest-Linux-x86_64.sh
```

---

### **Step 3: Verify the installer (Optional)**
You can verify the integrity of the installer by comparing its SHA256 checksum.

1. Get the checksum of the downloaded file:
   ```bash
   sha256sum Miniconda3-latest-Linux-x86_64.sh
   ```
   (Replace with the Anaconda filename if you downloaded it.)

2. Compare it with the checksum listed on the [Miniconda](https://docs.conda.io/en/latest/miniconda.html) or [Anaconda](https://www.anaconda.com/products/distribution#download-section) website.

---

### **Step 4: Run the installer**
1. Run the installation script:
   ```bash
   bash Miniconda3-latest-Linux-x86_64.sh
   ```
   (Replace with the Anaconda filename if you downloaded it.)

2. Follow the prompts:
   - Press **Enter** to continue reading the license agreement.
   - Type `yes` to accept the license agreement.
   - Choose the installation location (default is usually fine: `~/miniconda3` or `~/anaconda3`).
   - Confirm whether to initialize Conda.

---

### **Step 5: Activate Conda**
After installation, activate Conda by running:

```bash
source ~/miniconda3/bin/activate
```

(Replace `miniconda3` with `anaconda3` if you installed Anaconda.)

You should now see the `(base)` environment in your terminal prompt, indicating that Conda is active.

---

### **Step 6: Test Conda**
Verify that Conda is installed correctly by checking its version:

```bash
conda --version
```

---

### **Step 7: (Optional) Add Conda to your PATH**
If you skipped the option to initialize Conda during installation, you can manually initialize it:

```bash
~/miniconda3/bin/conda init
```

(Replace `miniconda3` with `anaconda3` if needed.)

Restart your terminal for the changes to take effect.

---

### **Step 8: Update Conda**
Finally, update Conda to ensure you have the latest version:

```bash
conda update -n base -c defaults conda
```

---

You now have Conda installed on your Ubuntu system! Let me know if you need help with setting up environments or installing packages. 😊
