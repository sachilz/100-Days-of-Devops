# Question 12

## Question:

Our monitoring tool has reported an issue in Stratos Datacenter. One of our app servers has an issue, as its Apache service is not reachable on port `8087` (which is the Apache port). The service itself could be down, the firewall could be at fault, or something else could be causing the issue.

Use tools like `telnet`, `netstat`, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.

Once fixed, you can test the same using command `curl http://stapp01:8087` command from jump host.

Note: Please do not try to alter the existing `index.html` code, as it will lead to task failure.

## Answer:

*Refer to the [Infrastructure Details](../Questions_01_to_10/Infrastructure_Details.md) for server credentials if needed.*

### Step 1: Connect to Application Server 1
From the Jump Host, SSH into App Server 1 (`stapp01`) using `tony`'s credentials:
```bash
ssh tony@stapp01
```
*(Password: `Ir0nM@n`)*

### Step 2: Find What Is Using Port 8087
Check what is currently listening on port 8087. The task mentions Apache should be using it, but another service might be conflicting.
```bash
sudo ss -lntp | grep :8087
```
If you see something like `users:(("sendmail"...))`, then `sendmail` is using the required Apache port.

### Step 3: Stop the Conflicting Service
Stop the conflicting service (`sendmail` in this case):
```bash
sudo systemctl stop sendmail
```
Check the port again to confirm it is free:
```bash
sudo ss -lntp | grep :8087
```

### Step 4: Start Apache
Now that the port is free, start the Apache (`httpd`) service:
```bash
sudo systemctl start httpd
sudo systemctl status httpd
```
*(The status should now be `Active: active (running)`).*

Verify Apache is listening on port 8087:
```bash
sudo ss -lntp | grep :8087
```
You should see `users:(("httpd"...))` listening on `*:8087`.

### Step 5: Test Apache Locally
Before opening the firewall, test it locally on `stapp01`:
```bash
curl http://localhost:8087
```
*(You should see the existing webpage HTML. **Do not edit `index.html`** as per the note).*

### Step 6: Fix the Firewall (iptables)
Check the current `iptables` rules:
```bash
sudo iptables -L INPUT -n -v
```
If you see a `REJECT all` rule but no rule explicitly allowing port 8087, traffic from the jump host will be blocked.

Insert a rule to allow incoming TCP traffic on port `8087` before the `REJECT` rule:
```bash
sudo iptables -I INPUT -p tcp --dport 8087 -j ACCEPT
```
Verify the rule was added:
```bash
sudo iptables -L INPUT -n -v
```
*(You should see an `ACCEPT` rule for `tcp dpt:8087` before the `REJECT` rule).*

### Step 7: Test From the Jump Host
Exit back to the Jump Host:
```bash
exit
```
Run the final validation curl command from the Jump Host:
```bash
curl http://stapp01:8087
```
*(If you receive the HTML output, the issue is fully resolved).*
