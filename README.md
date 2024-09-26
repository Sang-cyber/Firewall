# Firewall

Here are real-time examples of performing firewall reviews and implementations for **Palo Alto** and **Cisco firewalls**, including steps to implement firewall policies:

---

### **Palo Alto Firewall Review and Implementation Example**

#### **Scenario:**  
You are tasked with reviewing and implementing firewall rules for a healthcare organization. The goal is to secure sensitive patient data and limit access to internal systems while allowing authorized personnel to perform their jobs.

#### **Steps to Implement Firewall Policies:**

1. **Log in to the Palo Alto Web Interface:**
   - Access the Palo Alto firewall by logging into the web interface using the administrator credentials.

2. **Review Current Security Policies:**
   - Navigate to **Policies > Security** to see the list of existing rules.
   - Check whether existing policies follow the "least privilege" principle. For example, if there's a rule allowing access to the database server from all IPs, it should be reviewed for potential risks.

3. **Identify Unused or Broad Rules:**
   - Navigate to **Monitor > Logs > Traffic** to review traffic logs.
   - Look for unused or overly permissive rules (e.g., any rule that allows traffic from "any" source IP to "any" destination IP on any port).
   - Modify or remove these rules as needed to reduce risk.

4. **Create a New Security Policy (Example: Restrict Database Access):**
   - Navigate to **Policies > Security > Add**.
   - Name the policy, for example, **"Restrict_DB_Access."**
   - **Source Zone**: Specify the zone from which the traffic originates, such as **"Internal_Users"**.
   - **Source Address**: Define the specific IP addresses or subnets that require access to the database, such as **"192.168.10.0/24"** (representing internal user machines).
   - **Destination Zone**: Set this to the destination zone where the database resides, e.g., **"DB_Servers"**.
   - **Destination Address**: Define the IP of the database server (e.g., **"192.168.100.10"**).
   - **Application**: Select the application relevant to your database, such as **"MS-SQL"** or **"MySQL"**.
   - **Action**: Set the action to **"Allow"**.
   - **Log at Session End**: Ensure this option is enabled to log traffic for auditing purposes.
   - Commit the changes to activate the policy.

5. **Test and Validate the New Policy:**
   - Verify that only authorized users in the specified subnet can access the database.
   - Perform a penetration test or network scan to confirm that unauthorized users are blocked.

6. **Ongoing Monitoring and Alerts:**
   - Set up monitoring for this policy by navigating to **Monitor > Logs > Traffic** and filtering based on the new rule.
   - Configure alerts for suspicious or excessive access attempts.

---

### **Cisco Firewall (ASA) Review and Implementation Example**

#### **Scenario:**  
You are securing remote access to your corporate VPN using Cisco ASA (Adaptive Security Appliance) to ensure only specific IP addresses and user groups can access sensitive resources.

#### **Steps to Implement Firewall Policies:**

1. **Log in to the Cisco ASA Console or ASDM (GUI):**
   - Use SSH for CLI access or the Cisco Adaptive Security Device Manager (ASDM) for the graphical interface.
   
2. **Review Existing Access Lists (ACLs):**
   - In CLI, use the command:
     ```
     show access-list
     ```
     - This will display the current Access Control Lists (ACLs) configured on the firewall.
     - Review the rules for any overly permissive ACLs (e.g., allowing all traffic from the internet).

3. **Create a New ACL (Example: Restrict VPN Traffic to Specific Subnet):**
   - Define an ACL that restricts VPN access to a specific internal subnet.
   - For example, to allow VPN users to access only the internal subnet `10.10.10.0/24`, use the following command in the CLI:
     ``` 
     access-list VPN_ACL extended permit ip 10.10.10.0 255.255.255.0 any 
     ```
     This permits access to the internal subnet and blocks other resources.

4. **Apply the ACL to the VPN Interface:**
   - Apply the newly created ACL to the interface handling VPN traffic:
     ```
     access-group VPN_ACL in interface outside
     ```
     This applies the rule to the "outside" interface, which handles incoming VPN traffic.

5. **Configure NAT Exemptions for VPN Traffic:**
   - If the VPN traffic is allowed to bypass NAT, you must configure NAT exemption rules:
     ```
     nat (inside,outside) source static obj-internal obj-internal destination static obj-vpn obj-vpn no-proxy-arp route-lookup
     ```
     This prevents VPN traffic from being translated.

6. **Enable Logging and Alerts:**
   - Enable logging to monitor traffic:
     ```
     logging enable
     logging trap informational
     logging host inside 192.168.1.100
     ```
     This sends log information to the internal syslog server for analysis.

7. **Test the VPN Access Restrictions:**
   - Attempt to connect to the VPN from an authorized IP address and check that you can access only the allowed resources.
   - Try connecting from a restricted IP to confirm that access is denied.

8. **Monitor and Review:**
   - Use the `show logging` or ASDM monitoring tools to track attempts to access the VPN.
   - Review logs for unusual activity, such as failed login attempts or connections from untrusted IPs.

---

### **Key Points in Implementation:**
- **Palo Alto Firewalls** allow granular control over security policies, including application-level filtering and deep visibility into traffic.
- **Cisco ASA Firewalls** focus on network-layer access control using ACLs, VPN policies, and NAT configurations, and are often used for edge security.

Both firewalls require regular reviews and monitoring to ensure policies stay up-to-date and security risks are minimized.
