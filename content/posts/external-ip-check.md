---
title: "Linux sistemlerde dış IP'yi konsoldan sorgulama"
date: 2023-10-06
author: Ahmet Önder Moğol
url: /check-external-ip/
featured_image: images/2023/10/ipaddress.png
categories:
  - Linux
tags: [Linux, IP]  
---
Aşağıda ki komutlar herhangi bir Linux sistemi için Linux Terminalinde harici IP'yi kontrol etmenize yardımcı olur.<!--more-->

### Komutlar

##### Dış IP için sorgu

`curl -s checkip.dyndns.org | sed -e 's/.*Current IP Address: //' -e 's/<.*$//'`

##### Hatırlaması Basit ve Kısa olan yöntem

`curl ifconfig.co`

##### Güvenlik kaygısı olanlar için curl kullanmadan yapılan sorgu

`dig TXT +short o-o.myaddr.l.google.com @ns1.google.com | awk -F'"' '{ print $2}'`

Bu komutlardan hepsi aynı sonuca ulaşacaktır, bu nedenle istediğinizi seçebilirsiniz. 
İş ve profesyonel ortamlarda kullanmak için **dig** ile sorgulamak daha güven verir. 
Bunu hatırlamak zor ise bir alias oluşturarak yapabilirsiniz.
Ancak kişisel kullanımlar için basit olan **curl ifconfig.co** komutuda işinizi görecektir.

![](/images/2023/10/ipaddress.png)

