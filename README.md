ZHdK Switch-AAI Shibboleth leihs Authentication-System
======================================================

This repository hosts code to authenticate users via
[SWITCHaai](https://www.switch.ch/aai/) for the [_leihs_](https://github.com/leihs/leihs).

The resulting service is used as an external authentication-system, see
https://github.com/leihs/leihs/wiki/external_authentication, of leihs.


Production Set-up and Deployment
--------------------------------

We recommend to install this authentication system on dedicated host accessible
under a dedicated hostname via HTTPS. It is in theory possible to install this
service on the same machine which runs Leihs, but neither the automated
deployment supplied with leihs nor the one supplied with this software is
prepared for this case.


### Switch AAI Configuration and Registration

Install and set up Switch AAI and register application, see

* https://www.switch.ch/aai/guides/sp/installation/
* https://rr.aai.switch.ch/


https://www.switch.ch/aai/guides/sp/configuration/?osType=nonwindows&os=centos8&federation=SWITCHaai&hostname=switch-auth.hkb.leihs.app&entityID=https%3A%2F%2Fswitch-auth.hkb.leihs.app%2Fshibboleth&configDir=%2Fetc%2Fshibboleth%2F&keyPath=%2Fetc%2Fshibboleth%2Fsp-key.pem&certPath=%2Fetc%2Fshibboleth%2Fsp-cert.pem&targetURL=https%3A%2F%2Fswitch-auth.hkb.leihs.app%2FShibboleth.sso%2FSession&supportEmail=ops%40functional.swiss&submit=Update+Configuration+Guide+with+above+Data#setupprofile


### Deployment

This project comes with deployment recipes for [ansible](https://docs.ansible.com/).

We recommend to add this repository as an git submodule to the inventory of
your leihs installation. This inventory should also contain leihs itself as an
submodule.

An installation script, say located in `bin/aai-auth-deploy` from the top
of your inventory directory could look like:

```
#!/usr/bin/env bash

set -eux

INVENTORY_DIR="$(cd -- "$(dirname "${BASH_SOURCE}")" ; cd .. > /dev/null 2>&1 && pwd -P)"
HOSTS_FILE=hosts

${INVENTORY_DIR}/leihs/deploy/bin/ansible-playbook \
  -i ${INVENTORY_DIR}/${HOSTS_FILE} \
  ${INVENTORY_DIR}/leihs-switchaai-shibboleth-auth/deploy/deploy_play.yml $@
```



Development
-----------

### Run the application

1. Install ruby version >= 2.7.2, e.g. with [ruby-install](https://github.com/postmodern/ruby-install).

2. Export the path, e.g. with `bash`

      export PATH=~/.rubies/ruby-2.7.2/bin:$PATH

3. Install dependencies

      bundle

4. Start the server

      bundle exec passenger start --port 4000

5. Set up (per request) automatic Reloading (optional)

      touch tmp/always_restart.txt

### Prototyping and debugging

Try

		curl http://localhost:4000/debug

and

		bundle exec pry-remote

in a separate terminal.



