# Lession 02 - Virtuelle Maschine erstellen

Nach der Erstellung der virtuellen Maschine muss auf dieser noch Betriebssystem installiert werden. Die Installation erfolgt im Grundsatz den Standardeinstellungen von Ubunutu 24.04.

Hast du im vorigen Schritt alles korrekt gemacht, sollte direkt nach dem Start der Installationsdialog von Ubuntu starten. Wähle hier den Menüpunkt "Try or Install Ubuntu Server".

![Start installation](Screenshot%202024-06-07%20121622.png)

Als Anzeigesprache kannst du eine beliebige wählen, im Rahmen dieser Anleitung wird "English" verwendet.

![Set display language](Screenshot%202024-06-07%20121653.png)

Das Tastaturlayout und die Variante (die die korrekte Kommasetzung und die Währung definiert) setzt du gemäß deinen Anforderungen.

![Set keyboard layout](Screenshot%202024-06-07%20121719.png)

Als Installationsart verwenden wir hier "Unbuntu Server (minimized)". Wir benötigen weder eine UI noch irgendwelche speziellen Serverkomponenten.

![Choose minimal installation](./Screenshot%202024-06-07%20121749.png)

Die Proxy Adresse kann leer gelassen werden.

![No proxy needed](./Screenshot%202024-06-07%20121841.png)

Warten im nächsten Dialog ein paar Sekunden. Der Ubuntu Installationsdialog überprüft hier, ob die Installationsquellen erreicht werden können.

![Check availability of source-repositories](./Screenshot%202024-06-07%20121904.png)

Bei der Partitionierung können alle Einstellungen im Standard übernommen werden.

![Partioning options](./Screenshot%202024-06-07%20121926.png)
![Partioning overview](./Screenshot%202024-06-07%20121952.png)

Es folgt noch einmal eine Sicherheitsabfrage, ob du wirklich die Festplatte formatieren möchtest. Diese msus mit "Continue" bestätigt werden.

![Confirm destructive action](./Screenshot%202024-06-07%20122020.png)

Während nun im Hintergrund die Installation durchläuft, kannst du schon einmal alle notwendigen Daten eingeben. In diesem Training werden folgenden Daten verwendet:

- Name: aadmin
- Servername: Master
- Username: aadmin
- Password: aadmin

![Set personal information](./Screenshot%202024-06-07%20122118.png)

Im folgenden Dialog kannst du die aktivierung von "Ubunutu Pro" überspringen.

![Skip Ubunutu Pro](./Screenshot%202024-06-07%20122147.png)

Der SSH Server ist dringed benötigt. Daher aktiviere hier bitte die Option "Install OpenSSH server". Ein SSH Key muss nicht import werden, im Rahmen dieses Trainings ist die simple Authentifizierung via Kennwort ausreichend.

![SSH Server](./Screenshot%202024-06-07%20122218.png)

Du kannst noch diverse Rollen auswählen, die auf dem System installiert werden können. Es wird keine dieser Rollen benötigt, daher kannst du mit der Installation einfach fortfahren.

![Install additional roles](./Screenshot%202024-06-07%20122239.png)

Im letzten Schritt wird das System noch einmnal neugestartet.

![Last step, installation log](./Screenshot%202024-06-07%20122529.png)

Nach dem Neustart teste bitte einmal den Zugriff auf den Server via SSH. Die Übertragung von Inhalten aus der Zwischenablage funktioniert eher Semi-gut, daher ist ein direkter SSH Zugriff von deinem Host-System auf die virtuelle Maschine absolut empfehlenswert. Es ist kein spezielles SSH Tools notwendig. Öffne einfach ein Powershell Session und starte die SSH Session mit folgendem Befehle:

```ssh aadmin@<IP ADDRESS OF VIRTUAL MASCHINE>```

Bestätige einmal den Fingerprint mit ```yes``` und authentifiziere dich mit deinem Benutzernamen und Kennwort.

![Logon via SSH](./Screenshot%202024-06-07%20132558.png)

- [Zurück zur Startseite](./../README.md)
- [Voriges Kapitel](./../Lesson01-Create_VM/Lesson01.md)
- [Nächstes Kapitel](./../Lesson03-Create_GH_Repo/Lesson03.md)