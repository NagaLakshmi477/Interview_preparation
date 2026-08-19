# Ansible Interview Preparation

## Scenario-Based Questions & Answers

---

# 1. Shell Script vs Ansible

### Q1. You have a shell script that installs NGINX on one server. Now your company has 100 servers. What problem will you face?

**Answer:**

With a shell script, I need to execute the script on every server separately.

For example:

```bash
./install-nginx.sh
```

For 1 or 2 servers it is manageable, but for 100 or 1000 servers it becomes difficult to manage.

With Ansible, I can define the task once and execute it on multiple servers.

---

### Q2. Your shell script is executed twice and it creates duplicate entries every time. What is the problem?

**Answer:**

The script is not idempotent.

Idempotent means if I run the same operation multiple times, the final result should remain the same.

If every execution creates another entry, then the script is not idempotent.

---

### Q3. Your manager asks why you prefer Ansible over shell scripts for server configuration. What will you say?

**Answer:**

Shell scripts are difficult to manage when the number of servers increases.

Ansible provides:

* Remote server management
* Multiple server execution
* Modules for common tasks
* Idempotent operations
* SSH-based communication
* No agent required on managed nodes

So Ansible is easier to manage for multiple servers.

---

# 2. Configuration Management

### Q4. You received a new server and need to prepare it for an application. What activities will you perform?

**Answer:**

First I will provision the server.

Then I will configure it by:

1. Installing required packages
2. Installing required programming languages
3. Creating required users and folders
4. Downloading application code
5. Installing dependencies
6. Creating the systemd service
7. Starting the service

After that I can deploy the application.

---

### Q5. Your application is already running, but you need to deploy a new version. What steps will you follow?

**Answer:**

I will:

1. Remove the old version if required
2. Download the new version
3. Install/update dependencies
4. Restart the service

This is part of application deployment.

---

# 3. Pull vs Push Model

### Q6. Your interviewer asks: "What is the difference between Pull and Push configuration management?"

**Answer:**

In the Pull model, nodes contact the configuration management server and check whether there are any new configurations.

In the Push model, the control server directly connects to the nodes and pushes the configuration.

For example:

* Chef → Pull model
* Ansible → Push model

---

### Q7. Suppose you have 100 servers and every server checks the CM server every 30 minutes. What problems can this cause?

**Answer:**

It can cause:

* Unnecessary network traffic
* Wasted system resources
* Increased power usage
* Agent software requirement
* Additional communication protocols

---

### Q8. Your company does not want to install any agent on the application servers. Which approach would you prefer?

**Answer:**

I would prefer Ansible because Ansible does not require an agent on the managed nodes.

It uses SSH to connect to the nodes and execute the tasks.

---

# 4. Ansible Architecture

### Q9. Your laptop has Ansible installed and you want to configure three EC2 servers. Which machine is the control node?

**Answer:**

My laptop is the control node because Ansible is installed there.

The three EC2 servers are the managed nodes.

---

### Q10. Where should the Ansible playbooks and inventory normally be maintained?

**Answer:**

They are maintained on the Ansible control node.

They can also be stored in a source control system such as GitHub as the source of truth.

---

### Q11. You need to install NGINX on three servers at the same time. Do you need to install Ansible on all three servers?

**Answer:**

No.

Ansible needs to be installed on the control node.

The managed nodes do not need an Ansible agent.

Ansible connects to them using SSH.

---

# 5. SSH and Remote Execution

### Q12. From server1, you want to create a file on server2. How can you do it using SSH?

**Answer:**

I can execute a command remotely:

```bash
ssh ec2-user@54.85.119.91 'echo "Hello Ansible" >/tmp/ansible.txt'
```

The command executes on server2 and creates the file there.

---

### Q13. What happens in the background when you execute a remote SSH command?

**Answer:**

1. SSH connection is established.
2. The command is executed on the remote server.
3. The result is returned.
4. The SSH connection is closed.

---

# 6. Ansible Ad-Hoc Commands

### Q14. You have one server and want to check whether Ansible can connect to it. Which command can you use?

**Answer:**

