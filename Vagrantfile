VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "trusty64"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
  config.vm.network "private_network", ip: "192.168.13.37"
  config.vm.hostname = "appdev"

  #Add any alias:
  config.hostsupdater.aliases = [
    "app.dev"
  ]

  config.vm.synced_folder ".", "/vagrant", :nfs => true

  #Fix for Ansible bug resulting in an encoding error
  ENV['PYTHONIOENCODING'] = "utf-8"

  config.vm.provision "ansible" do |ansible|
    ansible.limit = "all"
    ansible.playbook = "ansible/development.yml"
    ansible.inventory_path = "ansible/hosts"
  end

  config.vm.provision "shell", inline: <<-SCRIPT
    ps cax | grep mailcatcher > /dev/null
    if [ ! $? -eq 0 ]; then
      mailcatcher --http-ip 192.168.13.37
    fi
  SCRIPT

  config.vm.post_up_message = "\n\nProvisioning is done, visit http://app.dev for your CakePHP application! \n\n(MySQL credentials are root:pass).\n\n"
end
