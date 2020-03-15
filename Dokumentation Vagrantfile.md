# Vagrantfile Dokumentation

## VM Konfiguration

```
Vagrant.configure(2) do |config|
```

Die `2` bestimmt, welche Version des `config` Objekts verwendet wird. Dieses Objekt ist von Version zu Version sehr verschieden.

Zurzeit gibt es nur zwei unterstützte Versionen des `config` Objekts. Die Version 1, welche die Konfiguration von Vagrant 1.0.x wiederspiegelt und die Version 2, welche die Konfiguration von Vagrant 1.1+ bis 2.0.x wiederspiegelt.

Generell ist es wichtig zu wissen, dass in einem Vagrantfile nur eine Version des `config` Objekts verwendet werden kann.
#

```
config.vm.box = "ubuntu/xenial64"
```
  
In dieser Zeile wird festgelegt, welche Box gebraucht wird um die zu erstellen.  Der Wert sollte hier der Name einer heruntergeladenen Box oder der Kurzname einer Box von der Vagrant Cloud sein
#
```
config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
```

Hier wird festgelegt, welcher Hostport für den Webserver verwendet wird. Auf Basis dieser Definition wird dann ein Portforwarding eingerrichtet.

Die Option `auto_correct: true` überprüft ob der gesetzte Port bereits für etwas anderes verwendet wird. Falls dies der Fall wäre, würde der Port automatisch geändert werden.
Die Option ist standardmässig deaktiviert.
#
 ```
 config.vm.synced_folder ".", "/var/www/html"
```
Mit dieser Zeile wird ein Ordner, welcher zwischen dem Host System und der VM synchronisiert wird, definiert.
Die erste Option definiert den Pfad auf dem Host System und die zweite den Ort auf der VM. In diesem Fall definiert `"."` also, dass der Pfad auf dem Host System im selben Ordner ist wie das Vagrantfile und `"/var/www/html"` definiert den Webordner auf der VM.

Zusätzlich zu diesen Optionen kann der spezifische synchronisierte Ordnertyp weitere Optionen zulassen:

*- `create` Wenn true, wird der Host-Pfad erstellt, wenn er nicht existiert. Standardeinstellung: falsch.*

*- `disabled` Wenn true, wird dieser synchronisierte Ordner deaktiviert und nicht eingerichtet. 
Dies kann verwendet werden, um einen zuvor definierten synchronisierten Ordner zu deaktivieren oder um eine Definition auf der Grundlage eines externen Faktors bedingt zu deaktivieren.*

*- `group`  Die Gruppe, der der synchronisierte Ordner gehören wird. Standardmäßig wird dies der SSH-Benutzer sein. Einige synchronisierte Ordnertypen unterstützen die Änderung der Gruppe nicht.*

*- `mount_options`  Eine Liste zusätzlicher Einhängeoptionen, die an den Einhängebefehl übergeben werden.*

*- `owner` Der Benutzer, der der Eigentümer dieses synchronisierten Ordners sein sollte. Standardmäßig wird dies der SSH-Benutzer sein. Einige synchronisierte Ordnertypen unterstützen die Änderung des Eigentümers nicht.*

*- `type`  Der Typ des synchronisierten Ordners. Wenn dies nicht angegeben wird, wählt Vagrant automatisch die beste synchronisierte Ordneroption für Ihre Umgebung. Andernfalls können Sie einen bestimmten Typ wie "nfs" angeben.*

*- `id` Der Name für den Mount-Point dieses synchronisierten Ordners auf dem Gastcomputer. Dieser wird angezeigt, wenn man den Mount auf dem Gastsystem ausführt.*

  #
 ```
config.vm.provider "virtualbox" do |vb|
```
In dieser Zeile wird der Provider oder auch VM Engine definiert. In diesem Fall ist das VirtualBox. Zusätlich können je noch Provider noch mehr Konfigurationen angegeben werde. 

Diese Konfigurationen unterscheidet sich von Provider zu Provider.
Einige Anbieter wie zum Beispiel VirtualBox brauchen keine anbieterspezifische Konfiguration sonder funktionieren "out of the box".
Laut dem offiziellem Wiki Artikel von Vagrant, ist die anbieterspezifische Konfiguration als eine Möglichkeit gedacht, mehr Optionen bereitzustellen, um den Provider optimal zu nutzen. Sie ist nicht als Hindernis gedacht, gegen einen bestimmten Anbieter anzutreten.

Vorrangige Konfiguration
Man kann auch nicht-anbieter-spezifische Konfigurationen wie config.vm.box und jede andere Vagrant-Konfiguration außer Kraft setzen. Dies geschieht durch die Angabe eines zweiten Arguments an config.vm.provider. 
In unserem Fall ist der festgelegte Anbieter Virtualbox. Mit den Paramterangaben "do |vb|" wird alles überschrieben und Virtualbox für dieses Vagrantfile als Anbieter gesetzt.
#
```
  vb.memory = "512"
  ```

Diese Zeile bestimmt wie viel Arbeitsspeicher die VM haben wird. Mit dieser Angabe wird der Arbeitsspeicher für die Maschine festgelegt. Die CPU Anzahl könnte auch festgelegt werden. Dies würde man mit folgender Zeile erledigen: 
```
v.cpus = 2
```
## Betriebssystemkonfiguration
```
config.vm.provision "shell", inline: <<-SHELL
```
Diese Zeile definiert Befehle, welche auf dem Betriebssystem ausgeführt werden sollen.

```
sudo sed -i -e"1i deb {{config.server}}/apt-mirror/mirror/archive.ubuntu.com/ubuntu xenial main 
```

Dieser Befehl holt das Ubuntu Image von einem Debian Mirror.
```
sudo apt-get update
```

Dieser Befehl aktualisiert alle Pakete auf dem Betriebssystem
```
sudo apt-get -y install apache2 
```

Dieser Befehl installiert den Webserver Apache.
#
```
Titel: Vagrantfile Dokumentation
Autor: Nando Rigonalli
Datum: 15.03.2020
```
