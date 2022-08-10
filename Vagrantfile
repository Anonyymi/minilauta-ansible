$bootstrap_script = <<SCRIPT
apt-get update -yq && apt-get install -yq python3 python3-pip
su - vagrant -c "pip install --user ansible"
su - vagrant -c "echo 'export PATH=\${PATH}:~/.local/bin' >> ~/.bashrc"
su - vagrant -c "ansible-galaxy install -r /vagrant/requirements.yml"
su - vagrant -c "cp /vagrant/ssh/id_rsa ~/.ssh/id_rsa || true"
su - vagrant -c "chmod 0700 ~/.ssh/id_rsa || true"
SCRIPT

$set_env_script = <<SCRIPT
tee "/etc/profile.d/set_env.sh" > "/dev/null" <<EOF
# Ansible env vars
#export ANSIBLE_VERBOSITY=4
EOF
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.define "deploy" do |machine|
    machine.vm.box = "bento/ubuntu-20.04"
    machine.vm.network "private_network", ip: "10.11.0.0"
    machine.vm.provision "shell", inline: $set_env_script, run: "always"
    machine.vm.provision "shell", inline: $bootstrap_script
  end
  config.vm.define "tinyib", primary: true do |machine|
    machine.vm.box = "bento/ubuntu-20.04"
    machine.vm.network "private_network", ip: "10.10.0.0"
    machine.vm.provision "shell", inline: $set_env_script, run: "always"
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.galaxy_role_file = "requirements.yml"
      ansible.galaxy_command = "ansible-galaxy install -r %{role_file}"
      ansible.inventory_path = "inventories/vagrant"
      ansible.limit = "all"
      ansible.install_mode = "pip"
      ansible.pip_install_cmd = "sudo apt-get update -yq && sudo apt-get install -yq python3 python3-pip"
      ansible.extra_vars = {
        ansible_python_interpreter: "/usr/bin/python3"
      }
    end
  end
end
