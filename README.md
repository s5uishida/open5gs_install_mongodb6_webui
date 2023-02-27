# Install MongoDB 6.0 and Open5GS WebUI
MongoDB 6.0.4 released on 2023.01.26 now supports Ubuntu 22.04.

---

<h2 id="toc">Table of Contents</h2>

- [Install MongoDB 6.0](#install_mongodb)
  - [For Ubuntu 20.04](#ubuntu2004)
  - [For Ubuntu 22.04](#ubuntu2204)
- [Install Open5GS WebUI](#install_webui)
- [Changelog (summary)](#changelog)

---
<h2 id="install_mongodb">Install MongoDB 6.0</h2>

<h3 id="ubuntu2004">For Ubuntu 20.04</h3>

```
# apt update
# apt install wget gnupg software-properties-common ca-certificates lsb-release
# wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | apt-key add -
# echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu $(lsb_release -cs)/mongodb-org/6.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-6.0.list
# apt update
# apt install -y mongodb-org
```
```
# systemctl enable mongod
# systemctl start mongod
```

<h3 id="ubuntu2204">For Ubuntu 22.04</h3>

```
# apt update
# apt install wget gnupg software-properties-common ca-certificates lsb-release
# wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | gpg --dearmor -o /etc/apt/trusted.gpg.d/mongodb-6.gpg
# echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu $(lsb_release -cs)/mongodb-org/6.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-6.0.list
# apt update
# apt install -y mongodb-org
```
```
# systemctl enable mongod
# systemctl start mongod
```

<h2 id="install_webui">Install Open5GS WebUI</h2>

It is assumed that MongoDB 6.0 has been installed already.  
First, install Node.js.
```
# apt install wget
# wget -qO - https://deb.nodesource.com/setup_18.x | bash -
# apt install nodejs
```
Then, install Open5GS WebUI.
```
# git clone https://github.com/open5gs/open5gs
# cd open5gs/docs/assets/webui
# bash install
```
If necessary, set the IP address and port to bind as follows (ex. `192.168.0.111:3000`).

`/lib/systemd/system/open5gs-webui.service`
```diff
--- open5gs-webui.service.orig  2023-02-10 22:59:41.808165291 +0900
+++ open5gs-webui.service       2023-02-10 21:56:30.191835861 +0900
@@ -7,6 +7,8 @@
 
 WorkingDirectory=/usr/lib/node_modules/open5gs
 Environment=NODE_ENV=production
+Environment=HOSTNAME=192.168.0.111
+Environment=PORT=3000
 ExecStart=/usr/bin/node server/index.js
 Restart=always
 RestartSec=2
```
Update system services.
```
# systemctl daemon-reload
# systemctl restart open5gs-webui
```
---
<h2 id="changelog">Changelog (summary)</h2>

- [2023.02.10] Initial release.
