# Lesson 07 - Anlegen der Ansible Dateien

Die Ansible Dateien bestehen im wesentlichen aus den folgenden:

- **ansible.cfg** - Die Ansible Konfigurationsdatei
- **inventory.yaml** - Die Inventarisierung aller Systeme die verwaltet werden sollen
- **playbooks** - Die Sequenzen, die gegen die Zielsystem ausgeführt werden
- **roles** - Rollen fassen mehrer Befehle und Variablen zu einer logischen Einheit zusammen

Aktuell ist die cicd.yaml so gestaltet, dass der Job auch durchläuft, wenn keine Dateien vorhanden sind. Dies wird bei den betroffenen Befehlen mit einer vorgeschalteten if-Abfrage erreicht. Beispiel:

```bash
if [ -f ${{ env.GITHUB_WORKFLOW_WORKDIR }}/AGSH/ansible/ansible.cfg ]; then cp ${{ env.GITHUB_WORKFLOW_WORKDIR }}/AGSH/ansible/ansible.cfg /home/github/ansible/ansible/ansible.cfg; fi
```

Es wird in diesem Beispiel geprüft, ob im lokalen Quell-Repository die Datei "ansible.cfg" existiert. Wenn dies der Fall ist, wird die Datei in das Zielverzeichnis kopiert, andernfalls eben nicht. Aktuell wird nichts kopiert, als lass und dies nun vervollständigen.

## Kopieren aller Dateien

Dieses Kapitel beschreibt, welche Dateien in deinem Repository angelegt werden müssen und welche Bedeutung diese haben. Erstelle in deinem Repository das Verzeichnis *ansible* um mit den weiteren Kapiteln fort zu fahren.

### ansible.cfg

