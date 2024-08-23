


Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/jammy64"

  machines = ["mail-server", "ftp-server"]
  
  machines.each do |machine_name|
    config.vm.define machine_name do |cfg|
      # perform custom network setup here 

      cfg.vm.provision :ansible do |ansible|
        ansible.playbook = "playbooks/#{machine_name}-playbook.yml"
      end
    end
  end

end
