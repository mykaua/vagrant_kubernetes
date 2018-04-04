Vagrant.configure('2') do |config|
  config.vm.box = 'centos/7'
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.vm.provider :virtualbox do |v|
    v.memory = 512
    v.cpus = 1
  end

  #centos  master VM
  config.vm.define 'master' do |master|
    master.vm.hostname = 'master.local'
    master.vm.network :private_network, ip: '192.168.13.100'
    master.vm.synced_folder '.', '/home/vagrant/Builds', mount_options:['dmode=755,fmode=644']
  end

  # centos 1 node VM
  config.vm.define 'node01' do |node01|
    node01.vm.box = 'centos/7'
    node01.vm.hostname = 'node01.local'
    node01.vm.network :private_network, ip: '192.168.13.101'
  end

  # centos 2 node VM
  config.vm.define 'node02' do |node02|
    node02.vm.hostname = 'node02.local'
    node02.vm.network :private_network, ip: '192.168.13.102'
  end
end