[Referenz](https://docs.ansible.com/ansible/latest/reference_appendices/config.html)

Die Datei "*ansible.cfg*" nimmt die grundsätzliche Konfiguration deiner Ansible Umgebung vor. Alle hier definierten Pfade sind relativ zum Arbeitsverzeichnis gesetzt. Führst du also z.B. ```ansible-playbook``` im Pfad */home/github/ansible/ansible* aus, wird das Inventar aufgrund der Zeile *inventory = ./hosts/inventory.yaml* im Pfad */home/github/ansible/ansible/hosts/inventory.yaml* gesucht.

**Todo:** Kopiere diese Datei in das ansible-root Verzeichnis in dein Repository.

### inventory.yaml

[Referenz](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html)

In dieser Datei notierst du alle Systeme, die du mit Ansible verwalten möchtest. Du kannst Gruppen bilden, für jeden Client/Gruppe beliebige Variablen definieren und die Verbindungseigenschaften zu den Systemen mit diesen Variablen steuern. Zudem kannst du auch die inventory.yaml mit beliebigen Datenquellen ersetzen. Zum Beispiel einer Maschinenliste aus AWS oder einer Datenbank.
In unserem Beispiel ist dort nur ein Eintrag vorhanden, nämlich *master* da wir alle folgenden Szenarien auf dem zentralen System *Master* umsetzen werden. Mit den [Ansible Connection Variables](https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html#connection-variables) kannst du den Verbindungsaufbau entsprechend anpassen. In diesem Beispiel wurde der Pfad zum ssh-private key und der Username mit dem die SSH Verbindung aufgebaut werden soll noch zusätzlich angegeben.

**Todo:** Kopiere diese Datei in das *hosts* Unterverzeichnis von *ansible* in deinem Repositories.

### playbooks

[Referenz](https://docs.ansible.com/ansible/latest/getting_started/get_started_playbook.html)

Playbooks enthalten alle Befehle, die gegen ein-, oder mehrere Systeme ausgeführt werden können. Playbooks werden mit der executable ```ansible-playbook``` ausgeführt. Die Befehle, die in den Playbooks notiert werden können, sind in der ansible Dokumentation ausführlich beschrieben und abhängig davon, welche Module Dir zur Verfügung stehen. Grundsätzlich kannst du mindestens die Befehle verwenden, die im [Ansible-Core](https://docs.ansible.com/ansible-core/devel/index.html) Modul enthalten sind. Mit [ansible-galaxy](https://galaxy.ansible.com/ui) kannst du weitere Module nachladen. 

**Todo:** Kopiere die Dateien "*playbooks/baseline.yaml*" und "*playbooks/master.yaml*" in das Unterverzeichnis "*ansible/playbooks*" in deinem Repositories.

### roles

[Referenz](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)

Roles sind den Playbooks sehr ähnnlich. Sie enthalten eine Auflistung von Befehlen, die gegen einen Client ausgeführt werden können. Nun ist es i.d.R. notwendig, für eine Applikation oder Konfiguration mehr als einen Befehl zu verwenden, die alle in einer Rolle zusammengefasst werden können. Zudem können in Rollen noch jede Menge weitere Informationen untergebracht werden, zum Beispiel Templates, Variables, Dateien, Meta-Informationen, und einiges mehr.

Rollen werden aus einem Plabook mit folgendem Befehl aufgerufen: 

```yaml
- name: Configure ssh
  include_role:
    name: cfg-ssh
```

Der Verzeichnisname der Rolle muss in diesem Fall "cfg-ssh" lauten. Ansible sucht die Rolle aufgrund der Pfadangabe in der ansible.cfg Datei.

**Todo:** Kopiere alle in "*ansible/playbooks/roles* enthaltenen Verzeichnisse und Dateien in das Unterverzeichnis "*ansible/playbooks/roles*" in deinem Repository.

**Hinweis:** Die Bereitstellung deiner Rollen im zentralen Ansible Repository ist total sinnvoll in kleinen Umgebungen mit einer überschaubaren Anzahl von Änderungen. In größeren Umgebungen kannst du Rollen auch in jeweils eigene Repositories ausgliedern und mit [ansible-galaxy](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#installing-roles-from-galaxy) dynamisch einbinden.

## Update deiner Ansible Server Installation

Wir haben in [Lesson05](./../Lesson05-create_cicd_workflow/Lesson05.md) den Workflow eingerichtet, der das System bei jedem Git-Push aktualisiert. Dies nutzen wir nun, um die dem GitHub Repository neu hinzugefügten Dateien auf die VM zu deployen. Pushe also nun die Änderungen in deinem Repository und überprüfe auf dem System, ob alles vorhanden ist.

## Testen deiner Ansible Installation

Zum testen der Installation wiederholen wir den Aufruf aus [Lesson05](./../Lesson05-create_cicd_workflow/Lesson05.md).

```bash
source ~/ansible/bin/activate;cd ~/ansible/ansible;ansible -m ping localhost;deactivate
```

Im Gegensatz zum ersten Test sollte jetzt keine Warnung mehr auftauchen, dass das inventory nicht geparsed werden konnte. Der Client wird im Inventory gefunden und Ansible verbindet sich erfolgreich via SSH:

```json
master | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.12"
    },
    "changed": false,
    "ping": "pong"
}
```

Jetzt wollen wir natürlich auch noch wissen, ob wir wirklich was am Client ausführen können. Also führen wir das allererste mal ein playbook aus.

```bash
source ~/ansible/bin/activate;cd ~/ansible/ansible;ansible-playbook ./playbooks/master.yaml;deactivate
```

Mit diesem Playbook passiert noch nicht all zu viel, aber wir bekommen im besten Fall eine Übersicht über die erfolgreiche Ausführung:

```bash
PLAY [Master] *************************************************************************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************************************************
ok: [master]

TASK [Output some basic stuff] ********************************************************************************************************************************************************
ok: [master] => {
    "msg": "This is a very first debug message from the AGSH workshop lesson"
}

PLAY RECAP ****************************************************************************************************************************************************************************
master                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

- [Zurück zur Startseite](./../README.md)
- [Voriges Kapitel](./../Lesson06-create_ansible_user/Lession06.md)
- [Nächstes Kapitel](./../Lesson08-create-playbook-workflow/Lesson08.md)