# Ten Nmap Commands Every Sysadmin Should Know

As a sysadmin, knowing your tools is crucial, and one of the first that comes to mind is nmap. This powerful network scanner goes beyond simple port scanning; it's a versatile tool for discovering vulnerabilities, service versions, operating systems, and even that unannounced printer on your network.

1. Discover IPs in a Subnet (No Root Required)
A simple yet effective use of nmap is the subnet IP discovery, also known as a "ping scan". The command is straightforward:

$ nmap -sP 192.168.0.0/24
This command sends various requests to the subnet, listing IPs that respond. It’s useful and doesn’t require root access, although root users get additional ARP request capabilities.

2. Scan for Open Ports (No Root)
The default nmap scan reveals open ports and is executed with:

$ nmap 192.168.0.0/24
This process can be time-consuming as it scans 1000 common ports and performs additional checks like DNS reverse lookup.

3. Identify Operating System (Root Required)
For a deeper analysis, nmap can guess a target's operating system with the -O option:

# nmap -O 192.168.0.164
This feature, requiring root access, leverages information obtained from port scans to make accurate guesses about the OS.

4. Identify Hostnames (No Root)
Uncover hostnames within a subnet without sending packets to individual hosts using:

$ nmap -sL 192.168.0.0/24
This subtle command performs DNS queries, offering insights into network structure and device roles.

5. TCP SYN and UDP Scan (Root Required)
A comprehensive yet stealthy scan for both TCP and UDP ports is performed with:

# nmap -sS -sU -PN 192.168.0.164
This scan, although time-consuming, is effective for a thorough network analysis.

6. Full Range Port Scan (Root Required)
For an exhaustive port scan across all available ports (1–65535), use:

# nmap -sS -sU -PN -p 1-65535 192.168.0.164
This command extends the TCP SYN and UDP scan to the full range of ports.

7. TCP Connect Scan (No Root)
A TCP connect scan, different from the SYN scan, requests the OS to establish a full TCP connection:

$ nmap -sT 192.168.0.164
This scan is more detectable but doesn’t require root privileges.

8. Aggressive Scan (Root Required)
For a detailed and aggressive analysis, combining port, OS, and service scans:

# nmap -A 192.168.0.164
This option should be used judiciously, as it is more intrusive and easily detected.

Remember, nmap is a powerful tool that should be used responsibly. Whether you're securing your network or troubleshooting, these commands offer a starting point for effective network management. Happy scanning!