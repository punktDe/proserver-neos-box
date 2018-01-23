Vagrant.configure("2") do |config|
  config.vm.box = 'punktde/proserver'
  config.vm.synced_folder '.', '/vagrant', id: 'vagrant-root', disabled: true
  config.vm.network 'private_network', ip: '172.17.28.28'
  config.ssh.forward_agent = true
  config.vm.provider 'virtualbox' do |vb|
    vb.memory = '2048'
    vb.cpus = '1'
    vb.name = 'neos'
  end
  config.vm.provision 'shell', keep_color: true, inline: <<-SHELL
    sudo -u proserver rm -rf /var/www/welcome
    sudo -u proserver composer create-project --no-dev neos/neos-base-distribution /var/www/neos
    sudo -u proserver sh -c "cd /var/www/neos/ && ./flow neos.flow:cache:warmup"
    sudo sed -i conf 's/welcome/neos/' /usr/local/etc/nginx/vhosts/ssl.conf && sudo service nginx restart
    echo "################################################################################"
    echo "Go to https://172.17.28.28 and follow the NEOS installation process."
    echo "These are your database credentials:"
    echo "User: root"
    echo "Password: $(sudo cat /usr/local/etc/mysql-password)"
    echo "################################################################################"
  SHELL
end
