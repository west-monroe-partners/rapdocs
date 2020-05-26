# User and Machine Authentication

Authentication in RAP is handled through Auth0.

On stock deployments, users are authenticated by email addresses and logins created in Auth0.  An email domain whitelist is also put in place to allow both West Monroe resources and client resources to self-enroll to log into RAP.

Auth0 supports the following additional features, all of which are not enabled by default but can be turned on for each RAP implementation as needed:

* Integration with other directories \(Azure Active Directory, etc\)
* Multi-factor authentication

TODO - machine auth for API and RAP Agents

