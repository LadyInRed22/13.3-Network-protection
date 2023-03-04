# Домашнее задание к занятию 13.3. «Защита сети» - `Виноградова Татьяна`

------

### Задание 1

Проведите разведку системы и определите, какие сетевые службы запущены на защищаемой системе:

**sudo nmap -sA < ip-адрес >**

**sudo nmap -sT < ip-адрес >**

**sudo nmap -sS < ip-адрес >**

**sudo nmap -sV < ip-адрес >**

По желанию можете поэкспериментировать с опциями: https://nmap.org/man/ru/man-briefoptions.html.


*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*
---

![image](https://user-images.githubusercontent.com/103531664/222885181-72e05225-8014-492a-b824-0cf26847ff0b.png)

В лог suricata попало сканирование с ключами -sT, -sV, -sS. Траффик классифицирован, как потенциально подозрительный.
В лог Fail2Ban не попало ничего, что и не удивительно, Fail2Ban предназначен для другого

------

### Задание 2

Проведите атаку на подбор пароля для службы SSH:

**hydra -L users.txt -P pass.txt < ip-адрес > ssh**

1. Настройка **hydra**: 
 
 - создайте два файла: **users.txt** и **pass.txt**;
 - в каждой строчке первого файла должны быть имена пользователей, второго — пароли. В нашем случае это могут быть случайные строки, но ради эксперимента можете добавить имя и пароль существующего пользователя.

Дополнительная информация по **hydra**: https://kali.tools/?p=1847.

2. Включение защиты SSH для Fail2Ban:

-  открыть файл /etc/fail2ban/jail.conf,
-  найти секцию **ssh**,
-  установить **enabled**  в **true**.

Дополнительная информация по **Fail2Ban**:https://putty.org.ru/articles/fail2ban-ssh.html.

*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*
---

### Kali:
![image](https://user-images.githubusercontent.com/103531664/222892312-4af8d72a-2354-43ec-b495-958ca710670a.png)

### Лог /var/log/auth.log на атакуемом хосте:

```
Mar  4 12:43:05 test2 sshd[3163]: Invalid user rfgegeg from 192.168.3.62 port 39030

Mar  4 12:43:05 test2 sshd[3163]: Received disconnect from 192.168.3.62 port 39030:11: Bye Bye [preauth]

Mar  4 12:43:05 test2 sshd[3163]: Disconnected from invalid user rfgegeg 192.168.3.62 port 39030 [preauth]

Mar  4 12:43:05 test2 sshd[3115]: error: beginning MaxStartups throttling

Mar  4 12:43:05 test2 sshd[3115]: drop connection #10 from [192.168.3.62]:39106 on [192.168.3.60]:22 past MaxStartups

Mar  4 12:43:06 test2 sshd[3169]: Invalid user rfgegeg from 192.168.3.62 port 39062

Mar  4 12:43:06 test2 sshd[3169]: pam_unix(sshd:auth): check pass; user unknown

Mar  4 12:43:06 test2 sshd[3169]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.3.62 

Mar  4 12:43:06 test2 sshd[3170]: Invalid user rfgegeg from 192.168.3.62 port 39068

Mar  4 12:43:06 test2 sshd[3172]: Invalid user user from 192.168.3.62 port 39088

Mar  4 12:43:06 test2 sshd[3173]: Invalid user rfgegeg from 192.168.3.62 port 39090

Mar  4 12:43:06 test2 sshd[3176]: Invalid user user from 192.168.3.62 port 39136

Mar  4 12:43:06 test2 sshd[3171]: Invalid user rfgegeg from 192.168.3.62 port 39082

Mar  4 12:43:06 test2 sshd[3165]: Invalid user rfgegeg from 192.168.3.62 port 39034

Mar  4 12:43:06 test2 sshd[3168]: Invalid user rfgegeg from 192.168.3.62 port 39056

Mar  4 12:43:06 test2 sshd[3167]: Invalid user rfgegeg from 192.168.3.62 port 39042

Mar  4 12:43:06 test2 sshd[3166]: Invalid user rfgegeg from 192.168.3.62 port 39040

Mar  4 12:43:06 test2 sshd[3173]: pam_unix(sshd:auth): check pass; user unknown

Mar  4 12:43:06 test2 sshd[3173]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.3.62 

Mar  4 12:43:06 test2 sshd[3165]: pam_unix(sshd:auth): check pass; user unknown

Mar  4 12:43:06 test2 sshd[3165]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.3.62 

Mar  4 12:43:06 test2 sshd[3172]: pam_unix(sshd:auth): check pass; user unknown

Mar  4 12:43:06 test2 sshd[3172]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.3.62 

Mar  4 12:43:06 test2 sshd[3171]: pam_unix(sshd:auth): check pass; user unknown

Mar  4 12:43:06 test2 sshd[3171]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.3.62 

Mar  4 12:43:06 test2 sshd[3176]: pam_unix(sshd:auth): check pass; user unknown

Mar  4 12:43:06 test2 sshd[3176]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.3.62 

Mar  4 12:43:06 test2 sshd[3174]: Invalid user rfgegeg from 192.168.3.62 port 39094

Mar  4 12:43:06 test2 sshd[3170]: pam_unix(sshd:auth): check pass; user unknown

Mar  4 12:43:06 test2 sshd[3175]: Invalid user rfgegeg from 192.168.3.62 port 39120

Mar  4 12:43:06 test2 sshd[3170]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.3.62 

Mar  4 12:43:06 test2 sshd[3175]: pam_unix(sshd:auth): check pass; user unknown

Mar  4 12:43:06 test2 sshd[3175]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.3.62 

Mar  4 12:43:06 test2 sshd[3174]: pam_unix(sshd:auth): check pass; user unknown

Mar  4 12:43:06 test2 sshd[3174]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.3.62 

Mar  4 12:43:06 test2 sshd[3168]: pam_unix(sshd:auth): check pass; user unknown

Mar  4 12:43:06 test2 sshd[3168]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.3.62 

Mar  4 12:43:06 test2 sshd[3167]: pam_unix(sshd:auth): check pass; user unknown

Mar  4 12:43:06 test2 sshd[3167]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.3.62 

Mar  4 12:43:06 test2 sshd[3166]: pam_unix(sshd:auth): check pass; user unknown

Mar  4 12:43:06 test2 sshd[3166]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.3.62 

Mar  4 12:43:08 test2 sshd[3169]: Failed password for invalid user rfgegeg from 192.168.3.62 port 39062 ssh2

Mar  4 12:43:08 test2 sshd[3172]: Failed password for invalid user user from 192.168.3.62 port 39088 ssh2

Mar  4 12:43:08 test2 sshd[3173]: Failed password for invalid user rfgegeg from 192.168.3.62 port 39090 ssh2

Mar  4 12:43:08 test2 sshd[3165]: Failed password for invalid user rfgegeg from 192.168.3.62 port 39034 ssh2

Mar  4 12:43:08 test2 sshd[3171]: Failed password for invalid user rfgegeg from 192.168.3.62 port 39082 ssh2

Mar  4 12:43:08 test2 sshd[3176]: Failed password for invalid user user from 192.168.3.62 port 39136 ssh2

Mar  4 12:43:08 test2 sshd[3170]: Failed password for invalid user rfgegeg from 192.168.3.62 port 39068 ssh2

Mar  4 12:43:08 test2 sshd[3175]: Failed password for invalid user rfgegeg from 192.168.3.62 port 39120 ssh2

Mar  4 12:43:08 test2 sshd[3174]: Failed password for invalid user rfgegeg from 192.168.3.62 port 39094 ssh2

Mar  4 12:43:08 test2 sshd[3168]: Failed password for invalid user rfgegeg from 192.168.3.62 port 39056 ssh2

Mar  4 12:43:08 test2 sshd[3166]: Failed password for invalid user rfgegeg from 192.168.3.62 port 39040 ssh2

Mar  4 12:43:08 test2 sshd[3167]: Failed password for invalid user rfgegeg from 192.168.3.62 port 39042 ssh2

```
### Лог /var/log/fail2ban.log:

![image](https://user-images.githubusercontent.com/103531664/222894001-ddd156bc-c9ab-4330-be3f-5cad775b4e48.png)

При первом сканировании в соответствии с настройками jail.conf доступ к авторизации был заблокирован, ip атакующего хоста был забанен, при повторном сканировании атакующему хосту было отказано в соединении

---
