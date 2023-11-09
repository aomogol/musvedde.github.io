---
title: "Linux sistemlerde dış IP'yi konsoldan sorgulama"
date: 2023-10-06
author: Ahmet Önder Moğol
url: /check-external-ip/
#image: images/2023-thumbs/ipaddress.jpg
categories:
  - Linux
tags: [Linux, Networking]  
---
Aşağıda ki komutlar herhangi bir Linux sistemi için Linux Terminalinde harici IP'yi öğrenmenize yardımcı olur.<!--more-->

## Komutlar

### Dış IP için sorgu
```console
curl -s checkip.dyndns.org | sed -e 's/.*Current IP Address: //' -e 's/<.*$//'
```
### Hatırlaması Basit ve Kısa olan yöntem
```console
curl ifconfig.co
```
### Güvenlik kaygısı olanlar için curl kullanmadan yapılan sorgu
```console
dig TXT +short o-o.myaddr.l.google.com @ns1.google.com
```

Bu komutlardan hepsi aynı sonuca ulaşacaktır, bu nedenle istediğinizi seçebilirsiniz.
İş ve profesyonel ortamlarda kullanmak için **dig** ile sorgulamak daha güven verir.


dig komutunu kullanmak için bind uygulamasının kurulu olması gerekiyor.

**Arch / Manjaro:**
```console
sudo pacman -S bind
```

**Debian / Ubuntu:**
```console
sudo apt-get install dnsutils
```

**CentOS / RedHat:**
```console
sudo yum install bind-utils
```

Kurulum bittikten sonra kontrol için aşağıdaki komutu çalıştırabilirsiniz.
```console
dig -v
```

dig komutunu parametreleri ile hatırlamak zor olabilir bir alias oluşturarak kolayca yapabilirsiniz.
``` console
alias myextip="dig TXT +short o-o.myaddr.l.google.com @ns1.google.com"
```

Basit olan **curl ifconfig.co** komutu da işinizi görecektir.

![External IP](featured.png)
