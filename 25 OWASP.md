# OWASP

## Top 10 blessing and curse
* Blessing: good for awareness
* Curse: Top 10 is only a top 10, there are many more security concerns

## Risk
OWASP has reoriented towards business and business risks.

## Security officers
Often don't know about the complexity of software development.
Security assessments end up with checklists of things to fix.

## Security development process
Security checks after the fact come too late.

Security needs to be a top priority in development activities from the very beginning.

## OWASP testing guide
Tells how to test an application, what to look for, how to test it with what tools and how to fix those issues.

Pentesters should follow that guide to provide more insightful reports.

## AppSec pipeline
https://www.appsecpipeline.org

* intake tools: assess frameworks and libraries you use (use versions, how fast are bugs fixed, known bugs? ...)
* triage tools: prioritize inbound requests and assess their testing needs based on the risk level
  * the more risky the app, the more activities assigned
  * these tools aim to provide automation and orchestration to reduce the startup time of the testing stage
* test tools: collect and normalize the data created during testing
* delivery tools: ...

## OWASP application security verification standard project
* https://www.owasp.org/index.php/Category:OWASP_Application_Security_Verification_Standard_Project
* https://www.owasp.org/images/3/33/OWASP_Application_Security_Verification_Standard_3.0.1.pdf

## SecurityRAT
* https://github.com/SecurityRAT

Helps handling security requirements and treat them as JIRAs.

## Cheat sheets
Tons of cheat sheets from OWASP

## Security Knowledge Framework
Tons of knowledge base items around security.

Input application information and it builds a list of recommendations and things to pay attention to.

## OWASP Zed Attack Proxy
Run functional tests against the Zed Attack proxy so that it knows the scope of normal application usage. Then it can be used to protect your application

## OWASP Dependency-Check
Check project dependencies (e.g., open source libraries) for known vulnerabilities

https://www.owasp.org/index.php/OWASP_Dependency_Check