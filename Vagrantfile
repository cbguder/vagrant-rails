# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'digest/md5'

def encrypt_password(username, password)
  password_hash = Digest::MD5.digest(password + username)
  "md5" + Digest.hexencode(password_hash)
end

Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.provision :chef_solo do |chef|
    chef.add_recipe "apt"
    chef.add_recipe "postgresql::server"
    chef.add_recipe "redisio::install"
    chef.add_recipe "redisio::enable"
    chef.add_recipe "rvm::system"
    chef.add_recipe "rvm::vagrant"

    chef.json = {
      "postgresql" => {"password" => {"postgres" => encrypt_password("postgres", "password")}},
      "rvm" => {"default_ruby" => "ruby-2.0.0-p195"}
    }
  end
end
