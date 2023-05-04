Vagrant.configure("2") do |config|
  config.vm.define "ansible" do |ansible|
    ansible.vm.box = "ubuntu/jammy64"
    ansible.vm.hostname = "ansible"
    ansible.vm.network "private_network", ip: "192.168.53.100"
    ansible.vm.provider "virtualbox" do |v|
      v.name = "ansiblemachine"
      v.memory = 2048
      v.cpus = 2
    end
    ansible.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y ansible git
      git clone http://github.com/richiinstructor/ex.git
    SHELL
  end

  config.vm.define "docker" do |docker|
    docker.vm.box = "ubuntu/jammy64"
    docker.vm.hostname = "docker"
    docker.vm.network "private_network", ip: "192.168.53.101"
    docker.vm.provider "virtualbox" do |v|
      v.name = "dockermachine"
      v.memory = 2048
      v.cpus = 2
    end
    docker.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release
      curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
      echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
      apt-get update
      apt-get install -y docker-ce docker-ce-cli containerd.io
      echo 'FROM hello-world\nMAINTAINER res@res.es\nCMD [ "/hello" ]' > Dockerfile
      docker build -t hello2 .
      usermod -aG docker vagrant
    SHELL
  end
end