I can use the Ansible ping module:

```bash
ansible all -i <private_ip>, \
-e ansible_user=ec2-user \
-e ansible_password=DevOps321 \
-m ping
```

If the connection is successful, Ansible returns a successful ping response.

---

### Q15. What does `-i` mean in an Ansible command?

**Answer:**

`-i` specifies the inventory.

For example:

```bash
-i 172.31.80.5,
```

Here I am providing the server IP as the inventory.

---

### Q16. What does `-m` mean?

**Answer:**

`-m` specifies the Ansible module.

For example:

```bash
-m ping
```

means I am using the ping module.

---

### Q17. What is the purpose of `-b`?

**Answer:**

`-b` enables privilege escalation.

For example, if I need root privileges to install a package, I can use:

```bash
-b
```

---

### Q18. You need to install NGINX on a remote server. How would you do it?

**Answer:**

I can use the `dnf` module:

```bash
ansible all -i 172.31.80.5, \
-e ansible_user=ec2-user \
-e ansible_password=DevOps321 \
-b \
-m dnf \
-a "name=nginx state=installed"
```

Here:

* `-b` gives privilege escalation
* `-m dnf` uses the dnf module
* `-a` provides module arguments

---

### Q19. NGINX is installed but it is not running. What will you do?

**Answer:**

I will start the service using the service module:

```bash
ansible all -i <PRIVATE_IP>, \
-e ansible_user=ec2-user \
-e ansible_password=DevOps321 \
-b \
-m service \
-a "name=nginx state=started"
```

---

# 7. Variables

### Q20. You are using the same value in multiple places in your playbook. What will you do?

**Answer:**

I will use a variable.

A variable holds a value that can be reused wherever required.

If I change the value in one place, all references to that variable will use the new value.

---

### Q21. You run the following command:

```bash
ansible-playbook -i inventory.ini 12-vars-preference.yaml \
-e "GREETING=hellofromarrgs"
```

What happens if `GREETING` is also defined somewhere else?

**Answer:**

The command-line variable has higher priority.

So:

```text
GREETING=hellofromarrgs
```

will be used.

---

### Q22. What is the variable precedence order you have learned?

**Answer:**

From higher priority to lower priority:

1. Command line / arguments
2. Task
3. File
4. Prompt
5. Play
6. Inventory

---

# 8. Conditions

### Q23. You want a task to execute only when a particular condition is true. What will you use?

**Answer:**

I will use the `when` condition.

Example:

```yaml
when: <condition>
```

The task will execute only when the condition is true.

---

### Q24. You have a task that should run only when a variable has a particular value. What will you use?

**Answer:**

I will use `when`.

For example:

```yaml
when: environment == "dev"
```

The task executes only when the condition is true.

---

# 9. Loops

### Q25. You need to install multiple packages using the same task. What can you use?

**Answer:**

I can use a loop.

The `item` variable is a reserved keyword used to represent the current value during the loop.

---

### Q26. During a loop you see `item`. What does it represent?

**Answer:**

`item` represents the current value from the list being processed by the loop.

For example, if the list contains:

```text
nginx
git
python
```

during each iteration, `item` represents the current package.

---

# 10. Modules

### Q27. What is an Ansible module?

**Answer:**

A module is a small tool that Ansible uses to perform a specific task.

For example:

* Installing packages
* Copying files
* Running commands
* Managing services

---

### Q28. You need to install a package. Would you prefer `shell` or a package module?

**Answer:**

I would use the appropriate package module, such as `dnf`, because Ansible already provides a module for package management.

I would use `shell` when I specifically need to execute shell commands.

---

# 11. Command vs Shell

### Q29. You need to execute this command:

```bash
ls -l | grep nginx
```

Which module would you use?

**Answer:**

I would use the `shell` module because the command uses the pipe `|`.

The shell module executes the command through a shell and supports shell features.

---

### Q30. What is the difference between `command` and `shell`?

**Answer:**

`command` executes the command directly without going through a shell.

It does not support shell features such as:

```text
|
>
>>
&&
```

The `shell` module executes the command through a shell, so shell features can be used.

---

