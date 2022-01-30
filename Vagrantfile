# -*- mode: ruby -*-
# vi: set ft=ruby :

#create a wallet at phantom.app
$wallet = "E5y6EU9LPWAZJpAaZehsjJDgSJPS75JXdsRDJdjqi5ep"
$cpu_threads = 1
$max_cpu = 80
$miner_name = "vb_rig"

$script = <<-'SCRIPT'
  WALLET=$1
  MINER_NAME=$2

  sudo apt update
  sudo apt install -y curl build-essential cmake libuv1-dev libssl-dev libhwloc-dev unzip

  if ! [ -e ./xmrig ]; then 
          curl -JLO https://github.com/xmrig/xmrig/archive/refs/heads/master.zip
          unzip xmrig-master.zip && mv xmrig-master xmrig
          pushd xmrig/src
          sed -i 's/DonateLevel = 1;/DonateLevel = 0;/g' donate.h
          popd
          mkdir xmrig/build && cd xmrig/build
          cmake ..
          make -j$(nproc)
  else
          pushd xmrig/build
  fi

  sudo ./xmrig -o rx.unmineable.com:3333 -a rx -k -u SOL:$WALLET.$MINER_NAME#mo97-e2x2
SCRIPT

Vagrant.configure("2") do |config|
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/focal64"
  #config.vm.provision :shell, path: "bootstrap.sh"
  # config.vm.network :forwarded_port, guest: 80, host:4567

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false 

    # Customize the amount of memory and CPUs on the VM:
    vb.memory = "2096"
    vb.cpus = $cpu_threads
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "#{$max_cpu}"]
  end

  random_num = rand(100..999)
  config.vm.provision "shell" do |s|
    s.inline = $script
    s.args = [$wallet, $miner_name << random_num.to_s]
  end
end

