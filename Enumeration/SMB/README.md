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
```

#### SMB CLIENT
```
smbclient -L //192.168.1.41

smbclient -L //192.168.1.41 -U ""

smbclient -L //192.168.1.41/data -U ""

smbclient -L //192.168.1.41/pass -U ""

smbclient -L //192.168.1.41/C$ -U ""

smbclient -L //192.168.1.41/ADMIN$ -U ""

smbclient -L //192.168.1.41/ADMIN$ -U "administrator"

smbclient -L //192.168.1.41/E$ -U "administrator"

```
#### SMBMAP

```
smbmap -H 192.168.41

smbmap -u "" -H 192.168.41

smbmap --help

smbmap -u ""  -H 192.168.41

```
