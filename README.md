# 🔐 Cybersecurity Implementation Project: Web Server Hardening & WAF Integration

**Author:** Serge Benit  
**Role:** Cybersecurity Researcher

This project demonstrates secure configuration and hardening of web servers using **Kali Linux**, including:
- Configuring virtual interfaces
- Setting up **Apache** and **NGINX** web servers
- Enabling **HTTPS** with self-signed certificates
- Deploying a **Web Application Firewall (WAF)** using **ModSecurity** and the **OWASP Core Rule Set (CRS)**

---

## 📁 Project Structure
```
portfolio-security/
│
├── apache/
│ ├── modsecurity/
│ └── site.conf
│
├── nginx/
│ ├── modsecurity/
│ └── nginx.conf
│
├── certs/
│ ├── cert.pem
│ └── privkey.pem
│
├── screenshots/
│ └── [Add screenshots here]
│
└── README.md

```

---

## ⚙️ System Configuration Overview

### 1. 🌐 Virtual Interfaces on Kali Linux

Two virtual interfaces were created using:


ifconfig eth0:1 192.168.1.101 up
ifconfig eth0:2 192.168.1.102 up
Domains mapped:
```
portfolio.auca.ac.rw → 192.168.1.101
auca.ac.rw → 192.168.1.102
```
![auca](https://github.com/Sergeb250/WEB-SECURITY/blob/ac6bad1507ce6fa9d7a6e77946ca25cb9ee24b62/SCREEN/Screenshot%202025-05-03%20210228.png?raw=true)
Tested using ping and browser.
2. 🌍 Web Servers Setup
Apache and NGINX were configured to serve websites on the two virtual IPs.
🔸 Apache Virtual Host
```apache<VirtualHost *:443>
    ServerName auca.ac.rw
    DocumentRoot /var/www/auca.ac.rw
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/cert.pem
    SSLCertificateKeyFile /etc/ssl/private/privkey.pem
</VirtualHost>
```
![auca](https://github.com/Sergeb250/WEB-SECURITY/blob/ac6bad1507ce6fa9d7a6e77946ca25cb9ee24b62/SCREEN/Screenshot%202025-05-03%20213309.png?raw=true)
![apache ](https://github.com/Sergeb250/WEB-SECURITY/blob/ac6bad1507ce6fa9d7a6e77946ca25cb9ee24b62/SCREEN/Screenshot%20(67).png?raw=true)

🔹 NGINX Server Block
```
nginxserver {
    listen 443 ssl;
    server_name portfolio.auca.ac.rw;
    root /var/www/portfolio.auca.ac.rw;
    ssl_certificate /etc/ssl/certs/cert.pem;
    ssl_certificate_key /etc/ssl/private/privkey.pem;
}
```
3. 🔐 HTTPS with Self-Signed Certificates
Certificates generated using:
```
openssl req -x509 -nodes -days 365 \
  -newkey rsa:2048 \
  -keyout /etc/ssl/private/privkey.pem \
  -out /etc/ssl/certs/cert.pem
```
![WAF Screenshot](https://github.com/Sergeb250/WEB-SECURITY/blob/ac6bad1507ce6fa9d7a6e77946ca25cb9ee24b62/SCREEN/Screenshot%202025-05-03%20213228.png?raw=true)

Deployed on both Apache and NGINX.
🛡️ Web Application Firewall (WAF)
✅ ModSecurity Installation
Installed using:
bashapt install libapache2-mod-security2
a2enmod security2
For NGINX:
bash
```
apt install libnginx-mod-security
Enabled and tested in DetectionOnly mode:
apacheSecRuleEngine DetectionOnly
```
🛠️ ModSecurity Configuration
```
Default config at /etc/modsecurity/modsecurity.conf
Unicode map: /etc/modsecurity/unicode.mapping
Rule engine logs requests without blocking.
```
🧰 OWASP Core Rule Set (CRS)
Downloaded and integrated:
```bash
cd /usr/share
git clone https://github.com/coreruleset/coreruleset.git modsecurity-crs
cd modsecurity-crs
cp crs-setup.conf.example crs-setup.conf
Included in Apache config:
apacheInclude /usr/share/modsecurity-crs/crs-setup.conf
Include /usr/share/modsecurity-crs/rules/*.conf
Included in NGINX config:
nginxmodsecurity on;
modsecurity_rules_file /etc/nginx/modsec/main.conf;
```


![mod ](
https://github.com/Sergeb250/WEB-SECURITY/blob/ac6bad1507ce6fa9d7a6e77946ca25cb9ee24b62/SCREEN/Screenshot%202025-05-19%20115038.png?raw=true)

![out](  https://github.com/Sergeb250/WEB-SECURITY/blob/ac6bad1507ce6fa9d7a6e77946ca25cb9ee24b62/SCREEN/Screenshot%202025-05-19%20115647.png?raw=true)
#ADDITIONAL SET UP FIREWALL



![fire](https://github.com/Sergeb250/WEB-SECURITY/blob/ac6bad1507ce6fa9d7a6e77946ca25cb9ee24b62/SCREEN/Screenshot%202025-05-16%20133200.png?raw=true)
Apache2, NGINX
ModSecurity v3
OWASP CRS
OpenSSL
curl, tcpdump, iptables
Kali Linux base system

📌 Future Improvements

Enable blocking mode in ModSecurity
Automate deployment with Ansible or Bash scripts
Add GeoIP blocking, Rate Limiting, DoS Mitigation
Configure audit logging and alerts



🤝 Contributions
Feel free to fork the repo and submit PRs to improve configuration, add automation, or enhance documentation.

🙋 Contact
Serge Benit
Cybersecurity Researcher
📧 hacksergeb@gmail.com
📍 Kigali, Rwanda
