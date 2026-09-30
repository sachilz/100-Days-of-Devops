# Question 7

## Question:

During the weekly meeting, the Nautilus DevOps team discussed about the automation and configuration management solutions that they want to implement. While considering several options, the team has decided to go with Ansible for now due to its simple setup and minimal pre-requisites. The team wanted to start testing using Ansible, so they have decided to use jump host as an Ansible controller to test different kind of tasks on rest of the servers.

Install `ansible` version `4.10.0` on `Jump host` using `pip3` only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.

## Answer:

*Refer to the [Infrastructure Details](Infrastructure_Details.md) for server credentials if needed.*

To solve this, you need to install a specific version of Ansible globally using `pip3`. Since you are already logged into the Jump Host (usually as `thor`), you just need to run the installation command with `sudo` privileges so that it installs system-wide for all users.

**Step-by-step solution:**

1. **Install Ansible globally using pip3:**
   Run the following command. Using `sudo` ensures that it is installed globally in the system paths (rather than just for your local user), and `==4.10.0` specifies the exact version requested:
   ```bash
   sudo pip3 install ansible==4.10.0
   ```

2. **Verify the installation:**
   Check the version to ensure it was successfully installed and that the binary is globally accessible:
   ```bash
   ansible --version
   ```
   *(The output should confirm that version 4.10.0 is installed and running from a global path like `/usr/local/bin/ansible` or `/usr/bin/ansible`).*
