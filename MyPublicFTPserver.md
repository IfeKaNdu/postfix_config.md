### Setting up a Public postfix server over SSH on CentOS 7.

Requirements: CentOS 7 on a computer with two or more NICs , firefox web browser, vsftpd package , iftp ,ssh.

In this article, we will look into the following:

1. Postfix installation.
2. Configuring postfix for the internet.
3. Starting postfix service.
4. Securing the postfix server.
5. Configuring postfix for the internet.
6. Accessing the server with a mail server client.
-------------------------------------------------------------- 

# 1. The postfix server installation
Postfix can be installed by using the following command:
```bash
[root@server ~]# yum remove sendmail
[root@server ~]# yum install postfix
```
To make postfix as default MTA (Mail transfer agent) for your system use the following command
```bash
[root@server ~]# alternatives --set mta /usr/sbin/postfix
```
If the above command doesn't work and you there is this output as `“/usr/sbin/postfix has not been configured as an alternative for mta“`. Then use the below command to do the same else skip it.
```bash
[root@server ~]# alternatives --set mta /usr/sbin/sendmail.postfix
```
-------------------------------------------------------------- 
# 2. Configuring postfix for the internet.
Edit Postfix configuration file `/etc/postfix/main.cf `in vim
```bash
[root@server ~]# vim /etc/postfix/main.cf 
  myhostname = mail.odii.cic
  mydomain = odii.cic
  myorigin = $mydomain
  inet_interfaces = all
  mydestination = $myhostname, localhost, $mydomain
  mynetworks = 127.0.0.0/8, /32, 192.168.8.2/24, /32,
  relay_domains = $mydestination
  home_mailbox = Maildir/
```
-------------------------------------------------------------- 
# 3. Starting postfix service.
 Restart Postfix service to read the changes in the configuration file. Also, set it to autostart on system boot.
```bash 
[root@server ~]# service postfix restart
[root@server ~]# chkconfig postfix on
```
-------------------------------------------------------------- 
# 4. Securing the postfix server.
Open firewall port by adding firewall rules to make postfix accessible from the outside.Type the following
```bash
[root@server ~]# iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 25 -j ACCEPT
[root@server ~]# iptables -A INPUT -m state --state NEW -m udp -p udp --dport 25 -j ACCEPT
```
-------------------------------------------------------------- 
5. Configuring postfix for the internet.

							    
#####                               References 
							
+ Negus, N.  2015: Linux Bible(9th edition). Indianapolis, Indiana: John Wiley & Sons Inc., pp. 347-375,477-497,708-713.

+ 2018, [Ahmer's SysAdmin Recipes ](https://tecadmin.net/install-and-configure-postfix-on-centos-redhat/ (accessed August 16,2019)) .

+ 2018, [MAIL TRANSPORT AGENTS ](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/s1-email-mta (accessed August 15,2019)) .


 							


