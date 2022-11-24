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
