# Question 11

## Question:

The Nautilus application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in Stratos DC. After an internal team meeting, they have decided to use the `tomcat` application server. Based on the requirements mentioned below complete the task:

a. Install `tomcat` server on `App Server 3`.
b. Configure it to run on port `8083`.
c. There is a `ROOT.war` file on `Jump host` at location `/tmp`.

Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e `curl http://stapp03:8083`

## Answer:

*Refer to the [Infrastructure Details](../Questions_01_to_10/Infrastructure_Details.md) for server credentials if needed.*

### 1. Connect to App Server 3
From the Jump Host, connect to App Server 3 (`stapp03`) using `banner`'s credentials:
```bash
ssh banner@stapp03
```
*(Password: `BigGr33n`)*

### 2. Install Tomcat
Install the Tomcat server package:
```bash
sudo yum install -y tomcat
```

### 3. Configure Tomcat to Use Port 8083
The default Tomcat HTTP port is `8080`. Change it to `8083` in the `server.xml` configuration file:
```bash
sudo sed -i 's/port="8080"/port="8083"/' /etc/tomcat/server.xml
```
Verify the change:
```bash
grep 'Connector port' /etc/tomcat/server.xml
```

### 4. Start and Enable Tomcat
Enable and start the Tomcat service so it starts automatically on boot:
```bash
sudo systemctl enable --now tomcat
sudo systemctl status tomcat --no-pager
```

### 5. Copy `ROOT.war` from Jump Host to App Server 3
Exit from `stapp03` to return to the Jump Host:
```bash
exit
```
Use `scp` to securely copy the `ROOT.war` file from the Jump Host's `/tmp` directory to App Server 3's `/tmp` directory:
```bash
scp /tmp/ROOT.war banner@stapp03:/tmp/
```
*(Password: `BigGr33n`)*

### 6. Deploy the WAR File
Reconnect to App Server 3:
```bash
ssh banner@stapp03
```
Copy the WAR file into Tomcat's `webapps` directory where it will be served from:
```bash
sudo cp /tmp/ROOT.war /var/lib/tomcat/webapps/ROOT.war
```

### 7. Restart Tomcat
Restart Tomcat to ensure the new application is fully deployed and loaded:
```bash
sudo systemctl restart tomcat
sudo systemctl status tomcat --no-pager
```

### 8. Verify the Deployment
Test that the web application is working correctly on port 8083 as required:
```bash
curl http://stapp03:8083
```
*(If the application deployed successfully, you will see its HTML response).*
