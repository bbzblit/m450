# LB2 Integration Testing


# 1 Einleitung

Die Integration von Softwarekomponenten ist ein kritischer Schritt in der Entwicklung von Anwendungen. Um die reibungslose Funktion und die Zuverlässigkeit sicherzustellen, ist ein umfassendes Integration Testing unerlässlich. Diese Dokumentation beschreibt die Testumgebung, die Schwerpunkte und die Testfälle für das Integration Testing der LB2-Anwendung.


# 2 Testumgebung


<table>
  <tr>
   <td>Kategorie
   </td>
   <td>Konfiguration
   </td>
  </tr>
  <tr>
   <td colspan="2" >Software Konfiguration
   </td>
  </tr>
  <tr>
   <td>Reverse Proxy
   </td>
   <td>Nginx
   </td>
  </tr>
  <tr>
   <td>Container Management
   </td>
   <td>Portainer EE
   </td>
  </tr>
  <tr>
   <td>Base Docker Image
   </td>
   <td>eclipse-temurin:17-jre
   </td>
  </tr>
  <tr>
   <td>Datenbank 
   </td>
   <td>MongoDB
   </td>
  </tr>
  <tr>
   <td>Frameworks
   </td>
   <td>JPA, Junit, lombok, Spring Rest
   </td>
  </tr>
  <tr>
   <td>OS
   </td>
   <td>Debian 12 (bookworm)
   </td>
  </tr>
  <tr>
   <td>Wireguard
   </td>
   <td>VPN Connection zu den Management Endpoints
   </td>
  </tr>
  <tr>
   <td colspan="2" >Hardware Konfiguration
   </td>
  </tr>
  <tr>
   <td>vCPU Cores
   </td>
   <td>4 - Ampere Processor (arm64)
   </td>
  </tr>
  <tr>
   <td>RAM
   </td>
   <td>8 GB - DDR4 ECC
   </td>
  </tr>
  <tr>
   <td>SSD
   </td>
   <td>80 GB - Raid 10
   </td>
  </tr>
  <tr>
   <td>Netzwerk
   </td>
   <td>10 GBit - Redundant Network
   </td>
  </tr>
</table>



# 


# 3 Schwerpunkte und Begründung


## 3.1 Datenbankverbindung

Begründung: Eine stabile Verbindung zur Datenbank ist entscheidend für die Funktionalität der Anwendung. Die Datenbank stellt das Herzstück dar, in dem Informationen gespeichert, abgerufen und aktualisiert werden. Eine unterbrochene Verbindung kann zu Ausfällen und Beeinträchtigungen der Anwendungsleistung führen. Daher ist es von grundlegender Bedeutung, die Robustheit und Zuverlässigkeit der Datenbankverbindung sicherzustellen.


## 3.2 Datenintegrität

Begründung: Die korrekte Speicherung und der Abruf von Daten sind essentiell, um die Datenintegrität sicherzustellen. Datenintegrität bezieht sich darauf, dass die in einem System gespeicherten Daten korrekt und unverändert bleiben. Dies ist von entscheidender Bedeutung, um sicherzustellen, dass Informationen zuverlässig und genau sind.


## 3.3 Datenvalidierung

Begründung: Es ist wichtig, dass korrekte Daten in der Datenbank gespeichert werden, um zu verhindern, dass inkonsistente Zustände in der Datenbank vorkommen. Inkonsistente oder falsche Daten können dazu führen, dass die Applikation schwerer bedienbar oder gar unbrauchbar ist. 


# 


# 4 Beschreibung der Testfälle


## 4.1 Datenbankverbindung


### Testfall 1: Testen einer erfolgreichen Verbindung

Schritte:



* Starten der Datenbank (wenn sie nicht bereits läuft)
* Starten der Applikation
* Sicherstellen, dass die Applikation erfolgreich ohne Fehler gestartet ist
* Versuchen, über die Applikation Testdaten in die Datenbank zu schreiben

Erwartetes Ergebnis:



* Daten können ohne Probleme eingefügt werden und die Applikation läuft


### Testfall 2: Testen einer nicht erfolgreichen Verbindung

Schritte:



* Sicherstellen das die Datenbank nicht läuft
* Starten der Applikation
* Überprüfen, ob die Applikation gestartet ist und wenn nicht weshalb sie nicht gestartet ist

Erwartete Ergebnisse:



* Applikation ist nicht gestartet
* Hilfreiche Errormessage wird in den Logs angezeigt


### Testfall 3: Testen einer abgebrochenen Verbindung

