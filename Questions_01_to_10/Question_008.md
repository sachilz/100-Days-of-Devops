# Question 8

## Question:

There is a critical issue going on with the Nautilus application in Stratos DC. The production support team identified that the application is unable to connect to the database. After digging into the issue, the team found that mariadb service is down on the database server. Look into the issue and fix the same.

## Answer:

*Refer to the [Infrastructure Details](Infrastructure_Details.md) for server credentials if needed.*

### Problem
The Nautilus application cannot connect to the database because the MariaDB service on the database server (`stdb01`) is down. The MariaDB startup process fails with an ownership error related to `/var/lib/mysql`.

### Server Details
- **Database Server:** `stdb01`
- **User:** `peter`
- **Password:** `Sp!dy`
- **MariaDB service:** `mariadb`
- **Database directory:** `/var/lib/mysql`

---

**Step-by-step solution:**

### Step 1: Connect to the Database Server
From the jump host, SSH into the database server using `peter`'s credentials:
```bash
ssh peter@stdb01
```

### Step 2: Check MariaDB Status
Initially, the service may show as inactive. Try starting it:
```bash
sudo systemctl start mariadb
```
If it fails, check the status again:
```bash
sudo systemctl status mariadb
```
The error will likely show:
`chown: changing ownership of '/var/lib/mysql': Operation not permitted`

### Step 3: Check the Database Directory
Check the ownership of the MySQL directory:
```bash
ls -ld /var/lib/mysql
```
If it is owned by `root`, MariaDB will fail because it expects ownership by the `mysql` user.

### Step 4: Fix the Ownership
Change the owner and group of the MariaDB data directory to `mysql` and set the correct permissions:
```bash
sudo chown mysql:mysql /var/lib/mysql
sudo chmod 755 /var/lib/mysql
```
Verify the change:
```bash
ls -ld /var/lib/mysql
```
*(It should now show `mysql mysql` ownership).*

### Step 5: Start and Enable MariaDB
Now start the service again and enable it to automatically start on boot:
```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```
Check its status to confirm it's running:
```bash
sudo systemctl status mariadb
```
*(The expected result is `Active: active (running)`).*

### Step 6: Verify MariaDB Is Responding
Finally, run a ping check to verify the database is up:
```bash
sudo mysqladmin ping
```
*(Expected output: `mysqld is alive`)*

**Important:**
Do **not** delete `/var/lib/mysql` or reinitialize the MariaDB database unless specifically required. The issue can be fixed purely by correcting the ownership/permissions and starting the existing MariaDB service.
