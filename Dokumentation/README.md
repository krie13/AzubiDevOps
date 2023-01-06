Einleitung

# Java REST-Service (Maven und Springboot)
# Nötige Software

IntelliJ Community Edition - https://www.jetbrains.com/de-de/idea/download/#section=windows
HeidiSQL - https://www.heidisql.com/download.php
Java 11 - https://jdk.java.net/archive/, https://adoptopenjdk.net
Postman -  https://www.postman.com/downloads/
XAMPP (MySQL Datenbank)- https://www.apachefriends.org/de/index.html

# Programme installieren

Jeder Software ist ohne weitere zu beachtende Einstellungen entsprechend zu Installieren.

# Setup XAMPP

Nach dem das Programm geöffnet ist unter dem Modul MySQL mit dem Button "Konfig" die Datei "my.ini" an folgenden Stellen anpassen:
```yml
password=1234 
```

Anschließend über den Button "Starten" den MySQL Service ausführen.

# Setup IntelliJ Projekt

Springboot Projekt erstellen:
https://start.spring.io

![[Pasted image 20221107112357.png]]

folgende Depencies werden benötigt

![[Pasted image 20221107112452.png]]

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

Nun wird im Ordner resources die Datei "application.properties" wie folgt bearbeitet:
```yml
spring.datasource.url=jdbc:mysql://localhost:3306/customerapi  
spring.datasource.username=root  
spring.datasource.password=  
spring.jpa.hibernate.ddl-auto=update
```

und bereits der Datenbank-Name "customerapi" in der ersten Zeile hinterlegt.

WICHTIG!!!!
Die Datei "application.properties" muss vor der Benutzung auf der Produktivumgebung angepasst werden. Dies wird noch einmal in der Doku thematisiert

# HeidiSQL Datenbank erstellen

Im Anschluss muss die Datenbank mit Hilfe von HeidiSQL erstellt werden:

![[Pasted image 20221130130957.png]]

Dazu fügen wir über den Button "Neu" eine neue Verbindung zu unserem lokalen Datenbank-Server hinzu.
Mit dem Button "Öffnen" verbinden wir uns auf diese und erstellen anschließen mit Rechtsklick auf die linke Bildschirmhälfte die erforderliche Datenbank "customerapi":

![[Pasted image 20221130131733.png]]
![[Pasted image 20221130131802.png]]
# Mavenprojekt bauen/installieren

Jetzt erstellen wir als Test das Maven Projekt. Dazu muss in IntelliJ das Maven Menü wie folgt geöffnet werden:
![[Pasted image 20221107132708.png]]

Nach dem sich das Menü geöffnet hat, installieren wir zum Test unsere Application:
![[Pasted image 20221107133358.png]]

Die Anwendung wird gebaut und in der local laufenden Datenbank wird die Tabelle "Customer" erstellt mit den Spalten "id", "Vorname" und "Nachname".

# Ausführbare Java Datei im Explorer

Die ausführbare Java Datei wird, nach dem man diese mit Hilfe von Intelliji gebaut hat, im Ordner 
"Intelliji-WorkSpace\Projektname\target" mit der Endung .jar abgelegt.
Des Weiteren wird auch am Ende des Install-Prozess in der Konsole der Pfad angegeben, in welchem die Datei abgelegt wurde.

# Datenbank mit Daten befüllen

Um eine Ausgabe zu erhalten, fügen wir nun wie folgt mit Hilfe von HeidiSQL Daten in der Datenbank ein:

![[Pasted image 20221130133931.png]]
um den Befehl nun noch auszuführen drücken wir den gelb markierten Button in der oberen Toolbar.

# Java Datei ausführen und GET-Request abschicken

Um den Rest-Service zu starten genügt es, die zuvor installierte .jar FIle lediglich auszuführen/ zu öffnen.
Die Funktion überprüfen wir, in dem wir in einem Webbrowser folgende URL eingeben um somit eine GET-Request zu schicken:

```URL
http://localhost:8080/customer
```

folgende Ausgabe sollte dabei so, oder ähnlich zurückgegeben werden:
![[Pasted image 20221130135001.png]]

da wir nun also sehen, dass alles soweit funktioniert, müssen wir die Datei "application.properties" für das Dockercontainerkonstrukt auf der AzureVM wie folgt anpassen:

