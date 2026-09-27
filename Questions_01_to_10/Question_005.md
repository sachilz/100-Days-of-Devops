# Question 5

## Question:

Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for App server 2 in the Stratos Datacenter:

1. Install the required `SELinux` packages.
2. Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.
3. No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.
4. Disregard the current status of SELinux via the command line; the final status after the reboot should be `disabled`.

## Answer:

To complete this task, you need to install the SELinux packages on App Server 2 and modify the persistent SELinux configuration file. 

**Important Notes:**
- **Do NOT** run `setenforce 0` as the solution. The task explicitly says to disregard the current runtime status via the command line. They are checking the persistent configuration in `/etc/selinux/config`.
- **Do NOT** reboot the server because a maintenance reboot is already scheduled.

**Step-by-step solution:**

1. **SSH into App Server 2:** 
   Connect to App Server 2 using the credentials from the [Infrastructure Details](Infrastructure_Details.md) reference (User: `steve`, Password: `Am3ric@`):
   ```bash
   ssh steve@stapp02
   ```

2. **Install the required SELinux packages:** 
   Use `yum` to install the `selinux-policy` and `selinux-policy-targeted` packages:
   ```bash
   sudo yum install -y selinux-policy selinux-policy-targeted
   ```

3. **Verify the installation:** 
   You can verify the packages were installed successfully:
   ```bash
   rpm -qa | grep selinux
   ```

4. **Permanently disable SELinux:** 
   Open the SELinux configuration file using the `vi` editor:
   ```bash
   sudo vi /etc/selinux/config
   ```
   - Press the `i` key to enter **Insert Mode**.
   - Find the line that says `SELINUX=enforcing` (or similar).
   - Change it exactly to: `SELINUX=disabled`
   - Press the `Esc` key to exit Insert Mode.
   - Type `:wq` and press `Enter` to save the file and quit the editor.

5. **Verify the configuration change:** 
   Check the `/etc/selinux/config` file to ensure the change was saved correctly:
   ```bash
   grep '^SELINUX=' /etc/selinux/config
   ```
   *(The output should be `SELINUX=disabled`)*

6. **Exit the server:**
   ```bash
   exit
   ```
