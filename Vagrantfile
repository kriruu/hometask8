
Vagrant.configure("2") do |config|
  config.vm.define "vm8" do |vm8|
    vm8.vm.hostname = "vm8"
    vm8.vm.box = "centos/7"
    vm8.vm.network "forwarded_port", guest: 80, host: 8888,host_ip: "127.0.0.1"
    vm8.vm.network "forwarded_port", guest: 443, host: 8443, host_ip: "127.0.0.1"



    vm8.vm.provider "virtualbox" do |vb|
      vb.name = "vm8"
      vb.gui = false
      vb.memory = "1024"
    end
    
    vm8.vm.provision "shell", run: "always",  inline: <<-SHELL
        echo "Hello from the Centos VM"
        setenforce 0
        sed -ie 's/^SELINUX=.*$/SELINUX=disabled/' /etc/selinux/config
        sudo mkdir -p /var/www/website.com/html/
        sudo mkdir -p /etc/httpd/conf.d/
        cp -rav /vagrant/www-content/* /var/www/website.com/html/
        cp -rav /vagrant/conf/* /etc/httpd/conf.d/
        sudo mkdir /etc/ssl/privatekey
        sudo chmod 700 /etc/ssl/privatekey
        sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/pki/tls/private/apache-selfsigned.key -out /etc/pki/tls/certs/apache-selfsigned.crt -subj "/C=UA/ST=Lvivska/L=Lviv/O=ITStep/OU=University/CN=127.0.0.1"
        sudo yum install -y epel-release
        sudo yum install -y httpd
        sudo yum install -y mod_ssl
        sudo httpd -t
        sudo systemctl start httpd
        
        
    SHELL
  end
  
  
end
