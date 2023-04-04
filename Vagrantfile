# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

# Get developers username.
user = ENV['USER']
# Opens developer specific vagrant config file containing options
# for ports and sync directories.
vars = YAML.load_file "./vagrant.config/vagrant.#{user}.yml"

# Reused vars for DRY.
guest_sync_dir = '/opt/www/iicreeks'
guest_port = '3000'
guest_db_port = '5432'

Vagrant.configure("2") do |config|
  config.vm.box = "mpasternak/focal64-arm"
  config.vm.define "development"
  config.vm.network "private_network", ip: "192.168.78.33"
  config.vm.network "forwarded_port", guest: guest_port,
    host: vars['hport'], auto_correct: true
  config.vm.network "forwarded_port", guest: guest_db_port,
    host: vars['dbhport'], auto_correct: true
  config.ssh.insert_key = false
  config.vm.synced_folder vars['hfolder'], guest_sync_dir, create: true

  config.vm.provider "parallels" do |prl|
    prl.name = "iicreeksApp-vm"
    prl.memory = 2048
    prl.cpus = 2
    prl.update_guest_tools = true
  end

  # Ansible provisioner.
  config.vm.provision "ansible" do |ansible|
    ansible.compatibility_mode="2.0"
    ansible.playbook = 'provision/setup.yml'
    ansible.host_vars = {
      "ansible_ssh_extra_args" => "-o StrictHostKeyChecking=no"
   }
    ansible.limit = "all"
    # ansible.galaxy_role_file = "ansible/requirements.yml"
    # ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} /
    # --roles-path=%{roles_path} --force"
    ansible.become = true
    ansible.extra_vars = {
      vagrant_ssh_keyfile: "~/.vagrant.d/insecure_private_key",
      host_db_port: vars['dbhport'],
      host_port: vars['hport'],
      remote_port: guest_port,
      remote_db_port:  guest_db_port,
      app_directory: guest_sync_dir
    }
    # Enables passing of args to Ansible from Vagrant CLI
    # via the ANSIBLE_ARGS environment variable
    if ENV["ANSIBLE_ARGS"]
      ansible.raw_arguments = Shellwords.shellsplit(ENV["ANSIBLE_ARGS"])
    end
  end
end