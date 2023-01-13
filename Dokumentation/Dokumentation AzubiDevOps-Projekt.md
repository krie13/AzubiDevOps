# Inhalt
1. Virtuelle Maschine
	1.1 VM erstellen
	1.2 Verbingung herstellen
	1.3 Port freigeben
	1.4 Netzwerkgruppe
		1.4.1 Erstellen
		1.4.2 Hinzufügen
1. Softwareübersicht
	2.1 Windows-Client
	2.2 Linux-Server
3. Software installation
	3.1 Windows-Client
	3.2 Linux-Server
4.  Java-Projekt
	4.1 Source-Code erstellen
	4.2 Datenbank erstellen
	4.3 Funktion prüfen
5.  Pipeline
# 1 Virtuelle Maschine
## 1.1 VM erstellen
Um eine neue Virtuelle Maschiene zu erstellen klicken Sie im Dienst "Virtuelle Computer" auf "Erstellen" und anschließend im Drop-Down-Menü auf "Azure-VM" 
Danach muss der virtuelle Computer konfiguriert werden. In den folgenden Bildern wird ein Beispiel gezeigt. Die Punkte müssen individuell angepasst werden. Durch klicken auf die Ausrufezeichen hinter den Punkten kann die jeweilige Erklärung zu diesen eingesehen werden 

### Grundeinstellungen:
Im Punkt Abonnement muss das eigene Azure-Abonnement ausgewählt werden. Im darauf folgenden Feld "Ressourcengruppe" muss die Ressourcengruppe eingetragen werden zu welcher der virtuelle Computer gehören soll. Bei Image ist Rocky Linux 8 ausgewählt, dieses wurde für die Dokumentation verwendet.
![[../Bilder/Azure-VM_Grundeinstellungen.PNG]]

### Datenträger:
Da der Speicher standardmäßig für den Anwendungszweck nicht ausreicht muss ein weiterer Datenträger eingebunden werden. Durch klicken auf "Neuen Datenträger erstellen und anfügen" wird ein Konfigurationsfenster geöffnet. Ein Beispiel ist auf dem zweiten Bild zu sehen.
Die empfohlene Speichergröße beträgt 64 GiB
![[../Bilder/Azure-VM_Datenträger.PNG]]

![[../Bilder/Azure-VM_Datenträger_Erweiterung.PNG]]

### Netzwerk
![[../Bilder/Azure-VM_Netzwerk.PNG]]

### Verwaltung
![[../Bilder/Azure-VM_Verwaltung.PNG]]

### Monitoring
![[../Bilder/Azure-VM_Monitoring.PNG]]

### Erweitert
![[../Bilder/Azure-VM_Erweitert.PNG]]

### Tags
![[../Bilder/Azure-VM_Tags.PNG]]
Auf überprüfen und anschließend erstellen klicken

Sobald angezeigt wird, dass die Bereitstellung abgeschlossen wurde kann der Computer genutzt werden  
![[../Bilder/Azure-VM_Abschluss.PNG]]
Durch klicken auf "Zur Ressource wechseln" erfolgt die weiterleitung auf die Überblicksseite der VM. Auf dieser sind dessen IP-Adressen zu finden, welche im späteren Verlauf nötig sind 
![[../Bilder/Azure-VM_IPs.PNG]]

## 1.2 Verbindung herstellen
Öffnen Sie die "Windows Terminal App", klicken sie oben rechts auf den Pfeil nach unten und wählen Sie Azure Cloud Shell aus
Führen Sie die gezeigten anweisungen aus

Sind die Schritte durchgeführt können Sie sich per SSH mit der öffentlichen IP-Adresse der Maschiene verbinden
```bash
ssh mxfo@20.160.221.206
```
anschließend muss das bei der Erstellung der VM festgelegte Passwort eingegeben werden

# 2 Softwareübersicht
## 2.1 Windows-Client
Es werden folgende Dienste zur Entwicklung der Java-Anwendung auf dem Windows-Client empfohlen:
- IntelliJ Community Edition - https://www.jetbrains.com/de-de/idea/download/#section=windows (latest)
- HeidiSQL - https://www.heidisql.com/download.php (latest)
- Java - https://jdk.java.net/archive/ (version 11.0.2)
- Postman -  https://www.postman.com/downloads/ (latest)
- XAMPP (MySQL Datenbank)- https://www.apachefriends.org/de/index.html (latest)

