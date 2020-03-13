Vagrantfile Dokumentation
-----------------------------
Vagrant.configure(2) do |config|
#
Die "2" steht für die Version der Objektkonfiguration, die für die Konfiguration dieses Blocks verwendet wird.


Konfigurationsobjektkonfiguration, die für die Konfiguration dieses Blocks verwendet wird (der Abschnitt zwischen dem Do und dem Ende). Dieses Objekt kann von Version zu Version sehr unterschiedlich sein.

Derzeit gibt es nur zwei unterstützte Versionen: "1" und "2". Version 1 repräsentiert die Konfiguration von Vagrant 1.0.x. "2" repräsentiert die Konfiguration für 1.1+ bis 2.0.x.

Beim Laden von Vagrantfiles verwendet Vagrant das richtige Konfigurationsobjekt für jede Version und führt sie ordnungsgemäß zusammen, wie jede andere Konfiguration auch.

Als allgemeiner Benutzer von Vagrant ist es wichtig zu verstehen, dass innerhalb eines einzigen Konfigurationsabschnitts nur eine einzige Version verwendet werden kann. Sie können die neuen config.vm.provider-Konfigurationen nicht in einem Konfigurationsabschnitt der Version 1 verwenden. Ebenso wird config.vm.forward_port in einem Konfigurationsabschnitt der Version 2 nicht funktionieren (er wurde umbenannt).
Es können mehrer Konfigurationsversionen in derselben Vagrantfile benutzt werden.
#
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
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
eyJoaXN0b3J5IjpbMTQxOTM4NTY1MCwtMTY0OTEyOTE2NCwtOT
kxNjMzODQsLTc1MDcxNTkyMl19
-->