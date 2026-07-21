## Linux / DevOps Scenario-Based Interview Notes

---

## 1. Server is running slowly – what steps would you take?

### Human interview answer

First, I log in to the server and check the overall performance using **top** or **htop**. This quickly tells me whether CPU, memory, or a particular process is consuming too many resources.

### Commands

```bash
top
htop
```

Then I check **memory usage** to see whether the server is running out of RAM or using a lot of swap.

```bash
free -m
```

Next, I check **disk space**.

```bash
df -h
```

If disk usage is high, I also check disk activity to identify any I/O bottleneck.

Then I verify whether important services are running properly.

```bash
systemctl status <service-name>
```

I also check **network connectivity and traffic**.

Finally, I review **system and application logs** for errors or warnings.

```bash
journalctl -xe
tail -f /var/log/messages
tail -f /var/log/syslog
```

### One-line summary

**I check CPU → Memory → Disk → Services → Network → Logs, then fix the actual bottleneck.**

---

## 2. User home directory is filling the root partition

### Human interview answer

First, I identify what is consuming space inside the user’s home directory.

```bash
du -sh /home/user/*
```

This shows the size of each folder in MB/GB.

Then I:

* Delete unnecessary files (old logs, downloads, temp files)
* Compress important files
* Move large files to another partition if needed

### Example

```bash
tar -czf backup.tar.gz large_folder/
mv backup.tar.gz /data/
```

### One-line summary

**Find the large files, remove unnecessary data, archive important data, and move it off the root partition if required.**

---

## 3. Secure a web server against common vulnerabilities

### Human interview answer

I would focus on **access control, network security, encryption, and patching**.

### Steps

* Disable root SSH login
* Use SSH keys instead of passwords
* Allow only required ports (80, 443, 22)
* Remove unnecessary services
* Keep the OS and web server updated
* Enable HTTPS (SSL/TLS)
* Set proper file permissions
* Monitor logs regularly

### Commands

```bash
# Disable root login
PermitRootLogin no

# Disable password login
PasswordAuthentication no

# Firewall
firewall-cmd --add-service=http --permanent
firewall-cmd --add-service=https --permanent
firewall-cmd --reload
```

### One-line summary

**Harden SSH, restrict network access, enable HTTPS, keep the server patched, and monitor logs.**

---

## 4. SSH brute-force attacks

### Human interview answer

I would stop attackers from repeatedly guessing passwords.

### Steps

* Disable password authentication
* Allow only SSH key-based login
* Disable root login
* Install Fail2Ban
* Restrict SSH access through the firewall
* Monitor authentication logs

### Commands

```bash
# SSH hardening
PasswordAuthentication no
PermitRootLogin no

# Fail2Ban
systemctl enable fail2ban
systemctl start fail2ban

# Check failed logins
tail -f /var/log/secure
```

### One-line summary

**Use SSH keys, disable root/password logins, block repeated failures with Fail2Ban, and monitor logs.**

---

## 5. Critical service crashed and will not restart

### Human interview answer

I follow a simple troubleshooting flow.

### Steps

#### Check service status

```bash
systemctl status <service>
```

#### Check logs

```bash
journalctl -u <service> -xe
```

#### Validate configuration

```bash
nginx -t
httpd -t
```

#### Check dependencies and ports

```bash
ss -tulnp
```

#### Check resources

```bash
free -m
df -h
```

#### Restart and monitor

```bash
systemctl restart <service>
systemctl status <service>
```

### One-line summary

**Check status → Logs → Configuration → Dependencies → Resources → Restart and monitor.**

---

## 6. Server time is out of sync

### Human interview answer

Incorrect time can break SSL certificates, Kerberos, databases, and distributed systems.

### Commands

```bash
date
timedatectl
```

Set the correct timezone:

```bash
timedatectl set-timezone Asia/Kolkata
```

Enable NTP synchronization:

```bash
timedatectl set-ntp true
```

Verify:

```bash
timedatectl status
```

### One-line summary

**Set the correct timezone, enable NTP, restart the time service if needed, and verify synchronization.**

---

## 7. Create a user with no shell access

### Command

```bash
useradd -s /sbin/nologin username
```

Verify:

```bash
grep username /etc/passwd
```

### Human answer

This creates a user account that can own files or be used by an application, but the user cannot log in interactively.

### One-line summary

**Use `/sbin/nologin` to prevent interactive shell access.**

---

## 8. Server is swapping heavily

