VAGRANTFILE_API_VERSION = "2"

$setup = <<SCRIPT

dependencies="htop vim nodejs npm"

function silent_install {
    apt-get -y install "$@" >/dev/null 2>&1
}

echo updating package information
apt-get -y update >/dev/null 2>&1

echo installing dependencies
silent_install $dependencies

if [ ! -f /usr/sbin/node ];
then
  echo creating node alias
  ln -s /usr/bin/nodejs  /usr/sbin/node
fi

echo done!
SCRIPT


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", type: "dhcp"
  config.vm.network :forwarded_port, guest: 8080, host: 8080, auto_correct: true # swagger interface
  config.vm.network :forwarded_port, guest: 10010, host: 10010, auto_correct: true # api port
  config.vm.synced_folder ".", "/vagrant", type: "nfs",  mount_options: ["rw", "vers=3", "tcp", "fsc" ,"actimeo=2"]
  config.vm.provision :shell, inline: $setup

end
