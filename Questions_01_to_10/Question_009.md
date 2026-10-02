# Question 9

## Question:

The production support team of xFusionCorp Industries is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for archiving website content files. They have a static website running on App Server 1 in Stratos Datacenter, and they need to create a bash script named `blog_archive.sh` which should accomplish the following tasks. (Also remember to place the script under the `/scripts` directory on App Server 1).

a. Create a zip archive named `xfusioncorp_blog.zip` of `/var/www/html/blog` directory.
b. Save the archive in the `/archives/` directory on the App Server 1. This is a temporary storage, as archives from this location will be cleaned on a weekly basis. Therefore, the archive should also be copied to the Nautilus Storage Server so it can be retrieved later for validation purposes.
c. Copy the created archive to the Nautilus Storage Server server in the `/archives/` location.
d. Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, `tony` in case of App Server 1) must be able to run it.
e. Do not use sudo inside the script.

Note:
The zip package must be installed on given App Server before executing the script. This package is essential for creating the zip archive of the website files. Install it manually outside the script.

## Answer:

*Refer to the [Infrastructure Details](Infrastructure_Details.md) for server credentials if needed.*

### 1. Connect to Application Server 1
From the jump host, SSH into App Server 1:
```bash
ssh tony@stapp01
```

### 2. Install `zip`
The task requires `zip` to be installed before creating the script.
```bash
sudo yum install -y zip
```

### 3. Create the `/scripts` directory
Create the directory and assign ownership to `tony`:
```bash
sudo mkdir -p /scripts
sudo chown tony:tony /scripts
```

### 4. Set up passwordless SSH
The script needs to copy the archive to the Storage Server without asking for a password.

**Generate an SSH key on `stapp01` as `tony`:**
```bash
ssh-keygen -t rsa -b 4096
```
*(Press **Enter** for all prompts to use the default location and an empty passphrase).*

**Copy the public key to the Storage Server:**
```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub natasha@ststor01
```
*(Enter `natasha`'s password: `Bl@kW` when prompted).*

**Test passwordless SSH:**
```bash
ssh natasha@ststor01
```
*(It should log in without asking for a password. Type `exit` to return to `stapp01`).*

### 5. Create the archive script
Open the script file in `vi`:
```bash
vi /scripts/blog_archive.sh
```
Add the following content:
```bash
#!/bin/bash

zip -r /archives/xfusioncorp_blog.zip /var/www/html/blog
scp /archives/xfusioncorp_blog.zip natasha@ststor01:/archives/
```
Save and exit: press `Esc`, type `:wq`, and press `Enter`.

### 6. Make the script executable
```bash
chmod +x /scripts/blog_archive.sh
```

### 7. Run the script
Execute the script to perform the archive and secure copy:
```bash
/scripts/blog_archive.sh
```

### 8. Verify the process
You can verify the archive was created locally:
```bash
ls -lh /archives/xfusioncorp_blog.zip
```
You can also connect to the storage server (`ssh natasha@ststor01`) and verify it was successfully copied:
```bash
ls -lh /archives/xfusioncorp_blog.zip
```
