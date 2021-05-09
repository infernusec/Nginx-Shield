# Prevent accessing from Tor Exit nodes



## Step #1 (Download Exit-Tor Nodes)

create a new Directory that called "nginx-tor-block"

```
mkdir -p /opt/nginx-tor-block/
cd /opt/nginx-tor/block/
touch tor-update.sh
```



Create a new file that called "tor-update.sh" at "/opt/nginx-tor/block"

```bash
#!/bin/bash
wget -q https://www.dan.me.uk/torlist/ -O - | sed "s/^/deny /g; s/$/;/g" > /opt/nginx-tor-block/tor-ip.conf;
systemctl restart nginx
```

Set the script as executable and run it for a first time.

```bash
chmod +x tor-update.sh
./tor-update.sh
```



## Step #2  (Setting Nginx Configuration)

Then load the file at the "server" block or at a specific "location"

For example:

```c++
server {
	include /opt/nginx-tor-block/tor-list.conf; # Block Access On Server Block
	
	location / {
		include /opt/nginx-tor-block/tor-list.conf; # Block Access only for location
	}
}
```



Tor Exit Nodes Mirrors:

1. https://check.torproject.org/cgi-bin/TorBulkExitList.py
2. https://www.dan.me.uk/tornodes



## Step #3 (Auto Update with Cronjob)

```
echo "* * * * * root /opt/nginx-tor-block/tor-update.sh
```

