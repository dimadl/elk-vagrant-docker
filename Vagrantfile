VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "bento/ubuntu-16.04"
  config.ssh.forward_agent = true

  config.vm.network "forwarded_port", guest: 9200, host: 9200
  config.vm.network "forwarded_port", guest: 5601, host: 5601
  config.vm.network "forwarded_port", guest: 5044, host: 5044

  config.vm.provision 'shell', inline: <<-SHELL
    echo vm.max_map_count=262144 > /etc/sysctl.d/vm_max_map_count.conf
    sysctl --system
    grep -qF 'vagrant - nofile 65536' /etc/security/limits.conf || echo 'vagrant - nofile 65536' >> /etc/security/limits.conf
  SHELL

  config.vm.provision :docker

  config.vm.provision :docker_compose, rebuild: true, run: "always", yml: "/vagrant/docker-compose.yml"
end