```yaml
spring.datasource.url=jdbc:mysql://172.25.36.4:3306/customerapi
spring.datasource.username=root
spring.datasource.password=adminadmin
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.platform=mysql
spring.jpa.hibernate.ddl-auto=update
```

in der ersten Zeile muss die öffentliche IPv4 Adresse der VM

# Git Repository auf Windows kopieren und die aktualisierungen pushen

Leeres Verzeichnis erstellen.
Anschließend die Git-Bash Konsole öffnen.

main Repo erstellen mit 

```git
git init -b main
```

Das Repo von GitHub als Ursprung adden

```git
git remote add origin https://github.com/krie13/AzubiDevOps.git
```

um das origin Repo auf den Rechner zu klonen

```git
git pull origin main
```

jetzt werden alle bestehenden mit geänderten Dateien überschrieben, sprich die neuen Projektdateien werden von den alten ersetzt. Anschließend die veränderungen commiten

```git
git commit .
```

und ins GitHub pushen mit

```git
git push origin main
```

# Virtuelle Maschinen erstellen

Um eine neue Virtuelle Maschiene zu erstellen klicken Sie im Dienst "Virtuelle Maschiene" auf "Erstellen" und anschließend im Drop-Down-Menü auf "Azure-VM" 

Danach können die folgenden Punkte, wie auf den Bildern zu sehen, konfiguriert werden. Durch klicken auf die Ausrufezeichen hinter den Punkten kann die jeweilige Erklärung zu diesen eingesehen werden 

![[Pasted image 20221205111848.png]]

![[Pasted image 20221205111935.png]]

![[Pasted image 20221205112029.png]]

![[Pasted image 20221205112109.png]]

![[Pasted image 20221205112259.png]]

![[Pasted image 20221205112326.png]]

![[Pasted image 20221205112351.png]]

### Mit virtueller Maschine verbinden
Öffnen Sie die "Windows Terminal App", klicken sie oben rechts auf den Pfeil nach unten und wählen Sie Azure Cloud Shell aus
Führen Sie die gezeigten anweisungen aus

Sind die Schritte durchgeführt können Sie sich per SSH mit der öffentlichen IP-Adresse der Maschiene verbinden
```bash
ssh mxfo@20.160.221.206
```
anschließend muss das bei der Erstellung der VM festgelegte Passwort eingegeben werden

### Automatisches Herunterfahren
es besteht die möglichkeit den virtuellen Computer täglich zu einer bestimmten Uhrzeit herunter zu fahren um Geld zu sparen.
Nach dem automatischen Herunterfahren findet kein automatischer Start statt was bedeutet, dass die Maschine ausgeschaltet bleibt so lange sie nicht manuell hochgefahren wird

Um automatisches Heruntefahren ein zu schalten klicken sie auf den Punkt "Automatisch Herunterfahren" im linken Menü in der ausgewählten VM.
![[Pasted image 20221205112811.png]]
Da kann nun der Zeitpunkt zum Ausschalten sowie die passende Zeitzone angegeben werden
![[Pasted image 20221205112851.png]]
Zum hochfahren muss nun der Punkt "Übersicht" und anschließend Start ausgewählt werden
![[Pasted image 20221205112915.png]]

# Dienste konfigurieren

Es werden folgende Dienste benötigt:
- Java
- Jenkins
- Maven
- Docker
- Git

# Java 11
Es wird openJDK in der Version 11.0.17 genutzt
### Installation
```bash
sudo dnf -y install java-11-openjdk
```

### Konfiguration
```bash
vim /etc/profile
```

zugehörige Java Datei ans Ende der Datei einfügen
```bash
export JAVA_HOME=/usr/lib/jvm/jre-11-openjdk-11.0.17.0.8-2.el8_6.x86_64/
export PATH=$JAVA_HOME/bin:$PATH
```

Anschließend die VM neustarten

# Jenkins
Es wird die Version 2.361.2 verwendet
### Installation
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

### Konfiguration
Private Adresse des Webservers mit Verweis auf Port 8080 im Browser öffnen:
"http://172.25.36.4:8080/"

in der VM die Datei öffnen und darin stehendes Initial-Admin-Passwort koprieren
```bash
sudo vim /var/lib/jenkins/secrets/initialAdminPassword
```

