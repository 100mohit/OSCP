nmap port extractor:
````cat d | grep ^[0-9] | cut -d '/' -f 1 | tr '\n' ','````