## 2.2 Linux-Server
Es werden folgende Dienste auf dem Linux-Client benötigt:
- Maven (latest)
- Java (openjdk 11 latest)
- Docker (latest)
- Git (latest)
- Jenkins (latest)

# 3 Software installation und konfiguration
## 3.1 Windows-Client
### IntelliJ Community Edition

### HeidiSQL

### Java

### Postman

### XAMPP

#### Setup XAMPP

Nach dem das Programm geöffnet ist unter dem Modul MySQL mit dem Button "Konfig" die Datei "my.ini" an folgenden Stellen anpassen:
```yml
password=1234 
```

Anschließend über den Button "Starten" den MySQL Service ausführen.

## 3.2 Linux-Server
### Maven
Für die Dokumentation wurde die Version 3.5.4 genutzt

#### Installation
```bash
sudo dnf -y install maven
```

### Java
Für die Dokumentation wurde openJDK in der Version 11.0.17 genutzt
#### Installation
```bash
sudo dnf -y install java-11-openjdk
```

#### Konfiguration
```bash
vi /etc/profile
```

zugehörige Java Datei ans Ende der Datei einfügen
```bash
export JAVA_HOME=/usr/lib/jvm/jre-11-openjdk-11.0.17.0.8-2.el8_6.x86_64/
export PATH=$JAVA_HOME/bin:$PATH
```

sollte so aussehen
```bash
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/bin

export PATH

export JAVA_HOME=/usr/lib/jvm/jre-11-openjdk-11.0.17.0.8-2.el8_6.x86_64
```

Anschließend die VM neustarten

java-version auswählen
```bash
sudo update-alternatives --config java
```

anschließend wird folgendes angezeigt
```bash
There are 2 programs which provide 'java'.

  Selection    Command
-----------------------------------------------
 + 1           java-11-openjdk.x86_64 (/usr/lib/jvm/java-11-openjdk-11.0.17.0.8-2.el8_6.x86_64/bin/java)
*  2           java-1.8.0-openjdk.x86_64 (/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el8_7.x86_64/jre/bin/java)

Enter to keep the current selection[+], or type selection number:
```
wählen sie die korrekte Version durch eingeben der zugehörigen Zahl

### Docker
Für die Dokumentation wurde die Version 20.10.21 genutzt

#### Installation
```bash
sudo dnf -y config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf -y install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

docker starten 
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

### Git
Für die Dokumentation wurde die Version 2.31.1 genutzt

#### Installation
```bash
sudo dnf -y install git 
```

### Jenkins
Für die Dokumentation wurde die Version 2.361.2 verwendet
#### Installation
```bash
sudo dnf -y install wget
```

```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

```bash
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
```

```bash
sudo dnf -y install jenkins
```

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

#### Konfiguration
Um Jenkins zu konfigurieren muss das Webinterface geöffnet werden. Dieses ist entweder über über die private oder öffentliche IP-Adresse erreichbar mit verweis auf den Port 8080
z.B. "http://172.25.36.4:8080/"

Im Webinterface wird nach dem Administrator-Passwort gefragt. Dieses ist in der angegebenen Datei auf der VM zu finden
```bash
sudo vi /var/lib/jenkins/secrets/initialAdminPassword
```

Anschließend folgt die Auswähl der Plugins. Die Auswahl der empfohlenen Plugins reicht für den Anwendungszweck aus, nur das Maven-Plugin wird im folgenden nachträglich installiert.
Nachdem die Plugins installiert wurden können entweder die Daten zur Erstellung des ersten Administrator-Kontos eingegeben oder dieser Schritt übersprungen werden
Durch klicken auf "Save and Finish" wird die konfiguration abgeschlossen

Um das Maven-Plugin zu installieren klicken sie auf "Jenkins verwalten" und anschließend auf "Plugins verwalten".
![[../Bilder/Jenkins_Verwalten.png]]
![[../Bilder/Jenkins_Plugins_Verwalten.png]]

