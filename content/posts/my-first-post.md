---
title: "My First Post"
date: 2023-10-06T02:00:31+03:00
tags: [Linux, IP]
url: /check-external-ip/
image: images/2023/10/ipaddress.jpg
categories:
  - Linux
---
Aşağıda ki komutlar herhangi bir Linux sistemi için Linux Terminalinde harici IP'yi kontrol etmenize yardımcı olur.<!--more-->

### Komutlar

##### Dış IP için

`curl -s checkip.dyndns.org | sed -e 's/.*Current IP Address: //' -e 's/<.*$//'`

##### Hatırlaması Basit ve Kısa

`curl ifconfig.co`

##### Güvenlik kaygısı olanlar için curl kullanmadan

`dig TXT +short o-o.myaddr.l.google.com @ns1.google.com | awk -F'"' '{ print $2}'`

Bu komutlardan hepsi aynı sonuca ulaşacaktır, bu nedenle istediğinizi seçebilirsiniz. 
İş ve profesyonel ortamlarda kullanmak için dig ile sorgulamak daha güven verir. 
Bunu hatırlamak zor ise bir alias oluşturarak yapabilirsiniz.
Ancak kişisel kullanımlar için basit olan curl ifconfig.co komutuda işinizi görecektir.

![pr](/images/2023/10/ipaddress.png)


![](/images/istanbuldive_yeni.png)