### Human interview answer

Heavy swap usually means the server does not have enough available RAM.

### Check memory

```bash
free -m
top
htop
```

### Find memory-hungry processes

```bash
ps aux --sort=-%mem | head
```

### Stop unnecessary processes

```bash
kill -15 <PID>
```

Use `-9` only if the process refuses to stop.

### Clear cache (temporary)

```bash
sync
echo 3 > /proc/sys/vm/drop_caches
```

### Reduce swap usage

```bash
sysctl vm.swappiness=10
```

### Add temporary swap

```bash
fallocate -l 1G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
```

### One-line summary

**Identify memory-hungry processes, stop unnecessary ones, tune swappiness, add temporary swap if required, and monitor the server.**

---

## 9. Restrict a user’s disk usage

### Human interview answer

I would use **disk quotas**.

### Steps

* Enable quotas on the filesystem
* Remount the filesystem
* Create quota database
* Set user limits

### Commands

```bash
mount -o remount,usrquota /

quotacheck -cum /

quotaon /

edquota username
```

### One-line summary

**Enable filesystem quotas and set hard/soft limits for the user.**

---

## 10. Ensure a service starts on boot

### Commands

```bash
systemctl enable <service-name>
systemctl start <service-name>
systemctl is-enabled <service-name>
```

### Human answer

`enable` creates the required Systemd links so the service starts automatically after every reboot.

### One-line summary

**Use `systemctl enable` and verify with `systemctl is-enabled`.**

---

## 11. 502 Bad Gateway troubleshooting

### Human interview answer

A 502 error usually means the web server (Nginx/Apache) cannot get a valid response from the backend application.

### Check web server

```bash
systemctl status nginx
systemctl status httpd
```

### Check backend application

```bash
systemctl status <app-service>
```

### Check logs

```bash
journalctl -u nginx -xe
journalctl -u <app-service> -xe
```

### Check listening ports

```bash
ss -tulnp
```

### Test backend directly

```bash
curl http://127.0.0.1:8080
```

### One-line summary

**Confirm the web server is healthy, confirm the backend is running, check logs, verify the port, and test the backend directly.**

---

## 12. Schedule recurring cleanup

### Commands

Open crontab:

```bash
crontab -e
```

Run daily at 2:00 AM:

```cron
0 2 * * * /usr/local/bin/cleanup.sh
```

### Important DevOps detail

Use **absolute paths** inside cron jobs because cron has a minimal environment.

Example:

```cron
0 2 * * * /bin/bash /usr/local/bin/cleanup.sh >> /var/log/cleanup.log 2>&1
```

### One-line summary

**Use cron with absolute paths and redirect output to logs.**

---

## 13. Compile and install software from source

### Commands

```bash
tar -xzf app.tar.gz
cd app
./configure
make
sudo make install
```

### Human answer

I first install required dependencies, extract the source code, configure the build, compile it, run tests if available, and then install it.

### One-line summary

**Install dependencies → Configure → Compile → Test → Install.**

---

## 14. Find and kill all processes of a user

### Commands

Find processes:

```bash
ps -u username
```

Gracefully stop them:

```bash
pkill -15 -u username
```

Force stop if required:

```bash
pkill -9 -u username
```

### One-line summary

**Use `pkill -15 -u user` first; use `-9` only if processes refuse to terminate.**

---

# Rapid-Fire Scenario Questions

---

## Q1. Load average = 20 on an 8-core server

### Human answer

Yes, the server is likely overloaded because 20 runnable tasks are competing for only 8 CPU cores. But load average can also increase due to disk I/O waits.

### Confirm the bottleneck

```bash
top
htop
vmstat 1
iostat -x 1
sar -u 1
```

### What I look for

* CPU near 100% → CPU bottleneck
* High `%wa` (I/O wait) → Disk bottleneck
* High run queue in `vmstat` → CPU saturation

### One-line summary

**Load > CPU cores suggests overload; confirm whether the bottleneck is CPU or disk I/O.**

---

## Q2. kill -15 vs kill -9

### Human answer

* `kill -15` (SIGTERM): asks the process to exit gracefully.
* `kill -9` (SIGKILL): immediately terminates the process.

### Which first?

Always try:

```bash
kill -15 <PID>
```

Use `kill -9` only if the process does not stop.

### One-line summary

**Use SIGTERM first; SIGKILL is the last resort.**

---

## Q3. firewalld vs iptables

### Human answer

