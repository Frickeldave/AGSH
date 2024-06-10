# Lesson 06 - Einrichten eines workflows zum Ausführen von Ansible Playbooks

Sind alle Vorbereitungen getroffen, wird der CICD Workflow eingerichtet, der die Installation und Wartung von Ansible auf dem Master System automatisiert und sich darum kümmert dass alle Ansible Dateien immer in aktuellem Stand auf dem Master Server liegen.


## Anlegen des Workflows

Offne in deinem favorisierten Code-Editor das zuvor heruntergeladene Repository und lege in diesem die Verzeichnisse ```.github/worklows``` an. Wenn du in diesem Verzeichnis eine YAML Datei mit der Workflow Definition ablegst, wird diese automatisch von GitHub konsumiert und entsprechend den Angaben in der YAML Datei ausgeführt. Das *wann*, *wo* und *was* definierst du dabei alles in der YAML Datei.

**HINWEIS:** Die genaue Beschreibung, was in der YMAL Datei passiert, ist Teil des interaktiven Workshops.

![Workflow directory](Screenshot%202024-06-07%20130623.png)

Kopiere anschliessend die Datei [cicd.yaml](./cicd.yaml) aus diesem Repository in dein Zielrepository in den Pfad ```.github/workflows``` und passe die Zeile 23 ensprechend an.

Anschließend kannst du alle Änderungen synchronisieren (push). Im Reiter "Actions" auf GitHub sollte nun der cicd Workflow starten und erfolgreich ausgeführt werden.

![Überprüfen des workflows](./Screenshot%202024-06-07%20154603.png)

Klickst du auf den Workflow erhälst du detaillierte Informationen über jeden einzelnen Workflow Schritt und kannst dir im Details die Ergebnisse anschauen.

- [Zurück zur Startseite](./../README.md)
- [Voriges Kapitel](./../Lesson04-Install_GH_Runner/Lesson04.md)
- [Nächstes Kapitel](./../Lesson06-Create_Ansible_playbook_workflow/Lesson06.md)