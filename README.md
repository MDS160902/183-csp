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








### Insecure Design
The cause of a vulnerability can be a software design that is from the beginning on insecure. In the case of insecure design software is built having vulnerabilities. One of the biggest problems with insecure design is, that it cannot be fixed with perfect implementation. In such a case the perfect implementation does still contain the flaw. Insecure design or verification patterns do also count as insecure design. 
If a server returns the profile information behind a phone number, it is possible for attackers to exploit this vulnerability and find the phone numbers of influential people through brute forcing these numbers.
In this case the developer who created the specific feature did nothing wrong. It seemed to them like it was made with intention and not thinking about the risks of this feature.

#### What could word?
- Users get deleted from the database and not as inactive flagged
- - Now if someone breaches a profile and deletes it, we do not have any chance to restore the deleted account. This flaw would most certainly result in data loss.
- No rate limits 
- - We currently do not have any rate limits in place. It theoretically would be possible to brute force every password of every user and we would not notice it. 
- No return restricions
-  - In one query we do return every card we have in our table. In theory this query does work just perfect. Until we add new cards for a future release. Then an attacker could see and leak these not released cards. 
- Returning password hashes
- - If a user registers on our application we return the user object, like it is stored in the database. So, we also return our password hashes which is a very bad idea.

#### What does work?
-	Users get deleted from the database and not as inactive flagged. 
- - Flagging a user as deleted and not allowing our endpoints to do anything with this user would have the same effect as a completely deleted user while adding the possibility of reinstating a wrongly deleted user.
-	No rate limits
- - The easiest way to fix this issue would be to create a maximum login tries per minute. Solving the problem this way would then open another design flaw. It would be very easy for an attacker to DOS a person’s login. Probably the best easy solution would be to add the IP address as a parameter. In this case we could rate limit requests from a specific IP, making it harder to DOS someone.
-	No return restrictions
- - This flaw could be fixed easy by adding a release status in the database and only requesting the “RELEASED” cards. This way it is not possible to get an “UNRELEASED” card.
-	Returning password hash
-	- The solution to this flaw is easy too. We either just replace the password hash with null in the returning object or we create a new object with no hash and return this to the client.



### Security Misconfiguration
Either software or a whole system can be misconfigured. The cause of a flaw is considered as that when it would be fixable by changing / crating a configuration. A good example for this would be not closing ports that are not used by the application. A wrong / non existing configuration in an application can also cause such a flaw. For example, if the software in production is still in development configured. This way test credentials, or filters could be an issue.

#### What could be an issue?
-	No port overviews
- - On our server we do not have an overview which ports are open, and which are not. That can cause, that services are accessible to the internet even though they shouldn’t.
-	 Whole routes are authentication free
- - Our backend needs some API Endpoints that are accessible without an Authentication Token. These could be the “register” and “login” endpoint. Since now only these two endpoints run on “/api/v1/users/” we defined in our security configuration that everything after [..]/users/ is whitelisted from our authentication. This could cause the problem, that new user related endpoints get added and are then unprotected. 
-	Database user has root access on all databases
- - In order to develop easier, we decided to create a DB user with all privileges on every database on the server. In case of a data breach or SQL Injection the attacker could do everything with every database on our system.

#### What could work
The above-mentioned vulnerabilities are all existing on our system.  The following steps could solve these issues.
-	No port overviews
- - With some easy commands it is possible to see which ports are open. For example, with ``` netstat -tulpn | grep LISTEN ``` it is possible to see all open ports with their services on a system. Since this requires some time to do this check, I would create a chron job that sends out an email every week with the open server ports. 
-	Whole routes are authentication free
- - This issue is easy to fix. It is possible to not use any wild cards in the whitelisting configuration. This way the configuration would grow, but our project would be way less susceptible to human error. 
-	Database user has root access on all databases
- - This problem could be solved by restricting access to needed databases, including restricting the action a database needs to perform. For example, would our backend user only need to read and write on one database. Another database could be restricted to read only. This would increase the time to manage accounts but also significantly increase application security.


