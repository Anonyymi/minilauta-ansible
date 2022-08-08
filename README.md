# Minilauta Ansible

Ansible playbook + roles deploying a [TinyIB](https://github.com/Anonyymi/minilauta-tinyib) imageboard.

## Vagrant operations
```bash
# create machine
vagrant up
# provision again
vagrant provision
# configure tinyib
vagrant ssh -c "cd /vagrant; ansible-playbook -i inventories/vagrant -t configure playbook.yml"
```

## Ansible operations
```bash
# deploy prod
ansible-playbook -i inventories/prod playbook.yml
# configure tinyib
ansible-playbook -i inventories/prod -t configure playbook.yml
```

## Notes

* create ssh folder with priv/pub keys for git repo access if pulling tinyib from private repo
* the acme role won't obviously work in local vagrant environment
* for production setups, you should create "prod" host with "prod" group and own group_vars
* steps for taking new ubuntu server into use
  * create server user
    * adm dialout cdrom floppy sudo audio dip video plugdev netdev lxd
  * create server ssh key
  * modify sudoers sudo group perms
    * %sudo   ALL=(ALL:ALL) NOPASSWD: ALL
