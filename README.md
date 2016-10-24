BrockResearch Rubyist [![Build Status](https://travis-ci.org/BrockResearch/rubyist.svg?branch=master)](https://travis-ci.org/BrockResearch/rubyist)
=========

BrockResearch Rubyist playbook builds an RVM managed Ruby development environment.  It includes the latest stable release of Ruby, RVM, and Git.  It also includes most popular gems for building command line and web applications Thor, Rake, Sinatra, Nokogiri, and SQLite.

Requirements
------------

BrockResearch Rubyist requires no software beyond Ansible itself. The playbook was built and tested with Ansible `2.1.1.0`.

```
$ ansible --version
ansible 2.1.1.0
  config file = 
  configured module search path = Default w/o overrides
```

Role Variables
--------------

All software installed by Rubyist (RVM, Git, Ruby, all GEMs, and GEM prerequisites) are pinned to specific release to ensure a repeatable and consistent deployment.

`rvm_installer` holds the full path to the RVM install script. 

```
rvm_installer: /tmp/rvm-installer
```

`rvm_installer_signature` holds the full path to the RVM install script's signature.

```
rvm_installer_signature: /tmp/rvm-installer.asc
```

The RVM install script and it's script signature are removed at the completion of the play.


The `rvm` variable is used in place of the actual, rvm binary.  Ansible's shell module does not load the user's bash profile. Prefixing the command with `source ~/.bash_profile` at the start of the session immediately before for the command loads the path to rvm.

```
rvm: source ~/.bash_profile;rvm
```


`default_ruby_version` sets RVM's current and default Ruby version.

```
default_ruby_version: 2.3.0
```

`ruby_versions` list all the Ruby versions installed and managed by RVM.  At a minimum `ruby_versions` includes `default_ruby_version`.

```
ruby_versions:
  - '{{default_ruby_version}}'
```


The `gem` variable is used in place of the actual, gem binary.  Just as with `rvm`, prefixing the command with `source ~/.bash_profile` at the start of the session immediately before for the command loads the path to gem.

```
gem: source .bash_profile;gem
```


The `default_gemset` variable is dictionary of RVM's default gems and their prerequisites.
```
default_gemset:
  prereq_pkgs:
    # nokogiri & libxml-ruby
    - {name: libxml2-dev, version: 2.9.1+dfsg1-3ubuntu4.8}
  gems:
    - {name: rspec, version: 3.5.0}
    - {name: cucumber, version: 2.4.0}
    - {name: gherkin, version: 4.0.0}
    - {name: aruba, version: 0.14.2}
    - {name: rake, version: 11.3.0}
    - {name: thor, version: 0.19.1}
    - {name: daemons, version: 1.2.4}
    - {name: eventmachine, version: 1.2.0.1}
    - {name: libxml-ruby, version: 2.9.0}
    - {name: nokogiri, version: 1.6.8.1}
    - {name: json, version: 2.0.2}
    - {name: bson, version: 4.1.1}
    - {name: psych, version: 2.1.1}
    - {name: mongo, version: 2.3.0}
    - {name: sqlite3, version: 1.3.12}
    - {name: httparty, version: 0.14.0}
    - {name: rest-client, version: 2.0.0}
    - {name: highline, version: 1.7.8}
    - {name: bcrypt, version: 3.1.11}
```

Dependencies
------------

Rubyist is a self contained roles.


Installation
----------------
Rubyist was initially developed and tested on Ansible 2.1.1.0 and Vagrant 1.8.5.

To install:
```
ansible-galaxy install brockresearch.rubyist
```


Test Playbook
----------------

To test the playbook by executing Ansible against localhost create a simple playbook, `rubyist.yaml` the local machine with the following contents.

```
---
- hosts: localhost
  connection: local
  remote_user: root
  roles:
    - BrockResearch.rubyist
```

Once the playbook is created execute:
```
$ ansible-playbook -i "localhost, " rubyist.yaml 
```

Success execution of the above command will produce output similar to the following.
```
statically included: /etc/ansible/roles/BrockResearch.rubyist/tasks/rvm.yaml
statically included: /etc/ansible/roles/BrockResearch.rubyist/tasks/ruby.yaml
statically included: /etc/ansible/roles/BrockResearch.rubyist/tasks/gem.yaml

PLAY [localhost] ***************************************************************

TASK [setup] *******************************************************************
ok: [localhost]

TASK [BrockResearch.rubyist : Install mpapis public key] ***********************
changed: [localhost]

TASK [BrockResearch.rubyist : Download the installer] **************************
changed: [localhost]

TASK [BrockResearch.rubyist : Download the installer signature] ****************
changed: [localhost]

TASK [BrockResearch.rubyist : Verify the installer signature] ******************
changed: [localhost]

TASK [BrockResearch.rubyist : Run the installer] *******************************
changed: [localhost]

TASK [BrockResearch.rubyist : Install RVM requirements] ************************
changed: [localhost]

RUNNING HANDLER [BrockResearch.rubyist : Remove the installer] *****************
changed: [localhost]

RUNNING HANDLER [BrockResearch.rubyist : Remove the installer signature] *******
changed: [localhost]

TASK [BrockResearch.rubyist : Install the interpreter] *************************
changed: [localhost] => (item=2.3.0)

TASK [BrockResearch.rubyist : Set the installed interpreter as the default] ****
changed: [localhost]

TASK [BrockResearch.rubyist : Install default gems prerequisite OS packages] ***
ok: [localhost] => (item={u'version': u'2.9.1+dfsg1-3ubuntu4.8', u'name': u'libxml2-dev'})

TASK [BrockResearch.rubyist : Install default gems] ****************************
changed: [localhost] => (item={u'version': u'3.5.0', u'name': u'rspec'})
changed: [localhost] => (item={u'version': u'2.4.0', u'name': u'cucumber'})
changed: [localhost] => (item={u'version': u'4.0.0', u'name': u'gherkin'})
changed: [localhost] => (item={u'version': u'0.14.2', u'name': u'aruba'})
changed: [localhost] => (item={u'version': u'11.3.0', u'name': u'rake'})
changed: [localhost] => (item={u'version': u'0.19.1', u'name': u'thor'})
changed: [localhost] => (item={u'version': u'1.2.4', u'name': u'daemons'})
changed: [localhost] => (item={u'version': u'1.2.0.1', u'name': u'eventmachine'})
changed: [localhost] => (item={u'version': u'2.9.0', u'name': u'libxml-ruby'})
changed: [localhost] => (item={u'version': u'1.6.8.1', u'name': u'nokogiri'})
changed: [localhost] => (item={u'version': u'2.0.2', u'name': u'json'})
changed: [localhost] => (item={u'version': u'4.1.1', u'name': u'bson'})
changed: [localhost] => (item={u'version': u'2.1.1', u'name': u'psych'})
changed: [localhost] => (item={u'version': u'2.3.0', u'name': u'mongo'})
changed: [localhost] => (item={u'version': u'1.3.12', u'name': u'sqlite3'})
changed: [localhost] => (item={u'version': u'0.14.0', u'name': u'httparty'})
changed: [localhost] => (item={u'version': u'2.0.0', u'name': u'rest-client'})
changed: [localhost] => (item={u'version': u'1.7.8', u'name': u'highline'})
changed: [localhost] => (item={u'version': u'3.1.11', u'name': u'bcrypt'})

PLAY RECAP *********************************************************************
localhost                  : ok=13   changed=11   unreachable=0    failed=0   
```

License
-------

Licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
