---
title: "install ssg on gnu/linux"
date: 2021-07-31T12:06:46-05:00
tags: ['linux']
---
# install ssg on gnu/linux
July 31, 2021

the [install process](https://www.romanzolotarev.com/ssg.html) provided has options for openbsd and mac os. here's what i did on my linux system. the mac os instrctions would work fine, but this is what i did. its the same but different.

```bash 
$ mkdir -p bin  
$ wget -O bin/ssg6 https://rgz.ee/bin/ssg6  
--2021-07-30 15:08:37--  https://rgz.ee/bin/ssg6
Resolving rgz.ee (rgz.ee)... 46.23.94.144, 2a03:6000:6f68:605::144
Connecting to rgz.ee (rgz.ee)|46.23.94.144|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 5717 (5.6K) [text/plain]
Saving to: ‘/home/gavin/bin/ssg6’

/home/gavin/bin/ssg6   100%[====================>]   5.58K  2.48KB/s    in 2.2s    

2021-07-30 15:08:41 (2.48 KB/s) - ‘/home/gavin/bin/ssg6’ saved [5717/5717]
$ chmod +x bin/ssg6  
# - dependencies: cpio lowdown 
$ pacman -S cpio
$ yay -S lowdown
```
###### using markdown's ```
{{<rawhtml>}} 
<pre><code><strong>mkdir -p bin</strong>  
<strong>wget -O bin/ssg6 https://rgz.ee/bin/ssg6</strong>  
--2021-07-30 15:08:37--  https://rgz.ee/bin/ssg6
Resolving rgz.ee (rgz.ee)... 46.23.94.144, 2a03:6000:6f68:605::144
Connecting to rgz.ee (rgz.ee)|46.23.94.144|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 5717 (5.6K) [text/plain]
Saving to: ‘/home/gavin/bin/ssg6’

/home/gavin/bin/ssg6   100%[====================>]   5.58K  2.48KB/s    in 2.2s    

2021-07-30 15:08:41 (2.48 KB/s) - ‘/home/gavin/bin/ssg6’ saved [5717/5717]
<strong>chmod +x bin/ssg6</strong>
</code></pre>
{{</rawhtml>}} 
###### using pre and code tags
