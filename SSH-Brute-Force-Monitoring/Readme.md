# Simulating SSH Brute-Force Attacks and Log Forwarding to Splunk

## Project Overview
This project demonstrates the simulation of SSH brute-force attacks on an Ubuntu server, with logs being forwarded to Splunk for real-time monitoring and analysis. The environment consists of an Ubuntu server as the target, a Kali Linux machine performing the brute-force attack, and a Splunk instance running on a Windows host for log collection and visualization. This setup highlights both attack simulation and detection through Splunk log analysis.

---

## Features
1. Simulate SSH brute-force attacks using Hydra.
2. Forward authentication logs from Ubuntu server to Splunk.
3. Monitor failed and successful login attempts in Splunk.
4. Gain insights using Splunk queries.

---

## Repository Structure
- **Screenshots**: Contains all relevant screenshots of the project steps.
- **`writeup.md`**: A detailed write-up explaining the entire project setup with screenshots.

---

## Tools Used
1. **VMware**: Virtualization platform used to set up Ubuntu and Kali VMs.
2. **Ubuntu Server**: Target machine for SSH brute-force attack.
3. **Kali Linux**: Used for performing brute-force attacks with Hydra.
4. **Splunk**: Log collection and analysis platform.
5. **Hydra**: Brute-forcing tool for attacking SSH.
6. **Rsyslog**: Log forwarding tool on Ubuntu.

---

## Steps to Reproduce

### 1. **Configure Ubuntu Server**
1. Install and configure OpenSSH server.
2. Modify `/etc/ssh/sshd_config` to allow root login and enable verbose logging:
   ```plaintext
   PermitRootLogin yes
   LogLevel VERBOSE
   ```
3. Restart SSH service:
   ```bash
   sudo systemctl restart sshd
   ```

### 2. **Set Up Log Forwarding**
1. Edit `/etc/rsyslog.d/50-default.conf` to forward authentication logs to Splunk:
   ```plaintext
   auth.*    @@<Splunk_IP>:514
   ```
2. Restart Rsyslog:
   ```bash
   sudo systemctl restart rsyslog
   ```

### 3. **Configure Splunk**
1. Add a TCP listener on port 514 in Splunk.
2. Set the source type to `syslog`.
3. Confirm that logs are being ingested from the Ubuntu server.

### 4. **Perform Brute-Force Attack**
1. Use `arp-scan` on Kali to find the Ubuntu server's IP.
   ```bash
   sudo arp-scan --localnet
   ```
2. Run Hydra to brute-force SSH credentials:
   ```bash
   hydra -l sam47 -P /path/to/wordlist.txt ssh://<Ubuntu_IP> -t 4
   ```

### 5. **Query Logs in Splunk**
1. Query failed login attempts:
   ```spl
   index="ubuntu-syslog" sourcetype="syslog" "Failed password"
   ```
2. Query successful logins:
   ```spl
   index="ubuntu-syslog" sourcetype="syslog" "Accepted password"
   ```

---

## Key Insights
- Captured detailed logs of SSH brute-force attempts.
- Identified attacker IPs, targeted usernames, and patterns of failed and successful login attempts.

---

## Screenshots
Refer to the **`writeup.md`** file for all screenshots documenting the project steps.

---

## Future Enhancements
1. Implement automated IP blocking for repeated failed login attempts.
2. Integrate threat intelligence feeds into Splunk to identify malicious IPs.
3. Use machine learning to detect anomalies in login behavior.

---
