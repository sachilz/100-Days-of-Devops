# Question 4

## Question:

In a bid to automate backup processes, the xFusionCorp Industries sysadmin team has developed a new bash script named `xfusioncorp.sh`. While the script has been distributed to all necessary servers, it lacks executable permissions on App Server 1 within the Stratos Datacenter.

Your task is to grant executable permissions to the `/tmp/xfusioncorp.sh` script on App Server 1. Additionally, ensure that all users have the capability to execute it.

## Answer:

To grant executable permissions to a file so that all users can execute it, you can use the `chmod` command with the absolute mode `755`.

**Step-by-step solution:**

1. **SSH into App Server 1:** 
   Connect to App Server 1 (`stapp01`) using the credentials from the [Infrastructure Details](Infrastructure_Details.md) reference. The user is `tony` and the password is `Ir0nM@n`:
   ```bash
   ssh tony@stapp01
   ```

2. **Grant executable permissions:** 
   Use the `chmod` command with the absolute mode `755` (read, write, execute for owner; read and execute for group and others) to ensure the script is executable by everyone:
   ```bash
   sudo chmod 755 /tmp/xfusioncorp.sh
   ```

3. **Verify the permissions:** 
   Check the file permissions using the `ls -l` command to ensure the `x` (executable) bit is set for the owner, group, and others:
   ```bash
   ls -l /tmp/xfusioncorp.sh
   ```
   *You should see an output containing `rwxr-xr-x` (or similar), indicating that the `x` flag is present for user, group, and others.*

4. **Exit the server:**
   ```bash
   exit
   ```
