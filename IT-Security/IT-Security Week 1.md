# Malicious Software

## The CIA Triad

**C.I.A.**

* Confidentiality - Keeping things hidden
* Integrity - Keeping our data accurate and untampered with
* Availability - The information we have is readily accessible to those people that should have it

## Essential Security Terms

**Risk** - The possibility of suffering a loss in the event of an attack on the system

**Vulnerability** -  A flaw in a system that could be exploited to compromise the system

**0-day vulnerability (zero day)** - A vulnerability that is not known to the software developer or vendor, but is known to an attacker

**Exploit** - Software that is used to take advantage of a security bug or vulnerability

**Threat** - The possibility of danger that could exploit a vulnerability

**Hacker** - Someone who attempts to break into or exploit a system

**Attack** - An actual attempt at causing harm to a system

## Malicious Software

**Malware** - A type of malicious software that can be used to obtain your sensitive information, or delete or modify files

* trojans
* rootkits
* backdoors
* botnets
* spyware
* viruses
* etcetera

Virus - attaches itself to executable code and runs when executable runs and replicates on any files it comes in contact with from the executable

Worms - are like viruses except they can live on their own and spread through the network

Adware - software that displays advertisements and collects data

Trojan - malware that disguises itself as one thing but does something else

Spyware - a type of malware that's meant to spy on you

Keylogger -  a common type of spyware that's used to record every keystroke you make

Ransomware - a type of attack that holds your data or system hostage until you pay some sort of ransom

## Malware Continued

Bots - compromised machines that's resources are being hijacked by an attacker

Botnets - designed to utilize the power of the inter-connected machines to perform some distributed function

Backdoor -  a way to get into a system if the other methods to get in the system aren't allowed

Rootkit - a collection of software or tools that an Admin would use

Logic Bomb - a type of malware that's intentionally installed

[Logic Bomb Bank](https://www.independent.co.uk/news/business/news/disgruntled-worker-tried-to-cripple-ubs-in-protest-over-32000-bonus-481515.html)

# Network Attacks

## Network Attacks

DNS Cache Poisoning attack - tricks DNS to accept a fake record to point to a compromised DNS server and feeds fake addresses when websites are attempted to be accessed

Man-in-the-middle attack (MITM) - places an attacker in the middle of two hosts that think they're communicating directly with each other

Session hijacking or cookie hijacking - common MITM attack

Rogue Access Point (Rogue AP) attack - an access point that is installed on the network without the network administrator's knowledge

Evil twin - similar to Rogue AP attack; connect to a network that is identical to yours controlled by the attacker

[DNS Cache Poisoning Attack](https://threatpost.com/major-dns-cache-poisoning-attack-hits-brazilian-isps-110711/75859/)

## Denial-of-Service

Denial-of-Service (DoS) attack - an attack that tries to prevent access to a service for legitimate users by overwhelming the network or server

Ping of Death (POD) - sends a malformed ping to a server; it will be larger in size than what the internet protocol was made to handle so it results in a buffer overflow

Ping flood - sends tons of ping packets to a system (ICMP echo requests) to overwhelm the system

SYN flood - similar to ping flood attacks but sends SYN packets for a TCP connection; attacker sends SYN packets, the system replies with SYN ACK packets but the attacker doesn't send ACK packets back thus keeping the connection open and taking up service resources. Also referred to as half-open attacks

Distributed denial-of-service attack (DDoS) - a DoS attack using multiple systems

[Dyn Attack](https://dyn.com/blog/dyn-analysis-summary-of-friday-october-21-attack/)

# Other Attacks

## Client-Side Attacks

Injection attacks - injection of malicious code

* can be mitigated with good software development principles, like validating input and sanitizing data.

Cross-site scripting (XSS) attacks - a type of injection attack where the attacker can insert malicious code and target the user of the service

* commonly used for session hijacking

SQL injection attack - targets entire website if the website is using a SQL database

* Can delete, modify, copy, and run other malicious commands on the database

## Password Attacks

Password attacks - utilize software like password-crackers that try and guess your password

Brute force attack - continuously tries combinations of passwords

Dictionary attack - tries commonly used words in passwords

## Deceptive Attacks

Social engineering - an attack method that relies heavily on interactions with humans instead of computers

Phishing attack - masquerading as a trusted entity and duping a victim into performing an action, commonly:

* email
* instant message
* text

Spear phishing - specifically targets an individual or group

Spoofing - a source masquerading around as something else

Baiting - entices a user to do something

* leave a usb drive somewhere hoping someone will randomly plug it in

Tailgating - gaining access into a restricted area or building by following a real employee in







