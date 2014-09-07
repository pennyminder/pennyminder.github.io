---
layout: post
title:  Cashbook Security Overview
category: Cashbook
tags:
  - pfm
  - security
  - cashbook
  - featured
image: /img/41561977-CashbookInteractions_(7)_(1).png
---
### Introduction
In this document we will describe the security of Cashbook and the steps we have taken to ensure the privacy of member data is protected and the integrity of the core banking system is protected.

 * Cashbook The PFM application
 * Juno: Single sign on authentication service
 * Core: The core banking system
 * Transaction Delivery: Service that sends transactions to Cashbook as they occur in the host system.

### How Do We Protect Member Data?

Secure Sockets Layer (SSL) is used for all communications: User to Cashbook, User to Juno, Cashbook to Juno, and Core system to Cashbook. This protects the data in transit.
Cashbook Single Sign On (SSO) authentication (via Juno) is done by the OAuth2 protocol. This was used primarily so that Cashbook would have zero knowledge of the login credentials. Moreover Juno does not store the login credentials instead it makes a request to the core system to verify the supplied credentials at every login.

<strong>More information on OAuth2:</strong>

 * Background on OAuth: <a href="http://oauth.net/about/">http://oauth.net/about/</a>
 * Background on OAuth2: <a href="http://hueniverse.com/2010/05/introducing-oauth-2-0/">http://hueniverse.com/2010/05/introducing-oauth-2-0/</a>
 * Security Considerations for OAuth2: <a href="http://tools.ietf.org/html/draft-ietf-oauth-v2-25#section-10">http://tools.ietf.org/html/draft-ietf-oauth-v2-25#section-10</a>

###Effectively Anonymized Data
No personally identifiable information sent to Cashbook.

###Risks and How We Handle Them
Both juno and Cashbook are Rails 3.x applications and use the built-in Cross-Site Request Forgery (XSRF) protection and accepted practices for avoiding SQL injection attacks.

One risk to both Cashbook and Juno is a breach at the operating system level. To help combat this risk, our servers expose minimal services, run firewalls, and require the use ssh keys to access the servers.

Each credit union uses separate databases and virtual machine instances so that a breach in one does not necessarily affect any others.

In the event of a breach on the Cashbook servers, the credit union can revoke either the application&rsquo;s credentials or a set of user credentials (depending on the nature of the breach).

If the application&rsquo;s credentials are canceled, Cashbook would work to rectify the breach and once that has been accomplished, the credit union could issue new credentials for Cashbook.

A breach on the Juno side is a bit more serious, but does not represent a risk to the core banking system due the limited nature of access that Juno requires. If a breach were to occur, we would reset the remote access token in Casbbook to protect user information.
