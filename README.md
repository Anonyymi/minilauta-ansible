# Minilauta Ansible

Ansible playbook + roles deploying a [TinyIB](https://github.com/Anonyymi/minilauta-tinyib) imageboard.

## Vagrant operations (local dev env)
```bash
# create machine
vagrant up
# provision again
vagrant provision
# configure tinyib
vagrant ssh -c "cd /vagrant; ansible-playbook -i inventories/vagrant -t configure playbook.yml"
```

## Ansible operations (remote prod env)
```bash
# deploy prod
ansible-playbook -i inventories/prod playbook.yml
# configure tinyib
ansible-playbook -i inventories/prod -t configure playbook.yml
```

## Preparations (any env)

1. Set `tinyib.init` & `tinyib.install_banners` to `true` on 1st provision

## Preparations (remote prod env)

1. Add private SSH key for ansible (production) deployments to `ssh/id_rsa`
2. Add new ansible inventory `prod` and configure it properly

## Notes

* The acme-role won't obviously work in local vagrant environment
* Steps for bootstrapping a fresh Ubuntu 20.04 server
  * Create server user and group, eg. server:server
  * Add the server user to following groups
    * adm dialout cdrom floppy sudo audio dip video plugdev netdev lxd
  * Modify sudoers sudo group permissions
    * %sudo   ALL=(ALL:ALL) NOPASSWD: ALL
  * Create a SSH key for the server
    * Add the private SSH key as instructed in the `Preparations` section above
