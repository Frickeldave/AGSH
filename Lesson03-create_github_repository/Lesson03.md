# Lesson 03 - GitHub Repository erstellen

Alle Dateien die für die Verwaltung der Umgebung notwendig sind, werden in einem privaten Repository verwaltet. Privat deswegen, weil in dem Repository 2 wesentlichen Secrets verwaltet werden (mehr dazu in späteren Lessons). In diesem Kapitel wird erläutert, wie das Repository angelegt und auf das Entiicklungssystem gecloned wird.

## Anlegen des Repositories

Melde dich bei deinem GitHub Account an, wechsel in den Bereich "Repositories" und lege ein neues Repository an. Vergib einen Namen, eine Beschreibung und setze das Repository auf "Privat". Füge zudem ein "README.md" file hinzu, was dazu führt, dass das Repository gleich initialisiert ist.

![Create a new repository](./Screenshot%202024-06-07%20125143.png)

## Clone des repositories erstellen

Öffne nun eine Powershell Konsole und clone das Repository. Du hast nun die Möglichkeit, dich entweder über Web/SSO zu authentifizieren (empfohlen) oder ein "Personal access token" zu nutzen (wenn keine UI oder kein Browser) zur Verfügung steht.

![Lokaler clone des repositories erzeugen](./Screenshot%202024-06-07%20125959.png)

## Personal access token (PAT) erstellen

***Wenn du dich via Web/SSO authentifizierst, kannst du dieses Kapitel überspringen.***

Wenn du über keine UI oder einen Browser verfügst und die 2-Faktor-Authentifizierung in deinem GitHub Konto aktiv ist, musst du ein PAT erzeugen, mit dem du dich auf der Konsole anmelden kannst.

Um ein PAT zu erstellen, klicke in GitHub oben rechts auf den Benutzersymbol und wechsel in die Sektion "Settings".

![GitHub settings](./Screenshot%202024-06-07%20130105.png)

Innerhalb der Settings findest du im unteren Bereich die Option "Developer settings".

![Developer settings](./Screenshot%202024-06-07%20130156.png)

In den Devopler settings hast du die Möglichkeit, ein PAT anzulegen. GitHub unterscheidet hier in die klassichen PATs und die "new PATs". Auch wenn die neuen PATs mit einer deutlich granulareren Berechtigungssteuerung gut funktionieren, setzen wir im Rahmen dieses Trainings auf die klassischen PATs.

![Generate classic PAT](./Screenshot%202024-06-07%20130234.png)

Erzeuge das PAT mit einem beliebigen Namen und Laufzeit und speichere dir den erzeugten String in deinem persönlichen Password vault. WICHTIG: Der String ist nur einmalig einsehbar. Verlierst du diesen, musst du ihn neu erzeugen.

Im Anschluss kannst du deinen GitHub-Benutzernamen und das PAT eingeben um dich zu authentifizieren.

![Clone repository](./Screenshot%202024-06-07%20130442.png)

## Username und E-Mail definieren

Ich bevorzuge es, den Username und die Mail Adresse die git benötigt pro Repository zu definieren. Wechsel also in deiner Shell in das Verzeichnis deines Repositories und setze den Username und die Mailadresse.

```bash
git config user.name "First Name"
git config user.email "your@mail.de"
```

Dieser Schritt ist nur notwendig, wenn du deinen Git-Username und deine Git-Mail nicht schon zuvor "*global*" gesetzt hast.

- [Zurück zur Startseite](./../README.md)
- [Voriges Kapitel](../Lesson02-install_ubuntu_in_vm/Lesson02.md)
- [Nächstes Kapitel](../Lesson04-install_github_runner/Lesson04.md)
