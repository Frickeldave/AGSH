# AGSH Project

In meinem SmartHome stehe ich immer wieder vor der Herausforderung, alle beteiligten Systeme auf Stand zu halten, ohne einen riesigen administrativen Overhead zu erzeugen.

Die Verwaltung eines SmartHome ist ein gut greifbares Szenario. Der geneigte Administrator steht dabei vor ähnlichen Herausforderungen wie bei der Verwaltung seiner CICD Umgebung. Dazu gehören u.a. Akzeptanz durch Stakeholder, wenig zeitliche Ressourcen, kein Budget für aufwändige Softwaremanagement Suiten.

Und seien wir mal ganz ehrlich, viele von uns haben diverse Systeme aus dem professionellen Umfeld schon zu Hause eingesetzt. Und wie ist es jedesmal geendet? Man bastelt 1-2 Wochen hochmotiviert vor sich hin, dann kommen andere Dinge dazwischen und nach einigen Monaten setzt man sich wieder hin und weiß gar nicht mehr was man da so alles getan hat. Also lässt man es wieder.

Genau aus dieser Idee ist der AGSH Workshop geboren. In diesem geht darum, mit **A**nsible und **G**itHub eine **S**mart**H**ome Umgebung zu automatisieren. Im Rahmen dieses Artikels wird dafür eine Hyper-V basierte VM genutzt (genauso gut lässt sich dies aber auf einer beliebigen Umgebung nachbilden) auf der die Konfiguration von Linux, Docker und die Installation eines Containers (HomeAssistant) automatisiert wird. Bedienen lässt sich alles über die grafische Oberfläche von GitHub, so das komplexe Kommandozeilen der Vergangenheit angehören.

Dieses GitHub Repository begleitet dich durch den kompletten 90 Minuten Workshop, den ich erstmals zur Cloudland 2024 gehalten habe.

- [Lesson 01 - Erstellen einer Hyper-V VM](./Lesson01-create_vm/Lesson01.md)
- [Lesson 02 - Installation von Ubuntu 24.04](./Lesson02-install_ubuntu_in_vm/Lesson02.md)
- [Lesson 03 - Erstellen des GitHub repositories](./Lesson03-create_gh_repository/Lesson03.md)
- [Lesson 04 - Installation des GitHub runners](./Lesson04-install_github_runner/Lesson04.md)
- [Lesson 05 - Erstellen des CICD workflows](./Lesson05-create_cicd_workflow/Lesson05.md)
- [Lesson 06 - Erstellen des Ansible users](./Lesson06-create_ansible_user/Lession06.md)
- [Lesson 07 - Hinzufügen der Ansible Dateien](./Lesson07-add_ansible_files/Lesson07.md)
- [Lesson 08 - Erstellen des Playbook workflows](./Lesson08-create-playbook-workflow/Lesson08.md)
- [Lesson 09 - Erstellen eines Systemworkflows](./Lesson09-Installation_of_a_baseline/Lession09.md)
- [Lesson 10 - Installation von HomeAssistent](./Lesson10-Install_homeassistant_as_container/Lesson10.md)