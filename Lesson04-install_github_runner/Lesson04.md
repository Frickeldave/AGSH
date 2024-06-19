# Lesson 04 - Installation des GitHub runners

In diesem Kapitel erfährst du, wie du den GitHub runner auf deinem System installierst. Der GitHub Runner ist eine Ausführungskomponente von GitHub, die sogenannte "Workflows" starten kann, die du in Form von YAML Dateien in deinem Repository ablegst (mehr dazu später).
Bei GitHub sind 2 Arten von Runnern verfügbar: Der Cloud-Native Runner für den GitHub Ausführungsinstanzen zur Verfügung stellt und "self-hosted-runner" die du selber, je nach Anwendungsgebiet, auf Windows-, Linux oder MAC Systeme installieren kannst.
In diesem Training ist ein self-hosted runner notwendig, da wir mit dem runner auf ein lokales System zugreifen wollen.

## Anlegen des Benutzers in dessen Kontext der github runner läuft

Der runner soll im Kontext eines expiziten Users "github" ausgeführt werden (Für den produktiven Einsatz wähle einen anderen Benutzernamen um den Agriffsvektor auf das System klein zu halten).

Die folgenden Schritte sind auf dem Ubunutu System auszuführen um den User anzulegen:

- Erstellen einer Gruppe, die es Mitgliedern ermöglicht, administrative Tasks auszuführen ohne Ihr Kennwort einzugeben ```sudo /sbin/addgroup sudo-nopasswd```
- Vergabe von sudo-Berechtigungen für die Gruppe ```sudo bash -c 'echo "%sudo-nopasswd ALL=(ALL:ALL) NOPASSWD:ALL" > /etc/sudoers.d/sudo-nopasswd'```
- Anlegen des Benutzers "github"  
  ```sudo /sbin/adduser --force-badname --home /home/github --disabled-password --gecos "" --shell /bin/bash github```
- Ändern des Kennworts des Benutzers  
  ```sudo passwd github```
- Hinzufügen des Benutzers zur zurov angelegten Gruppe
  ```sudo /sbin/usermod -a -G sudo-nopasswd github```
- Login as github runner  

## Installation des github runners

Ist der "github" benutzer angelegt, wird der GitHub Runner installiert. Wechsel hierfür zum  Benutzer *"github"* und wechsel in das Homeverzeichnis.

```bash
sudo su github
cd~
```

Die folgeden Script-Snippets sind nur Beispiele zum Stand der Erstellung dieses Artikels. Für ein aktuelles Script, wechsele bitte in das GitHub Repository, welches du in [Lesson 3](../Lesson03-create_gh_repository/Lesson03.md) angelegt hast. Navigiere dort zu *"Settings -> Actions -> Runners"*, klicke auf *"New self-hosted runner"* und wähle *"Linux"* als Betriebssystem.

![Settings of the github runner runner](./Screenshot%202024-06-07%20131043.png)

![Script to install the github runner](./Screenshot%202024-06-07%20131401.png)

- Erstelle das Verzeichnis "actions-runner" und wechsel in dieses  
  ```mkdir actions-runner && cd actions-runner```  
- Lade den aktuellen runner herunter
  ```curl -o actions-runner-linux-x64-2.317.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.317.0/actions-runner-linux-x64-2.317.0.tar.gz```  
- Validiere die hashsumme der heruntergeladenen Datei
  ```echo "9e883d210df8c6028aff475475a457d380353f9d01877d51cc01a17b2a91161d  actions-runner-linux-x64-2.317.0.tar.gz" | shasum -a 256 -c```  
- Entpacke den Installer
  ```tar xzf ./actions-runner-linux-x64-2.317.0.tar.gz```
- Entferne die heruntergeladene TAR Datei
  ```rm ./actions-runner-linux-x64-2.317.0.tar.gz```

## Konfiguration des github-runner

- Erstelle die Konfiguration des Runners (Nutze hier die Standardeinstellungen -> Alles mit "Enter" akzeptieren). Für diese Befehl sind einige Werte zu ersetzen, bitte schaue diese in deiner Runner-Konfigurationsseite nach.
  ```./config.sh --url <URLOFYOURREPO> --token <YOURTOKEN>```

- Führe nun **nicht**, wie auf der github runner Konfigrationsseite beschrieben, den Befehl ```./run.sh``` aus. Sondern mache folgendes:

```bash
sudo ./svc.sh install
sudo ./svc.sh start
```

Hiermit wird der runner als daemon installiert und startet automatisch, wenn du das System hochfährst.

## Verfügbarkeit des github-runners kontrollieren

Im letzten Schritt gehe auf deine GitHub-Runner Konfigurationsseite und prüfe, ob der GitHub Runner mit dem GitHub Backend kommuniziert.

![Check github runner](./Screenshot%202024-06-10%20080659.png)

- [Zurück zur Startseite](./../README.md)
- [Voriges Kapitel](../Lesson03-create_gh_repository/Lesson03.md)
- [Nächstes Kapitel](./../Lesson05-create_cicd_workflow/lesson05.md)