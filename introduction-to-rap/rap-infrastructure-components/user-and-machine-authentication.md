# Authentication Engine

Authentication in RAP is handled through [Auth0](https://auth0.com/).

On stock deployments, users are authenticated by email addresses and logins created in Auth0.  An e-mail domain whitelist is also put in place to allow only people with West Monroe and client e-mail addresses to self-enroll to log into RAP.

Auth0 supports the following additional features, all of which are not enabled by default but can be turned on for each RAP implementation as required:

* Integration with other directories \(Azure Active Directory, etc\)
* Multi-factor authentication

TODO - describe machine auth for API and RAP Agents, certificate setup

### Auth0 Rules Engine

Rules can be set up in Auth0 using JavaScript code.  Detailed information about how to set up rules are provided in [Auth0's documentation](https://auth0.com/docs/rules).

A few links for how to set up some common rules are below:

* Setting up an e-mail domain whitelist:  [https://auth0.com/rules/simple-domain-whitelist](https://auth0.com/rules/simple-domain-whitelist)

