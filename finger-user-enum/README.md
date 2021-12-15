# Finger (79)

**what is finger**

Finger is primarily used to enumerate user information on the target system. It can also find out detailed information (if exists) such as full name, email address, phone number etc. of all its users.

**Default port:**
 79


**Banner Grabbing**

## Telnet
````
telnet $ip 79
````

## Netcat 
````
nc -nv $ip 79

````

**Enumeration**

**finger-user-enum**

https://github.com/pentestmonkey/finger-user-enum

````
finger-user-enum is a script used to enumerate users

````
#### Using finger-user-enum

```
1. Displaying help

# perl finger-user-enum.pl -h

2. enumerate a single user

-u = user

-t = host IP

# perl finger-user-enum.pl -u root -t 10.10.10.76

3. Enumerate users using a list

# perl finger-user-enum.pl -U /usr/share/seclists/Usernames/Names/names.txt -t 10.10.10.76

4. Using a list of IPs

# perl finger-user-enum.pl -U /usr/share/seclists/Usernames/Names/names.txt -T IP_list.txt

5. Using another port than default 79

# perl finger-user-enum.pl -p 8000 -U /usr/share/seclists/Usernames/Names/names.txt -t 10.10.10.76

6. Showing detailed output

# perl finger-user-enum.pl -d -u root -t 10.10.10.76
```




