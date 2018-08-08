def main
  hosts = {
    'control' => '192.168.22.11',

    'web1' => '192.168.22.21',
    # 'web2' => '192.168.22.22',
    # 'web3' => '192.168.22.23',
    # 'web4' => '192.168.22.24',
    # 'web5' => '192.168.22.25',
    # 'web6' => '192.168.22.26',
  }

  create_development_inventory(hosts)
  build_vagrant_infrastructure(hosts)
end

def create_development_inventory(hosts)
  inventory = <<~INVENTORY
  [web]
  #{hosts.select { |h,_| h.match(/web/) }.keys.join("\n")}

  [web:vars]
  ansible_user = vagrant
  INVENTORY

  File.write('development', inventory)
end

def provision_control_script(hosts)
  <<~SCRIPT
  apt-add-repository -y ppa:ansible/ansible
  apt-get update -y
  apt-get install -y ansible

  cat <<SSH_CONFIG > /etc/ssh/ssh_config
  Host *
      StrictHostKeyChecking no
      SendEnv LANG LC_*
      HashKnownHosts yes
      GSSAPIAuthentication yes
      GSSAPIDelegateCredentials no
  SSH_CONFIG

  cat <<HOSTS > /etc/hosts
  127.0.0.1       localhost
  127.0.1.1       control control

  # The following lines are desirable for IPv6 capable hosts
  ::1     localhost ip6-localhost ip6-loopback
  ff02::1 ip6-allnodes
  ff02::2 ip6-allrouters
  #{hosts.map { |h,ip| [ip,h].join(' ')}.join("\n") }
  HOSTS

  if [ ! -L /home/vagrant/workdir ]; then
    ln -s /vagrant/ /home/vagrant/workdir
  fi
  SCRIPT
end

def build_vagrant_infrastructure(hosts)
  Vagrant.configure('2') do |config|
    config.ssh.insert_key = false

    config.vm.define :control do |t|
        t.vm.box = 'bento/ubuntu-16.04'
        t.vm.hostname = 'control'
        t.vm.network(:private_network, ip: hosts[t.vm.hostname])
        t.vm.provision('shell', inline: provision_control_script(hosts))
        t.vm.provision('file',
          source: '~/.vagrant.d/insecure_private_key',
          destination: '~/.ssh/id_rsa')
    end

    hosts.select { |h,_| h.match(/web/) }.each do |h, ip|
      config.vm.define h do |t|
          t.vm.box = 'bento/ubuntu-16.04'
          t.vm.hostname = h
          t.vm.network(:private_network, ip: ip)
      end
    end
  end
end

main
