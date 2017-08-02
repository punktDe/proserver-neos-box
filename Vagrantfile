Vagrant.configure("2") do |config|
  config.vm.box = 'punktde/proserver'
  config.vm.box_url = "https://boxes.hosting.punkt.de/freebsd-110-ufs-2017Q3-ap24-php70.json"
  config.vm.synced_folder '.', '/vagrant', id: 'vagrant-root', disabled: true
  config.vm.network 'private_network', ip: '172.17.28.28'
  config.ssh.forward_agent = true
  config.vm.provider 'virtualbox' do |vb|
    vb.memory = '2048'
    vb.cpus = '1'
    vb.name = 'neos'
  end
  config.vm.provision 'shell', inline: <<-SHELL
    sed -i '' 's/FLOW_CONTEXT Development/FLOW_CONTEXT Production/g' /usr/local/etc/nginx/include/neos.conf
    service nginx reload
    sudo -u proserver rm -rf /var/www/neos/*
    sudo -u proserver composer create-project --no-dev neos/neos-base-distribution /var/www/neos
    echo "################################################################################"
    echo "Go to https://172.17.28.28 and follow the NEOS installation process."
    echo "These are your database credentials:"
    echo "User: root"
    echo "Password: $(sudo cat /usr/local/etc/mysql-password)"
    echo "################################################################################"
  SHELL
end