Nun muss der Punkt "Available plugins" ausgewählt und in der Suchleiste nach Maven gesucht werden. Nachdem das Plugin ausgewählt wurde muss auf den Knopf "install without restart" geklickt werden.
![[../Bilder/Jenkins_Plugin-Manager.png]]
Nach der installation muss der Punkt zum neustart von Jenkins, am ende der Seite, ausgewählt werden.
![[../Bilder/Jenkins_Plugins_installieren.png]]
Anschließend muss unter "Jenkins verwalten" und "Konfiguration der Hilfsprogramme" bei dem Punkt  "JDK installation" die installierte Java-JDV wie zu sehen ausgewählt werden.
![[../Bilder/Jenkins_JDK_auswählen.png]]
Nun muss noch Maven für Jenkins installiert werden. Dafür muss am Ende der Seite auf "Maven hinzufügen" geklickt und anschließend wie im Bild konfiguriert werden.
![[../Bilder/Jenkins_Maven_hinzugefügt.png]]

# 4 Java-Anwendung
## 4.1 Source-Code erstellen
Springboot Projekt erstellen:
https://start.spring.io
![[../Bilder/SpringBoot_config.png]]

folgende Depencies werden benötigt
![[../Bilder/SpringBoot_Dependencies.png]]

Anschließend auf "GENERATE" klicken um "customerapi.zip" herunterzuladen.
Dieses Archiv wird nun entpackt und der Ordner in IntelliJ als Ordner (Projekt) geöffnet.

In dem Package "de.telekom.customerapi" folgende Projektdateien erstellen:

Customer (ist auch dann in der Datenbank eine automatisch erstellte Tabelle):
```Java
package de.telekom.customerapi;  
  
import javax.persistence.Entity;  
import javax.persistence.GeneratedValue;  
import javax.persistence.GenerationType;  
import javax.persistence.Id;  
  
@Entity  
public class Customer {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private int id;  
    private String vorname;  
    private String nachname;  
  
    public int getId() {  
        return id;  
    }  
  
    public void setId(int id) {  
        this.id = id;  
    }  
  
    public String getVorname() {  
        return vorname;  
    }  
  
    public void setVorname(String vorname) {  
        this.vorname = vorname;  
    }  
  
    public String getNachname() {  
        return nachname;  
    }  
  
    public void setNachname(String nachname) {  
        this.nachname = nachname;  
    }  
}
```

CustomerRepository (Interface):
```Java
package de.telekom.customerapi;  
  
import org.springframework.data.jpa.repository.JpaRepository;  
  
public interface CustomerRepository extends JpaRepository<Customer, Integer> {  
}
```

CustomerController:
```java
package de.telekom.customerapi;  
  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RestController;  
  
import java.util.List;  
  
@RestController  
@RequestMapping("/customer")  
public class CustomerController {  
    private CustomerRepository customerRepository;  
  
    public CustomerController(CustomerRepository kundenRepository) {  
        this.customerRepository = kundenRepository;  
    }  
  
    @GetMapping("")  
    public List<Customer> index() {  
        return customerRepository.findAll();  
    }  
}
```

Nun wird im Ordner resources die Datei "application.properties" wie gezeigt bearbeitet. 
```yml
spring.datasource.url=jdbc:mysql://localhost:3306/customerapi  
spring.datasource.username=root  
spring.datasource.password=  
spring.jpa.hibernate.ddl-auto=update
```

## 4.2 Datenbank erstellen
Im Anschluss muss die Datenbank mit Hilfe von HeidiSQL erstellt werden:
![[../Bilder/HeidiSQL_Datenbank.png]]

Dazu fügen wir über den Button "Neu" eine neue Verbindung zu unserem lokalen Datenbank-Server hinzu.
Mit dem Button "Öffnen" verbinden wir uns auf diese und erstellen anschließen mit Rechtsklick auf die linke Bildschirmhälfte die erforderliche Datenbank "customerapi":
![[../Bilder/HeidiSQL_Datenbank_erstellen.png]]
![[../Bilder/HeidiSQL_Datenbank_benennen.png]]

Um eine Ausgabe zu erhalten, fügen wir nun wie folgt Testdaten mit Hilfe von HeidiSQL Daten in der Datenbank ein:
![[../Bilder/HeidiSQL_Datenbank_Inhalt.png]]

um den Befehl nun noch auszuführen drücken wir den gelb markierten Button in der oberen Toolbar.

## 4.3 Funktion prüfen
Um den Rest-Service zu starten genügt es, die zuvor installierte .jar FIle lediglich auszuführen/ zu öffnen.
Die Funktion überprüfen wir, in dem wir in einem Webbrowser folgende URL eingeben um somit eine GET-Request zu schicken:

```URL
http://localhost:8080/customer
```

