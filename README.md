# 🔐 TLS Lab Solutions - Network Security (Kali Linux)  

This repository contains my solutions and notes for the **TLS Lab** from my **Network Security Course**. The exercises were performed in **Kali Linux** using **OpenSSL, Wireshark, and Docker**.  

## 📌 **Lab Overview**  
In this lab, we explored **TLS certificates, encryption, and attacks** using various tools. The main topics covered:  
✅ Generating RSA keys and certificate signing requests (CSR).  
✅ Creating and inspecting self-signed **X.509 certificates**.  
✅ Setting up a **TLS-secured server** using OpenSSL.  
✅ Analyzing **TLS traffic** using **Wireshark**.  
✅ Performing a **Man-in-the-Middle (MitM) attack** using ARP poisoning.  

---

## 🚀 **Solutions for Each Exercise**  

### 🔹 **Exercise 1: Generating a Private Key & CSR**  
1️⃣ Generate an **RSA Private Key (1024-bit)**:  
```sh
openssl genrsa -out mykey.pem 1024
```  
2️⃣ Create a **Certificate Signing Request (CSR)**:  
```sh
openssl req -new -key mykey.pem -out mycsr.csr
```  
❓ **Issues**:  
- **Weak Key Length (1024-bit RSA)** – should use **4096-bit** for better security.  
- **Sharing the Private Key** exposes all encrypted data to attackers.  

---

### 🔹 **Exercise 2: Self-Signed Certificate with OpenSSL**  
1️⃣ Sign the CSR and generate a **self-signed certificate**:  
```sh
openssl x509 -req -days 365 -in mycsr.csr -signkey mykey.pem -out mycert.crt
```  
2️⃣ Inspect the certificate details:  
```sh
openssl x509 -in mycert.crt -text -noout
```  

---

### 🔹 **Exercise 3: Setting Up a TLS-Enabled Server**  
1️⃣ Start a **TLS Server** on port `1443` with OpenSSL:  
```sh
openssl s_server -accept 1443 -cert mycert.crt -key mykey.pem
```  
2️⃣ Generate a **stronger RSA key (4096-bit)** and restart the server:  
```sh
openssl genrsa -out strongkey.pem 4096
openssl req -new -key strongkey.pem -out strongcsr.csr
openssl x509 -req -days 365 -in strongcsr.csr -signkey strongkey.pem -out strongcert.crt
openssl s_server -accept 1443 -cert strongcert.crt -key strongkey.pem
```  
3️⃣ Open a **browser** and visit:  
```
https://localhost:1443
```  

---

### 🔹 **Exercise 4: Analyzing TLS Traffic with Wireshark**  
1️⃣ Start **Wireshark** with root privileges:  
```sh
sudo -E wireshark
```  
2️⃣ Capture traffic on the **loopback (lo) interface**.  
3️⃣ Visit `https://localhost:1443` and reload the page.  
4️⃣ Analyze the **TLS handshake, encryption, and decryption process**.  

---

### 🔹 **Exercise 5: Man-in-the-Middle (MitM) Attack via ARP Poisoning**  
📌 **Setup**:  
- **Alice** (User) - Browses the web  
- **Bob** (Web Server) - Hosts a website  
- **Eve** (Attacker) - Performs MitM  

1️⃣ **Run the MitM Docker Containers**:  
```sh
docker compose up --build -d
```  
2️⃣ **Connect to Containers**:  
```sh
docker exec -it mitm_alice /bin/bash
docker exec -it mitm_bob /bin/bash
docker exec -it mitm_eve /bin/bash
```  
3️⃣ **Perform ARP Spoofing as Eve**:  
```sh
arpspoof -i eth0 -t <Alice_IP> -r <Bob_IP>
arpspoof -i eth0 -t <Bob_IP> -r <Alice_IP>
```  
4️⃣ **Monitor Traffic with mitmproxy**:  
```sh
source .pyenv/bin/activate
pip3 install mitmproxy
mitmproxy -T --host
```  

---

## 📂 **Folder Structure**  
```
📦 TLS-Lab-Solutions
 ┣ 📜 README.md  (This file)
 ┣ 📂 solutions  (Contains scripts & configurations)
 ┃ ┣ 📜 exercise1_key_csr.sh
 ┃ ┣ 📜 exercise2_certificate.sh
 ┃ ┣ 📜 exercise3_tls_server.sh
 ┃ ┣ 📜 exercise4_wireshark_notes.txt
 ┃ ┗ 📜 exercise5_mitm_attack.sh
 ┗ 📂 screenshots  (Screenshots of Wireshark and TLS setup)
```  

---

## 🛠 **Tools Used**  
✅ **OpenSSL** - Generating RSA keys & TLS certificates  
✅ **Wireshark** - Capturing & analyzing TLS traffic  
✅ **Docker** - Running MitM attack containers  
✅ **arpspoof** - ARP poisoning for MitM attacks  
✅ **mitmproxy** - Intercepting HTTPS traffic  

---

## 🔥 **Future Improvements**  
🔹 Automate TLS setup using scripts  
🔹 Implement **HSTS & Certificate Pinning** for security  
🔹 Experiment with **TLS 1.3 & newer cipher suites**  

---

## 💡 **Conclusion**  
This lab covered **TLS security, traffic analysis, and MitM attacks**. By using OpenSSL, Wireshark, and Docker, we gained hands-on experience with **certificate management and real-world cyber threats**.  

📢 **Contributions Welcome!** If you have improvements or additional insights, feel free to submit a PR.  

🚀 **Author**: *[Your Name]* | 🔗 *[Your GitHub Link]*  

