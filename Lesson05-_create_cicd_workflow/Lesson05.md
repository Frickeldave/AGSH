# Lesson 05 - Einrichten des CICD workflows

Sind alle Vorbereitungen getroffen, wird ein CICD Workflow eingerichtet, der bei jedem Push ausgeführt wird. Dieser Workflow führt folgende Dinge aus:

- **Auschecken des Repositories in ein dediziertes Verzeichnis.** Hier erzeugen wir das Verzeichnis mit Hilfe eines Zufallgenerators um Konflikte mit bestehenden Verzeichnissen zu vermeiden.
- **Aktualisierung des Betriebssystem falls notwendig.** Man kann vortrefflich darüber streiten, welche Strategie hier die die beste ist. Immer das System auf Stand halten oder zu definierten Zeitpunkten kontrollierte Updates fahren. Da letzteres ein erhöhtes Maßa an Planung und Disziplin benötigt, bevorzuge ich zeitnahe Updates. Möchte man nicht jede Änderung sofort einspielen, empfiehlt sich im professionellen Umfeld die Etablierung eines eigenen Ubuntu Repositories.
- **Installation von Ansible.** Ansible wird über ein virtuelles python environment (venv) installiert. Dabei wird die Versionsnummer explizit angegeben. Im Falle eines Updates muss diese entsprechend angepasst werden. Die venv Funktionalität ist auch wunderbar geeignet um mehrere Ansible Versionen prallel zu betreiben.
- **Kopieren aller Ansible Dateien.** Es werden alle Playbooks, Roles und Konfigurationsdateien für Ansible auf das lokale System kopiert.
- **Kopieren einer Secret Datei für den Ansible vault.** Erstellen einer Secret Datei aufgrund eines in GitHub hinterlegten Secure-Strings. Dies ist auch der Grund, warum das Repository auf "private" gestellt werden muss.
- **Kopieren einer private-key Datei für den Zugriff auf Zielsysteme**. Ansible benötigt für den Zugriff auf Linux Systeme einen Private Key (Im Falle von Windows gibt es mehrere Möglichkeiten, die nicht Teil dieses Workshops sind). Dieser Private Key ist, genauso wie das vault-secret, im GitHub Repository gespeichert.

**HINWEIS:** Die genaue Beschreibung, was in der YMAL Datei passiert, ist Teil des interaktiven Workshops.

## Eintragen des Secrets für den Ansible-Vault

Der Ansible vault ist  eine integrierte secret management Lösung, die es Dir ermöglicht, einzelne Strings oder auch ganze Dateien mit dem Befehl ```ansible-vault``` zu verschlüsseln und im Git Repository abzulegen. Damit Ansible während der Laufzeit aber die String auch wieder entschlüsseln kann, benötigt es das Kennwort mit dem die Verschlüsselung durchgeführt worden ist. Natürlich landet dieses Kennwort nicht einfach im Repository, sondern in einem GitHub Secret. Dieses Secret wird während der CICD Phase (also immer wenn neuer Code gepushed wird) ausgelesen und in eine gesicherte lokale Datei auf dem Ansible Server abgelegt.

Zuvor müssen die Secrets in GitHub eingetragen werden. Navigiere dafür in deine *Repository-Settings -> Secrets and variables -> Actions* und lege ein neues Secret mit dem Namen ```AGSH_ANSIBLE_VAULT``` mit dem Wert ```aadmin``` an.

![Create new secret navigation](./Screenshot%202024-06-07%20150207.png)

![Create new secret](./Screenshot%202024-06-07%20150500.png)

## Eintragen des Screts für den Private-Key

Als erstes muss ein private-key erzeugt werden. Öffne dafür eine Powershell Konosle (oder gerne auch eine Bash, Hauptsache ssh-keygen ist verfügbar) und erzeuge ein private/public key-pair wie folgt: 

```powershell
PS> ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\USERNAME/.ssh/id_rsa): c:\users\USERNAME\.ssh\id_rsa_AGSH
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in c:\users\USERNAME\.ssh\id_rsa_AGSH
Your public key has been saved in c:\users\USERNAME\.ssh\id_rsa_AGSH.pub
The key fingerprint is:
SHA256:HSpRpt3D7g5Big3rYj+joShDJvyCeklk2ewuISvA7P0 ansible@frickeldave
The key's randomart image is:
+---[RSA 4096]----+
|        o        |
|       = o       |
|   +. o o =      |
|  + o= + + o     |
|+o .o + S o      |
|+*o..  . o       |
|B+*+.   . .      |
|B=+*+    o       |
|BooooE    .      |
+----[SHA256]-----+
PS >
```

