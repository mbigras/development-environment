# Development Environment

> Ubuntu 16.04 instances to hack on

## Usage

```
git clone https://github.com/mbigras/development-environment
cd development-environment
vagrant up
vagrant status
vagrant ssh control -c 'echo $HOSTNAME: hello world'
vagrant ssh web1 -c 'echo $HOSTNAME: hello world'

ssh_config=$(mktemp)
vagrant ssh-config > $ssh_config

ssh -F $ssh_config control 'echo $HOSTNAME: hello world'
ssh -F $ssh_config web1 'echo $HOSTNAME: hello world'

vagrant ssh control
cd workdir
ansible all -m ping
ansible-playbook main.yml
```

## Important tips

* The /vagrant directory is shared with host machine

## Terms

* node - some machine (virtual or physical)
* host node - physical node used to create and connect to other nodes
* control node - virtual node, used to provision other nodes
* web node - virtual node, used to serve HTTP requests