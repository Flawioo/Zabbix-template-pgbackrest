# Zabbix Template - Pgbackrest
see what you can use, share what you like


1. [pgbackrest](pgbackrest/) has nice examples for jsonpath and does a nice job monitoring pgbackrest backups

## How to use

Zabbix Server Version 6.4

In Zabbix Server
- Import template [Link [pgbackrest/template-pg-backrest-v6.4.yaml]](https://github.com/Flawioo/Zabbix-template-pgbackrest/blob/main/pgbackrest/template-pg-backrest-v6.4.yaml)

![image](https://github.com/Flawioo/Zabbix-template-pgbackrest/assets/17605608/42dbe2da-13d4-4c18-a371-2b8f98157ce1)

![image](https://github.com/Flawioo/Zabbix-template-pgbackrest/assets/17605608/40604a73-e78f-44c5-91ec-e2986b56af92)


In pgbackrest server (host that has the backup repo)
- Download pgbackrest_zabbix.sh to $HOME/bin/ of the user that is running pgbackrest (my case pgbackrest user)
- Correct the user permissions
- Add script to crontab
- Check script execution

```
pgbackrest@srv-pgbackrest:~$ mkdir $HOME/bin/
pgbackrest@srv-pgbackrest:~$ cd $HOME/bin/
pgbackrest@srv-pgbackrest:~$ wget https://raw.githubusercontent.com/Flawioo/Zabbix-template-pgbackrest/main/pgbackrest/pgbackrest_zabbix.sh
pgbackrest@srv-pgbackrest:~$ chmod +x  $HOME/bin/pgbackrest_zabbix.sh
pgbackrest@srv-pgbackrest:~$ ls -l  $HOME/bin/pgbackrest_zabbix.sh
-rwxr-xr-x 1 pgbackrest pgbackrest 4337 Mar 12 10:44 /home/pgbackrest/bin/pgbackrest_zabbix.sh
pgbackrest@srv-pgbackrest:~$ crontab -l | grep -A1 By
# By Pinheiro
01 * * * * $HOME/bin/pgbackrest_zabbix.sh >>/var/log/pgbackrest/pgbackrest_zabbix.sh.log 2>&1
pgbackrest@srv-pgbackrest:~$
```
zabbix_agentd.conf should use pure Hostname directive or the script will not work correctely
```
root@srv-pgbackrest:/home/pgbackrest# grep ^Hostname /etc/zabbix/zabbix_agentd.conf
Hostname=srv-pgbackrest
root@srv-pgbackrest:/
```

Monitoring Flow
- Pgbackrest server always sending trapps to zabbix server
- All data is get with zabbix trapper
