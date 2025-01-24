# SSH Brute-Force Monitoring and Detection with Splunk

This document provides a detailed write-up on how to set up an environment for monitoring SSH brute-force attacks using Splunk, along with step-by-step explanations and screenshots.

---

## **1. Modifying Rsyslog Configuration on Ubuntu Server**
To forward SSH logs to Splunk, I edited the `/etc/rsyslog.d/50-default.conf` file.

### Steps:
1. Open the configuration file:
   ```bash
   sudo nano /etc/rsyslog.d/50-default.conf
   ```
2. Add the following line to forward authentication logs to Splunk on port 514 (Default port for listening to TCP data):
   ```plaintext
   auth.*    @@<Splunk_IP>:514
   ```
3. Restart the Rsyslog service to apply the changes:
   ```bash
   sudo systemctl restart rsyslog
   ```

**Screenshot:**

![Changing 50-default Configuration](https://github.com/user-attachments/assets/3caf5ce7-0b2b-40ec-8741-cea224f0212c)

---

## **2. Setting Up Splunk Listener**
The Splunk instance was configured to listen on port 514 to collect syslog data from the Ubuntu server.

### Steps:
1. Navigate to **Settings > Data Inputs > TCP/UDP** in Splunk.
2. Configure a new listener for port 514.
3. Assign the source type as `syslog` and ensure the listener is active.

**Screenshot:**

![Setting up Splunk Listener](https://github.com/user-attachments/assets/49043754-d4f3-40d4-92af-f22678f5df8d)

---

## **3. Modifying SSH Configuration on Ubuntu**
To capture detailed SSH logs and allow root login for testing purposes, the SSH configuration was modified.

### Steps:
1. Edit the SSH configuration file:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
2. Set the following parameters:
   ```plaintext
   PermitRootLogin yes
   LogLevel VERBOSE
   ```
3. Restart the SSH service:
   ```bash
   sudo systemctl restart sshd
   ```

**Screenshot:**

![SSHD Configuration for Splunk Logging](https://github.com/user-attachments/assets/c86b2d7a-a692-4b46-80af-ddcb5a47fb2c)

---

## **4. Identifying Ubuntu Server IP from Kali Machine**
To perform the brute-force attack, the Ubuntu server's IP address was identified using `arp-scan`.

### Command:
```bash
sudo arp-scan --localnet
```

**Screenshot:**

![Identifying Ubuntu IP](https://github.com/user-attachments/assets/bc3f79b7-e7d2-4450-a1b7-9b9e40500721)

---

## **5. Starting Rsyslog on Ubuntu**
The Rsyslog service was verified to ensure logs were being forwarded correctly.

### Command:
```bash
sudo systemctl start rsyslog
sudo systemctl status rsyslog
```

**Screenshot:**

![Starting Rsyslog on Ubuntu](https://github.com/user-attachments/assets/bd522219-1194-44c4-8fd2-224c7ced9e7d)

---

## **6. Performing Brute-Force Attack Using Hydra**
Hydra was used on Kali to brute-force SSH credentials for the Ubuntu server.

### Command:
```bash
hydra -l sam47 -P /path/to/wordlist.txt ssh://<Ubuntu_IP> -t 4
```

**Screenshot (Attack in Progress):**

![Hydra Brute-Force Attempt](https://github.com/user-attachments/assets/c5934f1f-63d4-46da-8410-430698cb9bf2)

**Screenshot (Success):**

![Hydra Brute-Force Success](https://github.com/user-attachments/assets/11986f44-d1bf-4fa4-afbe-fbd413c4ed65)

---

## **7. Monitoring Logs in Splunk**
SSH logs from the Ubuntu server were ingested into Splunk, and relevant events were queried and visualized.

### Steps:
1. Query for failed login attempts:
   ```spl
   index="ubuntu-syslog" sourcetype="syslog" "Failed password"
   ```
2. Query for successful login attempts:
   ```spl
   index="ubuntu-syslog" sourcetype="syslog" "Accepted password"
   ```
3. Filter logs by source IP (attacker's IP):
   ```spl
   index="ubuntu-syslog" rhost=<Kali_IP>
   ```

**Screenshot (Filter for Accepted Passwords):**

![Splunk Query for Accepted Passwords](https://github.com/user-attachments/assets/06cfb7f4-bd07-485a-b898-ea1bfa7db671)

**Screenshot (Filter for attacker's IP):**

![Applying filter to search for attacker IP](https://github.com/user-attachments/assets/f3abfb72-4b84-43ae-9dd1-d0bf54dac8e0)

---

## **8. Gaining Reverse Shell Access**
Following the successful brute-force attack, a reverse shell was initiated to demonstrate the attacker gaining access to the Ubuntu server.

**Screenshot:**

![Reverse Shell Access](https://github.com/user-attachments/assets/887ae5d8-f0e0-42eb-9de5-374d76005bd5)

---

This write-up includes all steps and corresponding screenshots to demonstrate the entire workflow of monitoring and detecting SSH brute-force attacks using Splunk.

