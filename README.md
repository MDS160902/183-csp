# Dokumentation Projekt @Loris Polenz, @Michael de Smitt, @Lenny Lam


### Über unser Projekt


### Über unser Backend
- Michi => 1 - 3
- Loris => 4 - 7
- Lenny => 8 - 10

### OWASP Top Ten
- Broken Access Control ()
- Cryptographic Failures
- Injection
- Insecure Desing
- Security Misconfiguration
- Vulnerable and outdated Components
- Identification and Authentification Failures
- Software and Data Integrity Failures
- Security Logging and Monitoring Failures
- Server-Side Request Forgery

#### Added in 2021
- Insecure Design
- Software and Data integrity failures
- Server-side request forgery



### Broken Access Control
#### Description

#### What could work?

#### What did work?

### Cryptographic Failures
#### Description

#### What could work?

#### What did work?

### Injection
#### Description

#### What could work?

#### What did work?

### Insecure Desing
#### Description

#### What could work?

#### What did work?

### Security Misconfiguration
#### Description

#### What could work?

#### What did work?

### Vulnerable and outdated Components
#### Description

#### What could work?

#### What did work?

### Identification and Authentification Failures
#### Description

#### What could work?

#### What did work?

### Software and Data Integrity Failures
#### Description
Insecure Deserialization vulnerability covers software and data integrity issues. This issue can happen when the identity of the apps or data is not checked or verification process is not well-rounded (that is, it can be bypassed or validation failure still lets the app to run) The Fix: Always ensure that the app you are using is trusted and uses sane security practices.

#### What could work?
Our dependencies may be outadet and contain security loopholes that we can exploit. For example, with the old Hibernate versions, you could perform SQL injections.

#### What did work?
Since all dependencies are up to date, nothing could be detected at the moment.

#### How to prevent?
As soon as a security vulnerability is known, update immediately, generally create regular updates. Update it only if it is safe. If it is open source look at the changes in the code.

### Security Logging and Monitoring Failures
#### Description
There is no direct vulnerability that can arise due to these issues but in general, logging and monitoring are quite critical. Logging and monitoring is critical and their absence or failures can directly impact visibility, incident alerting, and forensics. These are some of the actionable measures you must take to avoid these issues.

#### What could work?
We could actually bruteforce our password without being noticed.

#### What did work?
Sending unlimited requests with OWASP ZAP.

#### How to prevent?
Install logs that log the IP and monitor the traffic. Encrypt these logs and analyze them regularly. Update it only if it is safe. If it is open source look at the changes in the code.

### Server-Side Request Forgery
#### Description
If an attacker can make the server issue (typically) HTTP requests on it’s behalf, then that's SSRF. An attacker can get access to internal services or external services to access or send confidential stuff like AWS temporary credentials assigned to EC2 instances.
#### What could work?
That a request via our API sends a request directly to our DB server, which is only accessible in the internal network, and thus reads out all data of the database.

#### What did work?
At this point, the database was still publicly available for testing purposes. It was very easy to determine the port of the database by port scanning.

#### How to prevent?
Create whitelists or blacklists for requests and accesses from certain services. 

### Insecure Design
#### Description

#### What could work?

#### What did work?

### Software and Data integrity failures
#### Description

#### What could work?

#### What did work?

### Server-side request forgery
#### Description

#### What could work?

#### What did work?

#### Presentation (Canva link): https://www.canva.com/design/DAEuhcPueKY/share/preview?token=6YGSIv3_85jt-YAkM6a3ew&role=EDITOR&utm_content=DAEuhcPueKY&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton

