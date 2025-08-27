Byte_Eagle Walkthrough Report
Author: K. Jyothish

Overview:
This report details the step-by-step process of idenƟfying, enumeraƟng, and exploiƟng a
virtual machine designated as 'Byte_Eagle'. The process involves network scanning, service
enumeraƟon, file extracƟon, credenƟal discovery, and privilege escalaƟon.

Step 1: Identify the Target Machine
The target system operates within a Virtual Machine environment. Its IP address is detected,
but access credenƟals are unknown.
<img width="1452" height="937" alt="Screenshot 2025-08-05 124313" src="https://github.com/user-attachments/assets/8fdea946-6d68-4f38-ae95-c0433b117942" />


Step 2: Scan for Open Ports
Using the nmap tool with the command “nmap -T4 -sV [ip_address] -A -p-`, open ports and
associated services are idenƟfied. The machine runs HTTP, SSH, and Samba services.
<img width="830" height="866" alt="Screenshot 2025-08-05 130005" src="https://github.com/user-attachments/assets/399ec167-d612-4e7b-a18e-a2cc6166e6ea" />

Step 3: Access Web Service
The open HTTP port hosts a web interface. Browsing to the IP address reveals a hint message
indicaƟng "to follow the wave."
<img width="1919" height="1004" alt="Screenshot 2025-08-05 121141" src="https://github.com/user-attachments/assets/21dfe03e-219e-40f9-a7cc-6c76560018e0" />


Step 4: Enumerate Samba Shares
Employing `enum4linux [ip_address]`, shared files via Samba are discovered.
<img width="945" height="642" alt="Screenshot 2025-08-05 125233" src="https://github.com/user-attachments/assets/0e10d7a4-7b89-4adf-a21b-32454e2dd6e9" />
<img width="715" height="276" alt="Screenshot 2025-08-05 125250" src="https://github.com/user-attachments/assets/be31925c-83db-4954-9413-1f7c9aeb9ff4" />


Step 5: Retrieve Shared Files
Using `smbclient -N -L //[ip_address]`, shared directories are listed. The 'wave' share
contains two files: `FLAG1.txt` and `message_from_aveng.txt`. These files are downloaded
using `get` commands and their contents viewed with “cat`.


Step 6: Decode CredenƟals

The files contain base64 encoded data. Decoding them with “echo [base64_string] | base64
-d” yields login credenƟals for the machine.

Step 7: Access the Machine via SSH
Using the credenƟals, SSH login is performed with `ssh wave@[ip_address]`. Once inside, the
directory structure is explored.

Step 8: Find Writable Directories and Access Files
The command “find / -writable -type d 2>/dev/null` lists writable directories. NavigaƟng to
`/usr/share/av/westsidesecret`, scripts are executed.

Step 9: Run Bash Script to Retrieve CredenƟals
ExecuƟng `bash ififoget.sh` retrieves addiƟonal login details for the 'aveng' user.

Step 10: Elevate Privileges
Using the 'aveng' credenƟals, switch users with `su aveng` and then escalate privileges to
root with `sudo su`.

Step 11: Retrieve Flag and Change Password

The `FLAG2.txt` file contains a message, accessed via `cat FLAG2.txt`. The password is then
changed with the `passwd` command.

Conclusion:
This walkthrough illustrates the methodology for systemaƟcally exploiƟng a virtual machine
by leveraging service enumeraƟon, credenƟal harvesƟng, and privilege escalaƟon
techniques.

Enter the new password, and then the password will be changed.
