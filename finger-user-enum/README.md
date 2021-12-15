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
 




