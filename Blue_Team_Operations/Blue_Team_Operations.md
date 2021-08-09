# Blue Team: Summary of Operations

## Table of Contents
- Network Topology 
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology
![Network Diagram](https://github.com/rachelcamurphy/Final_Project/blob/main/Blue_Team_Operations/Images/Final_Project_Network_Diagram.PNG)

The following machines were identified on the network:
- Name of VM 1
  - **Azure ML-RefVm-684427**:
  - **Network Host and Hypervisor**:
  - **192.168.1.1**:
- Name of VM 2
  - **Debian GNU/Linux 8**:
  - **Target Machine 1**:
  - **192.168.1.110**:
- Name of VM 3
  - **Kali Linux**:
  - **Attacker Machine**:
  - **192.168.1.90**:
- Name of VM 3
  - **ELK Server**
  - **Network Log Collector**
  - **192.168.1.100**

### Description of Targets


The target of this attack was: `Target 1: 192.168.1.110` 

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Name of Alert 1
`Excessive HTTP Errors` 

Alert 1 is implemented as follows:
  - **Metric**: Packetbeat Indice
  - **Threshold**: `WHEN count() GROUPED OVER top 5 'http.response.status_code' IS ABOVE 400 FOR THE LAST 5 minutes`
  - **Vulnerability Mitigated**: Network Mapping,  Enumeration, and Port Scanning
  - **Reliability**: This alert can generate lots of false positives. Rated as low reliability.

#### Name of Alert 2
Alert 2 is implemented as follows:
  - **Metric**: Packetbeat Indice
  - **Threshold**: `WHEN sum() of http.request.bytes OVER all documents IS ABOVE 3500 for the last 1 minute`
  - **Vulnerability Mitigated**: Code Injection (XSS and CRLF), DDoS attacks
  - **Reliability**: This alert can generate lots of false positives. Rated as medium reliability.

#### Name of Alert 3
Alert 3 is implemented as follows:
  - **Metric**: Metricbeat Indice
  - **Threshold**: `WHEN max() OF system.process.cpu.total.pct OVER all documents IS ABOVE 0.5 FOR THE LAST 5 minutes`
  - **Vulnerability Mitigated**: Malicious software or specific viruses that utilize these resources. 
  - **Reliability**: This alert can generate lots of false positives, but is highly reliable, as it allows System Administrators to allocate or deallocate resources as well. 

### Suggestions for Going Further 

- Each alert above pertains to a specific vulnerability/exploit. Recall that alerts only detect malicious behavior, but do not stop it. For each vulnerability/exploit identified by the alerts above, suggest a patch. E.g., implementing a blocklist is an effective tactic against brute-force attacks. It is not necessary to explain _how_ to implement each patch.

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:
- Vulnerability 1: Insecure Web Application on http port 80 `http://192.168.1.110`
  - **Patch**: Install SSL Certificate
  - Affordable and secure web hosting can be found through the following link: [reference link](https://stablehost.com)
  - **Why It Works**: Encrypts data traversing the network connection and mitigates the possibility of a man in the middle attack against the web server. 
- Vulnerability 2: Open SSH Port 22 
  - **Patch**: Use a nonstandard port for ssh connections. For linux machines, navigate to the `/etc/ssh/ssh_config` file and modify the following line from Port 22 to Port 8022. 
 ![Nonstandard Port](https://github.com/rachelcamurphy/Final_Project/blob/main/Blue_Team_Operations/Images/change_ssh_port.PNG)
  - **Why It Works**: Attackers will actively enumerate network ports, and any internet facing SSH connections leave the network vulnerable to brute force attacks against ssh keys, and unauthorized access to the network.
- Vulnerability 3: Weak User Passwords
  - **Mitigation**: Enforce a strong password policy. All users must install a password manager such as Last Pass and generate strong passwords for all user accounts.
  - **Why It Works**: Significantly reduces the possibility of a brute force attack. 
