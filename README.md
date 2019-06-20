# Bluetooth to Steam (linux)
A simple way to start Steam big picture when your wireless controller is turned on.
## Pre-requisite
1. Have steam installed
2. Have your bluetooth controller pair with your PC and steam

## Step-by-step
1. Install `bluez-tools`
```sh
$ apt-get install bluez-tools
```
2. Create a file to store the script somewhere in your home folder:
```sh
$ touch ~/bluetooth-to-steam-linux.sh
$ nano ~/bluetooth-to-steam-linux.sh
```
3. Paste the following script into it (exit with Control + X)
```sh
#!/bin/sh

case $(bt-device -i "Wireless Controller" | grep "Connected" | grep -Eo '[0-1]+$') in 1)
	echo Controller detected
	if ps aux | grep "$whoami" | grep "/bin/sh" | grep "/usr/games/steam"; then
		echo Steam detected, doing nothing
	else
		echo Steam not detected
		DISPLAY=:0.0 /usr/games/steam -bigpicture
    fi
esac
```
4. Make it runnable:
```sh
$ chmod +x ~/bluetooth-to-steam-linux
```
5. Enter in your cronjob:
```sh
$ crontab -e
```
6. Select `nano` in case is your first time running this command
7. Append to the end of the file. Note, replace <user> with your username.
```sh
* * * * * /bin/sh /home/<user>/bluetooth-to-steam-linux.sh >/dev/null 2>&1
```
