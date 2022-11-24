Um eine neue Virtuelle Maschiene zu erstellen klicken Sie im Dienst "Virtuelle Maschiene" auf "Erstellen" und anschließend im Drop-Down-Menü auf "Azure-VM" 

Danach können die folgenden Punkte, wie auf den Bildern zu sehen, konfiguriert werden. Durch klicken auf die Ausrufezeichen hinter den Punkten kann die jeweilige Erklärung zu diesen eingesehen werden 

![[Azure-VM_Grundeinstellungen.PNG]]
![[Azure-VM_Datenträger.PNG]]
![[Azure-VM_Netzwerk.PNG]]
![[Azure-VM_Verwaltung.PNG]]
![[Azure-VM_Monitoring.PNG]]
![[Azure-VM_Erweitert.PNG]]
![[Azure-VM_Tags.PNG]]

### Mit virtueller Maschine verbinden
Öffnen Sie die "Windows Terminal App", klicken sie oben rechts auf den Pfeil nach unten und wählen Sie Azure Cloud Shell aus
Führen Sie die gezeigten anweisungen aus

Sind die Schritte durchgeführt können Sie sich per SSH mit der öffentlichen IP-Adresse der Maschiene verbinden
```bash
ssh mxfo@20.160.221.206
```
anschließend muss das bei der Erstellung der VM festgelegte Passwort eingegeben werden
