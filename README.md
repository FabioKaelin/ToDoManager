# ToDoManager

## Anforderungen

* Git
* Node
* Browser (Am besten Chrome weil manche Design-Dinge in anderen Browsern nicht funktioniert)

## Installieren

```sh
git clone https://github.com/FabioKaelin/ToDoManager.git
cd ToDoManager
npm install
npm run serve
```

## Benutzung

Mann kann mit Hilfe meines Backends ([ToDo-API](https://github.com/FabioKaelin/OpenAPI-ToDoManager)) kann man das Frontend benutzen.

Wenn man auf die "Webseite" geht muss man sich zuerst einloggen
(Standardbenutzer ist hugo@m295.local.zli.ch mit dem passwort Zli123)
oder sich neu Registrieren mit einem Klick auf registrieren.

Hier kann man, wenn man bereits ToDos erstellt hat, seine ToDos sehen.

Um eine ToDo hinzufügen kann man in das Feld vor Hinzufügen den Namen der
ToDo eingeben und entweder [enter] oder auf "Hinzufügen" drücken.

Um eine ToDo zu löschen kann man rechts neben der ToDo auf den Mülleimer
klicken und die ToDo wird gelöscht.

Um eine ToDo zu bearbeiten kann man auf den Stift rechts neben der ToDo klicken
und es kommt eine Texteingabe. In diese kann man den neuen Namen schreiben und mit
[enter] bestätigen.

## Bewertung

Für die Bewertung ist zu beachten dass die meisten Commits in dem
Main-Branch sind und dass nur am Ende noch ein wenig im Abgabe-Branch
gemacht wurde um Cleancode umzusetzen. Desshalb ist das zu bewertende
Resultat, der Branch "abgabe".
