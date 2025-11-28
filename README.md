<img width="1017" height="296" alt="image" src="https://github.com/user-attachments/assets/2b1fa369-97d0-4434-ab43-8e88cc28e1a9" />


**Winlogbeat → Logstash → Elasticsearch → Kibana**

This repository contains a complete, fully documented, end-to-end logging pipeline that collects, processes, and visualizes Windows event logs using the ELK Stack.  
The project is designed as a production-ready blueprint for building a centralized log management and monitoring environment.

---

## 📌 Overview

The pipeline integrates multiple Elastic components to create a seamless flow of logs:

- **Winlogbeat** – Collects Windows event logs  
- **Logstash** – Processes, filters, and enriches log data  
- **Elasticsearch** – Stores, indexes, and secures the logs  
- **Kibana** – Provides dashboards, visualization, and analysis  

This repository includes all installation steps, configuration files, verification commands, and security configurations needed to deploy the entire pipeline successfully.

---

## 🔥 Key Features

- Fully secure communication using HTTPS/SSL certificates  
- Dedicated Logstash pipeline for processing Windows logs  
- Automated index templates, dashboards, and ingest pipelines  
- Direct and pipeline-based log ingestion  
  - Winlogbeat → Elasticsearch  
  - Winlogbeat → Logstash → Elasticsearch  
- Validated through end-to-end testing to ensure reliability  
- Suitable for SOC labs, monitoring environments, DFIR tasks, and cybersecurity training

---

## 🎯 Purpose

The goal of this project is to provide a complete, reproducible, and professional implementation of a Windows-to-ELK logging ecosystem.  

It is perfect for:

- Security engineers  
- Blue Team analysts  
- SOC analysts / SIEM engineers  
- Students building cybersecurity portfolios  
- Anyone learning the ELK Stack  

---

## 📦 Repository Contents

- 📘 Full installation guide (Ubuntu + Windows)  
- ⚙️ Winlogbeat configuration  
- 🧩 Logstash pipeline configuration  
- 🔐 Elasticsearch security setup  
- 📊 Kibana enrollment and dashboard setup  
- 🛠️ Verification, testing, and troubleshooting commands  
- 📂 All steps documented in README format  

---

## ✔️ Result

By the end of the setup, you will have a fully functional ELK pipeline capable of:

1. Collecting Windows event logs  
2. Processing logs through Logstash  
3. Indexing logs into Elasticsearch  
4. Visualizing logs in Kibana dashboards

# 📘 The Steps

------------------------------------------------------------------------

## From 1 TO 3:

### **Winlogbeat → Elasticsearch → Kibana**

------------------------------------------------------------------------

# 1. Installing Elasticsearch on Ubuntu (Debian Package Method)

### ➤ Update & Upgrade System Packages

<img width="778" height="395" alt="image" src="https://github.com/user-attachments/assets/705c1aa1-6c4c-44c9-a36a-acc02b32325b" />


To make sure all system packages are up to date before installing
Elasticsearch, preventing dependency or version conflicts.\
**Command:**

    sudo apt update && sudo apt upgrade -y

### ➤ Import Elasticsearch PGP Key

<img width="975" height="49" alt="image" src="https://github.com/user-attachments/assets/f7387028-5007-41b3-b88b-f00fdee15766" />


Ensures authenticity of Elasticsearch packages.\
**Command:**

    wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg

### ➤ Install apt-transport-https

<img width="975" height="442" alt="image" src="https://github.com/user-attachments/assets/41779262-a008-469f-bd42-45eab84d47d5" />


**Command:**

    sudo apt-get install apt-transport-https

### ➤ Add Elasticsearch 9.x Repository

<img width="964" height="100" alt="image" src="https://github.com/user-attachments/assets/264a96d9-63fa-4afd-877e-761494b0bbef" />


**Command:**

    echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/9.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-9.x.list

### ➤ Update Package Index & Install Elasticsearch

<img width="964" height="100" alt="image" src="https://github.com/user-attachments/assets/397d0f3e-a833-46c3-a2bd-fefc37f0cb61" />

<img width="975" height="384" alt="image" src="https://github.com/user-attachments/assets/4ae678cd-be5f-4fba-a06f-8784549050c3" />


    sudo apt-get update && sudo apt-get install elasticsearch -y

### ➤ Reload Systemd & Start Elasticsearch

<img width="975" height="218" alt="image" src="https://github.com/user-attachments/assets/d10f4998-1cd9-4737-85cd-9f48ce0d49ea" />

    sudo systemctl daemon-reload
    sudo systemctl start elasticsearch.service
    sudo systemctl status elasticsearch.service

### ➤ Download Elasticsearch Debian Package


