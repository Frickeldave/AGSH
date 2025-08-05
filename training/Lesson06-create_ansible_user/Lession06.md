# Lesson 06 - Anlegen des Ansible User

Das besonderen an Ansible ist es, dass du per push auf das System zugreifst, anstelle der Installation eines Agents wie es sonst bei Configuration Management Systemen üblich ist. Im Falle von Linux via SSH, im Fall von Windows via [WinRM](https://docs.ansible.com/ansible/latest/os_guide/windows_setup.html) (alternativ SSH im experimental state). Um mit Ansible auf dein Zielsystem zuzugreifen, sind natürlich entsprechende Berechtigungen notwendig. Dieses Kapitel zeigt dir, wie du einen User "Ansible" anlegst und die entsprechenden Rechte vergibst.

## Anlegen des Users

Das unten stehende Script führt folgende Tätigkeiten aus:

- Anlegen eines Users "ansible"
- Vergabe von Rechten, administrative Tasks auszuführen 
- Hinterlegen eines public-keys der es erlaubt, dass Ansible sich mit dem private key anmeldet, den wir in [Lesson05](../Lesson05-create_cicd_workflow/Lesson05.md) in unserem GitHub Secrets hinterlegt haben.

Erstze im folgenden Script in der Zeile ```sudo bash -c 'echo "ssh-rsa <YOURPUBLICKEY>" > /home/ansible/.ssh/authorized_keys'``` den string ```<YOURPUBLICKEY>``` mit dem public key in deiner Datei "*C:\Users\USERNAME\.ssh\id_rsa_AGSH.pub*", die du in [Lesson05](../Lesson05-create_cicd_workflow/Lesson05.md) erzeugt hast.

**Hinweis:** Die ersten beiden Zeilen sind auch schon im Script für die Anlage des GitHub Users in [Lesson04](../Lesson04-install_github_runner/Lesson04.md) enthalten und der Vollständigkeit-halber hier noch einmal aufgezeigt. Das Script sollte trotzdem ohne Fehler durchlaufen. 

**Hinweis:** Solltest du von der voerherigen Lesson noch als User GitHub angemeldet sein, verlassen diesen mit "Exit". 

```bash
sudo /sbin/addgroup sudo-nopasswd
sudo bash -c 'echo "%sudo-nopasswd ALL=(ALL:ALL) NOPASSWD:ALL" > /etc/sudoers.d/sudo-nopasswd'
sudo /sbin/adduser --force-badname --home /home/ansible --disabled-password --gecos "" --shell /bin/bash ansible
sudo /sbin/usermod -a -G sudo-nopasswd ansible
sudo mkdir /home/ansible/.ssh -p
sudo bash -c 'echo "<YOURPUBLICKEY>" > /home/ansible/.ssh/authorized_keys'
sudo chown ansible:ansible /home/ansible/.ssh -R
sudo chmod 700 /home/ansible/.ssh
sudo chmod 600 /home/ansible/.ssh/authorized_keys
```

Setze als letztes noch ein Kennwort für den User. In diesem Fall nutzen wir "aadmin", ansonsten vergib ein möglichst kryptisches Kennwort. Alternativ kannst du dies auch leer lassen um den Zugang via Passwort komplett zu unterbinden:

```bash
sudo passwd ansible
```

Du kannst den Zugriff im Anschluss mit folgendem Befehl testen:

```powershell
ssh ansible@<IPADDRESS> -i C:\Users\USERNAME\.ssh\id_rsa_AGSH
```

- [Zurück zur Startseite](./../../README.md)
- [Voriges Kapitel](../Lesson05-create_cicd_workflow/Lesson05.md)
- [Nächstes Kapitel](../Lesson07-add_ansible_files/Lesson07.md)