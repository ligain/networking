# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :inetRouter => {
    :box_name => "centos/7",
    :net => [
      {ip: '192.168.255.1', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
    ]
  },
  :centralRouter => {
    :box_name => "centos/7",
    :net => [
      {ip: '192.168.255.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
      {ip: '192.168.0.1', adapter: 3, netmask: "255.255.255.240", virtualbox__intnet: "directors-net"},
      {ip: '192.168.0.33', adapter: 4, netmask: "255.255.255.240", virtualbox__intnet: "hardware-net"},
      {ip: '192.168.0.65', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "managers-net"},
    ]
  },
  :centralServer => {
    :box_name => "centos/7",
    :net => [
      {ip: '192.168.0.2', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "directors-net"},
      {adapter: 3, auto_config: false, virtualbox__intnet: true},
      {adapter: 4, auto_config: false, virtualbox__intnet: true},
    ]
  },
  :office1Router => {
    :box_name => "centos/7",
    :net => [
      {ip: '192.168.0.3', adapter: 3, netmask: "255.255.255.128", virtualbox__intnet: "directors-net"},
      {ip: '192.168.2.1', adapter: 4, netmask: "255.255.255.128", virtualbox__intnet: "developers-office1-net"},
      {ip: '192.168.2.65', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "testers-office1-net"},
      {ip: '192.168.2.129', adapter: 6, netmask: "255.255.255.192", virtualbox__intnet: "managers-office1-net"},
      {ip: '192.168.2.193', adapter: 7, netmask: "255.255.255.192", virtualbox__intnet: "hardware-office1-net"},
    ]
  },
  :office1Server => {
    :box_name => "centos/7",
    :net => [
      {ip: '192.168.2.2', adapter: 2, netmask: "255.255.255.128", virtualbox__intnet: "office1-net"},
      {adapter: 3, auto_config: false, virtualbox__intnet: true},
      {adapter: 4, auto_config: false, virtualbox__intnet: true},
    ]
  },
  :office2Router => {
    :box_name => "centos/7",
    :net => [
      {ip: '192.168.0.4', adapter: 3, netmask: "255.255.255.128", virtualbox__intnet: "directors-net"},
      {ip: '192.168.1.1', adapter: 4, netmask: "255.255.255.128", virtualbox__intnet: "developers-office2-net"},
      {ip: '192.168.1.129', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "testers-office2-net"},
      {ip: '192.168.1.193', adapter: 6, netmask: "255.255.255.192", virtualbox__intnet: "hardware-office2-net"},
    ]
  }, 
  :office2Server => {
    :box_name => "centos/7",
    :net => [
      {ip: '192.168.1.2', adapter: 2, netmask: "255.255.255.128", virtualbox__intnet: "office2-net"},
      {adapter: 3, auto_config: false, virtualbox__intnet: true},
      {adapter: 4, auto_config: false, virtualbox__intnet: true},
    ]
  }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|

      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxname.to_s

      boxconfig[:net].each do |ipconf|
        box.vm.network "private_network", ipconf
      end

      if boxname.to_s == "office2Server"
        box.vm.provision "ansible" do |ansible|
          ansible.host_vars = MACHINES
          ansible.playbook = "playbook.yml"
          ansible.compatibility_mode = "2.0"
          ansible.limit = "all"
        end
      end

    end
  end
end