* **iptables** directly edits kernel firewall rules and is mostly static.
* **firewalld** is a higher-level dynamic firewall manager built on top of iptables/nftables.

### Example

```bash
firewall-cmd --add-service=http --permanent
firewall-cmd --reload
```

### One-line summary

**iptables manages raw rules; firewalld manages zones/services dynamically without restarting the firewall.**

---

## Q4. Why is LVM important?

### Human answer

LVM allows storage to be resized and managed without repartitioning disks, which is extremely useful in production and high-availability environments.

### Three components

* **PV** – Physical Volume
* **VG** – Volume Group
* **LV** – Logical Volume

### One-line summary

**LVM provides flexible, online storage management using PV, VG, and LV.**

---

## Q5. "No space left on device" but disk is 85% free

### Human answer

The most likely cause is **inode exhaustion**.

### Confirm

```bash
df -i
```

If `IUse%` is 100%, the filesystem has run out of inodes even though disk space is still available.

### One-line summary

**The problem is usually inodes, not disk capacity; check with `df -i`.**

---

## Q6. Allow only `systemctl restart httpd` with sudo

### Human answer

Edit sudoers safely:

```bash
visudo
```

Add:

```sudoers
username ALL=(ALL) NOPASSWD: /bin/systemctl restart httpd
```

### One-line summary

**Grant only the exact command needed using a sudoers rule.**

---

## Q7. Why SSH keys are better for automation

### Human answer

SSH keys are much harder to brute-force, can be protected with passphrases, can be rotated centrally, and avoid storing passwords inside scripts, CI/CD pipelines, or configuration tools.

### One-line summary

**SSH keys provide stronger security and are safer for automated systems than passwords.**

---

## Q8. Loop through servers.txt and run hostname

### Command

```bash
while read server; do
  ssh "$server" hostname
done < servers.txt
```

### One-line summary

**Read each hostname from the file and execute the command over SSH.**

---

## Q9. grep vs sed vs awk

### Human answer

* **grep** → Find matching lines
* **sed** → Modify or replace text
* **awk** → Extract columns and perform calculations

### Example pipeline

```bash
grep ERROR app.log | awk '{print $1, $5}' | sed 's/ERROR/FAIL/'
```

### One-line summary

**grep finds, sed edits, awk extracts/analyzes.**

---

## Q10. Role of Systemd

### Human answer

Systemd is the **service and system manager** responsible for booting the system, starting/stopping services, managing dependencies, logging integration, and controlling targets (runlevels).

### Main command

```bash
systemctl
```

### One-line summary

**Systemd manages the entire Linux boot process, services, and targets.**

---

## Q11. Low-level vs high-level package manager

### Human answer

* **rpm/dpkg**: install a specific package file only.
* **yum/dnf/apt**: resolve dependencies, download packages from repositories, update packages, and remove packages cleanly.

### One-line summary

**Low-level tools install packages; high-level tools install packages plus dependencies and repository management.**

---

## Q12. Daily cleanup at 2 AM

### Tool

**Cron**

### Entry

```cron
0 2 * * * /usr/local/bin/cleanup.sh
```

### Often-forgotten detail

* Script must be executable
* Use absolute paths
* Redirect output to logs

```bash
chmod +x /usr/local/bin/cleanup.sh
```

### One-line summary

**Use cron, absolute paths, executable permissions, and log redirection.**

---

## Q13. What are cgroups?

### Human answer

Control Groups (cgroups) limit and isolate **CPU, memory, disk I/O, and network resources** for processes.

Docker and Kubernetes rely on cgroups to ensure one container cannot consume all system resources.

### One-line summary

**cgroups provide resource isolation and limits, which containers depend on.**

---

## Q14. View real-time logs for one Systemd service

### Command

```bash
journalctl -u <service-name> -f
```

### Example

```bash
journalctl -u nginx -f
```

### One-line summary

**Use `journalctl -u service -f` for live logs of a single service.**

---

## Q15. Why use `mtr` instead of `traceroute`?

### Human answer

`traceroute` shows the path once. `mtr` continuously sends probes and combines **traceroute + ping**, so it shows **latency, jitter, and packet loss over time**.

### Command

```bash
mtr <remote-host>
```

### Why it is better

For intermittent packet loss, a single traceroute may miss the problem, while `mtr` can reveal which hop is dropping packets and whether the issue is persistent or intermittent.

### One-line summary

**Use `mtr` when you need continuous visibility into latency and packet loss, not just the route.**
