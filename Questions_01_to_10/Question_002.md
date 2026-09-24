# Question 2

## Question:

As part of the temporary assignment to the Nautilus project, a developer named kareem requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed.

Create a user named kareem on App Server 2 in Stratos Datacenter. Set the expiry date to 2027-04-15, ensuring the user is created in lowercase as per standard protocol.

*Note: You can find the infrastructure details by clicking on the Details of all Users and Servers button on the top-right section of the page.*

## Answer:

To create a user with a specific expiry date on a Linux server, use the `useradd` command with the `-e` (expiredate) flag. The date must be provided in the format `YYYY-MM-DD`.

**Step-by-step solution:**

1. **SSH into the server:** 
   Connect to App Server 2 using the details provided in the infrastructure section.
   ```bash
   ssh <username>@stapp02
   ```

2. **Create the user:** 
   Run the following command with `sudo` privileges to create the user `kareem` with the expiry date `2027-04-15`:
   ```bash
   sudo useradd -e 2027-04-15 kareem
   ```

3. **Verify the user expiry:** 
   You can verify that the account expiration date was set correctly using the `chage` command:
   ```bash
   sudo chage -l kareem
   ```
   *(Look for the "Account expires" field in the output to confirm it says April 15, 2027).*
