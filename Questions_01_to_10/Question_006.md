# Question 6

## Question:

The Nautilus system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in Stratos DC on a set schedule. Before that they need to test similar functionality with a sample cron job. Therefore, perform the steps below:

a. Install `cronie` package on all Nautilus app servers and start `crond` service.

b. Add a cron `*/5 * * * * echo hello > /tmp/cron_text` for `root` user.

## Answer:

### 1. SSH to Application Server 1

*(Password: `Ir0nM@n`)*
```bash
ssh tony@stapp01
```

Then:

```bash
sudo yum install -y cronie
sudo systemctl enable --now crond
```

Add the root cron job:

```bash
sudo sh -c 'echo "*/5 * * * * echo hello > /tmp/cron_text" | crontab -'
```

Verify:

```bash
sudo crontab -l
```

You should see:

```text
*/5 * * * * echo hello > /tmp/cron_text
```

### 2. Application Server 2

Exit:

```bash
exit
```

SSH: *(Password: `Am3ric@`)*

```bash
ssh steve@stapp02
```

Run:

```bash
sudo yum install -y cronie
sudo systemctl enable --now crond
sudo sh -c 'echo "*/5 * * * * echo hello > /tmp/cron_text" | crontab -'
```

Verify:

```bash
sudo crontab -l
```

### 3. Application Server 3

Exit:

```bash
exit
```

SSH: *(Password: `BigGr33n`)*

```bash
ssh banner@stapp03
```

Run:

```bash
sudo yum install -y cronie
sudo systemctl enable --now crond
sudo sh -c 'echo "*/5 * * * * echo hello > /tmp/cron_text" | crontab -'
```

Verify:

```bash
sudo crontab -l
```
