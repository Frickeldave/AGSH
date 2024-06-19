# Lesson 09 - Installation einer Baseline

Nachrichten ausgeben ist ja gut und schön, nun wollen wir aber auch ein paar coole Dinge auf dem System machen. Wir arbeiten die ganze Zeit auf einem Ubuntu 24.04. ohne großes customizing. Das nervt natürlich ein bisschen, kein "VI", keine schöne Kommandozeile, SSH ist nicht konfiguriert, etc. Dies sind alles Dinge, die wir auf jedem unsere Ubunutu Systeme anpassen wollen. Also bietet es sich an eine sogeannte "Baseline" zu installieren, in der wir den "Standard" aller zu verwaltenen Systeme definieren. In [Lessonv 07](./../Lesson07-add_ansible_files/Lesson07.md) haben die dafür notwendige Datei "Baseline.yaml" schon kopiert, müssen diese also nur noch einbinden.

## Einbinden der Baseline

Dies ist denkbar einfach: Füge einfach folgende Zeile als ersten Task in deine master.yaml ein und führe deinen "host-master" workflow erneut aus:

```yaml
- include_tasks: baseline.yaml
```

Du findest wie immer auch eine fertige *master02.yaml* und dazu passende *host-master02.yaml* Datei im Verzeichnis dieser Lesson.

## Erklärung der Baseline

Damit könnte ich dieses Kapitel schon schliessen, aber ein bisschen Erläuterung schadet nicht. Die *Baseline.yaml* ist nichts weiter als eine Ansammlung von Tasks ohne die Header-Deklarationen.

Im ersten Task wird das Betriebssystem aktualisiert. Es handelt sich um den gleichen Task wie "apt update;apt upgrade".

```yaml
---

- name: Update and upgrade apt packages
  become: true
  apt:
    upgrade: yes
    update_cache: yes
    cache_valid_time: 86400 #One day
```

Dann werden diverse Applikationen installiert, die auf jedem System immer vorhanden sein sollen.

```yaml
- name: Install mandatory components
  apt:
    name:
      - whois
      - unzip
      - vim
      - fontconfig
      - git
    state: present
    update_cache: yes
    cache_valid_time: 3600
```

Nun werden die Rollen aufgerufen.

```yaml
- name: Install and configure oh-my-posh
  include_role:
    name: app-ohmyposh
...
...
...
```

Im Falle dieser Baseline:

- Installation und Konfiguration von oh-my-posh, einem Kommandozeilen-Verkrasserer nebst notwendiger Schriftarten
- Installation und Konfiguration von VI
- Konfiguration von SSH

Die genaue Beschreibung der Rollen ist Teil des interaktiven Workshops, aber ich denke mit Hilfe der [Ansible Dokumentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html) und der in [Lession 07](./../Lesson07-add_ansible_files/Lesson07.md) hinterlegten Beispiele kommst du schon gut zurecht.

## Ausführung und Test

Führst du nun dein host-master Workflow aus, sind alle 3 Komponenten installiert/ konfiguriert. Nach Neu-Anmeldung siehst du gleich, wie sich deine Shell verändert hat. Wahrscheinlich ist es nicht super-hübsch, da du auf deinem System die Schriftart ["Nerdfont Meslo"](https://github.com/ryanoasis/nerd-fonts/releases/download/v3.2.1/Meslo.zip) benötigst und deinem verwendeten Terminal Client beibringen musst, diese Schriftart zu verwenden.

![New shell with oh-my-posh and ssh warning](./Screenshot%202024-06-13%20202915.png)

- [Zurück zur Startseite](./../README.md)
- [Voriges Kapitel](./../Lesson08-create-playbook-workflow/Lesson08.md)
- [Nächstes Kapitel](./../Lesson09-Installation_of_a_baseline/Lession09.md)
