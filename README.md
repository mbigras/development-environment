# Development Environment

> Ubuntu 16.04 instances to hack on

## Usage

```
git clone https://github.com/mbigras/development-environment
cd development-environment
vagrant up
vagrant status
vagrant ssh foo -c 'echo hello $HOSTNAME'
ansible all -m ping
ansible-playbook main.yml
```

