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
	sudo apt-get update
	sudo apt-get install -y python-software-properties make git-core curl

	echo VAGRANT SETUP: DOING NODE STUFF...
	export NODE_VERSION=0.10
	# Install Node with nvm
	sudo -u vagrant git clone git://github.com/creationix/nvm.git ~vagrant/nvm
	echo "source ~vagrant/nvm/nvm.sh; nvm use $NODE_VERSION" | tee -a ~vagrant/.profile
	source ~vagrant/nvm/nvm.sh
	nvm install v$NODE_VERSION
	nvm use $NODE_VERSION

	# Make vagrant automatically go to /vagrant when we ssh in.
	echo "cd /vagrant" | sudo tee -a ~vagrant/.profile

	echo VAGRANT IS READY.
EOF
