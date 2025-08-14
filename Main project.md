The project is deployed on a ubuntu VM, hosted on GNS3, using MikroTik's devices, where each component is a separate docker container.
The scenario simulates an enterprise network spread across 3 locations, with a dedicated server farm hosted in one the sites.

## **MAIN OBJECTIVE**

To design and implement a secure, multi-site network that enables seamless interconnectivity through firewalls and routing, safeguards communication with VPNs and encryption, and enhances resilience against threats through intrusion detection and well-defined access controls.

To begin with 
This image provides a basic setup of the infrastructure of the network.
<img width="1216" height="751" alt="image" src="https://github.com/user-attachments/assets/b33b9aa6-176e-4940-845e-22ea4a1131a5" />

The centrally depicted site hosts the server farm which is divided into two subnets as per the requirements. 
With SSH, Web and SMTP (mail) servers on a subnet and CA (certificate authority) and DNS located on another.
---------------------------------------------------------------------------------------------------------------------------------------------------------
The Certificate Authority (CA) was configured using OpenSSL to generate a self-signed root certificate. This root certificate was then used to sign the certificates of other servers, ensuring that all devices trusted them by installing the CA’s certificate in their configuration.
<img width="675" height="204" alt="image" src="https://github.com/user-attachments/assets/498589c7-4c8b-4d75-8af9-56cb16d74276" />

The rest of the servers run the following services -
Web server - apache2
DNS - Bind9
SMTP - Postfix
SSH - OpenSSL

The Web server Hosts a basic webpage which supports TLS ensuring encryption for traffic in transit.
<img width="664" height="486" alt="image" src="https://github.com/user-attachments/assets/fc7ace44-101c-4eda-8a14-622f3e1d89a7" />

<img width="667" height="465" alt="image" src="https://github.com/user-attachments/assets/d4b8d818-e0ad-451a-a3f5-8f9a363b840a" />

On the web server, the TLS key, CSR, and signed certificate were generated and configured for HTTPS. On the SMTP server (Postfix), TLS was enabled by specifying the CA-signed certificate and key in the configuration file (main.cf), allowing secure connections via STARTTLS. Because all clients were provisioned with the CA’s root certificate, they could successfully verify and trust these services.
After configuring TLS, SSH was configured for all the cient devices on each site. 
<img width="778" height="548" alt="image" src="https://github.com/user-attachments/assets/a80e31f3-0938-4f1b-8eef-12f3e2c1d4ed" />

<img width="1039" height="376" alt="image" src="https://github.com/user-attachments/assets/a28c4158-a1aa-41cf-85f7-d51e8cf28dd8" />

