Vagrant.configure("2") do |config|
  config.vm.box = "cloud-image/ubuntu-24.04"
  
  # ============================================
  # Машина А: 192.168.1.197
  # ============================================
  config.vm.define "machine-a" do |a|
    a.vm.hostname = "machine-a"
    
    a.vm.provider :libvirt do |libvirt|
      libvirt.memory = "4096"
      libvirt.cpus = 2
    end
    
    a.vm.network "private_network", 
      ip: "192.168.1.197",
      libvirt__network_name: "test_net",
      libvirt__dhcp_enabled: false
  end
  
  # ============================================
  # Машина Б: 192.168.1.198
  # ============================================
  config.vm.define "machine-b" do |b|
    b.vm.hostname = "machine-b"
    
    b.vm.provider :libvirt do |libvirt|
      libvirt.memory = 1024
      libvirt.cpus = 1
    end
    
    b.vm.network "private_network", 
      ip: "192.168.1.198",
      libvirt__network_name: "test_net",
      libvirt__dhcp_enabled: false
  end
  
end
