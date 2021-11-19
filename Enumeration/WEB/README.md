## Web Enumeration
Web enumeration means to look out info about services running on port, versions, languages, directories and more.
- [Web Enumeration](#web-enumeration)
  - [Ports](#ports) 
  - [nmap](#nmap)
  - [Finding Open Ports](#finding-open-ports)
  - [Checking Running Service Version](#check-running-service-version)
  - [Nmap Scripts](obtaining-information-using-nmap-scripts)
- [HTTP Method](#http-method)
  - [Available Methods](#detecting-available-methods)
  - [Upload File](#upload-file)
  - [Tools](#tools)
    - [netcat](#netcat)
    - [curl](#curl)
    - [nikto](#nikto)
    - [davtest](#davtest)
    - [cadaver](#cadaver)
- [HTTP Basic Authentication](#http-basic-authentication)
  - [Tools](#tools)
    - [hydra](#hydra)
    - [ncrak](#ncrack)
    - [medusa](#medusa)
   
## ports.
| Service | Port No. | Protocol |
|---------|----------|----------|
| HTTP | 80,8080,8081,8000 | TCP |
| HTTPS | 443,8443,4443 | TCP |
| Tomcat Startup | 8080 | TCP |
| Tomcat Startup (SSL) | 8443 | TCP |
| Tomcat Shutdown | 8005 | TCP |
| Tomcat AJP Connector | 8009 | TCP |
| Glassfish HTTP | 8080 | TCP |
| Glassfish HTTPS | 8181 | TCP |
| Glassfish Admin Server | 4848 | TCP |
| Jetty | 8080 | TCP |
| Jonas Admin Console | 9000 | TCP |
## nmap
	  -Pn: Treat all hosts as online -- skip host discovery
	  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
	  -p <port ranges>: Only scan specified ports
	  -sV: Probe open ports to determine service/version info
	  -sC: equivalent to --script=default
	  --script=<Lua scripts>: <Lua scripts> is a comma separated list of
		   directories, script-files or script-categories
	  -O: Enable OS detection
	  -oA <basename>: Output in the three major formats at once
	  -v: Increase verbosity level (use -vv or more for greater effect)
	  -A: Enable OS detection, version detection, script scanning, and traceroute
## Finding Open Ports

	# nmap -sT -v -p- $IP

	# nmap -sT -v -p- -oA allports $IP ##Save Output in File
	# nmap -sT -v -p- -oA allports $IP 			##Save Output in File

	# nmap -Pn -sT -v -p- $IP
	
## Check Running Service Version
	# nmap -sT -sV -sC -A -O -v -p 80,443 $IP
	# nmap -Pn -sT -sV -sC -A -O -v -p 80,443 $IP

