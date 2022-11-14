Ubuntu 20.04 canonical AD join WI
```
1. sudo apt-get update
```
```
2. sudo apt-get -y upgrade
```
```
3. hostnamectl set-hostname tbt_machine
```
```
4. vi /etc/hosts
        172.16.2.35 tbt_machine.theblackthreat.in tbt_machine
```
```
5. sudo apt -y install realmd sssd sssd-tools libnss-sss libpam-sss krb5-user samba-common adcli samba-common-bin oddjob oddjob-mkhomedir packagekit 
```
```
6. sudo apt install resolvconf
```
```
7. sudo systemctl start resolvconf.service
```

8. add nameserver to path : 
```
vi /etc/resolvconf/resolv.conf.d/head
                        search theblackthreat.in
                        nameserver 172.16.3.100
                        nameserver 172.16.2.122
```
```
9. sudo systemctl start resolvconf.service
```
```
10. sudo vi /etc/krb5.conf
        [libdefaults] 
        default_realm = theblackthreat.in 
        rdns = false 
```
```
11. vi /etc/sssd/sssd.conf
```
```
[sssd]
domains = theblackthreat.in
config_file_version = 2
services = nss, pam

[domain/theblackthreat.in]
ad_domain = theblackthreat.in
krb5_realm = THEBLACKTHREAT.IN
realmd_tags = manages-system joined-with-adcli
cache_credentials = True
id_provider = ad
krb5_store_password_if_offline = True
default_shell = /bin/bash
#default_shell = /bin/false
ldap_id_mapping = True
use_fully_qualified_names = False
fallback_homedir = /home/%u@%d
access_provider = ad
```
```
13. systemctl restart sssd
```
```
14. realm join theblackthreat.in --user=ravitbt
```
```
15. pam-auth-update --enable mkhomedir
```
