Vagrant.configure("2") do |config|
  # Definir variables para las máquinas
  web_ip1 = "192.168.33.10"
  web_ip2 = "192.168.33.11"
  nginx_ip = "192.168.33.20"

  # Configuración de la máquina 1 (Servidor Apache 1)
  config.vm.define "web1" do |web1|
    web1.vm.box = "ubuntu/bionic64"
    web1.vm.network "private_network", type: "static", ip: web_ip1
    web1.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end

    web1.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      sudo systemctl enable apache2
      sudo systemctl start apache2
    SHELL
  end

  # Configuración de la máquina 2 (Servidor Apache 2)
  config.vm.define "web2" do |web2|
    web2.vm.box = "ubuntu/bionic64"
    web2.vm.network "private_network", type: "static", ip: web_ip2
    web2.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end

    web2.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      sudo systemctl enable apache2
      sudo systemctl start apache2
    SHELL
  end

  # Configuración de la máquina 3 (Servidor Nginx)
  config.vm.define "nginx" do |nginx|
    nginx.vm.box = "ubuntu/bionic64"
    nginx.vm.network "private_network", type: "static", ip: nginx_ip
    nginx.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end

    nginx.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y nginx
      sudo systemctl enable nginx
      sudo systemctl start nginx

      # Configuración de Nginx para balanceo de carga
      sudo bash -c 'cat > /etc/nginx/sites-available/default <<EOF
server {
    listen 80;

    location / {
        proxy_pass http://web1:80;
        proxy_pass http://web2:80;
    }
}
EOF'
      sudo systemctl reload nginx
    SHELL
  end
end
