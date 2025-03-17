---
title: Development Status
layout: default
nav_order: 2
---

# Kazoo-Classic Development Status

***Last Update: `15/03/2025`***

Welcome to our ongoing efforts to **maintain** and **modernize** [Kazoo](https://github.com/2600hz/kazoo), a robust, open-source telephony platform leveraging FreeSWITCH, Kamailio, CouchDB, and others.

## Planned Stages

As to not spread ourselves too thin, we will approach development in the following stages.

---

### Stage 1 - Kazoo Classic 4.3 - Alma/Rocky 8 Build & Packages

As it has been shown that the existing code base for all components can with little to no modification can run on Alma/Rocky/RHEL8 or Debain 11, the first step is to have a release that works that doesn't rely on the EOL'd Centos 7.

Therefore, apart from compatibility fixes, the first step is to work on building binaries and releasing a repository for RHEL8, specifically Alma Linux and associated metapackages to allow distributed or all-in-one installs.

This also includes changing Kamailio to use PostgreSQL instead of KazooDB (which is closed-source) for presence, watcher lists, and carrier allow listing.

---

### Stage 2 - Kazoo Classic 4.3.X - Add STIR/SHAKEN support

To comply with the US FCC regulations, STIR/SHAKEN support will be added.

---

### Stage 3 - Kazoo Classic 4.4 - Erlang and Dependency Updates / Bugfixing

Getting things on a current operating system is only the first step. Kazoo runs on Erlang 19, which reached EOL in 2019 and doesn't support later OpenSSL versions. Work has already begun by other talented individuals to move the codebase towards Erlang OTP 24 and 26.

Testing and review of this work will begin in ernest once stage 1 is completed.

Several known bugs such as the SSO login, T.38 bugs, and potentially the queue system (ACDC) will be targeted to be fixed.

---

### Stage 4 - Kazoo Classic 4.5 - Technology Changes, Feature Adds

Components like the mod_kazoo freeswitch module will be replaced with mod_amqp, mod_kazoo had its purpose when it was developed, but mod_amqp has its own benefits.

Additional modules or guides will be created to handle things like email-to-fax security, DKIM signing system notifications, frontend updates.

As CouchDB (the database Kazoo runs on) clusters well over lan, but poorly over wan, and HTTP replication of couchdb can become overloaded at large scale, middlware or a custom service will need to be developed for better inter-region replication.

---

### Stage 5 - TBD

Nothing to add at this stage, though it will likely be continuing to maintain and adding more modules and functionality, potentially adding integrations to popular platforms, reworking the billing modules/etc.

We may look into an open-source kazoo based softphone with updated push notifications.


---

## Current Contributors

We have many contributors on the discord channel. Additional users will be added below as the efforts become more coordinated. The current notable works that are moving this project towards completion can be found here.

### https://github.com/kageds
Especially the Erlang Updates in;
- https://github.com/kageds/kazoo_core
- https://github.com/kageds/kazoo_applications

And his Kamailio Postgres updates in
- https://github.com/kageds/kazoo-configs-kamailio/tree/4.3-postgres

### https://github.com/ruhnet
Updates to callflows (forked from OpenTelecom)
- https://github.com/ruhnet/monster-ui-callflows-ng

### https://github.com/mooseable
Work on adding additional security filters
- https://github.com/mooseable/kazoo-configs-kamailio/tree/4.3-postgres-extrafilters

Kazoo Binary Build Process for Alma/RHEL8
- https://github.com/kazoo-classic/kazoo

Monster-UI build process through docker
- https://github.com/mooseable/monster-ui/tree/4.3-dockerbuild

## Help Desired
- Though a labour of love, any funding would be appreciated when we are ready to take them
- Building RPM packages
- Erlang, Elixir, Rebar, Node/JS developers
- Experience in configuring Kamailio Routing
- Testers, especially those with infrastructure

## Contributing

The best way to coordinate efforts is to join our Discord server.

[<img src="../assets/images/discord.png" width="300px">](https://discord.gg/zRRGRePqd2)