Schritte:



* Sicherstellen, dass die Datenbank läuft
* Starten der Applikation
* Sicherstellen, dass die Applikation erfolgreich ohne Fehler gestartet ist
* Herunterfahren der Datenbank
* Überprüfen, ob ein Error in den Logs angezeigt wird

Erwartete Ergebnisse:



* Applikation ist nicht abgestürzt
* Beim Benutzen der Applikation wird ein Error angezeigt
* Errormessage ist in den Logs ersichtlich


## 


## 4.2 Datenintegrität


### Testfall 4:** **Daten korrekt in die Datenbank einfügen (create)

Schritte:



* Starten der Applikation
* Eingabe der Daten in die entsprechenden Felder über eine API-Anfrage
* Senden der API-Anfrage
* Warten auf Response der API und überprüfen, ob diese erfolgreich war
* Überprüfen, ob die eingefügten Daten korrekt in der Datenbank erscheinen (mittels direkter Abfrage der Datenbank)

Erwartete Ergebnisse:



* Bestätigung, dass die Daten erfolgreich eingefügt wurden
* Daten sind korrekt und vollständig in der Datenbank gespeichert
* Keine Fehlermeldung in der Applikation oder in den Server Logs


### Testfall 5: Daten aus der Datenbank lesen (read)

Schritte:



* Starten der Applikation
* Daten mittels einer API-Anfrage anfordern
* Überprüfen, ob die angezeigten oder abgerufenen Daten mit den erwarteten Daten übereinstimmen
* Vergleich der Datenanzeige in der Applikation mit den direkten Datenbankinhalten, falls möglich

Erwartete Ergebnisse:



* Daten werden korrekt und vollständig angezeigt
* Ergebnis der Datenanzeige entspricht den gespeicherten Daten in der Datenbank
* Keine Fehlermeldung in der Applikation oder in den Server-Logs


### Testfall 6:** **Daten ändern (update)

Schritte:



* Starten der Applikation
* Ändern der Daten mittels einer API-Anfrage
* Überprüfen, ob die API-Response keine Fehler aufweist
* Überprüfen, ob die Änderungen korrekt in die Datenbank übernommen wurden

Erwartete Ergebnisse:



* Korrekte und vollständige Aktualisierung der Daten in der Datenbank
* Die Applikation zeigt eine Erfolgsmeldung an, keine Fehler
* Die Datenbank weist die neuen Werte auf, welche zuvor geändert worden sind


### Testfall 7: Daten löschen (delete)

Schritte:



* Starten der Applikation
* Auslösen des Löschvorgangs durch das Senden einer API-Anfrage
* Überprüfen, ob die API Response keine Fehler aufweist
* Überprüfen in der Datenbank, ob die Datensätze vollständig entfernt wurden

Erwartete Ergebnisse:



* Gelöschte Daten sind nicht mehr in der Datenbank vorhanden
* Keine Rückverweise oder Artefakte der gelöschten Datensätze in verwandten Tabellen, falls relationale Integrität gefordert ist
* Applikation oder API zeigt die erfolgreiche Löschung ohne Fehlermeldungen an


## 4.3 Datenvalidierung


### Testfall 8: Erfassung gültiger Daten

Schritte:



* Starten der Applikation
* Senden eines korrekten Datenmodells über die API
* Überprüfen, ob die API Request erfolgreich durchgelaufen ist
* Überprüfen in der Datenbank, ob der neue Datensatz korrekt eingefügt wurde

Erwartete Ergebnisse:



* Die Applikation zeigt eine Erfolgsmeldung an, keine Fehler
* Die Datenbank enthält den neuen Datensatz mit den korrekt eingetragenen Daten


### Testfall 9: Erfassung ungültiger Daten

Schritte:



* Starten der Applikation
* Eingaben von falschen Daten per API Request. (Irgendein beliebiges Feld weglassen)
* Überprüfen, ob die API mit einem Fehler geantwortet hat 
* Überprüfen der Datenbank, ob der fehlerhafte Datensatz nicht eingefügt wurde

Erwartete Ergebnisse:



*  Die Applikation zeigt eine klar verständliche Fehlermeldung an, die auf das spezifische Problem hinweist
* Keine fehlerhaften Daten werden in die Datenbank eingefügt


### Testfall 10: Aktualisierung von Daten mit ungültigen Werten

Schritte:



* Starten der Applikation
* Updaten eines Datensatzes mit ungültigen Daten (Daten, bei denen ein Feld weggelassen ist) mithilfe der API
* Überprüfen, ob die API mit einem Fehler antwortet
* Überprüfen in der Datenbank, ob die Daten unverändert bleiben

