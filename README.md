# apple-macbookair-lid
Automatically shut down macbook air when the lid is closed


angelika@MacBook-Air-Angelika ~ % sudo su
Password:

sh-3.2# cd /Library/LaunchDaemons/

sh-3.2# cat com.user.loginscript.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
   <key>Label</key>
   <string>com.user.loginscript</string>
   <key>Program</key>
   <string>/Users/angelika/Library/Scripts/launch.sh</string>
   <key>RunAtLoad</key>
   <true/>
</dict>
</plist>

sh-3.2# ls -la com.user.loginscript.plist
-rw-r--r--  1 root  wheel  377  8 lis 22:57 com.user.loginscript.plist

sh-3.2# ls -la /Users/angelika/Library/Scripts/launch.sh
-rwxr-xr-x  1 root  staff  501  5 sty 21:24 /Users/angelika/Library/Scripts/launch.sh

sh-3.2# cat /Users/angelika/Library/Scripts/launch.sh
#!/bin/bash

while true
do
	LID=`ioreg -r -k AppleClamshellState | grep AppleClamshellState | cut -d " " -f 8`
	if [ "${LID}" = "Yes" ]; then
		MYDATE=`date "+%Y-%m-%d% %H:%M:%S"`
		echo -n "${MYDATE}" >> /Users/angelika/Library/Scripts/pokrywa.log
		echo " Pokrywa zamknieta. Zamykam system za 10 sekund" >> /Users/angelika/Library/Scripts/pokrywa.log
		sleep 10
		CHROMEPID=`ps uxa | grep -i chrome | awk {'print $2'} | sort -n | head -n 1`
		kill "${CHROMEPID}"
		shutdown -h now
	fi
	sleep 1
done

sh-3.2# launchctl load com.user.loginscript.plist

sh-3.2# launchctl list | grep loginscript | grep 0
120	0	com.user.loginscript

