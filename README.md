# pihole-migration
pi-hole migration from v5 to v6
pi-hole -up / pi-hole -r works on (unspported) Rapsberry OS 10, but 
https://xxx.xxx.xxx.xxx/admin leads to 403 Forbidden message

# Solution
check pi-hole -d

This will show that lighttpd is using the standard ports 80 (http)/443 (https) as of 
https://docs.pi-hole.net/main/prerequisites/#ports

# Stop the lighttpd server
It was mandatory prior to pi-hole v6, which is no more the case
service lighttpd stop
Restart pi-hole
https://pi.hole/admin is working, but https://xxx.xxx.xxx.xxx/admin is not working

# Change the lighttpd ports
<<
sudo vim /etc/lighttpd/lighttpd.conf 

change from

server.port = 80 

to 

server.port = 8080

sudo vim /etc/lighttpd/conf-enabled/external.conf

change from 
$SERVER["socket"] == ":443" {

to

$SERVER["socket"] == ":8443" {
>>

# Start lighttpd server again
service lighttpd stop

# Change online in pi-hole 
Go to All settings / Websserver and API settings / webserver.domain
change Value (from) pi.hole (to) ip-addr

# Note
Reboot to see if all changes are persistent and pi-hole v6 is still working
