Vagrant.configure("2") do |config|
  # Ubuntu 20.04 base box
  config.vm.box = "ubuntu/focal64"

  # Forward guest port 80 (inside VM) to host port 8080
  config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true

  # Basic VM resources
  config.vm.provider "virtualbox" do |vb|
    vb.name = "ansible-docker-hello"
    vb.memory = 1024
    vb.cpus = 1
  end

  # Provisioning: install Ansible + Docker, then run the Ansible playbook
  config.vm.provision "shell", inline: <<-SHELL
    set -e
    export DEBIAN_FRONTEND=noninteractive

    # Update apt and install Ansible & Docker
    apt-get update -y
    apt-get install -y ansible docker.io

    # Ensure Docker is enabled and running
    systemctl enable docker
    systemctl start docker

    # Run Ansible playbook located in the shared folder /vagrant
    ansible-playbook /vagrant/provisioning/playbook.yml -i "localhost,"
  SHELL
end