### Vulnerable and outdated Components
Such a flaw can be caused by not maintaining software and dependencies. No developer codes everything by himself and creates a 100% secure system, so we use third party software such as frameworks, libraries or DBMS. Every single third-party software has security flaws too, some more and others less. Since a framework for example basically injects code into mine, every flaw in the framework becomes a problem for my software. 
There are automated tools to support the developer on keeping components save. But relying on third-party software to keep third-party software secure is not optimal too. The best way to keep a space save is limiting the use of third-party software and take the time to code it yourself. Also keeping track what is happening in the infosec space can help to get to know possible vulnerabilities firsthand.

#### How to work on a such issues?
Checking every dependency for possible flaws would overreach the scope of this project. There is one example that would illustrate a good process.
I know from reading about infosec that there were two known vulnerabilities to hibernate. We use hibernate in our project for database interaction our project would have been affected by at least one of two vulnerabilities.  These two vulnerabilities enabled SQL to databases through hibernate. This should not be possible. 
After a quick google search, I found the CVE id’s for these two vulnerabilities. 

- https://www.cvedetails.com/cve/CVE-2020-25638/
- https://www.cvedetails.com/cve/CVE-2019-14900/

After having a quick read on the above pages I knew that all Hibernate versions before 5.3.18, 5.4.18 and 5.5.0.Beta1 had this one of the flaws. The other flaw was existent until 5.4.23.Final. Based on this information I could have a look at our version of Hibernate and conclude that we aren’t affected anymore.

Also Intellij would tell us if a maven dependency is out of date, but as I said. Third-party software checking third-party software.

Following similar steps to the above can increase application security by a lot but it can be very time intensive, especially if the developer does this in their free time.



### Identification and Authentification Failures
As a service provider I must ensure that the person that is authenticating is authentic. If I fail to do so there is a design issue (insecure design) or authentication failures. Generally, an authentication failure is counted as such if it is possible for an attacker to imitate a user and make actions on behave of the user.  Example for such vulnerabilities would be CSRF, Cookie stealing or allowing very insecure passwords. Obviously, it is not possible to ensure 100% correct security since it is always a compromise between security and user experience. In. order to ensure security a developer must take the necessary steps to protect the user as good as possible. Security before usability.

#### What could work?
-	No 2FA
-- At the moment it is not possible for a user to enable 2FA such as security keys or authenticator apps. This takes a layer of security.
-	Not possible to revoke JWT
--	Our authentication works with JSON web tokens. These tokens do expire after some time but are not tied to a client’s browser or user-agent. If a user logs out of our service, we just destroy the cookie containing the key. 
-	Every password allowed.
-- We do not restrict passwords. So, it would be possible to have “a” as a password. 
-	Own services
-- We use authentication services created by us. This is a good method for smaller projects but not really suited for bigger production project. Especially with our resources and knowledge. 
-	SSH-Keys are not password protected
-- Some of our developers use ssh keys that are not password protected. This is a very big risk since we cannot ensure the authenticity of this key.

What did work? 
All the issues above are a real threat to our application. We could take the following steps to solve the issues.
-	No 2FA
-	Own Services
-	Every password allowed
-- All the above-mentioned issues could be resolved if we would use a authentication solution that is open source, self-hosted and maintained by a bigger group with more resources. Examples for such services could be the following.
ory.sh
keycloak.org

They provide the features we need and are way better maintained and tested than our own software. 
-	Not possible to revoke JWT
-- Since it is not possible to invalidate a JWT by design we would have to store every JWT in a database and then delete it when the user logs out. This would ruin the whole approach behind JWT’s. That’s why we could also include information about the user’s client such as ip-address, browser type, user-agent and create a fingerprint for every user. This would increase security since it then would be way harder to steal the token and use it elsewhere maliciously. 
-	SSH-Keys are not password protected
-- This issue is not solvable in a technical way. We need to make all our developers aware that using unprotected SSH-Keys can lead to serious problems. I think this point is especially important, since it shows that in security at the last instance is depended on the human user. 2FA can be perfect, but useless if no one uses it. 



































































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

