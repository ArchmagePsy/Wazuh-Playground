networking = {
  "mail-server" => [
    [:private_network, "192.0.1.1", "services-internal", "255.255.255.240"],
    [:private_network, "192.0.1.17", "wazuh-manager-agents", "255.255.255.240"]
  ],
  "ftp-server" => [
    [:private_network, "192.0.1.2", "services-internal", "255.255.255.240"],
    [:private_network, "192.0.1.18", "wazuh-manager-agents", "255.255.255.240"]
  ],
  "redis-db" => [
    [:private_network, "192.0.1.3", "services-internal", "255.255.255.240"],
    [:private_network, "192.0.1.19", "wazuh-manager-agents", "255.255.255.240"]
  ],
  "forward-proxy" => [
    [:private_network, "192.0.1.4", "services-internal", "255.255.255.240"],
    [:private_network, "192.0.1.20", "wazuh-manager-agents", "255.255.255.240"]
  ],
  "wazuh-manager" => [
    [:private_network, "192.0.1.21", "wazuh-manager-agents", "255.255.255.240"],
    [:private_network, "192.0.1.33", "wazuh-internal", "255.255.255.240"],
  ],
  "wazuh-indexer" => [
    [:private_network, "192.0.1.34", "wazuh-internal", "255.255.255.240"]
  ],
  "wazuh-dashboard" => [
    [:private_network, "192.0.1.35", "wazuh-internal", "255.255.255.240"]
  ]
}


Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/jammy64"

  machines = ["wazuh-indexer", "wazuh-manager", "mail-server", "ftp-server", "redis-db", "forward-proxy", "wazuh-dashboard"]
  
  machines.each do |machine_name|
    config.vm.define machine_name do |cfg|
      cfg.vm.hostname = machine_name

      if machine_name == "wazuh-dashboard" then
        cfg.vm.network :forwarded_port, guest: 443, host: 8080
      end

      networking[machine_name].each do |subnet|
        cfg.vm.network subnet[0], ip: subnet[1], virtualbox__intnet: subnet[2], netmask: subnet[3]
      end

      cfg.vm.provider :virtualbox do |vbox|
        if machine_name == "wazuh-indexer" then
          vbox.memory = 4096
        end

        vbox.name = "wazuh_playground-#{machine_name}"
      end

      cfg.vm.provision :ansible do |ansible|
        ansible.playbook = "playbooks/#{machine_name}-playbook.yml"
      end

    end
  end

end
