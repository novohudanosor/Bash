# Bash
Скрипт на Bash
Написать скрипт для CRON, который раз в час будет формировать письмо и отправлять на заданную почту.  Необходимая информация в письме:
1. Список IP адресов с (наибольшим количеством запросов) с указанием количества запросов с момента последнего
запуска скрипта;
2.  Список запрашиваемых URL (с наибольшим количеством запросов) с указанием количества запросов с момента последнего запуска скрипта;
3.  Ошибки веб-сервера/приложения с момента последнего запуска скрипта;
4.  Список всех кодов HTTP ответа с указанием их количества с момента последнего запуска скрипта;
5.  Скрипт должен предотвращать одновременный запуск нескольких копий до его завершения;
6.  В письме должен быть прописан обрабатываемый временной диапазон.
7.  Написали скрипт  parse_log.sh
8.  Проверка работы скрипта. Диапазон выводится для демонстрации работы. Данные выводятся на основании содержимого всего access-лога.
``` 
The processed range:
|Sat Jun 29 18:13:51 MSK 2024| - |Sat Jun 29 19:13:51 MSK 2024|

######## IP ########

Num_req IP's
---------------------------
     45 93.158.167.130
     39 109.236.252.130
     37 212.57.117.19
     33 188.43.241.106
     31 87.250.233.68
     24 62.75.198.172
     22 148.251.223.21
     20 185.6.8.9
     17 217.118.66.161
     16 95.165.18.146
     12 95.108.181.93
     12 62.210.252.196
     12 185.142.236.35
     12 162.243.13.195
      8 163.179.32.118
      7 87.250.233.75
      6 167.99.14.153
      6 165.22.19.102
      5 71.6.199.23
      5 5.45.203.12

######## Resources ########

Num_req Resources
-----------------------------------------
    157 /
    120 /wp-login.php
     57 /xmlrpc.php
     26 /robots.txt
     12 /favicon.ico
     11 400
      9 /wp-includes/js/wp-embed.min.js?ver=5.0.4
      7 /wp-admin/admin-post.php?page=301bulkoptions
      7 /1
      6 /wp-content/uploads/2016/10/robo5.jpg
      6 /wp-content/uploads/2016/10/robo4.jpg
      6 /wp-content/uploads/2016/10/robo3.jpg
      6 /wp-content/uploads/2016/10/robo2.jpg
      6 /wp-content/uploads/2016/10/robo1.jpg
      6 /wp-content/uploads/2016/10/aoc-1.jpg
      6 /wp-content/uploads/2016/10/agreed.jpg
      6 /wp-content/themes/llorix-one-lite/style.css?ver=1.0.0
      6 /wp-admin/admin-ajax.php?page=301bulkoptions
      5 /wp-includes/js/wp-emoji-release.min.js?ver=5.0.4
      5 /wp-includes/css/dist/block-library/style.min.css?ver=5.0.4

######## Server Errors ########

Num_err Server_errors
---------------------------
      3 500

######## Response Codes ########

Num_rsp Response codes
---------------------------
    498 200
     95 301
      1 304
      7 400
      1 403
     51 404
      1 405
      2 499
      3 500
``` 
9. Задание для **cron**
```
calculate dem # crontab -e
...
calculate dem # crontab -l
SHELL=/bin/bash
PATH=/bin:/sbin:/usr/bin:/usr/sbin

*/2 * * * * parse_log /tmp/access-4560-644067.log | mutt -s report_"`date`" -- spinal07@mail.ru
```
10. Скрипт сопировал в /bin. Для быстрой проверки работоспособности отправка осуществлялась каждую вторую минуту часа.
11. Проверка работоспособности CRON
``` 
calculate dem # cat /var/log/messages
...
Jun 29 14:12:00 calculate crond[2841]: (root) RELOAD (/var/spool/cron/crontabs/root)
Jun 29 14:12:00 calculate crond[4655]: pam_ldap: missing file "/etc/ldap.conf"
Jun 29 14:12:00 calculate crond[4655]: pam_unix(crond:session): session opened for user root(uid=0) by root(uid=0)
Jun 29 14:12:00 calculate CROND[4657]: (root) CMD (parse_log /home/dem/vagrant/10_DZ/access-4560-644067.log | mutt -s report_"`date`" -- spinal07@>
Jun 29 14:12:15 calculate mutt[4659]: DIGEST-MD5 common mech free
Jun 29 14:12:15 calculate CROND[4655]: (root) CMDEND (parse_log /home/dem/vagrant/10_DZ/access-4560-644067.log | mutt -s report_"`date`" -- spinal>
Jun 29 14:12:15 calculate CROND[4655]: pam_unix(crond:session): session closed for user root
Jun 29 14:14:00 calculate crond[14768]: pam_ldap: missing file "/etc/ldap.conf"
Jun 29 14:14:00 calculate crond[14768]: pam_unix(crond:session): session opened for user root(uid=0) by root(uid=0)
Jun 29 14:14:00 calculate CROND[14771]: (root) CMD (parse_log /home/dem/vagrant/10_DZ/access-4560-644067.log | mutt -s report_"`date`" -- spinal07>
Jun 29 14:14:10 calculate mutt[14773]: DIGEST-MD5 common mech free
Jun 29 14:14:10 calculate CROND[14768]: (root) CMDEND (parse_log /home/dem/vagrant/10_DZ/access-4560-644067.log | mutt -s report_"`date`" -- spina>
Jun 29 14:14:10 calculate CROND[14768]: pam_unix(crond:session): session closed for user root
Jun 29 14:16:00 calculate crond[25044]: pam_ldap: missing file "/etc/ldap.conf"
Jun 29 14:16:00 calculate crond[25044]: pam_unix(crond:session): session opened for user root(uid=0) by root(uid=0)
Jun 29 14:16:00 calculate CROND[25047]: (root) CMD (parse_log /home/dem/vagrant/10_DZ/access-4560-644067.log | mutt -s report_"`date`" -- spinal07>
Jun 29 14:16:10 calculate mutt[25049]: DIGEST-MD5 common mech free
Jun 29 14:16:10 calculate CROND[25044]: (root) CMDEND (parse_log /home/dem/vagrant/10_DZ/access-4560-644067.log | mutt -s report_"`date`" -- spina>
Jun 29 14:16:10 calculate CROND[25044]: pam_unix(crond:session): session closed for user root
Jun 29 14:18:00 calculate crond[2748]: pam_ldap: missing file "/etc/ldap.conf"
Jun 29 14:18:00 calculate crond[2748]: pam_ldap: missing file "/etc/ldap.conf"
Jun 29 14:18:00 calculate crond[2748]: pam_unix(crond:session): session opened for user root(uid=0) by root(uid=0)
Jun 29 14:18:00 calculate CROND[2751]: (root) CMD (parse_log /home/dem/vagrant/10_DZ/access-4560-644067.log | mutt -s report_"`date`" -- spinal07@>
Jun 29 14:18:09 calculate mutt[2753]: DIGEST-MD5 common mech free
Jun 29 14:18:09 calculate CROND[2748]: (root) CMDEND (parse_log /home/dem/vagrant/10_DZ/access-4560-644067.log | mutt -s report_"`date`" -- spinal>
Jun 29 14:18:09 calculate CROND[2748]: pam_unix(crond:session): session closed for user root
Jun 29 14:19:17 calculate crontab[13448]: (root) BEGIN EDIT (root)
Jun 29 14:20:00 calculate crond[13452]: pam_ldap: missing file "/etc/ldap.conf"
Jun 29 14:20:00 calculate crond[13452]: pam_unix(crond:session): session opened for user root(uid=0) by root(uid=0)
Jun 29 14:20:00 calculate CROND[13455]: (root) CMD (parse_log /home/dem/vagrant/10_DZ/access-4560-644067.log | mutt -s report_"`date`" -- spinal07>
Jun 29 14:20:10 calculate mutt[13457]: DIGEST-MD5 common mech free
Jun 29 14:20:10 calculate CROND[13452]: (root) CMDEND (parse_log /home/dem/vagrant/10_DZ/access-4560-644067.log | mutt -s report_"`date`" -- spina>
Jun 29 14:20:10 calculate CROND[13452]: pam_unix(crond:session): session closed for user root
...
``` 