Anschließend folgt die Auswahl ob die empfohlenen Plugins oder nur ausgewählte installiert werden sollen.
Nachdem die Plugins installiert sind können die Daten zur Erstellung des ersten Administrator-Kontos eingegeben werden oder dieser Schritt übersprungen werden
Durch klicken auf "Save and Finish" wird die konfiguration abgeschlossen

# Maven
Es wird die Version 3.5.4 genutzt

## wie nutze ich maven?
## richtig konfigurieren?
## richtige java version nutzen



### Installation
```bash
sudo dnf -y install maven
```

# Docker
Es wird die Version 20.10.21 genutzt

### Installation
```bash
sudo dnf -y config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

```bash
sudo dnf -y install docker-ce docker-ce-cli containerd.io
```

```bash
sudo systemctl start docker
```

```bash
sudo systemctl enable docker
```

```bash
sudo dnf -y install docker-compose-plugin
```

# Git
Es wird die Version 2.31.1 genutzt

### Installation
```bash
sudo dnf -y install git 
```

### Verbinden eines Reposetories auf GitHub
```bash
git remote add AzubiDevOps https://github.com/krie13/AzubiDevOps.git

git clone https://github.com/krie13/AzubiDevOps.git
```
Neue Datei erstellen und zu Git hinzufügen
```bash
cd AzubiDevOps

vim Test.md

git add Test.md
```

Änderungen commiten
```bash
git commit Test.md -m "commit 1"
```

Auf GitHub pushen
```bash
git push AzubiDevOps main
```

### Docker compose installieren
sudo dnf install docker-compose-plugin

### Java zu path hinzufügen
```bash
vim ~/.bash-profile
```

zugehörige Java Datei einfügen
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
vm neustarten

### Java Programm mit Maven kompilieren

Die Dockerfile "java-dockerfile" muss wie folgt aussehen und wird im Git Verzeichnis AzubiDevOps/Dockerfiles/ abgelegt:

```yaml
FROM openjdk:11

EXPOSE 8080

ADD /customerapi.jar AzubiDevOps-App.jar

ENTRYPOINT ["java","-jar","AzubiDevOps-App.jar"]
```

Für das initialisieren der Dockercontainer benötigen wir folgende Dockercompose-Datei welche als "docker-compose.yml" im Git Verzeichnis AzubiDevOps/Dockerfiles/ abzulegen ist:

```yaml
version: '3.8'

networks:
        AzubiDevOps-Netz:
                ipam:
                        driver: default
                        config:
                                - subnet: "192.168.0.0/24"

services:
        db:
                image: mysql
                cap_add:
                      - SYS_NICE
                hostname: mysql
                restart: always
                environment:
                      - MYSQL_DATABASE=customerapi
                      - MYSQL_ROOT_PASSWORD=adminadmin
                ports:
                       - '3306:3306'
                volumes:
                      - db:/var/lib/mysql
                container_name: mysql
                networks:
                        - AzubiDevOps-Netz
        java:
                ports:
                        - '8080:8080'
                build:  
                        context: /data/AzubiDevOps/Dockerfiles/
                        dockerfile: java-dockerfile
                links:
                        - "db:mysql"
                container_name: java
                depends_on:
                        - db
                networks:
                        - AzubiDevOps-Netz
volumes:
        db:
                driver: local
