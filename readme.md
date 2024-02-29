
# CONFIGURE OPEN SSH AND SCP IN LINUX CLIENT AND SERVER USING KERBEROS AUTHENTICATION

The aim of this project is to establish a robust Linux client-server environment that requires Kerberos authentication before granting SSH access and enabling SCP file transfers. This encompasses setting up the Kerberos server, distributing and managing Kerberos tickets, configuring OpenSSH to utilize Kerberos for authentication, and guaranteeing that SCP transmissions also utilize the Kerberos authentication.

## Server Configuration

### Step 1 : Setting up FQDN ‘krb5.ac.com’

Install DNS server
```bash
sudo apt install bind9 bind9utils dnsutils
```
Define hosts entry for new domain
```bash
sudo nano /etc/hosts
```
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/2648dd31-22a2-4f2f-ab39-be12bcad992d)

Create backup
```bash
sudo cp /etc/bind/db.127 /etc/bind/krb5.ac.com 
```
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/acd43c54-ccdb-44dc-8e88-d69e74e78b7e)
Create new file for domain
```bash
sudo nano /etc/bind/krb5.ac.com
```

![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/cd1f70c5-0bce-4084-9d15-70c334f7a5d4)


Define zone scope for domain
```bash
sudo nano /etc/bind/named.conf.local
```
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/5de2b12e-b11d-4487-9665-d50526573a69)

Define DNS nameserver
```bash
sudo nano /etc/resolv.conf 
```
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/1b6370b5-0e02-4969-98d7-0ebc2b4cec41)

 Testing
 
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/8cba1d40-514d-42e6-bd29-48449a106241)

### Step 2:Install KDC Server
```bash
sudo apt install krb5-admin-server krb5-config krb5-kdc
```
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/18a8f35a-9054-4fb4-956b-c4d22c44fe27)
Enter the Kerberos Server realm ‘krb5.ac.com’
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/f452352f-9f5f-4b08-a9a8-bd6aa24f5a7f)

Enter administrative server for Kerberos realm ‘krb5.ac.com’
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/3034d5f7-f228-4aff-b8b6-03abce653fd3)
### Step 3: Configure KDC Kerberos Server
```bash
sudo krb5_newrealm 
```
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/f480bebf-acf1-47da-bd2e-3b9639914293)

 Run ‘kadmin.local’ CLI interface for kerberos-

```bash
	sudo kadmin.local
```
```bash
	kadmin.local: addprinc root/admin 

```
```bsh
	kadmin.local: addprinc -randkey host/krb5.ac.com
```
```bash
	kadmin.local: ktadd host/krb5ac.com

```
 Adding ‘root’ admin principal to the access control list
```bash
	sudo nano /etc/krb5kdc/kadm5.acl
```
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/f495bc83-b0a3-4692-b499-f0faf914bbc7)
 Restart Kerberos Service and check the status
```bash
sudo systemctl restart krb5-admin-server.service (Restart)
```
```bash
sudo systemctl status krb5-admin-server.service (Status)
```
## Client Configuration
### Step 4: Installing and configuring Kerberos Client
Configure FQDN ‘client.ac.com’
```bash
sudo apt install bind9 bind9utils dnsutils (Install DNS server)
```
```bash
sudo nano /etc/hosts (Define hosts entry for new domain)

```
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/e44e7d8a-6afe-448a-a4a5-5f2fa34ffae7)


 Create backup
```bash
sudo cp /etc/bind/db.127 /etc/bind/client.ac.com

```
 Create new file for domain
```bash
sudo nano /etc/bind/client.ac.com
```
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/ad62adf4-503b-4120-af86-3f68f5830762)
 Define zone scope for domain
 ```bash
sudo nano /etc/bind/named.conf.local
```
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/353b3a1e-7397-4801-a63f-a3d7ee6c5f5a)

![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/910d8deb-d2a4-4788-8896-b97f51d90991)

Testing

Try to ping to ‘client.ac.com’ and ‘krb5.ac.com’

![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/dfda7388-f206-4e88-9a14-17167efc1fde)
# Install Kerberos client
```bash
sudo apt install –y krb5-user libpam-krb5 libpam-ccreds
```

Check for Kerberos server domain name as realm ‘AC.COM’

![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/58999ee7-a25a-4778-9b69-c7e880b3df15)

Enter the Kerberos server ‘krb5.ac.com’

![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/88be31e6-c04d-4db3-9921-b94e66a26d02)

Enter the admin server name as the Kerberos server name ‘krb5.ac.com’
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/43f3d859-01f4-412b-8e0f-71899f51d7b6)
Configure Kerberos Client & add Kerberos client to the Kerberos database and addthe keytab file for the client.

```bash
sudo kadmin
```
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/b0f3ac3d-d556-484b-9cb0-2cfcd0981981)

Step 5: Testing the configurations
```bash
sudo useradd -m -s /bin/bash kerbuser
```
```bash
 sudo kadmin.local
```
![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/5846f6cd-d50c-4d97-b331-206418919a8b)
	Edit ssh configuration ‘/etc/ssh/sshd_config’ & uncomment the ‘GSSAPIAuthentication	and enable it by changing the value to ‘yes’

 ![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/a35e826b-ff24-43db-9021-f1ab3c313b22)


Restart sshd service

```bash
 sudo systemctl restart sshd
```
```bash
  sudo systemctl status sshd
```
```bash
  sudo useradd -m -s /bin/bash kerbuser
```
```bash
   kinit kerbuser
```
```bash
  klist
```
```bash
   Run kinit kerbuser
```
```bash
   Run klist
```
Testing - SSH
```bash
ssh krb5.ac.com
```

![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/e5f85911-ae24-4d73-82ad-3621da64320e)

Testing - SCP
```bash
sudo scp test.txt shuhari@192.168.80.100:/home/shuhari
```

![image](https://github.com/akshata9597/cdac-pg-ditiss/assets/149655684/b830d5fe-c3db-4754-b174-b82543fccd08)
