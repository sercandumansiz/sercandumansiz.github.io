---
title: "SSL Incidents #1"
category: 
summary: We'll try to answer frequently asked qestions by users.
date: 2023-03-01
tags: ["Incidents", "Security", "SSL"]  
author: ["Sercan"]
draft: false
weight: 1
---

### What is SSL Pinning, also known as Certificate Pinning?
Imagine you have an iPhone application that communicates with a server over HTTPS. When your iPhone app connects to the server for the first time, it receives the server's SSL/TLS certificate. This certificate contains the server's public key and other identifying information.

In SSL Pinning, **your iPhone application stores a copy of this certificate**. Then, during subsequent connections with the server, your iPhone app checks the certificate presented by the server against the stored certificate. If the certificates match, the connection proceeds as usual. If they don't match, the connection is terminated.

By using SSL Pinning, you ensure that your iPhone application only communicates with a server that has a specific SSL/TLS certificate. This helps prevent man-in-the-middle attacks, where an attacker intercepts and modifies network traffic between your iPhone app and the server. Instead, SSL Pinning ensures that your app only communicates with the legitimate server, providing an additional layer of security for your app and your users.

### What are the potential issues or challenges that can arise with SSL Pinning?

Managing your SSL/TLS certificate on your own can be a challenging and error-prone process that can result in additional maintenance costs and risks when it comes to updating certificates.

If you encounter a situation where the server's certificate has changed but you haven't updated your client certificate, it could result in a significant downtime for your application.
Especially if it's a mobile application.

While SSL Pinning does provide additional security, it's important to be careful. :)

**AWS Warnning**

> We recommend that your application not pin an ACM certificate. ACM performs Managed renewal for ACM certificates to automatically renew your Amazon-issued SSL/TLS certificates before they expire. To renew a certificate, ACM generates a new public-private key pair. If your application pins the ACM certificate and the certificate is successfully renewed with a new public key, the application might be unable to connect to your domain.

**AWS Suggestion**
> If you decide to pin a certificate, the following options will not hinder your application from connecting to your domain Import your own certificate into ACM and then pin your application to the imported certificate. ACM doesn't try to automatically renew imported certificates. If you're using a public certificate, pin your application to all available Amazon root certificates. If you're using a private certificate, pin your application to the CA's root certificate.

references: [https://docs.aws.amazon.com/acm/latest/userguide/acm-bestpractices.html](https://docs.aws.amazon.com/acm/latest/userguide/acm-bestpractices.html)