<img width="975" height="184" alt="image" src="https://github.com/user-attachments/assets/2a059a9d-caad-4d81-8ffd-965c0a8166bc" />


    wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-9.2.1-amd64.deb

### ➤ Download SHA-512 Checksum


<img width="975" height="138" alt="image" src="https://github.com/user-attachments/assets/1a114733-3275-42ee-9e12-ff8c4f41ed86" />

    wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-9.2.1-amd64.deb.sha512

### ➤ Verify Package Integrity & Install


<img width="975" height="208" alt="image" src="https://github.com/user-attachments/assets/f38ac5e6-c00f-4d1b-9471-69f88553cf8a" />


<img width="975" height="196" alt="image" src="https://github.com/user-attachments/assets/955dbe95-055e-4f5a-aeda-5cb390810524" />

    shasum -a 512 -c elasticsearch-9.2.1-amd64.deb.sha512
    sudo dpkg -i elasticsearch-9.2.1-amd64.deb

### ➤ Edit elasticsearch.yml


<img width="975" height="401" alt="image" src="https://github.com/user-attachments/assets/06bdc5de-90d9-4b14-bdcb-d02b4c91572f" />

    sudo nano /etc/elasticsearch/elasticsearch.yml

### ➤ Enable & Start Elasticsearch


<img width="975" height="234" alt="image" src="https://github.com/user-attachments/assets/3be786fd-e45a-49a6-ad18-3cda70953f4f" />

    sudo systemctl daemon-reload
    sudo systemctl enable elasticsearch.service
    sudo systemctl start elasticsearch.service
    sudo systemctl restart elasticsearch.service
    sudo systemctl status elasticsearch.service

### ➤ Reset elastic User Password


<img width="975" height="394" alt="image" src="https://github.com/user-attachments/assets/0ee46c81-ed94-424d-8faa-adfcd4c1741d" />

    sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic

### ➤ Install curl


<img width="975" height="345" alt="image" src="https://github.com/user-attachments/assets/5147b08c-0549-402a-bf47-0677271b2600" />


    sudo apt install curl -y

### ➤ Verify Elasticsearch


<img width="975" height="162" alt="image" src="https://github.com/user-attachments/assets/83699be5-2890-4a84-a9c1-14c1f94a5966" />


    curl -k -u elastic:"20LkA230BLj04hLkUmFK" https://localhost:9200

------------------------------------------------------------------------

# 2. Installing Kibana on Ubuntu

### ➤ Import GPG Key, HTTPS Transport, Add Repo


<img width="975" height="369" alt="image" src="https://github.com/user-attachments/assets/e26d045d-66fe-44ad-9718-2137aa202c5f" />


    wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
    sudo apt-get install apt-transport-https -y
    echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/9.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-9.x.list

### ➤ Install Kibana


<img width="974" height="103" alt="image" src="https://github.com/user-attachments/assets/2bbb6881-939e-465e-8bac-0c5ca28fb97b" />


    sudo apt-get update && sudo apt-get install kibana -y

### ➤ Create Enrollment Token


<img width="975" height="331" alt="image" src="https://github.com/user-attachments/assets/5fbba9c7-4c4d-42b9-8a7e-1ae35ce34f24" />


    sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana

### ➤ Start & Check Kibana


<img width="975" height="438" alt="image" src="https://github.com/user-attachments/assets/47f20b44-cb5a-4a1e-bdeb-dc2fe44cef20" />


    sudo systemctl start kibana
    sudo systemctl status kibana

### ➤ Apply Enrollment Token


<img width="975" height="1205" alt="image" src="https://github.com/user-attachments/assets/c7db70ba-377e-4652-b387-c5a4e67f31c1" />


Linked Kibana securely to Elasticsearch.

### ➤ Login with username/password


<img width="975" height="567" alt="image" src="https://github.com/user-attachments/assets/182ce467-4967-4753-a169-ec0b0342acd3" />


Kibana now fully connected.

------------------------------------------------------------------------

# 3. Installing Winlogbeat on Windows 10 VM

### ➤ Download Winlogbeat ZIP


<img width="975" height="812" alt="image" src="https://github.com/user-attachments/assets/3e92c8d0-4836-44f8-9594-a652a97e6e64" />


Obtain required package.

### ➤ Extract ZIP


<img width="960" height="556" alt="image" src="https://github.com/user-attachments/assets/f121bfaa-dd22-42dc-8e43-f0c70479bc70" />


    cd "C:\Users\Khaled\Downloads"
    Expand-Archive ".\winlogbeat-9.2.1-windows-x86_64.zip" -DestinationPath "C:\Program Files\Winlogbeat"

### ➤ Navigate to Installation Directory


