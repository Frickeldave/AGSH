# Lession 01 - Virtuelle Maschine erstellen

Der Workshop setzt eine vorinstallierte VM auf Basis von Hyper-V voraus. Alle Voraussetzungen die zu treffen sind, sind in diesem Kapitel beschrieben.

## Voraussetzungen

Folgende Voraussetzungen müssen getroffen werden, um diese Workshop durchzuführen:

- Du benötigst ein aktuelles Ubuntu 24.04. Server-ISO
- Dein Rechner (oder die vielmehr die VM die in diesem Kapitel erstellt wird), benötigt Internet Zugang
- Kostenloses GitHub Konto
- Installierter [Git-Client](https://git-scm.com/downloads)
- Installierter Code-Editor deiner Wahl (In diesem Guide wird Visual Studio Code verwendet)

## Zusammenfassung der wichtigsten Informationen

- Username: aadmin
- Alle Passwörter: aadmin
- Name der virtuellen Maschine: Master
- Username für das GitHub Konto auf "Master": GitHub
- Username für das Ansible Konto: Ansible

Selbstverständlich sollten produktive Syteme nicht auf diese Art konfiguriert werden. Auch "Ansible" und "GitHub" sollten nicht als Username verwendet werden um den Agriffsvektor auf die Systeme bei einer Brute-Force Attacke möglichst klein zu halten.

## Hyper-V aktivieren

Willst du eine Hyper-V VM installieren, ist als erstes die Aktivierung der Hyper-V Rolle notwendig. Alle folgenden Hinweise basieren auf Windows 11 Version 24H1.
Starte damit, dass du den Windows 11 Feature Wizard aufrufst.

![Call windows feature wizard](Screenshot%202024-06-07%20115920.png)

Aktiviere nun die Hyper-V Rolle mit allen Sub-Features.

![Activate Hyper-V role](./Screenshot%202024-06-07%20120047.png)

Nach einen Neustart des Systems steht dir die Hyper-V Rolle zur Verfügung. Entweder kannst du den hyper-V Manager nun das Startmenp starten, oder in eine bestehende MMC einbinden.

## Erstellen der virtuellen Maschine

Nach dem Start des Hyper-V Managers, erstelle ein neue VM mit Rechtsklick auf deinen Hyper-V Host -> New -> Virtual Maschine....

![Create new VM](Screenshot%202024-06-07%20120144.png)

Gib deiner virtuellen Maschinen einen Namen. Ich verwende hier schlicht "Master".

![Name your new VM](Screenshot%202024-06-07%20121138.png)

Wähle den Mschin-type "Generation 2"

![Define maschine type](Screenshot%202024-06-07%20121204.png)

Definiere, wie viel Arbeitsspeicher der virtuellen Maschine zur Verfügung stehen soll. Mindestens sollten 4096 MB zur Verfügung gestellt werden.

![Set RAM](Screenshot%202024-06-07%20121221.png)

Nun wird die virtuelle MAschine mit einem Netzwerk verbunden. Du kannst hierfür den "Default Switch" verwenden. Dieser stellt sicher, dass die virtuelle Maschine ins Internet kommt, und du dich von deinem Host auf die virtuelle Maschine per SSH verbinden kannst.

![Connect virtual maschine to network](Screenshot%202024-06-07%20121239.png)

Jetzt wird die virtuelle Festplatte definiert. Setze die Größe heirbei auf 1024 GB. Es macht nichts, wenn die angegebene Größe die Größe deiner physikalischen Festplatte übersteigt, es wird nur der Platz verwedendet, den du innerhalb deiner virtuellen Maschine wirklich verwendest. ACHTUNG: Virtuelle Festplatten werden zwar automatisch durch den Hypervsior vergrößert, aber nie verkleinert, dies musst du selbst erledigen.

![Configure virtual harddisk](Screenshot%202024-06-07%20121303.png)

Bei der Konfiguration des virtuellen CD-ROM Laufwerks, wird für die Installation des Betriebssystems wird die zuvor heruntergeladene Ubuntu 24.04. ISO Datei angegeben.

![Maount Ubuntu 24.04 ISO](Screenshot%202024-06-07%20121339.png)

Nach dem Durchlaufen des Wizards zur Erstellung einer VM, müssen noch einige Dinge an der VM angepasst werden. Wechsel nun in die Einstellungen der VM und aktiviere den Menüpunkt "Security". Dort wird in der Secure Boot Sektion das Template "Microsoft UEFI Certificate Authority" ausgewählt.

![Configure Secure Boot](Screenshot%202024-06-07%20121435.png)

Nun ist deine virtuelle Mschine fertig für den Einsatz.

- [Zurück zur Startseite](./../README.md)
- [Nächstes Kapitel](./../Lesson02-Install_Ubuntu/Lesson02.md)
