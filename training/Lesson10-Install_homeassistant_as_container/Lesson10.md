# Lesson 10 - Installation einer Baseline

Nach Lession 9 ist eigentlich nichts besonderes mehr zu implementieren. In deinem Repo führst du Änderungen durch, ein Push transportiert diese auf dein GitHub Repo, eine Action verteilt diese wiederum auf deinen Ansible Master-Server von wo aus du Playbooks gegen deine zu verwaltenden Clients in Form von weiteren Workflows starten kannst.
Ist dieses Verfahren einmal implementiert, kannst du mit wenigen Klicks jedes deiner Systeme in deinem Heimnetzwerk easy verwalten. Egal ob es sich um Raspberries, "richtige" Linux Hosts oder Windows Systeme handelt. Und wenn du es darauf anlegst, kannst du mit Ansible auch völlig proprietäre Systeme ansprechen. Schau dich einfach mal in der [Ansible Galaxy](https://galaxy.ansible.com/ui/) um.

Da ich aber versprochen habe, wir wollen ein SmartHome System aufsetzen und das handling von Docker Container immer etwas speziell ist, setzen wir in diesme letzten Kapitel noch die Verwaltung eines HomeAssistent Docker Container auf.

## Erweitern des Master-Workflows

Die folgenden Änderungen führe bitte an deiner host-master.yaml Datei durch oder kopiere die beiden Daten *host-master03.yaml* und *master03.yaml*. Dadurch erhälst du die Möglichkeit, den Port deiner HomeAssistant Installation über die GitHub GUI zu modifizieren. Zudem kannst du über ein Bool Wert bestimmen, ob die Installation komplett resetted werden soll, oder die bestehenden Daten übernommen werden.

```yaml

name: host-master
on: 
  workflow_dispatch:
    inputs:
      homeassistant_port:
        description: The port for the homeassistant application
        required: false
        default: 8123
        type: number
      homeassistant_reset:
        description: Reset the homeassistant installation
        required: false
        default: false
        type: boolean
jobs:

  execute-ansible:
    uses: ./.github/workflows/exec-playbook.yaml
    with:
      ans_target_playbook: master.yaml
      ans_limit: master
      ans_vars: homeassistant_reset=${{ inputs.homeassistant_reset }} homeassistant_port=${{ inputs.homeassistant_port }}

```

## Erweitern des Master-Playbooks

Die notwendigen Rollen sind schon in [Lession07](./../Lesson07-add_ansible_files/Lesson07.md) kopiert wurden. Du musst also nur dein *master* playbook wie folgt umschreiben:

```yaml
---

- name: Master
  hosts: master
  become: true
  gather_facts: true
  vars:
    homeassistant_reset: "{{ homeassistant_reset_input | default(false) }}"
    homeassistant_port: "{{ homeassistant_port_input | default(8123) }}"

  tasks:

  - include_tasks: baseline.yaml

  - name:
    ansible.builtin.debug:
      msg: "Remove exisiting homeassistant data: {{ homeassistant_reset }}"

  - name:
    ansible.builtin.debug:
      msg: "Homeassistant port: {{ homeassistant_port }}"
    
  - name: Execute the docker role
    include_role:
      name: app-docker

  - name: Execute the docker-compose role
    include_role:
      name: app-docker-compose

  - name: Execute the homeassistant role
    include_role:
      name: app-homeassistant
```

Statt der alten String Variable werden nun, analog zum playbook, 2 Variablen für den Port von HomeAssistent und die Reset Funktion entgegen genommen. 
Dann wird in den Tasks *docker* und *docker compose* installiert und anschließend eine *compose-Datei* auf das System verteilt, um mit der die darin definierten Container zu starten.

## Community plugin oder Kommandozeile

Es gibt durchaus auch Module, um mit Ansible den Docker Daemon oder auch Docker Compose direkt anzusprechen. Wie so oft, musst du für dich abwägen, willst du ein zusätzliches Modul aktivieren, oder wie ich, mit Commandline Kommandos auskommen.

## Testen

Nun schnapp Dir die IP-Adresse deiner virtuellen Maschine und gib im Browser die HomeAssistent URL ein: http://<IPADDRESS>:8123

- [Zurück zur Startseite](./../../README.md)
- [Voriges Kapitel](../Lesson09-Installation_of_a_baseline/Lession09.md)