<img width="975" height="464" alt="image" src="https://github.com/user-attachments/assets/7120df4d-193a-4026-893a-798a6779e1d2" />


    cd "C:\Program Files\Winlogbeat"
    ls

### ➤ Install Winlogbeat Service


<img width="888" height="136" alt="image" src="https://github.com/user-attachments/assets/e4785c0e-846e-4e78-9f06-745ba7177527" />


    Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
    .\install-service-winlogbeat.ps1

### ➤ Verify Service Status


<img width="975" height="376" alt="image" src="https://github.com/user-attachments/assets/1f1bccc1-dac7-4f7e-95d4-52b42f078479" />


    Get-Service winlogbeat

### ➤ Configure winlogbeat.yml


<img width="980" height="268" alt="image" src="https://github.com/user-attachments/assets/ef7d7def-2ea6-4507-96c9-da483a3e0a32" />


<img width="975" height="690" alt="image" src="https://github.com/user-attachments/assets/742a899e-8231-41d4-a8ba-a15d7cf11223" />


Set Elasticsearch + Kibana output.

### ➤ Validate Configuration


<img width="975" height="625" alt="image" src="https://github.com/user-attachments/assets/562f55e0-d78e-497c-aa7c-6673d0fcd4ca" />

    .\winlogbeat.exe test config -c .\winlogbeat.yml -e

### ➤ Run Setup


<img width="975" height="625" alt="image" src="https://github.com/user-attachments/assets/c0f7c1d1-ae51-499f-b849-bd14866f00ec" />


<img width="975" height="436" alt="image" src="https://github.com/user-attachments/assets/8c2a8d4a-5393-4e5b-9e83-bcc47a62594f" />


Loads dashboards/templates.

### ➤ Start & Verify Service

<img width="971" height="209" alt="image" src="https://github.com/user-attachments/assets/341fb6c5-314a-4083-a7be-bdc1a7e96e3a" />

<img width="972" height="67" alt="image" src="https://github.com/user-attachments/assets/b293b54f-c691-41f9-a95a-bc96f3dad65a" />


<img width="975" height="542" alt="image" src="https://github.com/user-attachments/assets/9f3fb66d-21bf-4454-a105-09d307939096" />


    Start-Service winlogbeat
    Get-Service winlogbeat
    services.msc

### ➤ Verify Index in Kibana

<img width="975" height="604" alt="image" src="https://github.com/user-attachments/assets/c8cae4b8-f4fd-415a-a591-dab5fadaad1b" />


Check winlogbeat-\* index.

------------------------------------------------------------------------

# 4. Building Full Log Pipeline

### **Winlogbeat → Logstash → Elasticsearch → Kibana**

### ➤ Add Elastic APT Repo


<img width="975" height="382" alt="image" src="https://github.com/user-attachments/assets/f183c15a-e93f-4ea0-938a-3041ea4d845e" />


    wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic-keyring.gpg
    sudo apt-get install apt-transport-https
    echo "deb [signed-by=/usr/share/keyrings/elastic-keyring.gpg] https://artifacts.elastic.co/packages/9.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-9.x.list

### ➤ Update APT


<img width="975" height="604" alt="image" src="https://github.com/user-attachments/assets/b82045f3-0cd0-4220-9193-29b46a4b42f8" />


    sudo apt update

### ➤ Install Logstash


<img width="975" height="329" alt="image" src="https://github.com/user-attachments/assets/369f0785-baee-43be-9fb8-f60e6f462836" />


    sudo apt install logstash -y

### ➤ Start & Verify Logstash


<img width="975" height="271" alt="image" src="https://github.com/user-attachments/assets/70b6f29f-62e4-4b70-89f6-e922c34e9d6f" />


    sudo systemctl start logstash
    sudo systemctl status logstash

### ➤ Understand Logstash Config


<img width="975" height="379" alt="image" src="https://github.com/user-attachments/assets/f536132e-693b-4ed5-9052-359e8d8ef777" />


    cat /etc/logstash/logstash-sample.conf

### ➤ Create Pipeline Config

<img width="975" height="53" alt="image" src="https://github.com/user-attachments/assets/47f00364-d1c8-472b-872b-f8a3dc098288" />

<img width="975" height="606" alt="image" src="https://github.com/user-attachments/assets/3b287450-bbe3-42ce-bb5d-a2474fb651b5" />

    cd /etc/logstash/conf.d
    sudo nano winlogbeat.conf

### ➤ Restart & Verify Logstash


<img width="975" height="296" alt="image" src="https://github.com/user-attachments/assets/d1bd8c26-2984-49a4-8574-e421925c49fb" />

    sudo systemctl restart logstash
    sudo systemctl status logstash

### ➤ Fix Logstash Permissions

