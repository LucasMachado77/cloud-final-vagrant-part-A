Vagrant.configure("2") do |config|

  config.vm.define "lb" do |lb|
    lb.vm.box = "bento/ubuntu-22.04"
    lb.vm.hostname = "lb"
    lb.vm.network "private_network", ip: "192.168.56.10"
    lb.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y nginx
      echo 'Load Balancer' > /var/www/html/index.html
    SHELL
    lb.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
    end
  end

  config.vm.define "lb2" do |lb02|
    lb02.vm.box = "bento/ubuntu-22.04"
    lb02.vm.hostname = "lb2"
    lb02.vm.network "private_network", ip: "192.168.56.20"
    lb02.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
    end
  end

  config.vm.define "web1" do |web1|
    web1.vm.box = "bento/ubuntu-22.04"
    web1.vm.hostname = "web1"
    web1.vm.network "private_network", ip: "192.168.56.11"
    web1.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apache2
      echo 'Web Server 1' > /var/www/html/index.html
    SHELL
    web1.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
    end
  end

  config.vm.define "web2" do |web2|
    web2.vm.box = "bento/ubuntu-22.04"
    web2.vm.hostname = "web2"
    web2.vm.network "private_network", ip: "192.168.56.12"
    web2.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apache2
      echo 'Web Server 2' > /var/www/html/index.html
    SHELL
    web2.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
    end
  end

    config.vm.define "ansible" do |ansible|
    ansible.vm.box = "bento/ubuntu-22.04"
    ansible.vm.hostname = "ansible"
    ansible.vm.network "private_network", ip: "192.168.56.13"
    ansible.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y ansible
    SHELL
    ansible.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
    end
  end

    config.vm.define "db" do |db|
    db.vm.box = "bento/ubuntu-22.04"
    db.vm.hostname = "db"
    db.vm.network "private_network", ip: "192.168.56.14"
        db.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
    end
  end

  config.vm.define "db_replica" do |db|
    db.vm.box = "bento/ubuntu-22.04"
    db.vm.hostname = "db2"
    db.vm.network "private_network", ip: "192.168.56.15"
        db.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
    end
  end

  config.vm.define "prometheus" do |prometheus|
    prometheus.vm.box = "bento/ubuntu-22.04"
    prometheus.vm.hostname = "prometheus"
    prometheus.vm.network "private_network", ip: "192.168.56.16"
        prometheus.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
    end
  end

  config.vm.define "grafana" do |grafana|
    grafana.vm.box = "bento/ubuntu-22.04"
    grafana.vm.hostname = "grafana"
    grafana.vm.network "private_network", ip: "192.168.56.17"
      grafana.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
    end
  end

  config.vm.define "consul" do |consul|
    consul.vm.box = "bento/ubuntu-22.04"
    consul.vm.hostname = "consul"
    consul.vm.network "private_network", ip: "192.168.56.18"
    consul.vm.provider "virtualbox" do |vb|
      vb.memory = 768
    end
  end

end