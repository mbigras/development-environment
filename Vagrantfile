# Basic development environment with 3 hosts
os = 'bento/ubuntu-16.04'
hosts = [
    { 'name' => 'foo', 'ip' => '192.168.1.101'},
    { 'name' => 'bar', 'ip' => '192.168.1.102'},
    { 'name' => 'baz', 'ip' => '192.168.1.103'},
]

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.define hosts[0]['name'] do |t|
        t.vm.box = os
        t.vm.hostname = hosts[0]['name']
        t.vm.network(:private_network, ip: hosts[0]['ip'])
    end

    config.vm.define hosts[1]['name'] do |t|
        t.vm.box = os
        t.vm.hostname = hosts[1]['name']
        t.vm.network(:private_network, ip: hosts[1]['ip'])
    end

    config.vm.define hosts[2]['name'] do |t|
        t.vm.box = os
        t.vm.hostname = hosts[2]['name']
        t.vm.network(:private_network, ip: hosts[2]['ip'])
    end
end

ansible_inventory = 'development.ini'
inventory = <<EOF
[development]
#{hosts.map { |h| h['ip'] }.join("\n")}
EOF
File.write(ansible_inventory, inventory)
