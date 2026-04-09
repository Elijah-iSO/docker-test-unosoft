Vagrant.configure("2") do |config|
  config.vm.box = "cloud-image/ubuntu-24.04"
  config.vm.hostname = "machineB"

  config.vm.provider :libvirt do |lv|
    lv.memory = "1024"
    lv.cpus = 1
  end

  # macvtap для реального IP в локальной сети
  config.vm.network "public_network",
    dev: "wlp0s20f3",
    mode: "bridge",
    type: "direct",
    ip: "192.168.1.198",
    auto_config: false

  config.vm.provision "shell", privileged: true, inline: <<-SHELL
    echo "=== Установка cqlsh ==="
    snap install cqlsh

    echo "=== Настройка hostname ==="
    hostnamectl set-hostname machineB
    echo "machineB" > /etc/hostname

    echo "=== Настройка статического IP 192.168.1.198 ==="
    # Находим второй интерфейс (ens6 или подобный)
    INTERFACE=$(ip -o link show | awk -F': ' '{print $2}' | grep -E '^(ens|enp|eth)' | tail -n1)

    cat > /etc/netplan/99-static-ip.yaml <<'EOF'
network:
  version: 2
  ethernets:
    '${INTERFACE}':
      dhcp4: no
      addresses:
        - 192.168.1.198/24
      # gateway4: 192.168.1.1   # раскомментируй и поменяй, если нужен интернет с machineB
EOF

    netplan apply

    echo "=== Machine B ГОТОВА ==="
    echo "Hostname          : $(hostname)"
    echo "Интерфейс с IP    : ${INTERFACE}"
    echo "IP адреса:"
    ip -4 addr show | grep -E "inet "
    echo ""
    echo "Проверка подключения к Cassandra:"
    echo "cqlsh 192.168.1.200"
  SHELL
end

