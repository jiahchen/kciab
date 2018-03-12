# K8S Prepare VM Vagrantfile

nodes = [
  { :hostname => 'master', :ip => '10.101.136.210', :box => 'ubuntu/xenial64', :ram => '16384', :cpu => '4' },
  { :hostname => 'node1', :ip => '10.101.136.211', :box => 'ubuntu/xenial64', :ram => '16384', :cpu => '4' },
  { :hostname => 'node2', :ip => '10.101.136.212', :box => 'ubuntu/xenial64', :ram => '16384', :cpu => '4' }
]

Vagrant.configure("2") do |config|
  nodes.each do |node|
    config.vm.define node[:hostname] do |node_config|
      node_config.vm.hostname = node[:hostname]
      node_config.vm.box = node[:box]
      node_config.vm.network "public_network", ip: node[:ip], bridge: "eno3"
      node_config.vm.provider "virtualbox" do |v|
        v.name = node[:hostname]
        v.memory = node[:ram]
        v.cpus = node[:cpu]
      end

      node_config.vm.provision "shell",
        run: "always",
        inline: "route add default gw 10.101.136.1"

      node_config.vm.provision "shell",
        run: "always",
        inline: "eval `route -n | awk '{ if ($8 ==\"enp0s3\" && $2 != \"0.0.0.0\") print \"route del default gw \" $2; }'`"

      node_config.vm.provision "shell",
        run: "always",
        inline: "sudo ufw disable"

      node_config.vm.provision "shell",
        run: "always",
        inline: "sudo swapoff -a"

      node_config.vm.provision "shell",
        run: "always",
        path: "install.sh"
    end
  end
end
