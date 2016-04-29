---
layout: post
title: "How To Set Up Mailgun DNS Records on Digital Ocean"
permalink: "mailgun-on-digital-ocean"
date: 2016-04-07
og_description=Steps to avoid wasting time when connecting Mailgun to Digital Ocean.
---

There are a few pitfalls that can make you waste a lot of time when setting up [Mailgun](https://mailgun.com) on a [Digital Ocean](https://digitalocean.com) server. Here's how to avoid them.

##Apex or subdomain?

If you wish to receive email via Mailgun and already have MX records for your domain, you must create a Mailgun-specific subdomain. This is the case if, for example, you own mydomain.com and have connected info@mydomain.com to Gmail or another email provider.

In Digital Ocean, go to `Networking > Domains` and add a domain. It does not matter which droplet you point it to. In our example, you might create mg.mydomain.com. All subsequent DNS records will be created for this domain.

##SPF Record

Use `@` as the name of the TXT record and the value provided by Mailgun as the record's text. For example, `v=spf1 include:mailgun.org ~all`.

##DKIM Record

Do not include the fully-qualified domain name in the name of the TXT record. If Mailgun gives you `mx._domainkey.mydomain.com`, only put `mx._domainkey` (no trailing dot).

You must wrap the DKIM value in double quotes, for example: `"k=rsa; p=MIG"`.

##CNAME Record

Only enter the subdomain, not the fully-qualified name, as the name of the CNAME record. If Mailgun asks you to set up email.mydomain.com, then the CNAME record's name is `email`, not email.mydomain.com. The hostname is `mailgun.org`.

The same holds if you are setting Mailgun up on a subdomain. To create email.mg.mydomain.com, create the record in the mg.mydomain.com domain, with `email` as name. The hostname is `mailgun.org`.

##MX Records

These are necessary only to receive emails via Mailgun. No pitfalls here, if there are no other MX records, as explained above. You can enter the hostname and priority as given by Mailgun.

##Debugging

If Mailgun reports any errors, the zone file at the bottom of Digital Ocean's DNS settings page is the best way to debug the issue. It allows you to see exactly what values have been configured.

##References

Krister Viirsaar's [Mailgun + DigitalOcean](http://code.krister.ee/mailgun-digitalocean/) article and comment thread helped me sort through these issues.
