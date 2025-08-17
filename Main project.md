
## **MAIN OBJECTIVE**

Design and implement a secure, multi-site enterprise network using GNS3, MikroTik routers, and Ubuntu VMs. The network was structured to:  
Enable inter-site connectivity through firewalls and routing  
Safeguard communication with VPN/IPsec and encryption  
Enhance resilience against threats using Intrusion Detection (Snort)  
Provide core enterprise services (Web, DNS, SMTP, SSH, Certificate Authority)  

## **Network Architecture**

The simulation was deployed in a GNS3 environment with MikroTik devices acting as edge firewalls/routers, and Ubuntu VMs hosting services.  

*Figure 1: High-level topology of the simulated enterprise network.*  

<img width="1057" height="744" alt="image" src="https://github.com/user-attachments/assets/3cf437cb-4137-457e-8650-821f57a87f35" />


The centrally depicted site hosts the server farm which is divided into two subnets as per the requirements.   
With SSH, Web and SMTP (mail) servers on a subnet and CA (certificate authority) and DNS located on another.  

## **Server Setup**
The Certificate Authority (CA) was configured using OpenSSL to generate a self-signed root certificate. This root certificate was then used to sign the certificates of other servers, ensuring that all devices trusted them by installing the CA’s certificate in their configuration.  

*Figure 2: self-signed certificates and files of root CA*  
<img width="675" height="204" alt="image" src="https://github.com/user-attachments/assets/498589c7-4c8b-4d75-8af9-56cb16d74276" />

**Core Services Implemented**   
 
Web Server: Apache2  
DNS Server: Bind9  
Mail Server: Postfix (SMTP)  
SSH Server: OpenSSH  
Certificate Authority (CA): For issuing SSL/TLS certificates   

The Web server Hosts a basic webpage which supports TLS ensuring encryption for traffic in transit.  

***Figure 3: Test Webpage***    
  
<img width="664" height="486" alt="image" src="https://github.com/user-attachments/assets/fc7ace44-101c-4eda-8a14-622f3e1d89a7" />

***Figure 4: Encrypted TLS traffic***  

<img width="667" height="465" alt="image" src="https://github.com/user-attachments/assets/d4b8d818-e0ad-451a-a3f5-8f9a363b840a" />

On the web server, the TLS key, CSR, and signed certificate were generated and configured for HTTPS. On the SMTP server (Postfix), TLS was enabled by specifying the CA-signed certificate and key in the configuration file (main.cf), allowing secure connections via STARTTLS.  
Because all clients were provisioned with the CA’s root certificate, they could successfully verify and trust these services.

## **SSH Configuration**
SSH was configured for all the cient devices on each site using OpenSSH as seen in the images below. 

*Figure 5*  
<img width="778" height="548" alt="image" src="https://github.com/user-attachments/assets/a80e31f3-0938-4f1b-8eef-12f3e2c1d4ed" />

*Figure 6: This image shows the packet capture of the SSH traffic*  
<img width="1039" height="376" alt="image" src="https://github.com/user-attachments/assets/a28c4158-a1aa-41cf-85f7-d51e8cf28dd8" />

## **BGP Protocol** 

These three sites are considered as three Autonomous systems which are connected to each other via Border Gateway Protocol(BGP) as seen below.  
*Figure 7*  

<img width="936" height="356" alt="image" src="https://github.com/user-attachments/assets/d56362d5-2e2d-4813-bcbb-a6b136ffb56f" />

## **IPSec rules (VPN tunnelling)**

All the data in transit of these systems in protected by encryption using IPSec, basically creating a VPN tunnel between the sites.
The traffic is encapsulated in secure tunnel where no one from outside can sniff the traffic, and its encapsulated using ESP protocol.

*Figure 8: VPN setup using IPSec protocols*  
<img width="1387" height="537" alt="image" src="https://github.com/user-attachments/assets/f0b0a637-63c7-4bd8-99ae-96525f891336" />

*Figure 9: Encapsulated Packets in packet capture*  
<img width="825" height="592" alt="image" src="https://github.com/user-attachments/assets/d17207c0-f1e8-4abf-affb-3b51a4e03014" />

## **Firewall (MikroTik)** 

Implemented rules to allow only necessary traffic (e.g., HTTP/HTTPS, DNS, SMTP).  
Blocked external recursive DNS queries and restricted management ports.  

*Figure 10: Custom Firewall rules (similar rules applied on firewalls at remaining sites as well)*  
<img width="1228" height="895" alt="image" src="https://github.com/user-attachments/assets/d4272c4e-4a40-4ffe-b587-a81a00bfde8a" />

## **IDS**

Deployed Snort IDS to monitor mirrored traffic from core services.  
Custom rules created to detect:  
TCP SYN port scans  
SYN flood DoS attacks  

*Figure 11: Custom Snort rules*  

<img width="1298" height="257" alt="image" src="https://github.com/user-attachments/assets/95cd297d-7cae-4b70-8422-668a96f8a126" />


## **Outcomes & Key Learnings**

Successfully demonstrated end-to-end secure network design.  
Showed how Snort IDS rules detect scanning & flooding.  
Learned practical deployment of IPsec tunnels between sites.  
Understood firewall hardening on MikroTik for enterprise scenarios.   

## *Disclaimer: This project was completed in an academic lab environment for educational purposes only. All screenshots and outputs are from a controlled simulation, not from production systems.*