```

# Jenkins

Jenkins Web Oberfläche:
	URL: http://20.160.221.206:8080 ohne Proxy
	URL: http://172.25.36.4:8080 mit Proxy
Login:
	User: admin
	Passwort: ad1af075123f4e84ae9b480a6624965c

Die Pipline nutzen wir das Maven plugin als Unterstützung. Dieses installiert man wie folgt:
![[10 1.png]]

![[11.png]]

![[12.png]]

![[13.png]]

Zusätzlich müssen folgende Änderungen und Einstellungen vorgenommen werden:

![[14.png]]

![[15.png]]

an der Stelle bitte die GitHub Credentials

# Erstellung der Pipeline

![[Pasted image 20221221145227.png]]

Im Bereich "General" müssen keine Veränderung vorgenommen werden. Keiner der Checkboxen besitzt einen Haken.

Im Bereich "Sours-Code-Management" müssen unter "Credentials" Zugangsdaten für einen Benutzer mit Zugriffsrechten auf das Git Repository hinzugefügt werden. Durch klicken auf "Add" und "Jenkins" öffnet sich das folgende Fenster, in dieses müssen nun Benutzername und Passwort eingegeben werden.
Der Bereich "Sours-Code-Management" muss wie folgt aussehen:

![[Pasted image 20221221145649.png]]

Der Bereich muss nun wie folgt aussehen:

![Pipeline_Add_Credentials](../Bilder/Pipeline_Add_Credentials.PNG)

Der Bereich "Build-Auslöser" muss wie folgt aussehen:

![[Pasted image 20221221145736.png]]

Der Bereich "Pre-Build-Schritte" muss wie folgt aussehen, alle Kommentare mit # dienen lediglich als Beschreibung/Unterstützung:

![[Pasted image 20221221150800.png]]

```shell
# dort werden alle projektrelevanten Dateien abgelegt, DIESER MUSS EIGENSTÄNDIG AUF DEM
# SYSTEM ERSTELLT WERDEN
cd /data/

# damit der User Jenkins keine Rechte braucht, um Docker auszuführen
echo adminadmin | sudo -S chmod 777 /var/run/docker.sock

# stoppt und löscht alle bestehenden und laufenden Container für den Fall, das vorher schon mal
# die Pipeline ausgeführt wurde
docker ps -q --filter "name=mysql" | grep -q . && docker stop mysql && docker rm -fv mysql
docker ps -q --filter "name=java" | grep -q . && docker stop java && docker rm -fv java

# um die komplette Docker Cache zu löschen, zum einen aus Speicherplatzgründen und zum anderen
# deshalb, dass sich die Docker-Enginen in zukunft bei neuen Builds keine alten Projektrückstände
# aus der Cache zieht
echo y | sudo -S docker system prune -a

#löscht das alte Git Repo und cloned das neue
echo adminadmin | sudo -S rm -rf AzubiDevOps/
echo adminadmin | sudo -S git clone https://github.com/krie13/AzubiDevOps/
```

Der Bereich "Build" muss wie folgt aussehen:

![[Pasted image 20221221150915.png]]

Der Bereich "Post-Build-Schritte" muss wie folgt aussehen:

![[Pasted image 20221221151649.png]]

![[Pasted image 20221221151710.png]]

```shell
# Navigieren in den entsprechenden Ordner um später Docker Compose zu nutzen
cd /data/AzubiDevOps/Dockerfiles

# Die alte Java Datei wird entfernt und die neu-kompilierte des Build-Prozesses aus dem Jenkins Ordner in das
# obenstehende kopiert
echo adminadmin | sudo -S rm -f /data/AzubiDevOps/Dockerfiles/customerapi.jar
echo adminadmin | sudo -S cp /var/lib/jenkins/.m2/repository/de/telekom/customerapi/0.0.1-SNAPSHOT/customerapi-0.0.1-SNAPSHOT.jar /data/AzubiDevOps/Dockerfiles/customerapi.jar

# Das Netzwerk für die Kommunikation zwischen den Containern wird erstellt
docker network create AzubiDevOps-Netz

# Nicht mehr benötigte Daten werden entfernt
echo adminadmin | sudo -S rm -rf /var/lib/jenkins/workspace/BuildRestService/
echo adminadmin | sudo -S rm -rf /var/lib/jenkins/.m2/repository/

# Die Container werden erstellt und gestartet
sudo docker compose up -d
# da der Java Container beim ersten Mal failed hiermit ein zweites mal (vermutlich weil der MySQL Container nicht schnell genug läuft)
sleep 20
sudo docker compose up -d
sleep 10
docker ps
echo sollte der Java Container noch nicht laufen, dann die Pipeline erneut durchlaufen lassen
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

# Abschluss

nach dem die Pipeline ausgeführt wurde und beide Container laufen, erreicht man die Datenbank im Webbrowser wie folgt:

http://172.25.36.4:8080/customer dort geht die Post Request hin (ohne Proxy)
http://20.160.221.206:8080/customer (mit Proxy)

um die Datenbank zu befüllen, wird 172.25.36.4:3306 mit dem Benuzer "root" und dem Passwort "adminadmin" verwendet
