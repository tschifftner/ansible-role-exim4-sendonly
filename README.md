# Ansible Role: exim4 (send only)

[![Build Status](https://travis-ci.org/tschifftner/ansible-role-exim4-sendonly.svg?branch=master)](https://travis-ci.org/tschifftner/ansible-role-exim4-sendonly)

Installs exim4 (send only) and handles email addresses on Debian/Ubuntu linux servers.

## Requirements

None

## Dependencies

None.

## Installation

```
$ ansible-galaxy install tschifftner.exim4_sendonly
```

## Example Playbook

Available variables are listed below, along with default values (see `defaults/main.yml`):

```
 exim4_sendonly_email_addresses:
   root: 'your@email.com'
```

The playbook could look like:

    - hosts: webservers

      roles:
         - { role: tschifftner.exim4_sendonly }

## Use Smart Proxy

```
exim4_sendonly_enable_tls: true
exim4_sendonly_smarthost: ''
exim4_sendonly_username: ''
exim4_sendonly_password: ''
```

## Use as standalone email sender

### Set reverse dns for ipv6

Figure out your ipv6 address

```
ifconfig eth0
```

Use the ip addresses that ends with `::2/64 Scope:Global`

Also add AAAA Record for this IPv6 address!

### Set SPF records

TXT Record for Domain

```
v=spf1 a mx -all
v=spf1 a mx a:{{ ansible_fqdn }} -all
```

### Exim commands

Summary of all emails

```
mailq | exiqsumm
```

Print a list of messages in the queue

```
exim -bp
```

Remove single message

```
exim -Mrm {message-id}
```

Delete all messages from queue

```
exim -bp | awk '/^ *[0-9]+[mhd]/{print "exim -Mrm " $3}' | bash
```

## Test email sending

```
echo "This is a testmail." | mail -s "Testmail" root
echo "This is a testmail." | mail -s "Testmail" your@email.com
```

## Supported OS

 - Debian 9 (Stretch)
 - Debian 8 (Jessie)
 - Ubuntu 18.04 (Bionic Beaver)
 - Ubuntu 16.04 (Xenial Xerus)
 
## Required ansible version

Ansible 2.5+

## License

[MIT License](http://choosealicense.com/licenses/mit/)

## Author Information

*   [Tobias Schifftner](https://twitter.com/tschifftner), [ambimax® GmbH](https://www.ambimax.de)
