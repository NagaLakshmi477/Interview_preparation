# What is Ansible?
Ansible is an **agentless configuration management, automation, and orchestration tool**. It uses the **SSH protocol** to connect to remote Linux servers and push configuration changes. Since it is agentless, there is no need to install any agent on the managed nodes. Whenever we run a playbook, Ansible connects to the target servers over SSH, executes the required tasks (such as installing packages, updating configurations, or deploying applications), and then disconnects.

# What is Ansible used for?
Ansible is used to automate IT tasks and manage infrastructure. It helps reduce manual work by automating tasks such as:

- Configuration management
- Application deployment
- Package installation
- Service management
- User and permission management
- Installing application dependencies
- Creating and managing systemd services

Using Ansible, we can manage multiple servers at the same time with a single playbook. This makes deployments faster, more consistent, and reduces manual errors.

# Why is Ansible agentless?
Ansible is called an **agentless** tool because it doesn't require any agent or software to be installed on the target servers. It connects to Linux servers using **SSH** and executes the required tasks remotely. Once the tasks are completed, it disconnects from the server. This makes Ansible easy to set up, maintain, and manage.

# What are Ansible modules?
Ansible modules are built-in tools that perform specific tasks on remote servers. Instead of writing commands manually, we use modules to automate tasks like installing packages, managing services, copying files, creating users, and managing directories.

**Examples:**
- `apt` / `yum` – Install packages
- `service` – Manage services
- `copy` – Copy files
- `file` – Manage files and directories
- `user` – Create users

Modules make automation simple and reduce manual work.

# Command vs Shell Module

| Command Module | Shell Module |
|---------------|--------------|
| Executes a command directly on the remote server. | Executes a command through the shell (`/bin/sh`). |
| Does **not** support shell features like pipes (`|`), redirection (`>`), or variables (`$HOME`). | Supports shell features like pipes, redirection, variables, and command chaining. |
| More secure because it doesn't invoke a shell. | Less secure if user input is not handled properly. |
| Use it for simple commands. | Use it when shell features are required. |

**Example:**

**Command module:**
```yaml
- command: ls -l
```

**Shell module:**
```yaml
- shell: "cat file.txt | grep nginx"
```
# What is a Playbook?
A playbook is a **YAML file** that contains a set of tasks to be executed on one or more managed servers. It defines **what needs to be done**, such as installing packages, configuring services, copying files, or deploying applications, in an automated and repeatable way.

# What are Handlers in Ansible?
Handlers are special tasks that run **only when they are notified by another task**. They are mainly used to **restart or reload services** after a configuration change.

**Example:**

If a configuration file is updated, the task can notify a handler to restart the Nginx service. If there is no change in the configuration, the handler won't run.

# What is Ansible Vault?
Ansible Vault is a feature used to **encrypt sensitive data** in Ansible, such as passwords, API keys, SSH keys, and secret variables. This helps keep confidential information secure while storing it in playbooks or variable files.

When running the playbook, Ansible decrypts the data using the vault password.

# How is Ansible used in CI/CD?
Ansible is used in CI/CD to automate the deployment process. After the application is built, Ansible playbooks can be executed to deploy the application, install dependencies, update configuration files, and restart services on the target servers.

This reduces manual work, ensures consistent deployments, and makes the deployment process faster and more reliable.

# Have you integrated Ansible with Jenkins?

"No, I haven't worked on integrating Ansible with Jenkins yet. In my projects, I used Ansible for automating server configuration and deployment tasks. I know that Ansible can be triggered from CI/CD tools like Jenkins or GitHub Actions, but I haven't implemented that integration myself."

# What will you do if an Ansible playbook gets stuck?

If an Ansible playbook gets stuck, I would:

- Check if the target server is reachable using SSH.
- Verify the inventory file and host details.
- Check if any task is waiting for user input.
- Verify network connectivity and firewall rules.
- Check the logs on the target server for any errors.

# How do you debug an Ansible playbook?
To debug an Ansible playbook, I would:

- Run the playbook with verbose mode:
  ```bash
  ansible-playbook playbook.yml -vvv
  ```
- Check if the target servers are reachable:
  ```bash
  ansible all -m ping
  ```
- Verify the inventory file and host details.
- Check SSH connectivity to the target servers.
- Review the playbook syntax:
  ```bash
  ansible-playbook playbook.yml --syntax-check
  ```
- Check the logs on the target server to identify the root cause.

# Why is an Ansible task skipped?
An Ansible task may be skipped due to the following reasons:

- The `when` condition evaluates to **false**.
- The task has a **tag**, and the playbook is executed with different tags.
- The host doesn't match the target hosts defined in the playbook.
- The task is skipped because of a failed previous task (if configured with conditions).

The first thing I would check is the playbook output to see why the task was skipped, then verify the `when` condition, tags, and target hosts.

# How would you deploy NGINX on 100 servers using Ansible?

**Answer:**

To deploy NGINX on 100 servers, I would:

- Add all the servers to an inventory group (e.g., `web_servers`).
- Create an **NGINX role** to install and configure NGINX.
- Use a **handler** to restart the NGINX service only if the configuration changes.
- Run the playbook once, and Ansible will connect to all the servers over SSH and deploy NGINX.

This approach is reusable, easy to maintain, and ensures consistent deployment across all servers.

# What is Zero-Downtime Deployment?

Zero-downtime deployment is a deployment strategy where a new version of an application is deployed without interrupting service for users. Instead of updating all servers at once, the deployment is done in batches so that some servers continue handling user requests while others are being updated.

In Ansible, this can be achieved using the **`serial`** keyword, which updates a few servers at a time.

# How do you perform a zero-downtime deployment in Ansible?

To achieve a zero-downtime deployment, I use **serial execution**. The `serial` keyword deploys the application to a few servers at a time instead of all servers simultaneously.

**Example:**

```yaml
- hosts: web_servers
  serial: 2
```

In this example, Ansible updates **2 servers at a time**. After those servers are updated, it moves to the next two. This ensures the remaining servers continue serving requests, resulting in little or no downtime.
