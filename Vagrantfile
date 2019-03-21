Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.synced_folder "../projects", "/projects"
  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 1
    vb.memory = 4096
    vb.name = "workspace"
  end

  # Root user
  config.vm.provision :shell, inline: <<-SHELL
    ############################################################################
    # Add Docker
    ############################################################################
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
    add-apt-repository -y "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

    ############################################################################
    # Add Node.js
    ############################################################################
    curl -fsSL https://deb.nodesource.com/setup_10.x | bash

    ############################################################################
    # Add Yarn
    ############################################################################
    curl -fsSL https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
    echo "deb [arch=amd64] https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

    ############################################################################
    # Add Vim 8
    ############################################################################
    add-apt-repository -y ppa:jonathonf/vim

    ############################################################################
    # Update / upgrades
    ############################################################################
    apt-get update
    apt-get upgrade -y

    ############################################################################
    # Packages
    ############################################################################
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
      git \
      vim \
      apt-transport-https \
      ca-certificates \
      software-properties-common \
      docker-ce \
      nodejs \
      yarn \
      awscli \
      zsh \
      asciinema \
      mysql-server

    ############################################################################
    # Enable ZSH
    ############################################################################
    echo "$(which zsh)" | tee -a /etc/shells
    sed -i -- 's/auth       required   pam_shells.so/# auth       required   pam_shells.so/g' /etc/pam.d/chsh
    chsh -s $(which zsh) vagrant

    ############################################################################
    # Add user vagrant to group docker
    ############################################################################
    usermod -a -G docker vagrant

    ############################################################################
    # Configure MySQL
    ############################################################################
    sed -i -- 's/bind-address		= 127.0.0.1/# bind-address		= 127.0.0.1/g' /etc/mysql/mysql.conf.d/mysqld.cnf
    sed -i -- 's/skip-external-locking/# skip-external-locking/g' /etc/mysql/mysql.conf.d/mysqld.cnf
    mysql --user=root --execute="GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION; FLUSH PRIVILEGES;"
    # zcat /vagrant/provisioning/mysql/dump.sql.gz | mysql --user=root
    service mysql restart

    ############################################################################
    # Clean the mess
    ############################################################################
    apt-get autoremove -y
    apt-get clean all -y
  SHELL

  # Vagrant user
  config.vm.provision :shell, privileged: false, inline: <<-SHELL
    ############################################################################
    # Configure Git
    ############################################################################
    cp /vagrant/provisioning/git/gitconfig ~/.gitconfig
    cp /vagrant/provisioning/git/gitignore_global ~/.gitignore_global
    chmod 644 ~/.git*

    ############################################################################
    # Configure SSH
    ############################################################################
    cp /vagrant/provisioning/ssh/config ~/.ssh/config
    cp /vagrant/provisioning/ssh/id_rsa ~/.ssh/id_rsa
    cp /vagrant/provisioning/ssh/id_rsa ~/.ssh/id_rsa.pub
    chmod 644 ~/.ssh/config
    chmod 400 ~/.ssh/id_rsa*

    ############################################################################
    # Configure ZSH
    ############################################################################
    git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
    cp /vagrant/provisioning/zsh/zshrc ~/.zshrc

    ############################################################################
    # Configure Vim
    ############################################################################
    curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
    cp /vagrant/provisioning/vim/vimrc ~/.vimrc
  SHELL
end

