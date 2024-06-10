# Lesson 06 - Anlegen der Ansible Dateien

Die Ansible Dateien bestehen im wesentlichen aus den folgenden:

- **ansible.cfg** - Die Ansible Konfigurationsdatei
- **inventory.yaml** - Die Inventarisierung aller Systeme die verwaltet werden sollen
- **playbooks** - Die Sequenzen, die letztendlich gegen die Zielsystem ausgeführt werden
- **roles** - Rollen fassen mehrer Befehle und Variablen zu einer loogischen Einheit zusammen

Aktuell ist die cicd.yaml so gestaltet, dass der Job auch durchläuft, wenn keine Dateien vorhanden sind. Dies wird bei den betroffenen Befehlen mit einer vorgeschalteten if-Abfrage erreicht. Beispiel:

```bash
if [ -f ${{ env.GITHUB_WORKFLOW_WORKDIR }}/AGSH/ansible/ansible.cfg ]; then cp ${{ env.GITHUB_WORKFLOW_WORKDIR }}/AGSH/ansible/ansible.cfg /home/github/ansible/ansible/ansible.cfg; fi
```

Es wird also geprüft, ob im lokalen Quell-Repository die Datei "ansible.cfg" existiert. Wenn dies der Fall ist, wird die Datei in das Zielverzeichnis kopiert, andernfalls eben nicht.

## Kopieren aller Dateien

Dies Kapitel beschreibt, welche Dateien in deinem Repository angelegt werden müssen und welche Bedeutung diese haben. Erstelle in deinem Repository das Verzeichnis *ansible* um mit den weiteren Kapiteln fort zu fahren.

### ansible.cfg

[Referenz](https://docs.ansible.com/ansible/latest/reference_appendices/config.html)

Diese Datei nimmt die grundsätzliche Konfiguration deiner Ansible Umgebung vor. Alle hier definierten Pfade sind relativ zum Arbeitsverzeichnis gesetzt. Führst du also z.B. ```ansible-playbook``` im Pfad */home/github/ansible/ansible* aus, wird das Inventar aufgrund der Zeile *inventory = ./hosts/inventory.yaml* im Pfad */home/github/ansible/ansible/hosts/inventory.yaml* gesucht.

Kopiere diese Datei in das ansible-root Verzeichnis in deinem Repositories.

### inventory.yaml

[Referenz](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html)

In dieser Datei notierst du alle Systeme, die du mit Ansible verwalten möchtest. In unserem Beispiel ist dort nur ein Eintrag vorhanden, nämlich *localhost*. Das Inventar ist beliebig erweiterbar. Du kannst Gruppen bilden, für jeden Client/Gruppe beliebige Varialben definieren und die Verbindungseingeschaften zu den Systemen mit diesen Variablen steuern. Zudem kannst du auch die inventory.yaml mit beliebigen Datenquellen ersetzen. Zum Beispiel einer Maschinenliste aus AWS oder einer Datenbank.

Kopiere diese Datei in das *hosts* Unterverzeichnis von *ansible* in deinem Repositories.

- [Zurück zur Startseite](./../README.md)
- [Voriges Kapitel](./../Lesson05-Create_cicd_workflow/Lesson05.md)
- [Nächstes Kapitel](./../Lesson06-Create_Ansible_playbook_workflow/Lesson06.md)