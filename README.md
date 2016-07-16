# Ansible Role: exim4 (send only)

[![Build Status](https://travis-ci.org/tschifftner/ansible-role-exim4-sendonly.svg)](https://travis-ci.org/tschifftner/ansible-role-exim4-sendonly)

Installs exim4 (send only) and handles email addresses on Debian/Ubuntu linux servers.

## Requirements

None

## Dependencies

None.

## Installation

```
$ ansible-galaxy install tschifftner.exim4-sendonly
```

## Example Playbook

Available variables are listed below, along with default values (see `defaults/main.yml`):

```
 exim4_sendonly_hostname: '{{ ansible_hostname }}'
 
 exim4_sendonly_email_addresses:
   root: 'your@email.com'
```

The playbook could look like:
    
    - hosts: webservers
    
      roles:
         - { role: tschifftner.exim4-sendonly }


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

Use the ip addresses that ends with ```::2/64 Scope:Global```

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

Ansible          | Debian Jessie    | Ubuntu 14.04    | Ubuntu 12.04
:--------------: | :--------------: | :-------------: | :-------------: 
2.1           | Yes              | Yes             | Yes

## License

[MIT License](http://choosealicense.com/licenses/mit/)

## Author Information

 - [Tobias Schifftner](https://twitter.com/tschifftner), [ambimax® GmbH](https://www.ambimax.de)
