Vagrantfile Dokumentation
-----------------------------
Vagrant.configure(2) do |config|

Die "2" steht für die Version der Objektkonfiguration, die für die Konfiguration dieses Blocks verwendet wird.
Das Objekt kann aber von Version zu Version sehr unterschiedlich sein.

Derzeit gibt es nur zwei unterstützte Versionen: "1" und "2". Version 1 repräsentiert die Konfiguration von Vagrant 1.0.x. "2" repräsentiert die Konfiguration für 1.1+ bis 2.0.x.
Es ist wichtig zu verstehen, dass innerhalb eines einzigen Konfigurationsabschnitts nur eine einzige Version von Vagrant verwendet werden kann.
#
  config.vm.box = "ubuntu/xenial64"
  
  Hier wird spezifiziert welche VM Box verwendet werden.
#
  config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
 Hier wird der Port festgelegt über der die Virtuelle Maschine erreichbar ist. In diesem Fall hier wäre das der Port 
  config.vm.synced_folder ".", "/var/www/html"  
config.vm.provider "virtualbox" do |vb|
  vb.memory = "512"  
end
config.vm.provision "shell", inline: <<-SHELL
  # Packages vom lokalen Server holen
  # sudo sed -i -e"1i deb {{config.server}}/apt-mirror/mirror/archive.ubuntu.com/ubuntu xenial main restricted" /etc/apt/sources.list 
  sudo apt-get update
  sudo apt-get -y install apache2 
SHELL
end
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ2MDUyMjA4NiwxMjUwNDM2MjkyLDY4OD
Y0OTk0MiwxNDA0Mjc1Mzk2LC0xNjQ5MTI5MTY0LC05OTE2MzM4
NCwtNzUwNzE1OTIyXX0=
-->