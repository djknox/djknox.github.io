---
layout: post
title: Finding Domain Names with WHOIS
permalink: "/finding-domain-names-with-whois/"
date: 2019-09-20 10:39 -0400
---
Before securing a domain name for a new project, you first need to see if it's even available. There are plenty of stories of people who searched for a particular domain name on registrars like Network Solutions and GoDaddy, only to have it immediately purchased and re-listed with a new, higher price. This is known as [domain name front running](https://en.wikipedia.org/wiki/Domain_name_front_running) and is enabled through [domain tasting](https://en.wikipedia.org/wiki/Domain_tasting), which is securing a domain name with the ability to refund it within the five day grace period.  

A simple way to find information on domain names that eliminates any chances of this happening is to use the `whois` command to search the databases of registrars and registries.  

Within a terminal window, type `whois <domain-name>` and press `ENTER` to get all the WHOIS information.  

For example,

```
whois google.com
```

will return (amongst a lot of other information):

```
   Domain Name: GOOGLE.COM
   .
   .
   .
   Updated Date: 2019-09-09T15:39:04Z
   Creation Date: 1997-09-15T04:00:00Z
   Registry Expiry Date: 2028-09-14T04:00:00Z
```  

With this output, we can see that Google registered its domain name on September 15, 1997.

So, if you search for a domain name and it returns:

```No match for domain "<domain-name>".```

then the domain name is most likely available to you to register and you won't have to worry about any domain name front runners ruining your day.