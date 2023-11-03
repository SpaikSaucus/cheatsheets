# CheatSheets AWS

## Table of Contents
- [Best Practices](#best-practices)
- [EC2 Linux - Access from Windows](#ec2-linux---access-from-windows)
- [EC2 Linux - Example](#ec2-linux---examples)

## Best Practices
* https://aws.amazon.com/es/organizations/getting-started/best-practices/

## EC2 Linux - Access from Windows
* Install:
	https://www.putty.org/

* Generate private key:
  * Get "amazon-ami.pem"
  * Open app PuTTYgen
    * File -> Load PrivateKey (select file amazon-ami.pen)
    * Save PrivateKey (Generate file amazon-ami.ppk)
	
* Access:
  * Open app PuTTY
    * Session
      * Host Name : ec2-user@172.XX.XX.XX	
    * Connection
	  * SSH 
	    * Auth 
		  * Private key file for authentication 
		     * Browse.. (Search private key generate previously)

## EC2 Linux - Examples
```bash
[ec2-user@ip-172-XX-XX-XX ~]$ sudo service my-project-api status
[ec2-user@ip-172-XX-XX-XX ~]$ sudo service my-project-api start
[ec2-user@ip-172-XX-XX-XX ~]$ sudo service my-project-api stop

[ec2-user@ip-172-XX-XX-XX ~]$ cd /tmp/logs/
[ec2-user@ip-172-XX-XX-XX logs]$ tail -100 my-project-api.log
[ec2-user@ip-172-XX-XX-XX ~]$ start-docker.sh
```