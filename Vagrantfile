Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
config.vm.network "forwarded_port", guest: 80, host: 8080
  #carpeta compartida
  config.vm.synced_folder ".", "/home/vagrant/proyecto"

 
  #la primera vez
  
  config.vm.provision "shell", run: "once", inline: <<-SHELL
    echo "---------------update upgrade------------------"
    apt-get update -y
    apt-get upgrade -y

    echo "------------docker compose-----------"
   #descargando docker de los repositorios    

    apt-get install -y ca-certificates curl gnupg
    
    install -m 0755 -d /etc/apt/keyrings

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg

    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo $VERSION_CODENAME) stable" \
    > /etc/apt/sources.list.d/docker.list
    
    apt-get update -y
    apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    
    systemctl enable docker
    systemctl start docker

    # carpeta centralizada
    mkdir -p /home/vagrant/proyecto/app

    cd /home/vagrant/proyecto

    echo "---------------arrancando dockers---------------------"
    docker compose up -d
  SHELL

 #vagrant provision
  config.vm.provision "shell", inline: <<-SHELL
    echo "---------------actualizando docker------------------"

    cd /home/vagrant/proyecto
    docker compose run --rm git-updater
    docker compose up -d
  SHELL

#esto esta puesto porque sino la pantalla parpadea la primera vez que lo enciendes
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--graphicscontroller", "vmsvga"]
    vb.customize ["modifyvm", :id, "--vram", "128"]
    vb.customize ["modifyvm", :id, "--accelerate3d", "off"]
  end
end
