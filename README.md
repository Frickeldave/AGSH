# AGSH Project

In meinem SmartHome stehe ich immer wieder vor der Herausforderung, alle beteiligten Systeme auf Stand zu halten, ohne einen riesigen administrativen Overhead zu erzeugen. Um dies zu erreichen, nutze ich Ansible auf einem Ubuntu 24.04 System.

Diese Seite beschreibt den AGSH Workshop. In diesem geht darum, mit Ansible und GitHub eine SmartHome Umgebung zu automatisieren. Im Rahmen dieses Artikels wird dafür eine Hyper-V basierte VM mit Ubuntu 24.04 genutzt, genauso gut lässt sich dies aber auf einer beliebigen Umgebung nachbilden.

Die Verwaltung eines SmartHome ist ein gut greifbares Szenario aber vor ähnlichen Herausforderungen wie daheim zu Hause (Akzeptanz durch Stakeholder, wenig zeitliche Ressourcen, kein Budget für aufwändige Softwaremanagement Suiten) stehen viele CICD Umgebungen in denen "Dinge" automatisiert werden müssen.

Dieses GitHub Repository begleitet dich durch den kompletten 70 Minuten Workshop, den ich erstmals zur Cloudland 2024 gehalten habe.

- [Lesson 01 - Erstellen einer Hyper-V VM](./Lesson01-Create_VM/Lesson01.md)
- [Lesson 02 - Installation von Ubuntu 24.04](./Lesson02-Install_Ubuntu/Lesson02.md)
- [Lesson 03 - Erstellen des GitHub repositories](./Lesson03-Create_GH_Repo/Lesson03.md)
- [Lesson 04 - Installation des GitHub runners](./Lesson04-Install_GH_Runner/Lesson04.md)