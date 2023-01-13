
Diese Dokumentation umfasst die gesamte normale Dokumentation und wird mit der Dokumentation zur Pipeline als jenkinsfile (im github) und der entfernung aller Sudo rechte des Jenkins-Users erweitert
Das Jenkinsfile funktioniert noch nicht richtig. es kommt immer ein Fehler beim kompilieren
eigentlich sollte es vom sysntax her stimmen
Die Jenkinsfiles (auch testfiles) liegen im GitHub 
die Pipeline steht schon im Jenkins

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
	5.1 Maven-Projekt-Pipeline
	5.2 Scripted Pipeline


## Sourcecode management nur nötig wenn Script von Github
Im Bereich "Sours-Code-Management" müssen unter "Credentials" Zugangsdaten für einen Benutzer mit Zugriffsrechten auf das Git Repository hinzugefügt werden. Durch klicken auf "Add" und "Jenkins" öffnet sich das folgende Fenster, in dieses müssen nun Benutzername und Passwort eingegeben werden.
Der Bereich "Source-Code-Management" muss wie folgt aussehen:
Der Bereich muss nun wie folgt aussehen:
