The project is deployed on a ubuntu VM, hosted on GNS3, using MikroTik's devices, where each component is a separate docker container.
The scenario simulates an enterprise network spread across 3 locations, with a dedicated server farm hosted in one the sites.

## **MAIN OBJECTIVE**

To design and implement a secure, multi-site network that enables seamless interconnectivity through firewalls and routing, safeguards communication with VPNs and encryption, and enhances resilience against threats through intrusion detection and well-defined access controls.

To begin with 
This image provides a basic setup of the infrastructure of the network.

<img width="1057" height="744" alt="image" src="https://github.com/user-attachments/assets/3cf437cb-4137-457e-8650-821f57a87f35" />


The centrally depicted site hosts the server farm which is divided into two subnets as per the requirements. 
With SSH, Web and SMTP (mail) servers on a subnet and CA (certificate authority) and DNS located on another.

## **Server Setup**
The Certificate Authority (CA) was configured using OpenSSL to generate a self-signed root certificate. This root certificate was then used to sign the certificates of other servers, ensuring that all devices trusted them by installing the CA’s certificate in their configuration.

<img width="675" height="204" alt="image" src="https://github.com/user-attachments/assets/498589c7-4c8b-4d75-8af9-56cb16d74276" />

The rest of the servers run the following services -  
**Web server - apache2**  
**DNS - Bind9**  
**SMTP - Postfix**  
**SSH - OpenSSL**  

The Web server Hosts a basic webpage which supports TLS ensuring encryption for traffic in transit.  
**Webpage**

<img width="664" height="486" alt="image" src="https://github.com/user-attachments/assets/fc7ace44-101c-4eda-8a14-622f3e1d89a7" />

**Encrypted TLS traffic**

<img width="667" height="465" alt="image" src="https://github.com/user-attachments/assets/d4b8d818-e0ad-451a-a3f5-8f9a363b840a" />

On the web server, the TLS key, CSR, and signed certificate were generated and configured for HTTPS. On the SMTP server (Postfix), TLS was enabled by specifying the CA-signed certificate and key in the configuration file (main.cf), allowing secure connections via STARTTLS. Because all clients were provisioned with the CA’s root certificate, they could successfully verify and trust these services.

## **SSH Configuration**
SSH was configured for all the cient devices on each site using OpenSSH as seen in the images below. 

<img width="778" height="548" alt="image" src="https://github.com/user-attachments/assets/a80e31f3-0938-4f1b-8eef-12f3e2c1d4ed" />

<img width="1039" height="376" alt="image" src="https://github.com/user-attachments/assets/a28c4158-a1aa-41cf-85f7-d51e8cf28dd8" />

## **BGP Protocol** 

These three sites are considered as three Autonomous systems which are connected to each other via Border Gateway Protocol(BGP) as seen below.

<img width="936" height="356" alt="image" src="https://github.com/user-attachments/assets/d56362d5-2e2d-4813-bcbb-a6b136ffb56f" />

## **IPSec rules (VPN tunnelling)**

All the data in transit of these systems in protected by encryption using IPSec, basically creating a VPN tunnel between the sites.
The traffic is encapsulated in secure tunnel where no one from outside can sniff the traffic, and its encapsulated using ESP protocol.
As seen in the image below custom IPSec rules were configured for each site.

<img width="1387" height="537" alt="image" src="https://github.com/user-attachments/assets/f0b0a637-63c7-4bd8-99ae-96525f891336" />

<img width="825" height="592" alt="image" src="https://github.com/user-attachments/assets/d17207c0-f1e8-4abf-affb-3b51a4e03014" />

## **Firewall** 

Custom Firewall rules configured on each router for selective access to each site as per reqiurements.

<img width="1228" height="895" alt="image" src="https://github.com/user-attachments/assets/d4272c4e-4a40-4ffe-b587-a81a00bfde8a" />

## **IDS**

Open source IDS Snort was configured on the server farm with custom alerts, to Identify port scanning attempts and any other suspicious traffic by external attackers.

<img width="1298" height="257" alt="image" src="https://github.com/user-attachments/assets/95cd297d-7cae-4b70-8422-668a96f8a126" />
