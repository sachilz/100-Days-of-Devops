# Question 3

## Question:

Following security audits, the xFusionCorp Industries security team has rolled out new protocols, including the restriction of direct root SSH login.

Your task is to disable direct SSH root login on all app servers within the Stratos Datacenter.

## Answer:

To disable direct SSH root login on Linux servers, you need to modify the SSH daemon configuration file (`sshd_config`) and restart the SSH service.

**Step-by-step solution:**

1. **SSH into each app server:** 
   You need to perform this task on all app servers. Use the credentials from the [Infrastructure Details](Infrastructure_Details.md) reference:
   - App Server 1 (`stapp01`): `ssh tony@stapp01` (Password: `Ir0nM@n`)
   - App Server 2 (`stapp02`): `ssh steve@stapp02` (Password: `Am3ric@`)
   - App Server 3 (`stapp03`): `ssh banner@stapp03` (Password: `BigGr33n`)

2. **Edit the SSH configuration file:** 
   Open the `/etc/ssh/sshd_config` file using a text editor like `vi` or `nano` with `sudo` privileges:
   ```bash
   sudo vi /etc/ssh/sshd_config
   ```

3. **Disable root login:** 
   Find the line that contains `PermitRootLogin`. It might be commented out with a `#` or set to `yes` (or `prohibit-password`). 
   To edit this in the `vi` editor:
   - Press the `i` key on your keyboard to enter **Insert Mode**.
   - Modify the line to exactly: `PermitRootLogin no` (make sure to remove the `#` at the beginning if there is one).
   - Press the `Esc` key to exit Insert Mode.
   - Type `:wq` and press `Enter` to save the file and quit the editor.

4. **Test and restart the SSH service:** 
   Before restarting the service, it's a best practice to test the SSH configuration for any syntax errors. If the test passes, restart the daemon, and then `exit` to disconnect from the server:
   ```bash
   sudo sshd -t
   sudo systemctl restart sshd
   exit
   ```

5. **Repeat:** 
   Ensure you repeat steps 1-4 for **all** the app servers specified in the Datacenter infrastructure.
