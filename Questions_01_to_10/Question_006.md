# Question 6

## Question:

The system admins team of xFusionCorp Industries has set up some scripts on jump host that run on regular intervals and perform operations on all app servers in Stratos Datacenter. To make these scripts work properly we need to make sure the `thor` user on jump host has password-less SSH access to all app servers through their respective sudo users (i.e `tony` for app server 1). Based on the requirements, perform the following:

Set up a password-less authentication from user `thor` on jump host to all app servers through their respective sudo users.

## Answer:

*Refer to the [Infrastructure Details](Infrastructure_Details.md) for server credentials if needed.*

To set up password-less SSH authentication, you need to generate an SSH key pair on the Jump Host (where you are already logged in as `thor`) and then copy the public key to the respective user's account on each app server.

**Step-by-step solution:**

*You start logged in as `thor` on the Jump Host. Refer to the [Infrastructure Details](Infrastructure_Details.md) for server credentials if needed.*

### 1. Generate SSH Key Pair on Jump Host

Generate the SSH key pair by running the following command. Press `Enter` for all the prompts to accept the default file location and to leave the passphrase empty (this is crucial for it to be "password-less"):
```bash
ssh-keygen -t rsa
```

### 2. Copy the Public Key to App Server 1

Use `ssh-copy-id` to copy the public key to the `tony` user on App Server 1 (`stapp01`).
*(Password: `Ir0nM@n`)*
```bash
ssh-copy-id tony@stapp01
```
*Verify it works by logging in without being prompted for a password:*
```bash
ssh tony@stapp01
exit
```

### 3. Copy the Public Key to App Server 2

Use `ssh-copy-id` to copy the public key to the `steve` user on App Server 2 (`stapp02`).
*(Password: `Am3ric@`)*
```bash
ssh-copy-id steve@stapp02
```
*Verify it works by logging in without being prompted for a password:*
```bash
ssh steve@stapp02
exit
```

### 4. Copy the Public Key to App Server 3

Use `ssh-copy-id` to copy the public key to the `banner` user on App Server 3 (`stapp03`).
*(Password: `BigGr33n`)*
```bash
ssh-copy-id banner@stapp03
```
*Verify it works by logging in without being prompted for a password:*
```bash
ssh banner@stapp03
exit
```