Öffne nun die erzeugte Datei "*C:\Users\dakoenig\.ssh\id_rsa_AGSH*" und kopiere den Inhalt in die Zwischenablage, lege ein GitHub Secret mit dem Namen ```AGSH_ANSIBLE_PRVKEY``` und dem Wert ```aadmin``` an und kopiere den Inhalt der Zwischenablage in den "Value". Beachte dabei, dass nach dem private Key noch eine Leerzeile enthalten sein muss. 

## Anlegen des Workflows

Offne in deinem favorisierten Code-Editor das zuvor heruntergeladene Repository und lege in diesem die Verzeichnisse ```.github/worklows``` an. Wenn du in diesem Verzeichnis eine YAML Datei mit der Workflow Definition ablegst, wird diese automatisch von GitHub konsumiert und entsprechend den Angaben in der YAML Datei ausgeführt. Das *wann*, *wo* und *was* definierst du dabei alles in der YAML Datei.

![Workflow directory](Screenshot%202024-06-07%20130623.png)

Kopiere anschliessend die Datei [cicd.yaml](./cicd.yaml) aus diesem Repository in dein Zielrepository in den Pfad ```.github/workflows``` und passe die Zeile 23 ensprechend an.

## überprüfen der Ergebnisse

Anschließend kannst du alle Änderungen synchronisieren (push). Im Reiter "Actions" auf GitHub sollte nun der cicd Workflow starten und erfolgreich ausgeführt werden.

![Überprüfen des workflows](./Screenshot%202024-06-07%20154603.png)

Klickst du auf den Workflow erhälst du detaillierte Informationen über jeden einzelnen Workflow Schritt und kannst dir im Details die Ergebnisse anschauen.

# Erklärung des Zielverzeichnisses

Hast du einmal in die cicd.yaml reingeschaut, ist Dir vielleicht aufgefallen, dass alle Ansible Dateien in das Verzeichnis ```/home/github/ansible/ansible``` kopiert werden. Für das bessere Verständnis aller Folge-Kapitel, sollte dies noch näher erläutert werden.

- **/home/github** ist das Verzeichnis des Users, der den Prozess ausführt.
- **/home/github/ansible** ist das virtuelle Environment in dem Ansible läuft. Dies kann mit ```source ~/ansible/bin/activate``` aktiviert werden. Ab diesem Moment gelten die Einstellungen die in diesem Environment (Variablen, Pfade, etc.) definiert sind. Dies gilt nicht durchgängig, manche Pfade wie ```/``` oder ```~``` gelten nach wie vor weiter. Somit sind aber die Ansible executables wie *ansible*, *ansible-vault*, oder *ansible-playbook* nur verfügbar, wenn zuvor das venv aktiviert wird. Selbstverständlich kann dies in einer einzigen Semikolon-getrennten Befehlskette passieren: ```source ~/ansible/bin/activate; ansible --version;deactivate```.
- **/home/github/ansible/ansible** bezeichnet dann das Verzeichnis, in dem alle notwendigen Ansible Dateien liegen, die im nächsten Kapitel erzeugt werden.

## Testen von ansible

Führe nun einen Ansible-Ping aus, der überprüft ob der Zugriff funktioniert.

```bash
ansible -m ping localhost
```

Du wirst die folgende Ausgabe erhalten: 

  ```json
[WARNING]: No inventory was parsed, only implicit localhost is available
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

Das "WARNING" sagt aus, dass ansible aktuell kein Inventar findet und dewegen virtuell, nur für diese Ausführung, einen localhost Eintrag erzeugt. Dies werden wir in den folgenden 2 Lessons korrigieren.

- [Zurück zur Startseite](./../README.md)
- [Voriges Kapitel](../Lesson04-install_github_runner/Lesson04.md)
- [Nächstes Kapitel](./../Lesson06-create_ansible_user/Lession06.md)
