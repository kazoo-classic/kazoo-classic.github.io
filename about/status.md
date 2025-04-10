---
title: Development Status
layout: default
nav_order: 2
---

# Kazoo-Classic Development Status

***Last Update: `10/04/2025`***

Welcome to our ongoing efforts to **maintain** and **modernize** [Kazoo](https://github.com/2600hz/kazoo), a robust, open-source telephony platform leveraging FreeSWITCH, Kamailio, CouchDB, and others.

## Planned Stages

As to not spread ourselves too thin, we will approach development in the following stages.

---

### Stage 1 - Kazoo Classic 4.3 - Alma/Rocky 8 Build & Packages

#### Part 1 - Build Server & Packages for 4.3

You can currently build from source on a mixture of Alma 8/9 and Debial 11/12. But having to build from source every time isn't efficient. Additionally, it doesn't suit those that want all in one servers.

Progress is underway to build first, all package binaries for Alma Linux 8 as a stop-gap until we can move to Alma/Rocky 9 and Debian 12.

Additionally, to support development and testing, a build server shall be built to build packages from the offical github repository for all planned releases of Kazoo on Alma/Rocky/Debian.

Milestones:
- Build packages for Alma/Rocky 8
- Build packages for Debian 11
- Automate nightly builds of development branches
- Test AIO Install
- Test Cluster Install

---

#### Part 2 - STIR/SHAKEN Implementation

STIR/SHAKEN combats caller ID spoofing by adding a signed Identity header to SIP INVITEs. This
will likely involve:

- Integrating with a certificate authority for digital certificates.
- Modifying the stepswitch module to generate and add the signed Identity header to
outbound SIP INVITEs.
- Configuring options for enabling/disabling STIR/SHAKEN and managing certificates.
- Working on Erlang OTP19 and 4.3 builds

---
### Stage 2 - Kazoo Classic 4.4

#### Part 1 - Erlang and Dependency Updates / Bugfixing

Kazoo version 4.3 targets Erlang OTP19, while the master branch targets OTP22+. Updating to
OTP26 will involve:

- Updating the build system, including the `make/erlang_version` file, to support OTP26.
- Checking for deprecated functions and modules from OTP19, updating them to modern
APIs.
- Testing compatibility with OTP26, focusing on distributed Erlang features critical to Kazoo

Milestones
- Version 4.4: Achieve compatibility with OTP26 and RHEL9, ready for production.
- Version 4.5: Add STIR/SHAKEN functionality, ensuring production readiness

---
### Stage ? - Technology Changes / Feature Adds

Components like the mod_kazoo freeswitch module will be replaced with mod_amqp, mod_kazoo had its purpose when it was developed, but mod_amqp has its own benefits.

As CouchDB (the database Kazoo runs on) clusters well over lan, but poorly over wan, and HTTP replication of couchdb can become overloaded at large scale, middlware or a custom service will need to be developed for better inter-region replication.

---
## Survey Notes

### Detailed Analysis of Updating Kazoo for Modern Systems and STIR/SHAKEN Implementation

This survey note provides a comprehensive analysis of the tasks required to update the Kazoo open-source VoIP project to run on modern operating systems and add STIR/SHAKEN functionality, based on available documentation and research. Kazoo, located at GitHub repository, is currently at version 4.3, targeting Erlang OTP19, and has not been maintained for 3-4 years. The goal is to achieve two milestones: version 4.4, bringing the software current and running on RHEL9, and version 4.5, adding STIR/SHAKEN support, both ready for production.

#### Background on Kazo

Kazoo is an open-source, distributed, highly scalable platform designed for telecom services, built on components like Erlang, FreeSWITCH, Apache CouchDB, and RabbitMQ. It provides API-based VoIP functionality for voice, video, and SMS services. The project's documentation, such as the installation guide at Kazoo Installation Guide, indicates that version 4.3 requires Erlang 19.3.x, while version 5.x and the master branch target Erlang 22.3, with recommendations to consult `make/erlang_version` for specific versions.

#### Updating to Erlang OTP26

Given that Kazoo currently targets OTP19 and the latest stable Erlang version is 27.3.2 as of April 2025, with OTP26 being a recent major release, updating to OTP26 or newer requires significant compatibility work. The process involves:

- **Build System Update**: The build system, defined in the `make/` directory, includes `make/erlang_version`, which likely contains a single line specifying the Erlang version (e.g., "22.3" for the master branch). This needs to be updated to "26.0" or later to support OTP26. Documentation suggests using tools like `kerl` for managing Erlang installations, which can be found at Erlang Downloads.
- **Deprecated Functions and Modules**: OTP19 is several versions behind OTP26, and changes between these versions include deprecations and new features. For example, OTP26 introduced support for Kernel TLS and changes in binary syntax, as noted in OTP26 Release Notes. Developers must review the codebase for deprecated functions, using tools like `dialyzer` to identify issues, and update to modern APIs.

- **Testing Compatibility**: Testing with OTP26 involves building Kazoo and running unit and integration tests, focusing on distributed Erlang features critical for Kazoo's architecture. This includes testing modules like `callflow`, `stepswitch`, `ecallmgr`, and `crossbar`, ensuring no regressions in functionality

#### Ensuring Compatibility with RHEL9

To run on RHEL9, Kazoo must be compatible with modern Linux distributions. This involves:
- Verifying that dependencies like FreeSWITCH, CouchDB, and RabbitMQ support RHEL9. For instance, RabbitMQ's compatibility with Erlang versions is detailed at RabbitMQ Erlang Requirements, noting support for Erlang 26.x starting with RabbitMQ 3.12.0.
- Updating installation scripts in `doc/installation.md` to reflect RHEL9 requirements, such as package dependencies and configuration.
- Testing the entire stack on RHEL9 to ensure no  OS-specific issues, such as kernel version incompatibilities or library conflicts.

#### Adding STIR/SHAKEN Functionality
STIR/SHAKEN, a protocol to combat caller ID spoofing, involves adding a digitally signed Identity header to SIP INVITE messages. Research from sources like STIR/SHAKEN Wikipedia and TransNexus Whitepaper indicates it uses public key infrastructure for authentication, with the Identity header containing a JSON Web Token (JWT) signed with a private key.

- Understanding Requirements: STIR/SHAKEN requires obtaining digital certificates from a trusted CA, such as the Secure Telephony Identity Policy Administrator (STI-PA), and generating signed Identity headers. The header includes the calling and called party numbers and an attestation level (e.g., "A" for full attestation), as detailed in CarrierX Documentation.
- Implementation Location: Kazoo's architecture involves `callflow` for call processing and `stepswitch` for carrier management. Given the requirement to add the Identity header to outbound SIP INVITEs, `stepswitch` is likely the appropriate module, as it handles routing calls to carriers. Documentation at Stepswitch Documentation confirms stepswitch manages offnet routing, suggesting modifications here for outbound calls.

- Steps to Implement:
  - Certificate Management: Integrate with a CA to obtain and manage certificates, storing the private key securely (e.g., in `system_config`). This involves implementing functions to load keys and generate signatures, potentially using Erlang's public_key module.
  - Modify Stepswitch: Locate the code in `stepswitch` (e.g., `stepswitch_resources`) responsible for constructing SIP INVITEs. Add logic to generate the Identity header, sign it with the private key, and include it in the SIP message. An example format is `Identity: <sip:originating-provider.com>;alg=ES256;sig=Base64EncodedJWT,` as seen in Twilio SHAKEN/STIR Blog.
  - Configuration: Add configuration options in system_config for enabling STIR/SHAKEN, specifying certificate paths, and managing attestation levels.
  - Testing: Test outbound calls with STIR/SHAKEN enabled, using tools like Wireshark to verify the Identity header, and ensure compatibility with carriers supporting verification.

#### Milestones and Deliverables

The milestones are:
-  Version 4.4: Achieve compatibility with OTP26 and RHEL9, ready for production. Deliverables include an updated build system, RHEL9 installation scripts, and test reports confirming stability.
- Version 4.5: Add STIR/SHAKEN functionality, ready for production. Deliverables include updated `stepswitch` code, configuration options, and test reports confirming functionality.

#### Additional Considerations

- Ensure all dependencies are updated to versions compatible with OTP26 and RHEL9, checking compatibility matrices like RabbitMQ Erlang Compatibility.
- Update documentation, such as doc/installation.md and applications/callflow/doc/custom_sip_headers.md, to reflect changes, as seen in Kazoo Custom SIP Headers.
- Address security concerns, such as secure handling of private keys, and test for performance impacts from adding STIR/SHAKEN signatures.

#### Summary Table of Tasks
| Task                              | Module/Location      | Description                                                            |
|-----------------------------------|----------------------|------------------------------------------------------------------------|
| Update to OTP26                   | Build System (make/) | Update erlang_version to OTP26, test build process.                    |
| Check Deprecated Code             | Codebase             | Review and update deprecated functions using dialyzer.                 |
| Test OTP26 Compatibility          | All Modules          | Run tests, focus on distributed Erlang features.                       |
| Ensure RHEL9 Compatibility        | Installation Scripts | Update scripts for RHEL9, verify dependencies, test stack.             |
| Integrate CA for STIR/SHAKEN      | System Configuration | Obtain certificates, implement key management.                         |
| Modify Stepswitch for Identity    | stepswitch_resources | Add logic to generate and sign Identity header in outbound SIP INVITEs |
| Configure STIR/SHAKEN Options     | system_config        | Add enable/disable options, certificate paths, attestation levels.     |
| Test STIR/SHAKEN Functionality    | Outbound Calls       | Verify Identity header, test with carriers, ensure compatibility.      |

This detailed plan ensures Kazoo is updated for modern systems and equipped with STIR/SHAKEN, meeting the specified milestones.

#### Key Citations
-  GitHub repository for Kazoo
-  Kazoo Installation Guide for CentOS7
-  Kazoo Custom SIP Headers Documentation
-  STIR/SHAKEN Overview on Wikipedia
-  Erlang OTP Downloads and Versions
-  OTP26 Release Notes
-  RabbitMQ Erlang Version Requirements
-  TransNexus Understanding STIR/SHAKEN Whitepaper
-  CarrierX STIR/SHAKEN How It Works
-  Twilio Everything You Need to Know About SHAKEN/STIR
-  Stepswitch VoIP Line Finder Documentation

---

## Current Contributors

We have many contributors on the discord channel. Additional users will be added below as the efforts become more coordinated. The current notable works that are moving this project towards completion can be found here.

### https://github.com/kageds
Especially the Erlang Updates in;
- [https://github.com/kageds/kazoo_core](https://github.com/kageds/kazoo_core)
- [https://github.com/kageds/kazoo_applications](https://github.com/kageds/kazoo_applications)

And his Kamailio Postgres updates in
- [https://github.com/kageds/kazoo-configs-kamailio/tree/4.3-postgres](https://github.com/kageds/kazoo-configs-kamailio/tree/4.3-postgres)

### https://github.com/ruhnet
Updates to callflows (forked from OpenTelecom)
- [https://github.com/ruhnet/monster-ui-callflows-ng](https://github.com/ruhnet/monster-ui-callflows-ng)

### https://github.com/mooseable
Work on adding additional security filters
- [https://github.com/mooseable/kazoo-configs-kamailio/tree/4.3-postgres-extrafilters](https://github.com/mooseable/kazoo-configs-kamailio/tree/4.3-postgres-extrafilters)

### https://github.com/openkazoo
Implementation of STIR/SHAKEN and other security features for Kazoo
- [https://github.com/openkazoo/martini](https://github.com/openkazoo/martini)


Kazoo Binary Build Process for Alma/RHEL8
- [https://github.com/kazoo-classic/kazoo](https://github.com/kazoo-classic/kazoo)

Monster-UI build process through docker
- [https://github.com/mooseable/monster-ui/tree/4.3-dockerbuild](https://github.com/mooseable/monster-ui/tree/4.3-dockerbuild)

## Help Desired
- Though a labour of love, any funding would be appreciated when we are ready to take them
- Building RPM packages
- Erlang, Elixir, Rebar, Node/JS developers
- Experience in configuring Kamailio Routing
- Testers, especially those with infrastructure

## Contributing

The best way to coordinate efforts is to join our Discord server.

[<img src="../assets/images/discord.png" width="300px">](https://discord.gg/zRRGRePqd2)