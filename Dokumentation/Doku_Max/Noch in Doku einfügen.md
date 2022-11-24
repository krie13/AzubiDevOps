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


### Datenbank erstellen

shell des MySQL-Containers öffnen
```bash
docker exec -it <container name> /bin/bash
```

Datenbank und Tabelle erstellen
```bash
CREATE DATABASE customerapi;

USE customerapi;

CREATE TABLE Customer (id int, Vorname varchar(255), Nachname varchar(255) );
```
exit eingeben um shell zu verlassen