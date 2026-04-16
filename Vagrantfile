Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  config.vm.provision "shell", inline: <<-SHELL
    set -e

    apt-get update -y
    apt-get upgrade -y

    apt-get install -y ca-certificates curl gnupg lsb-release git

    install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    chmod a+r /etc/apt/keyrings/docker.gpg

    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo $VERSION_CODENAME) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null

    apt-get update -y
    apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

    systemctl enable docker
    systemctl start docker

    PROJECT_DIR="/home/vagrant/app"

    if [ -d "$PROJECT_DIR/.git" ]; then
      cd $PROJECT_DIR
      git pull origin main
    else
      git clone https://github.com/antonio676767/projectocarlosdocker.git $PROJECT_DIR
    fi

    cd $PROJECT_DIR
    docker compose up -d
  SHELL

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--graphicscontroller", "vmsvga"]
    vb.customize ["modifyvm", :id, "--vram", "128"]
    vb.customize ["modifyvm", :id, "--accelerate3d", "off"]
  end
end
