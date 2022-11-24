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

Customer:
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
spring.datasource.password=1234  
spring.jpa.hibernate.ddl-auto=update
```

Jetzt erstellen wir als Test das Maven Projekt. Dazu muss in IntelliJ das Maven Menü wie folgt geöffnet werden:
![[Pasted image 20221107132708.png]]

Nach dem sich das Menü geöffnet hat, installieren wir zum Test unsere Application:
![[Pasted image 20221107133358.png]]

Die Anwendung wird gebaut und in der local laufenden Datenbank wird die Tabelle "Customer" erstellt mit den Spalten "id", "Vorname" und "Nachname".

# Setup HeidiSQL

