Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.provision :shell, :inline => $BOOTSTRAP_SCRIPT # see below
end

$BOOTSTRAP_SCRIPT = <<EOF
	set -e # Stop on any error

	export DEBIAN_FRONTEND=noninteractive

	echo VAGRANT SETUP: DOING APT-GET STUFF...
	# Install prereqs
  sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
  echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
	sudo apt-get update
	sudo apt-get install -y python-software-properties make git-core curl g++ htop redis-server mongodb-org=2.6.1

	echo VAGRANT SETUP: DOING NODE STUFF...
	export NODE_VERSION=0.10
	# Install Node with nvm
	sudo -u vagrant git clone git://github.com/creationix/nvm.git ~vagrant/nvm
	echo "source ~vagrant/nvm/nvm.sh; nvm use $NODE_VERSION" | tee -a ~vagrant/.profile
	source ~vagrant/nvm/nvm.sh
	nvm install v$NODE_VERSION
	nvm use $NODE_VERSION

  # Chef/Knife
  echo '----> Upgrading Chef/Knife to latest version...'
  curl -L https://www.opscode.com/chef/install.sh | sudo bash
  echo '----> Chef/Knife upgrade complete:'
  knife --version

  # Heroku toolbelt
  wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh

	# Make vagrant automatically go to /vagrant when we ssh in.
	echo "cd /vagrant" | sudo tee -a ~vagrant/.profile

	echo VAGRANT IS READY.
EOF