Erwartete Ergebnisse:



* Die Applikation zeigt eine Fehlermeldung an, die den Benutzer auf das Problem mit den ungültigen Daten hinweist
* Die ursprünglichen, gültigen Daten bleiben in der Datenbank unverändert, es erfolgt kein Update mit ungültigen Daten


# 


# 5 Der Tests und Resultate


<table>
  <tr>
   <td>Testfall
   </td>
   <td>Ergebnis
   </td>
   <td>Bemerkung
   </td>
  </tr>
  <tr>
   <td colspan="3" >Datenbankverbindung
   </td>
  </tr>
  <tr>
   <td>Testfall 1
   </td>
   <td>0
   </td>
   <td>Funktioniert ohne Probleme.
   </td>
  </tr>
  <tr>
   <td>Testfall 2
   </td>
   <td>0
   </td>
   <td>Funktioniert ohne Probleme.
   </td>
  </tr>
  <tr>
   <td>Testfall 3
   </td>
   <td>2
   </td>
   <td>Beim Benutzen der Applikation wird zwar ein Error angezeigt. Allerdings handelt es sich dabei um den default Error von Spring Boot. (Screenshot 1 im Anhang)
<p>
Für eine bessere Verwendbarkeit wäre es vorteilhaft, wenn diese auf Deutsch ersichtlich wäre.
   </td>
  </tr>
  <tr>
   <td colspan="3" >Datenintegrität
   </td>
  </tr>
  <tr>
   <td>Testfall 4
   </td>
   <td>0
   </td>
   <td>Daten sind in der Datenbank ersichtlich und der HTTP Code 201 wird zurückgegeben.
   </td>
  </tr>
  <tr>
   <td>Testfall 5
   </td>
   <td>0
   </td>
   <td>Alle Daten werden korrekt und vollständig in der UI angezeigt. Auch Umlaute sind korrekt dargestellt.
   </td>
  </tr>
  <tr>
   <td>Testfall 6
   </td>
   <td>0
   </td>
   <td>Alle Daten werden korrekt geändert + API request gibt einen HTTP 200 zurück.
   </td>
  </tr>
  <tr>
   <td>Testfall 7
   </td>
   <td>0
   </td>
   <td>Daten wurden gelöscht.
   </td>
  </tr>
  <tr>
   <td colspan="3" >Datenvalidierung
   </td>
  </tr>
  <tr>
   <td>Testfall 8
   </td>
   <td>0
   </td>
   <td>Funktioniert.
   </td>
  </tr>
  <tr>
   <td>Testfall 9
   </td>
   <td>1
   </td>
   <td>Funktioniert - Allerdings hat es einen Rechtschreibfehler in der Fehlermeldung.
<p>
(Screenshot 2 im Anhang)
   </td>
  </tr>
  <tr>
   <td>Testfall 10
   </td>
   <td>1
   </td>
   <td>Gleiches Problem wie bei Testfall 9. (Screenshot 2 im Anhang)
   </td>
  </tr>
</table>


**Bewertungsskala**:


<table>
  <tr>
   <td>0
   </td>
   <td>kein Mangel
   </td>
  </tr>
  <tr>
   <td>1
   </td>
   <td>belangloser Mangel Verwendung möglich, Brauchbarkeit ist vorhanden, Mängel sollte dennoch nicht vorkommen
   </td>
  </tr>
  <tr>
   <td>2
   </td>
   <td>leichter Mangel Verwendung möglich, Brauchbarkeit ist nur wenig beeinträchtigt
   </td>
  </tr>
  <tr>
   <td>3
   </td>
   <td>schwerer Mangel Verwendung ist noch möglich, Brauchbarkeit ist stark verringert
   </td>
  </tr>
  <tr>
   <td>4
   </td>
   <td>kritischer Mangel unbrauchbar
   </td>
  </tr>
</table>



# 6 Anhang

**Screenshot 1**:



![Image1](images/image1.png)


Hier wird die default Fehlermeldung von Spring Boot angezeigt, der besagt, dass die Verbindung zur Datenbank nicht aufgebaut werden konnte. Dieser ist schwer für den user verständlich und könnte auch sensitive Daten beinhalten

**Screenshot 2**:


![Image2](images/image2.png

![alt_text](images/image2.png "image_tooltip")


Hier wurde ein Rechtschreibfehler in der Fehlermeldung gemacht. Anstatt “an title” müsste “a title” stehen.
