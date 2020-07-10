### Spis treści
- [Chapter 2: Attacks, Concepts and Techniques](#chapter-2-attacks-concepts-and-techniques)
  - [2.1 Analyzing a cyberattack](#21-analyzing-a-cyberattack)
  - [2.2 The Cybersecurity Landscape](#22-the-cybersecurity-landscape)

# Chapter 2: Attacks, Concepts and Techniques

## 2.1 Analyzing a cyberattack

`Blended attack` - attack which use multiple techniques to infiltrate and attack a system

`Malware` - malicious software is any code that can be used to steal data, bypass access controls, or cause harm to, or compromise a system.

>`Security vulnerabilities` are any kind of software or hardware defect. An `exploit` is the term used to describe a program written to take advantage of a known `vulnerability`.

Vulnerabilities:
- `software` - errors in OS or application code
  - `buffer overflow` - occurs when data is written beyond the limits of a buffer. This can lead to system crash, data compromise, or provide escalation of privlieges. Buffers are memory areas allocated to application.
  - `non-validated input` - sometimes incoming data can have mailcious content
  - `race conditions` - becomes a source of vulnerability when the required ordered or timed events do not occur in the correct order or proper timing.
  - `weakness in security practises` - improper implementation of authentication, authorization and encryption. Developers should use libraries that have already created, tested and verified.
  - `access-control problems` - improper use of access control
- `hardware` - often introduced by hardware design flaws eg. Rowhammer

>Nearly all access controls and security practices `can be overcome` if the attacker has `physical access` to target equipment. To protect the machine and the data it contains, `physical access must be restricted and encryption techniques must be used` to protect data from being stolen or corrupted.

Types of malware:
- `spyware` - designed to spy on the user eg. activity trackers, keystroke collection, data capture. Spyware often modifies security settings. 
  >Spyware often bundles itself with legitimate software or with Trojan horses.

- `adware` - automaticaly delivery advertisements. It is common for adware to come with spyware or/and it is installed with with some versions of software.

- `bot` - malware designed to automatically perfotm action, usually online. Group of bots controlled by attacker is called botnet, this devices quietly wait for commands provided by attacker.

- `ransomware` - hold, usually encrypts computer OS or data until a payment is made. It is spread by downloaded files or some software vulnerability.

- `scareware` - designed to persuade a user to take specific action based on fear. 
  >It conveys forged messages stating the system is at risk or needs the execution of a specific program to return to normal operation. n reality, no problems were assessed or detected and if the user agrees and clears the mentioned program to execute, his or her system will be infected with malware.

- `rootkit` - main goal is to modify the OS to create backdoor, which enables attackeer to access computer remotely. Rootkits uses a software vulnerabilities to perform privilege escalation and modify system files.
  >It is also common for rootkits to modify system forensics and monitoring tools, making them very hard to detect. Often, a computer infected by a rootkit must be `wiped and reinstalled`.

- `virus` - mailicious code that is attached to executable files, most of tehm requires end-user activation and can active on specific date of time. They also can mutate to avoid detection.
  >Most viruses are now spread by USB drives, optical disks, network shares, or email.

- `trojan horse` - carries out mailicious operation under the guise of desired operation. It exploits the privileges of the user that runs it.
  >A Trojan horse differs from a virus because it binds itself to non-executable files.

- `worms` - designed to replicate themselves by independently exploiting vulnerabilities in neworks

- `MitM` - Man-In-The-Middle, an attacker can intercept/captuer user information before relaying it to its intended destination. Sometimes it allows the attacker to take control over a device without user\`s knowledge

- `MitMO` - Man-In-The-Mobile, a variation of MitM used to take control over mobile devices eg ZeuS capabilities to capture 2-step verification SMS
  >When infected, the mobile device can be instructed to exfiltrate user-sensitive information and send it to the attackers

Symptoms of malware:
- increase in CPU usage, 
- decrease in computer, web browsing speed, more often freezes or crashes,
- unexplainabe problems with network connections,
- some files are modified, deleted,
- presence of unknown files, programs,
- unknown process are running,
- emails is beaing sent witout the user\`s knowledge or consent,
- programs are turning off or reconfiguring themseleves

Social engineering attack based on manipulating individuals into performing actions or divulging confidential information. It relies on people willingness to be helpful an people\`s weaknesses.
Types of social engineering attacks:
- `pretexting` - an attacker calls an individual and lies to them in an attempt to gain access to privileged data.
  >An example involves an attacker who pretends to need personal or financial data in order to confirm the identity of the recipient.

- `tailgatoring` - when an attacker quickly follows an authorized person into a secure location

- `Something for Something` (Quid pro quo) - This is when an attacker requests personal information from a party in exchange for something, like a free gift.

Wi-Fi password cracking - main goal is to obtain the password used to portect a wireless network. Possible techniques:
- `social engineering` - manipulate a individual who knows the password into providing it
- `brute-force attack` - attacker tries several possible passwords in an attempt to guess the password
- `dictionary attack` - brute force attack using common word list file
- `network sniffing` - listening and capturing packets to discover unencrypted, plain text passwords

>A few password brute-force tools include `Ophcrack, L0phtCrack, THC Hydra, RainbowCrack, and Medusa`.

`Phishing` - malicious party sends a fraudulent email disguised as being from a legitimate, trusted source. The message intent is to trick the recipient into installing malware on their device, or into sharing personal or financial information.
 
>An example of phishing is an email forged to look like it was sent by a retail store asking the user to click a link to claim a prize. The link may go to a fake site asking for personal information, or it may install a virus.

Vulnerability exploitation - whole process that consist of gathering information about machines such as running services and OS versions, then the attacker is looking for any know vulnerabilities specific to that system.

>`APT` - Advanced Persistent Threats, multi-phase, long term, stealthy and advanced operation against a specific target. Due to its complexity and skill level required, an APT is usually well funded. An APT targets organizations or nations for business or political reasons.

`DoS` - Denial-of-Service
 - `Overwhelming Quantity of Traffic` - an enormous quantity of data at a rate which server cannot handle 
 - `Maliciously Formatted Packets` - in some cases it can cause that attacked machine is running very slowly or can even crash

>DoS attacks are considered a major risk because they can easily interrupt communication and cause significant loss of time and money. These attacks are relatively simple to conduct, even by an unskilled attacker.

`DDoS` - Distributed DoS, DoS attack form multiple, coordinated sources, usually using botnet(infected host is called zombies).

`SEO poisoning` - mailicous user could use Search Engine Optimalization(SEO) to make that mailicous website appear higher in search results. 

## 2.2 The Cybersecurity Landscape

`Blended attack` - attacks that use multiple techniques to compromise a target. By using several different attack techniques at once, attackers have malware that are a hybrid of worms, Trojan horses, spyware, keyloggers, spam and phishing schemes.

>The most common type of blended attack uses spam email messages, instant messages or legitimate websites to distribute links where malware or spyware is secretly downloaded to the computer. Another common blended attack uses DDoS combined with phishing emails. First, DDoS is used to take down a popular bank website and send emails to the bank's customers, apologizing for the inconvenience. The email also directs the users to a forged emergency site where their real login information can be stolen.

`Impact Reduction`, important measures a company should take when a security breach is identified:
- communicate the issue
- be sincere and accountable in case the company is at fault
- provide details, why the situation took place and what was copromised
- understand what caused and facilitated the breach
- apply what was learned from the forensics investigation to ensure similar breaches do not happen in the future
- ensure all systems are clean and no backdoors were installed and nothing else has been compromised
- educate employees, partners, and customers on how to prevent future breaches

---

<div>
<a href="chapter-01.md">Prev: Chapter 1</a>
</div>
<div align="right">
<a href="chapter-03.md">Next: Chapter 3</a>
</div>