folgende Ausgabe sollte dabei so, oder ähnlich zurückgegeben werden:
![[../Bilder/Get-Request_Ausgabe.png]]

da nachgewiesen wurde, dass alles soweit funktioniert, müss die Datei "application.properties" für das Dockercontainerkonstrukt auf der AzureVM wie folgt angepassen werden:

```yaml
spring.datasource.url=jdbc:mysql://mysql:3306/customerapi
spring.datasource.username=root
spring.datasource.password=adminadmin
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.platform=mysql
spring.jpa.hibernate.ddl-auto=update
```

# 5 Pipeline

![[../Bilder/Jenkins_Pipeline_Auswahl.png]]

Im Bereich "General" und "Buildumgebung" müssen keine Änderungen vorgenommen werden.
Unter "Source-Code-Management" muss das Git Repository in dem der Java-Source-Code liegt eingetragen werden, wie auf dem Bild zu sehen
![[../Bilder/Jenkins_Source-Code-Management.png]]
Die Credentials müssen durch klicken auf "Add" hinzugefügt werden. Es sind der Nutzername und das Passwort eines GitHub-Nutzers mit zugriff auf das Repository nötig
![[../Bilder/Jenkins_Pipeline_Credentials_hinzufügen.png]]

Im Bereich "Build-Auslöser" können bereits ausgewählte Punkte abgewählt werden.
als nächstes  muss ein Pre-Build-Schritt mit der Auswahl "Shell ausführen" hinzugefügt werden. Die nötigen Shell befehle lauten wie folgt
```shell
# dort werden alle projektrelevanten Dateien abgelegt
cd /data/

# stoppt und löscht alle bestehenden und laufenden Container für den Fall, das vorher schon mal die Pipeline ausgeführt wurde
cd /data/AzubiDevOps/Dockerfiles
sudo docker compose down
cd /data/

# löscht das alte Git Repo und clont das neue
sudo rm -rf AzubiDevOps/
sudo git clone https://github.com/krie13/AzubiDevOps/
```

Der Bereich "Build" muss wie folgt aussehen:
![[../Bilder/Jenkins_Maven_Build.png]]

Da der Vorgang abbrechen soll wenn der Build nicht erfolgreich war muss der gezeigte Punkt ausgewählt werden
![[../Bilder/Jenkins_Post-Build-Schritte.png]]

Unter "Post-Build-Schritte" muss ein Post-Build-Schritt mit der Auswahl "Shell ausführen" hinzugefügt und folgender Code eingefügt werden.
```shell
# Navigieren in den entsprechenden Ordner um später Docker Compose zu nutzen
cd /data/AzubiDevOps/Dockerfiles

# Die alte Java Datei wird entfernt und die neu-kompilierte des Build-Prozesses aus dem Jenkins Ordner in das obenstehende kopiert
sudo rm -f /data/AzubiDevOps/Dockerfiles/customerapi.jar
sudo cp /var/lib/jenkins/.m2/repository/de/telekom/customerapi/0.0.1-SNAPSHOT/customerapi-0.0.1-SNAPSHOT.jar /data/AzubiDevOps/Dockerfiles/customerapi.jar

# Die Container werden erstellt und gestartet
sudo docker compose up --build -d
sudo docker ps

#löscht alle alten Docker-Images
echo y | sudo -S docker system prune -a
```

Anschließend die Pipeline Speichern. Nun muss der Button "Jetzt bauen" in der linken Navigationsleiste gedrückt werden, um die Pipeline auszuführen

## WICHTIG:

bevor die Pipline ausgeführt werden kann, muss noch der Jenkins auf den Port 8081 geändert werden, um Konflikte mit dem Rest-Service (Tomcat auf Port 8080) zu vermeiden.
dazu muss die Zeile in:

```shell
nano /etc/sysconfig/jenkins
```
von
```
JENKINS_PORT="8080"
```
auf 
```
JENKINS_PORT="8081"
```
geändert werden.

Anschließend den Jenkins mit 
```shell
systemctl restart jenkins
```
neustarten.

nach dem die Pipeline ausgeführt wurde und beide Container laufen, erreicht man die Datenbank im Webbrowser wie folgt:

http://172.25.36.4:8080/customer dort geht die Post Request hin (ohne Proxy)
http://20.160.221.206:8080/customer (mit Proxy)

um die Datenbank zu befüllen, wird 172.25.36.4:3306 mit dem Benuzer "root" und dem Passwort "adminadmin" verwendet