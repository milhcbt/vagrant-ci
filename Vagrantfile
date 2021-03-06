# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|
  config.vm.share_folder "v-apt-cache", "/var/cache/apt/archives", "~/tmp/vagrant/apt/cache"
  config.vm.share_folder "v-gem-cache", "/opt/vagrant_ruby/lib/ruby/gems/1.8", "~/tmp/vagrant/gems/1.8"

  config.vm.box = "precise64"

  config.vm.forward_port 8080, 8080
  config.vm.forward_port 9000, 9000

  config.vm.provision :shell, :path => "update_chef.sh"

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "cookbooks"

    chef.add_recipe "mysql::server"
    chef.add_recipe "git"
    chef.add_recipe "java"
    chef.add_recipe "jenkins"
    chef.add_recipe "sonar"
    chef.add_recipe "sonar::database_mysql"

    chef.json = {
      :java => {
        :install_flavor => "oracle",
        :jdk_version => "7",
        :jdk => {
          "7" => {
            :x86_64 => {
              :url => "http://10.0.2.2/chef/jdk-7u5-linux-x64.tar.gz",
              :checksum  => "2a118ce9350d0c0cbaaeef286d04980df664b215d6aaf7bc1d4469abf05711bf"
            }
          }
        }
      },
      :jenkins => {
        :mirror_url => "http://10.0.2.2/chef/jenkins/war",
        :version    => "1.474",
        :checksum   => "6ff0ce7c0426d12dd46f9a62e0a8b2a503862ac0d0c7ba687034d05fb884966e"
      },
      :mysql => {
        :server_debian_password => "root",
        :server_root_password => "root",
        :server_repl_password => "root",
        :bind_address => "0.0.0.0"
      },
      :sonar => {
        :mirror    => "http://10.0.2.2/chef/sonar",
        :version   => "3.1.1",
        :checksum  => "9606c6f6c79c6b944d9d4a7c5eb6531b",
        :os_kernel => "linux-x86-64",
        :jdbc_url => "jdbc:mysql://localhost:3306/sonar",
        :jdbc_driverClassName => "com.mysql.jdbc.Driver",
        :jdbc_validationQuery => "select 1"
      }
    }
  end

end
