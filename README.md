# Week 16 Homework Submission File: Penetration Testing 1

## Step 1: Google Dorking

Using Google, can you identify who the Chief Executive Officer of Altoro Mutual is:

site:demo.testfire.net executives

This is the result: http://demo.testfire.net/index.jsp?content=inside_executives.htm

How can this information be helpful to an attacker:

This can be useful to an attacker to send phishing attempts directly to executive numbers


## Step 2: DNS and Domain Discovery
Enter the IP address for demo.testfire.net into Domain Dossier and answer the following questions based on the results:


Where is the company located:

Sunnyvale CA 94085 US

What is the NetRange IP address:

65.61.137.64 - 65.61.137.127

What is the company they use to store their infrastructure:

CustName:       Rackspace Backbone Engineering
Address:        9725 Datapoint Drive, Suite 100
City:           San Antonio
StateProv:      TX
PostalCode:     78229
Country:        US


What is the IP address of the DNS server:

65.61.137.117


## Step 3: Shodan

What open ports and running services did Shodan find:

80, 443, 8080

## Step 4: Recon-ng

Install the Recon module xssed.

marketplace install xssed
keys add shodan_api DbzEk5puJ1SScPMWYOduJBXk3PDeIeac
modules load recon/domains-vulnerablilities/xssed

Set the source to demo.testfire.net.

options set SOURCE demo.testfire.net

Run the module.

run

Is Altoro Mutual vulnerable to XSS:

Vulnerable to 1.

## Step 5: Zenmap
Your client has asked that you help identify any vulnerabilities with their file-sharing server. Using the Metasploitable machine to act as your client's server, complete the following:


Command for Zenmap to run a service scan against the Metasploitable machine:

nmap -T4 -A 192.168.0.10

Bonus command to output results into a new text file named zenmapscan.txt:

nmap -T4 -A 192.168.0.10 -oN zenmapscan.txt

Zenmap vulnerability script command:

nmap -T4 -F --script ftp-vsftpd-backdoor,smb-enum-shares 192.168.0.10

Once you have identified this vulnerability, answer the following questions for your client:


What is the vulnerability:

PORT    STATE SERVICE
21/tcp  open  ftp
  ftp-vsftpd-backdoor:
	VULNERABLE:
	vsFTPd version 2.3.4 backdoor 
      State: VULNERABLE(Exploitable)
      IDs:  BID:48539  CVE:CVE-2011-2523
        vsFTPd version 2.3.4 backdoor, this was reported on 2011-07-04.
      Disclosure date: 2011-07-03
      Host script results:
  smb-enum-shares:
	account_used: <blank>
  
	......
    \\192.168.0.10\IPC$:
	Type: STYPE_IPC
	Comment: IPC Service (metasploitable server (Samba 3.0.20-Debian))
	Max Users: <unlimited>
	Path: C:\tmp
	Anonymous access: READ/WRITE
	......
	\\192.168.0.10\tmp:
	Type: STYPE_DISKTREE
	Comment: oh noes!
	Max Users: <unlimited>
	Path: C:\tmp
	Anonymous access: READ/WRITE
  	......

Why is it dangerous:

Sends a specific amount of bytes on port 21 and when successful, allows backdoor access of the system as root user

What mitigation strategies can you recommendations for the client to protect their server:

  This issue was only available for a day as an update was pushed afterwards, therefore apply an SMB patch.
 
