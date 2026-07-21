# 🛠️ DevOps

DevOps is the process of building, deploying, and testing the code written by the developer on the same day instead of waiting to complete the entire development.

We can achieve this using:
- Continuous Integration
- Continuous Testing
- Continuous Deployment

We can do faster releases with fewer defects using DevOps.

---

# 🐧 Components in Linux

- Kernel  
- File System  
- Utilities  
- Shell  

---

## 🔹 Kernel

- It is the heart of the OS.  
- It directly interacts with hardware.  
- It manages process scheduling and memory management.  

**Example:**  
When we open any application, the kernel allocates resources.

---

## 🔹 Shell

- It is the interface between the user and the kernel.  
- It takes commands from the user and sends them to the kernel.  

---

## 🔹 File System

- Organizes how data is stored and retrieved.  

---

## 🔹 Utilities

- Tools that perform basic tasks.  

**Examples:**  
`cp`, `mv`, `rm`

---

# 📦 Linux Distributions

- CentOS, Fedora, RedHat → `dnf`, `yum`  
- Ubuntu → `apt`  

---

# 🏗️ Linux Architecture

Linux architecture is the structure that shows how the kernel, shell, applications (browser), and hardware work together in the Linux OS.

It consists of four components:
- Kernel → interacts with hardware and allocates resources  
- Shell → acts as an interface between user and kernel  
- Applications → programs used by users  
- Hardware  

---

# 🔍 How to Show Kernel Version

```bash
uname -r
