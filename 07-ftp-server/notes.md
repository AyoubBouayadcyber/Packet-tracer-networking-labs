# Project 7 — FTP Server

## What I Built
A file transfer server allowing authenticated clients to upload 
and download files over the network using the File Transfer 
Protocol (FTP).

## Topology

Server — Switch — PC


## Configuration

**Server FTP Service:**

Service: On
Username: ayoub
Password: [set password]
Permissions: Read, Write, Delete, Rename, List


**Client PC — Connecting via Command Prompt:**

ftp 192.168.10.100
Username: ayoub
Password: [entered password]


## Test

Connected to the FTP server from the client PC's command line, 
authenticated successfully, and transferred a file using the 
`put` command to upload it to the server.

C:>ftp 192.168.10.100
Connected to 192.168.10.100
220- Welcome to PT Ftp server
Username: ayoub
331- Username ok, need password
Password:
230- Logged in
(passive mode On)
ftp> put test.txt
200 PORT Command Successful
150 File status okay; about to open data connection
226 File transfer complete
ftp: 15 bytes sent


## What This Proves
FTP requires explicit authentication before allowing any file 
operations, and permissions can be scoped per user (read-only 
versus full read/write/delete access). This models a real 
scenario where a server administrator controls exactly what 
each authenticated user is allowed to do once connected.

## Real World Application
FTP is still used in many legacy enterprise environments for 
file distribution, though it transmits credentials and data in 
plaintext — a known security weakness. Understanding how FTP 
authentication and permissions work is directly relevant to 
recognizing why more secure alternatives (SFTP, FTPS) are 
recommended in real production environments, and to spotting 
FTP-based vulnerabilities during a security assessment.
