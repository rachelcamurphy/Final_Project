# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

Nmap scan results for each machine reveal the below services and OS details:

```bash
$ nmap 192.168.1.0/24 
```
Target Machine 1: ```192.168.1.110```

This scan identifies the services below as potential points of entry:
- Target 1
  - List of
  - Exposed Services
    - Port 22/tcp SSH 
    - Port 80/tcp http
    - Port 111/tcp rpcbind
    - Port 139/tcp netbios/ssn
    - Port 445/tcp microsoft-ds
    
![nmap scan results](https://github.com/rachelcamurphy/Final_Project/blob/main/Red_Team_Operations/Images/Nmap_scan_of_target1.PNG)


The following vulnerabilities were identified on each Target 1:

| Vulnerability                   | Description                                                                                                                        | Impact                                                                                         |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| Network Mapping and Enumeration | Nmap was used to discover open ports.                                                                                              | The attackers were able to discover open ports and tailor their attacks accordingly.           |
| Wordpress Scan                  | Wpscan was utilized by attackers in order to gain username information.                                                            | The username info was used by the attackers to help gain access to the web server.             |
| Weak Passwords                  | A user had a weak password and the attackers were able to discover it by guessing.                                                 | The attackers were able to correctly guess a users password and SSH into the web server.       |
| MySQL Database Access           | The attackers were able to discover a file containing login information for the MySQL database.                                    | The attackers were able to use the login information to gain access to the MySQL database.     |
| MySQL Data Exfiltration         | By browsing through the various tables in the MySQL database the attackers were able to discover password hashes of all the users. | The attackers were able to exfiltrate the password hashes and crack them with John the Ripper. |
| Privilege Escalation            | The attackers noticed that Steven had sudo privileges for python.                                                                  | The attackers were able to utilized Steven’s python privileges in order to escalate to root.   |

### Exploitation

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt: b9bbcb33e11b80be759c4e844862482d`
    - **Exploit Used**
        - Visited the IP address of the target `192.168.1.110` over HTTP port 80.
        
            -![http port 80 browser](https://github.com/rachelcamurphy/Final_Project/blob/main/Red_Team_Operations/Images/website_browser_1.PNG)

        - Identified a list of users for the website utilizing the WordPress Security Scanner. User steven and user michael
            - Wordpress Scanner Command: `wpscan --url http://192.168.1.110 -eu`
            
            - ![wordpress scan](https://github.com/rachelcamurphy/Final_Project/blob/main/Red_Team_Operations/Images/enumerate_users_wpscan.PNG)


        - Secure Shelled in as the user michael over SSH port 22.
            - `ssh michael@192.168.1.110`
            - The username and password "michael" were identical, allowing for the ssh connection. 
            
                - ![insert image of ssh connection](https://github.com/rachelcamurphy/Final_Project/blob/main/Red_Team_Operations/Images/logged_in_as_michael_password_michael.PNG)
                
            - Located `flag1.txt` in michael's `/var/www/html` file.
                - ![insert image of flag1.PNG](https://github.com/rachelcamurphy/Final_Project/blob/main/Red_Team_Operations/Images/flag1.PNG)
                
            - Located the MySQL database password within the `wp-config.php` file in `/var/www/html` 

  - `flag2.txt : fc3fd58dcdad9ab23faca6e9a36e581c`
  
    - **Exploit Used**
      - Utilized the locate command as the user michael to find the relative path to flag2. 
      - Commands: `locate flag2` and `cat /var/www/flag2.txt`
          - ![insert image of flag2](https://github.com/rachelcamurphy/Final_Project/blob/main/Red_Team_Operations/Images/flag2.PNG)
      
 - `flag3.txt : afc01ab56b50591e7dccf93122770cd2` 
    - **Exploit Used**
     - As the user michael, located the password to the MySQL Database by navigating to the `/var/www/html/wordpress`directory and viewing the `wp-config.php` file.
      - ![wp config](Red_Team_Operations\Images\locating_my_sql_password.PNG)
      - ![mysql database password](Red_Team_Operations\Images\mysql_database_password.PNG)
      - Logged into the MySQL database utilizing the password found. `mysql -u root   -pR@v3nSecurity`  
       - ![mysqlcommand](Red_Team_Operations\Images\logged_into_mysql_using_password_found.PNG)
       - Revealed flag 3 logged into the mySQL database with the following commands.
        - `mysql> show databases;`
        - `mysql> use wordpress;`
        - `mysql> show tables;`
        - `mysql> select * from wp_posts;`
        - ![flag3](https://github.com/rachelcamurphy/Final_Project/blob/main/Red_Team_Operations/Images/flag3.png)

 - `flag4.txt : 715dea6c055b9fe3337544932f2941ce` 
    - **Exploit Used**
      - Logged in as the root user to the MySQL  Database. Revealed the password hashes for the user's steven and michael with the following MySQL commands. 
        - `mysql> show databases;`
        - `mysql> use wordpress;`
        - `mysql> show tables;`
        - `mysql> select * from wp_users;`
        - ![mysqlshowdatabases](https://github.com/rachelcamurphy/Final_Project/blob/main/Red_Team_Operations/Images/mysql_showdatabases.PNG)
        - Saved these password hashes to a text file to be cracked offline. [hashes](https://github.com/rachelcamurphy/Final_Project/blob/main/Red_Team_Operations/wp_hashes/wp_hashes.txt)

       - Cracked the user steven's password hash with john the ripper. Steven's password: Pink84.
        - ![cracked](https://github.com/rachelcamurphy/Final_Project/blob/main/Red_Team_Operations/Images/john_the_ripper_password.PNG)
     - Logged in as the user steven via ssh utilizing the password Pink84.
     - Executed the following python script in order to escalate privileges to the root user.
       - ![priv esc](https://github.com/rachelcamurphy/Final_Project/blob/main/Red_Team_Operations/Images/priv_esc_from_steven_to_root.PNG)
     - Located the final flag as the root user on target 1. 
        - ![flag4]()
    

        