### Q31. You have this task:

```yaml
- name: Check nginx
  command: echo $HOME
```

You expect `$HOME` to be expanded. Will it work like a normal shell?

**Answer:**

No.

The command module does not execute the command through a shell, so shell features and variables such as `$HOME` do not work in the same way.

If I need shell behavior, I can use the `shell` module.

---

### Q32. When would you use the shell module?

**Answer:**

I would use the shell module when I need shell features such as:

```bash
|
>
>>
&&
```

or shell variables and other shell behavior.

---

### Q33. When would you use the command module?

**Answer:**

I would use the command module when I simply need to execute a command and do not need shell features.

---

# 12. Scenario: Shell vs Command

### Q34. Your manager gives you the following requirement:

```bash
cat /var/log/messages | grep error > /tmp/errors.txt
```

Which module will you use and why?

**Answer:**

I will use the `shell` module because the command contains:

```text
|
>
```

These are shell features.

---

### Q35. Your manager asks you to execute:

```bash
mkdir /tmp/test
```

There is no pipe, redirection, or shell variable. Which module can you use?

**Answer:**

I can use the `command` module because this is a simple command and does not require shell features.

---

# 13. Roles

### Q36. Your playbook has become very large and contains tasks, files, templates, and variables. How can you organize it better?

**Answer:**

I can use Ansible Roles.

A role provides a proper directory structure for organizing Ansible code.

It also makes the code reusable and easier to maintain.

---

### Q37. What is the purpose of `tasks/main.yml` in a role?

**Answer:**

`tasks/main.yml` contains the main tasks of the role.

Execution of the role starts from `tasks/main.yml`.

Additional task files can be included using `include_tasks`.

---

### Q38. You have some static configuration files that should be copied to the server. Where will you keep them in a role?

**Answer:**

I will keep static files inside:

```text
files/
```

The `files` directory is used for static files that do not require modification.

---

### Q39. You have a configuration file where values need to change based on the environment. Where will you keep it?

**Answer:**

I will use the:

```text
templates/
```

directory.

Templates use Jinja2 and allow placeholders where values can be provided at runtime.

---

### Q40. What is the difference between `files/` and `templates/`?

**Answer:**

`files/` is used for static files.

`templates/` is used for dynamic files where values can be changed at runtime using Jinja2 placeholders.

---

### Q41. Where do you define variables inside a role?

**Answer:**

I can define role variables inside:

```text
vars/
```

I can also define default values inside:

```text
defaults/
```

---

### Q42. What is the purpose of `defaults/`?

**Answer:**

`defaults/` is used to define default variable values.

These variables have the lowest priority.

---

# 14. Role Scenario

### Q43. You have 10 different applications and all of them require the same common server configuration. What will you do?

**Answer:**

I can create a common role containing the common tasks.

Then I can reuse that role for different applications.

This avoids writing the same tasks again and again.

---

### Q44. Your team has copied the same Ansible tasks into five different playbooks. What problem do you see?

**Answer:**

There is duplicate code.

I would create a reusable role and keep the common tasks there.

Then different playbooks can reuse the role.

This follows the DRY principle:

```text
Don't Repeat Yourself
```

---

# 15. Dynamic Roles

### Q45. You want the same playbook to execute different roles based on a variable provided at runtime. What approach can you use?

**Answer:**

I can use dynamic roles.

The role can be selected based on a variable, allowing the same playbook to execute different roles depending on the runtime input.

---

# 16. Complete Interview Scenario

### Q46. You joined a project where there are 50 EC2 servers. Every server needs NGINX, a specific application directory, application code, dependencies, and a systemd service. How would you approach this?

**Answer:**

I would use Ansible.

I would create an inventory containing the servers.

Then I can create roles for different responsibilities.

For example:

```text
roles/
  common/
  nginx/
  application/
```

The roles can contain:

```text
tasks/
files/
templates/
vars/
defaults/
```

I can use Ansible to configure all servers through SSH without installing an agent on them.

---

# 17. Complete Deployment Scenario

### Q47. Your application is currently running on 20 servers. A new version needs to be deployed. How would you use Ansible?

