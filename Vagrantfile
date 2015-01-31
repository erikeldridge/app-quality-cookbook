# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = true

    # Customize the amount of memory on the VM:
    vb.memory = "2048"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update

    # virtualbox gui
    sudo apt-get install -y virtualbox-guest-dkms virtualbox-guest-utils virtualbox-guest-x11
    sudo VBoxClient --clipboard --draganddrop --seamless --display --checkhostversion

    # ubuntu desktop
    sudo apt-get install -y xorg gnome-core gnome-system-tools gnome-app-install

    # allow vagrant user to log into gui
    sed -ie 's/allowed_users=.*/allowed_users=anybody/' /etc/X11/Xwrapper.config

    # java
    sudo apt-get install -y default-jre default-jdk

    # checkstyle
    wget http://downloads.sourceforge.net/project/checkstyle/checkstyle/6.2/checkstyle-6.2-all.jar

    # junit
    wget -O junit-4.12.jar http://search.maven.org/remotecontent?filepath=junit/junit/4.12/junit-4.12.jar
    wget -O hamcrest-core-1.3.jar http://search.maven.org/remotecontent?filepath=org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.jar

    # mockito
    wget https://bintray.com/artifact/download/szczepiq/maven/org/mockito/mockito-all/1.10.14/mockito-all-1.10.14.jar

    # pmd
    wget -O pmd-bin-5.2.3.zip http://downloads.sourceforge.net/project/pmd/pmd/5.2.3/pmd-bin-5.2.3.zip
    unzip pmd-bin-5.2.3.zip
    rm pmd-bin-5.2.3.zip

    # git
    sudo apt-get install git

    # intellij
    # http://askubuntu.com/a/353948
    wget -O /tmp/intellij.tar.gz http://download.jetbrains.com/idea/ideaIC-14.0.3.tar.gz
    mkdir idea
    tar xfz /tmp/intellij.tar.gz -C idea
    sudo mv idea/* /opt/idea
    rm -rf idea
    sudo desktop-file-install  <<- DesktopConfig
[Desktop Entry]
Name=IntelliJ IDEA 
Type=Application
Exec=idea.sh
Terminal=false
Icon=idea
Comment=Integrated Development Environment
NoDisplay=false
Categories=Development;IDE;
Name[en]=IntelliJ IDEA
DesktopConfig
    cd /usr/local/bin
    sudo ln -s /opt/idea/bin/idea.sh
    sudo cp /opt/idea/bin/idea.png /usr/share/pixmaps/idea.png

    # log into terminal and run `startx`
  SHELL
end