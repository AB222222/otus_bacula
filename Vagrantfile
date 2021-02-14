Vagrant.configure("2") do |config|

config.vm.define "baculaserver" do |baculaserver|
    baculaserver.vm.box = "centos/7"
    baculaserver.vm.network "private_network", ip: "192.168.10.22"
    baculaserver.vm.hostname = "baculaserver"
    baculaserver.vm.provider :virtualbox do |vb|
      end

    baculaserver.vm.provision "shell", inline: <<-SHELL
       mkdir -p ~root/.ssh; cp ~vagrant/.ssh/auth* ~root/.ssh
       sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
       systemctl restart sshd
       timedatectl set-timezone Europe/Moscow
       yum install -y epel-release nano mc
       yum install -y ntp && systemctl start ntpd && systemctl enable ntpd
       yum install -y bacula-director bacula-storage bacula-console mariadb-server; systemctl start mariadb; systemctl enable mariadb
    SHELL

    end

 

    config.vm.define "baculaclient" do |baculaclient|
      baculaclient.vm.box = "centos/7"
      baculaclient.vm.network "private_network", ip: "192.168.10.23"
      baculaclient.vm.hostname = "baculaclient"
      baculaclient.vm.provider :virtualbox do |vb|
        end


      baculaclient.vm.provision "shell", inline: <<-SHELL
       mkdir -p ~root/.ssh; cp ~vagrant/.ssh/auth* ~root/.ssh
       sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
       systemctl restart sshd
       timedatectl set-timezone Europe/Moscow
       yum install -y epel-release mc nano
       yum install -y ntp && systemctl start ntpd && systemctl enable ntpd
       yum install -y bacula-client
       
    SHELL

  end
end
