
# CONFIGURE OPEN SSH AND SCP IN LINUX CLIENT AND SERVER USING KERBEROS AUTHENTICATION

The aim of this project is to establish a robust Linux client-server environment that requires Kerberos authentication before granting SSH access and enabling SCP file transfers. This encompasses setting up the Kerberos server, distributing and managing Kerberos tickets, configuring OpenSSH to utilize Kerberos for authentication, and guaranteeing that SCP transmissions also utilize the Kerberos authentication.

## Server Configuration

### Setting up FQDN ‘krb5.ac.com’

Install DNS server
```bash
sudo apt install bind9 bind9utils dnsutils
```
Define hosts entry for new domain
```bash
sudo nano /etc/hosts
```
Create backup
```bash
sudo cp /etc/bind/db.127 /etc/bind/krb5.ac.com 
```
Create new file for domain
```bash
sudo nano /etc/bind/krb5.ac.com
```
image 1 

Define zone scope for domain
```bash
sudo nano /etc/bind/named.conf.local
```
Define DNS nameserver
```bash
sudo nano /etc/resolv.conf 
```
image 2![Screenshot 2024-02-21 151016](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/595fe9fc-0333-4b39-95c7-384a73076f20)

