# Project 6 — Email Server

## What I Built
A mail server enabling two client accounts to send and receive 
email directly within the simulated network, using SMTP for 
sending and POP3 for retrieving messages.

## Topology

Server — Switch — PC1, PC2


## Configuration

**Server EMAIL Service:**

Service: On
Domain Name: mail.ayoub.com


**User Accounts Created:**

Account 1: ayoub@mail.ayoub.com
Account 2: bouayad@mail.ayoub.com


**Client PC1 — Email App Configuration:**

Your Name: Ayoub
Email Address: ayoub@mail.ayoub.com
Incoming Mail Server (POP3): 192.168.10.100
Outgoing Mail Server (SMTP): 192.168.10.100
User Name: ayoub
Password: [set password]


**Client PC2 — Email App Configuration:**

Your Name: Bouayad
Email Address: bouayad@mail.ayoub.com
Incoming Mail Server (POP3): 192.168.10.100
Outgoing Mail Server (SMTP): 192.168.10.100
User Name: bouayad
Password: [set password]


## Test

Sent a test email from PC1 (ayoub@mail.ayoub.com) to PC2 
(bouayad@mail.ayoub.com). On PC2's Email app, clicked Receive — 
the message arrived successfully in the inbox.

## What This Proves
Email delivery relies on two separate protocols working 
together: SMTP handles the outbound sending of a message from 
client to server, while POP3 handles the client retrieving 
messages waiting on the server. Both clients had to authenticate 
against the same mail server, which acted as the central point 
for both accounts — mirroring how a company's internal mail 
system operates before any external internet mail exchange is 
involved.

## Real World Application
Understanding SMTP/POP3 fundamentals is directly relevant to 
cybersecurity — email remains one of the most common attack 
vectors (phishing, spoofing, business email compromise). Knowing 
how mail servers authenticate users and route messages is 
foundational knowledge for any SOC analyst or security engineer 
investigating email-based incidents.
