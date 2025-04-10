---
title: Home
layout: home
nav_order: 1
---
![Kazoo Classic](assets/images/logo-name.svg)

# Kazoo Classic

This is a community supported hard-fork of 2600hz's Kazoo, from version 4.3 as their v5.x version is not likely to be released any time soon and that 4.3 packages are only available for CentOS 7, which is End of Life.

The community has put efforts in to support Kazoo v4.3 on later, in-support operating systems, and add additional functionality.

If you would like to contribute to the maintenance or improvement of Kazoo-Classic, you can join our Discord or by forking any of our repositories at [https://github.com/kazoo-classic/](https://github.com/kazoo-classic/) and submitting a pull request.

If you would like to contribute to this documentation or add guides, please fork https://github.com/kazoo-classic/kazoo-classic.github.io, make your changes then submit a Pull Request.

[<img src="assets/images/discord.png" width="300px">](https://discord.gg/zRRGRePqd2)

# Testing Status

Current versions and operating systems that have been successfully tested.

🛋️  CouchDB [Version: 3.2.3] [OS: Debian 12] \
☑️ Running and working. Requires special HAPROXY config to reroute port 5986

🛜  Freeswitch [Version: 1.10.9] [OS: Debian 11] \
☑️ Running and working. Anything later than 1.10.9 fails to work with legacy messaging from kazoo-applications on the AMQP bus and crashes freeswitch.  Requires a signalwire personal access token to access the repo

🔐  Kamailio [Version: 5.5.7] [OS: Debian 11] \
☑️ Running and with a phone registered. Requires use of PostgreSQL instead of KazooDB. Config exists thanks to a fork from ruhnet. Added another fork for additional security filtering.

⚙️  Kazoo-Applications [Version: 4.3 with OTP 19.3] [OS: Alma Linux 8] \
☑️ Running and working. Requires separate RabbitMQ node/docker image. 

🐇  RabbitMQ  [Version: 3.13.7] [OS: Docker] \
☑️ No notes other than its best to be run inside a docker container on the same node as your kazoo-applications node.

👾  Monster-UI [Version: 4.3] [OS: Alma Linux 9] \
☑️ Working. Compiles via a docker image. Published the code in a separate branch.

## Feature Testing
✅ SUP commands (create account, get rates, etc)\
✅ API access (auth, get devices, create devices, get callflows)\
✅ Web Interface/Monster UI (create accounts, create device, create callflows)\
✅ Device Registration\
✅ Place an internal call\
✅ Media playback audio working\
✅ Place an internal call to another device\
✅ Two way audio\
✅ Call Hold/Transfer\
✅ Feature codes (park/pickup/etc)\
✅ Place an outbound trunk call\
✅ Receive an inbound trunk call\
✅ Calls last > 90 seconds\
❔ Faxing\
✅ Voicemail\
✅ Voicemail to email\
✅ Email notifications\
✅ Additional Kamailio roles (Traffic Filter, antiflood, etc)\
❔ SIP SIMPLE messaging\
✅ BLF/Presence


# Official Links

-  [2600hz Kazoo Forums](http://forums.2600hz.com/)

-  [2600hz Open Source Website](https://2600hz.org/)