- Beim erstellen der Java Files wird kein vernünftiger Pfad angegeben 
- Was für ne Datei, es gibt n haufen mögliche

- Wie solln die Datein heißen (Endungen)
- Es gibt mehrere Wahlmöglichkeiten für File-Endungen

- einrichtung der Java jdk nicht beschrieben
- alles beschreiben(bei installation stehen auch optionale sachen zur verfügung)

- Beim compilieren kommt der Fehler "cannot find symbol"
-  ich musste komplett entfernen:
```java
    @GetMapping("")  
    public List<Customer> index() {  
        return customerRepository.findAll();  
    }
```

- wo wird die .jar Datei gespeichert
- wie finde ich die Datei

- Wenn du Tests rein machst dann beschreib auch was du testest

- wie heißt die datenbank auf den die Tabelle liegt (vermutlich customer api aber dinge aus quellqode herauslesen zu müssen ist scheiße)
- 