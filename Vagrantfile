


Vagrant.configure("2") do |config|

  config.vm.box = "hasicorp/bionic64"

  machines = ["forward-proxy", "redis-db", "ftp-server", "mail-server"]
  
  machines.each do |machine_name|
    config.vm.define machine_name do |cfg|
      # perform custom network setup here 

      cfg.vm.provision :ansible do |ansible|
        ansible.playbook = "playbooks/#{machine_name}-playbook.yml"
      end
    end
  end

end
