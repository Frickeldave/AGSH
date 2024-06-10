# Lesson 05 - Einrichten des CICD workflows

Sind alle Vorbereitungen getroffen, wird ein CICD Workflow eingerichtet, der bei jedem Push ausgeführt wird. Dieser Workflow führt folgende Dinge aus:

- **Auschecken des Repositories in ein dediziertes Verzeichnis.** Hier erzeugen wir das Verzeichnis mit Hilfe eines Zufallgenerators um Konflikte mit bestehenden Verzeichnissen zu vermeiden.
- **Aktualisierung des Betriebssystem falls notwendig.** Man kann vortrefflich darüber streiten, welche Strategie hier die die beste ist. Immer das System auf Stand halten oder zu definierten Zeitpunkten kontrollierte Updates fahren. Da letzteres ein erhöhtes Maßa an Planung und Disziplin benötigt, bevorzuge ich zeitnahe Updates. Möchte man nicht jede Änderung sofort einspielen, empfiehlt sich im professionellen Umfeld die Etablierung eines eigenen Ubuntu Repositories.
- **Installation von Ansible.** Ansible wird ein virtuelles python environment (venv) installiert. Dabei wird die Versionsnummer explizit angegeben. Im Falle eines Updates muss diese entsprechend angepasst werden. Die venv Funktionalität ist auch wunderbar geeignet um mehrere Ansible Versionen prallel zu betreiben.
- **Kopieren aller Ansible Dateien.** Es werden alle Palybooks, Roles und Konfigurationsdateien für Ansible auf das lokale System kopiert.
- **Erzeugen einer Secret Datei für den Ansible vault.** Das Secret für diese Datei wird aus einem Scret generiert, welches wir im Github Repository anlegen. DIes ist auch der Grund, warum das Repository auf "private" gestellt werden muss.
- **Erzeugen einer private-key Datei für den Zugriff auf Zielsysteme**. Ansible benötigt für den Zugriff auf Linux Systeme einen Private Key (Im Falle von Windows gibt es mehrere Möglichkeiten, die nicht Teil dieses Workshops sind). Dieser Private Key ist, genauso wie das vault-secret, im GitHub Repository gespeichert.

**HINWEIS:** Die genaue Beschreibung, was in der YMAL Datei passiert, ist Teil des interaktiven Workshops.

## Eintragen der Secrets

Der Ansible vault ist  eine integrierte secret management Lösung, die es Dir ermöglicht, einzelne Strings oder auch ganze Dateien mit dem Befehl ```ansible-vault``` zu verschlüsseln und im Git Repository abzulegen. Damit Ansible während der Laufzeit aber die String auch wieder entschlüsseln kann, benötigt es das Kennwort mit dem die Verschlüsselung durchgeführt worden ist. Natürlich landet dieses Kennwort nicht einfach im Repository, sondern in einem GitHub Secret. Dieses Secret wird während der CICD Phase (also immer wenn neuer Code gepushed wird) ausgelesen und in eine gesicherte lokale Datei auf dem Ansible Server abgelegt.

Zuvor müssen die Secrets in GitHub eingetragen werden. Navigiere dafür in deine *Repository-Settings -> Secrets and variables -> Actions* und lege ein neues Secret mit dem Namen ```AGSH_ANSIBLE_VAULT``` mit dem Wert ```aadmin``` an.

![Create new secret navigation](./Screenshot%202024-06-07%20150207.png)

![Create new secret](./Screenshot%202024-06-07%20150500.png)

Wiederhole diesen Schritt, diesmal lege aber ein Secret mit dem Namen ```AGSH_ANSIBLE_PRVKEY``` und dem Wert ```aadmin``` an.

## Anlegen des Workflows

Offne in deinem favorisierten Code-Editor das zuvor heruntergeladene Repository und lege in diesem die Verzeichnisse ```.github/worklows``` an. Wenn du in diesem Verzeichnis eine YAML Datei mit der Workflow Definition ablegst, wird diese automatisch von GitHub konsumiert und entsprechend den Angaben in der YAML Datei ausgeführt. Das *wann*, *wo* und *was* definierst du dabei alles in der YAML Datei.

![Workflow directory](Screenshot%202024-06-07%20130623.png)

Kopiere anschliessend die Datei [cicd.yaml](./cicd.yaml) aus diesem Repository in dein Zielrepository in den Pfad ```.github/workflows``` und passe die Zeile 23 ensprechend an.

## überprüfen der Ergebnisse

Anschließend kannst du alle Änderungen synchronisieren (push). Im Reiter "Actions" auf GitHub sollte nun der cicd Workflow starten und erfolgreich ausgeführt werden.

![Überprüfen des workflows](./Screenshot%202024-06-07%20154603.png)

Klickst du auf den Workflow erhälst du detaillierte Informationen über jeden einzelnen Workflow Schritt und kannst dir im Details die Ergebnisse anschauen.

- [Zurück zur Startseite](./../README.md)
- [Voriges Kapitel](./../Lesson04-Install_GH_Runner/Lesson04.md)
- [Nächstes Kapitel](./../Lesson06-Create_Ansible_playbook_workflow/Lesson06.md)