<img width="975" height="75" alt="image" src="https://github.com/user-attachments/assets/e2d681c8-a674-46bd-9f74-c68430686f29" />

    sudo chown -R logstash:logstash /usr/share/logstash/data
    sudo chmod -R 750 /usr/share/logstash/data

### ➤ Validate Logstash Config

<img width="975" height="574" alt="image" src="https://github.com/user-attachments/assets/39014523-f247-4447-b90e-2067350813da" />


    sudo -u logstash /usr/share/logstash/bin/logstash --path.settings /etc/logstash -t

### ➤ Monitor Logstash Logs

<img width="975" height="442" alt="image" src="https://github.com/user-attachments/assets/c61d43ef-261b-40b3-bc99-2cc45e3f2de1" />


    sudo journalctl -u logstash -f

### ➤ Configure Winlogbeat to Logstash

<img width="975" height="637" alt="image" src="https://github.com/user-attachments/assets/6c29855f-d694-4fc7-8840-c7d3e7f4bfc2" />

<img width="980" height="503" alt="image" src="https://github.com/user-attachments/assets/495ee6ad-51a6-4db1-91b9-8882e579a9b9" />

<img width="975" height="708" alt="image" src="https://github.com/user-attachments/assets/24020c85-2690-4d31-99dd-694c4ff49d3e" />


Setup HTTPS forwarding.

### ➤ Configure Winlogbeat to Elasticsearch

<img width="933" height="604" alt="image" src="https://github.com/user-attachments/assets/ab25387c-5e89-4ebb-a961-41967d97267a" />

Secure connection and indexing.

### ➤ Firewall Rule for Winlogbeat

<img width="975" height="424" alt="image" src="https://github.com/user-attachments/assets/81d63901-b8e5-4381-a7f5-91572dba6264" />

    New-NetFirewallRule -DisplayName "Allow Winlogbeat to Logstash" -Direction Inbound -Protocol TCP -LocalPort 5044 -Action Allow

### ➤ Validate Winlogbeat Before Start

<img width="975" height="666" alt="image" src="https://github.com/user-attachments/assets/d3009d5a-2e7c-4caf-8500-cd59b626ab9f" />

<img width="975" height="729" alt="image" src="https://github.com/user-attachments/assets/2b9637dd-a80e-492a-a4ae-b25bdb98ecdc" />

    Restart-Service winlogbeat
    .\winlogbeat.exe test config -c .\winlogbeat.yml -e

### ➤ Test Winlogbeat Output

<img width="975" height="144" alt="image" src="https://github.com/user-attachments/assets/8973c574-8217-4d99-a027-cc7a128395f4" />

    .\winlogbeat.exe test output -c winlogbeat.yml

### ➤ Verify Service in Windows

<img width="982" height="50" alt="image" src="https://github.com/user-attachments/assets/d2df6695-670f-4947-a231-690e838e3ae1" />


<img width="982" height="50" alt="image" src="https://github.com/user-attachments/assets/98356a75-b2e9-405b-b960-3e6fc83edb06" />


    services.msc

### ➤ Restart & Verify Elasticsearch

    sudo systemctl restart elasticsearch
    sudo systemctl status elasticsearch
    curl -X GET "localhost:9200/?pretty"

### ➤ Verify Winlogbeat Index in Elasticsearch


<img width="975" height="895" alt="image" src="https://github.com/user-attachments/assets/e6fe43c0-3274-4e9f-950c-82d2b89b7c1e" />

<img width="975" height="1035" alt="image" src="https://github.com/user-attachments/assets/b8d269b0-c965-47f9-8544-1d21f744ec46" />


    curl -X GET -u elastic:CalEunKfLKYKBm7-Me62 "https://localhost:9200/.ds-winlogbeat-9.2.1-2025.11.21-000001?pretty" -k

### ➤ Final Elasticsearch Status

<img width="975" height="617" alt="image" src="https://github.com/user-attachments/assets/4b72dd62-01ec-46fe-99d9-dca82ff6bac2" />

<img width="975" height="321" alt="image" src="https://github.com/user-attachments/assets/cfe86f5b-5bf9-404f-af58-565cabb10a1f" />

<img width="975" height="321" alt="image" src="https://github.com/user-attachments/assets/e21d2052-8e5d-4452-ba2d-5c94c35e2b1d" />

<img width="975" height="1131" alt="image" src="https://github.com/user-attachments/assets/b94bf74d-7fa8-4201-956a-2183780741f9" />



    curl -k -u elastic:FYQEm=bUNr-6yUeyfdiC https://192.168.6.130:9200
    curl -k -u elastic:FYQEm=bUNr-6yUeyfdiC "https://192.168.6.130:9200/winlogbeat*/_search?pretty&size=5"

------------------------------------------------------------------------

🎉 **Full Pipeline Documented Successfully!**