**Answer:**

I would create tasks to:

1. Remove the old application version if required
2. Download the new version
3. Install dependencies
4. Update required configuration
5. Restart the systemd service

Then Ansible can perform the same deployment on all required servers.

---

# 18. Troubleshooting Scenario

### Q48. An Ansible task is failing because it needs root privileges. What will you check?

**Answer:**

I will check whether privilege escalation is enabled.

For an ad-hoc command, I can use:

```bash
-b
```

For example:

```bash
ansible all -b -m dnf -a "name=nginx state=installed"
```

---

### Q49. An Ansible command is failing because the server cannot be reached. What will you check first?

**Answer:**

I will check the SSH connection.

I will verify:

* Server IP
* Username
* Password or SSH authentication
* Network connectivity
* SSH access

I can also test the Ansible connection using:

```bash
ansible all -i <private_ip>, -e ansible_user=ec2-user -m ping
```

---

# 19. Quick Interview Questions

### Q50. Does Ansible require an agent on managed nodes?

**Answer:**

No. Ansible uses SSH to communicate with managed nodes.

---

### Q51. Which side contains Ansible configuration and playbooks?

**Answer:**

The control node.

---

### Q52. Which module can execute commands through a shell?

**Answer:**

`shell`

---

### Q53. Which module executes commands without a shell?

**Answer:**

`command`

---

### Q54. Which module can be used to install packages on a DNF-based system?

**Answer:**

`dnf`

---

### Q55. Which module can be used to manage services?

**Answer:**

`service`

---

### Q56. What keyword is commonly used for conditions?

**Answer:**

`when`

---

### Q57. What is `item` in an Ansible loop?

**Answer:**

`item` represents the current value being processed in the loop.

---

### Q58. Where are the main role tasks stored?

**Answer:**

```text
tasks/main.yml
```

---

### Q59. Where are static files stored in a role?

**Answer:**

```text
files/
```

---

### Q60. Where are Jinja2 dynamic files stored?

**Answer:**

```text
templates/
```

---

### Q61. Where are role variables stored?

**Answer:**

```text
vars/
```

---

### Q62. Where are default variables stored?

**Answer:**

```text
defaults/
```

---

# Important Scenarios to Practice Yourself

Before an interview, practice answering these without looking at the answer:

### Scenario 1

You have 100 servers and need to install NGINX on all of them. Why would you choose Ansible instead of a shell script?

### Scenario 2

You need to execute:

```bash
cat file.txt | grep error > error.txt
```

Would you use `command` or `shell`? Why?

### Scenario 3

You need to run a task only when the environment is `production`. What will you use?

### Scenario 4

You have the same tasks repeated in multiple playbooks. How will you avoid duplicate code?

### Scenario 5

You need a configuration file where the application name and port change at runtime. Where will you keep the file?

### Scenario 6

You need to copy an unchanged configuration file to a server. Where will you keep it?

### Scenario 7

Your Ansible task needs root privileges to install a package. What will you use?

### Scenario 8

You have multiple packages to install and want to use one task repeatedly. What will you use?

### Scenario 9

A variable is defined in the inventory and also passed using `-e` from the command line. Which value will be used?

### Scenario 10

You have 20 servers and don't want to install an Ansible agent on them. Which configuration management approach can you use?

---

# Short Revision

```text
Shell Script
    ↓
Good for simple/local tasks
    ↓
Difficult to manage many servers

Ansible
    ↓
Push model
    ↓
Control Node
    ↓
SSH
    ↓
Managed Nodes
    ↓
No agent required
```

```text
Ansible Module
    ↓
ping
dnf
service
shell
command
```

```text
Role
    ↓
tasks/
files/
templates/
vars/
defaults/
```

```text
command
    ↓
No shell
    ↓
No | > >> && shell variables

shell
    ↓
Uses shell
    ↓
Supports | > >> && variables
```

```text
Idempotent
    ↓
Run multiple times
    ↓
Final state remains the same
```

```text
when
    ↓
Condition

loop
    ↓
Repeat task

item
    ↓
Current loop value
```
