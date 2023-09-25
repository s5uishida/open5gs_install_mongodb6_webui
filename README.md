# Install MongoDB 6.0 and Open5GS WebUI
MongoDB 6.0.4 released on 2023.01.26 now supports Ubuntu 22.04.
The process for the following OS is shown here.

- Ubuntu 20.04
- Ubuntu 22.04

---

<a id="toc"></a>

## Table of Contents

- [Install MongoDB 6.0](#install_mongodb)
- [Install Open5GS WebUI](#install_webui)
- [Changelog (summary)](#changelog)

---
<a id="install_mongodb"></a>

## Install MongoDB 6.0

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

<a id="install_webui"></a>

## Install Open5GS WebUI

It is assumed that MongoDB 6.0 has been installed already.  
First, install Node.js, see [here](https://github.com/nodesource/distributions).
```
# apt install -y ca-certificates curl gnupg
# mkdir -p /etc/apt/keyrings
# curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
# echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_20.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list
# apt update
# apt install -y nodejs
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
<a id="changelog"></a>

## Changelog (summary)

- [2023.09.02] Updated the Node.js installation procedure.
- [2023.02.10] Initial release.
