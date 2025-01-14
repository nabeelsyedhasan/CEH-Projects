# Nikto Scan Writeup

## Overview
Nikto is an open-source web server scanner that tests for various security vulnerabilities, misconfigurations, and outdated software on a web server. This scan was performed on the **Damn Vulnerable Web Application (DVWA)** to identify potential risks and insecure configurations.

---

## **Steps Taken**

### 1. Running the Nikto Scan
- Started the scan against the DVWA instance running on `http://127.0.0.1`:
  ```bash
  nikto -h http://127.0.0.1
  ```

- The scan checked for common vulnerabilities, insecure server settings, and outdated components.

### 2. Results
The Nikto scan identified the following issues:

1. **Outdated Apache Version:**
   - The server was running Apache 2.4.25, which has known vulnerabilities.

2. **Missing Security Headers:**
   - `X-Content-Type-Options` header was not set, increasing the risk of MIME type-based attacks.
   - `X-Frame-Options` header was missing, making the application vulnerable to clickjacking attacks.

3. **Exposed Files and Directories:**
   - Accessible sensitive files like `/config/` and `/docs/` were found.

4. **Cookies without Security Attributes:**
   - Cookies were created without the `HttpOnly` and `Secure` flags, exposing them to theft via client-side attacks.

Refer to the screenshot:
- ![Nikto-Scan-Results](https://github.com/user-attachments/assets/2bcc1f88-b4df-4e67-b920-afa6abade1f2)
  <p align="center">Nikto Scan Results</p>

---

## **Analysis**
- **Impact:**
  - **Outdated Software:**
    - Vulnerabilities in older versions of Apache can lead to unauthorized access or Denial of Service (DoS).
  - **Missing Security Headers:**
    - Lack of headers increases the risk of Cross-Site Scripting (XSS) and clickjacking attacks.
  - **Exposed Files and Directories:**
    - Directory indexing allows attackers to gather information for further exploitation.
  - **Insecure Cookies:**
    - Cookies without proper flags are more susceptible to interception and misuse.

- **Exploitability:**
  - The identified vulnerabilities are easy to exploit with basic tools and scripts.

---

## **Mitigation Strategies**
1. **Update Software:**
   - Upgrade Apache to the latest stable version to address known vulnerabilities.

2. **Implement Security Headers:**
   - Add the following headers to the web server configuration:
     ```apache
     Header set X-Content-Type-Options "nosniff"
     Header set X-Frame-Options "DENY"
     ```

3. **Restrict Directory Access:**
   - Disable directory indexing to prevent unauthorized access to directories.
     Example (Apache):
     ```apache
     Options -Indexes
     ```

4. **Secure Cookies:**
   - Configure cookies with the `HttpOnly` and `Secure` attributes to enhance their security.

5. **Regular Scanning:**
   - Schedule periodic scans using tools like Nikto to proactively identify and address vulnerabilities.

---

## Conclusion
The Nikto scan effectively highlighted several critical vulnerabilities and misconfigurations in the DVWA instance. Addressing these issues will significantly improve the security posture of the application and its underlying server.

