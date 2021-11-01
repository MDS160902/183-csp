# Documentation Project @Loris Polenz, @Michael de Smitt, @Lenny Lam


### About our Project


### About our Backend


### OWASP Top Ten
- Broken Access Control
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
In Web Apps there are often different kind of groups who have different kind of Access Permissions on Data or parts of the application. Such as „admins“, „normal users“ or „audits“.  A „normal user“ is not allowed to login as an admin and doesn’t has the same permissions as an admin.

#### What could work?
If a user has a URL from an admin page, he could theoretically reach the page without having to identify himself, beacuse we dont have an Access Control

#### What did work?
Everything worked, because we do not have an Access Control. To prevent Broken Access Control we could of course build a strong Access Control. On top oft that we could use the "Principle of least privilege". With this we give the minimum of time and functionalities to this particular user. 

### Cryptographic Failures aka Sensitive Data Exposures
#### Description
Hackers want to expose data. They use different kind of attacks to do that. A website has often a few Pages. If one of these pages doesn’t has sensitive data, they are sometimes encrypted with HTTP. So its easier to expose Data from this website

#### What could work?
We dont have an SSL Certificat - we transfer all user data via http. Sensitive Data could be exposed.

#### What did work?
Everything worked. To prevent that we could enforce HTTPS. The Server should reject all HTTP Calls. We can Classify data into which data is sensitive and which not and apply Controls to that data. We should definitly encrypt our data:
- https://letsencrypt.org/
- https://www.openssl.org/

### Injection
#### Description
Inject malicious Code in a form field to trick the backend DB to return all of the Data. This could be made by writing SQL Statements into the fields and trick the backend and get personal Data as a response.

#### What could work?
Cross Site Scripting attacks could work.

#### What did work?
We are using libraries like hibernate and Java-spring security. To increase the safety we could use "Query parameterization". This means that we seperate the SQL statement from any kind of parameters if someone would try to get data with an sql statement in our login form for example.
To prevent Cross Site Scripting we could filter "input on arrival" (When user input is received, filter as strictly as possible) and "encode data on output" (When user-controllable data is output in HTTP responses, encode the output. Like that the output wiull not be interpreted as active content).

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

#### What could work?

#### What did work?

### Security Logging and Monitoring Failures
#### Description

#### What could work?

#### What did work?

### Server-Side Request Forgery
#### Description

#### What could work?

#### What did work?

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

