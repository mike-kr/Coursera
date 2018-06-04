# Risk in the Workplace
## Intro to Risk in the Workplace
**Defense in Depth** - The concept of having multiple, overlapping systems of defense to protect IT systems
## Security Goals
If your company handles credit card payments, then you have to follow the **PCI DSS**, or **Payment Card Industry Data Security Standard**.
1. Build and maintain a secure network and systems
2. Protect Cardholder Data
3. Maintain a vulnerability management program
4. Implement strong access control measures
5. Regularly monitor and test networks
6. Maintain an information security policy

## Measuring and Assessing Risk
Security is all about determining **risks** or exposure; understanding the likelihood of **attacks**; and designing **defenses** around these risk to **minimize** the impact of an attack.

Security risk assessment starts with **threat modeling**.
1. Identify threats
2. Assign priorities that correspond to severity and probability  
High-value data usually includes account information, like usernames and passwords.  Typically, **any kind of user data is considered high value**, especially if payment processing is involved.  
**Vulnerability Scanner** - a computer program designed to asses computers, computer systems, networks or applications for weaknesses  
[Nessus](https://www.tenable.com/products/nessus/nessus-professional)  
[OpenVAS](http://www.openvas.org/)  
[Qualys](https://www.qualys.com/forms/freescan/)  
**Penetration testing** - the practice of attempting to break into a system or network to verify the systems in place

## Privacy Policy
**Privacy policies** oversee the access and use of sensitive data.

audit data access logs

It's good practice to apply the principle of **least privilege** here, by not allowing access to this type of data by default.  
Any access that doesn't have a corresponding request should be flagged as a **high-priority potential breach** that needs to be investigated as soon as possible.  
**Data-handling policies** should cover the details of how different data is classified.  
Once different data classes are defined, you should create **guidelines** around how to handle these different types of data.  

# Users
## User Habits