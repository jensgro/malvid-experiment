# Ein erster Versuch mit Malvid

## Was soll das hier?

Dieses Projekt ist ein Testprojekt für den Umgang mit [Malvid](https://github.com/Malvid/Malvid). Malvid erzeugt aus einer Dateistruktur einen Styleguide. Integraler Bestandteil ist der Development-Server [Rosid](https://github.com/electerious/Rosid), der bei der Verarbeitung von Sass und Nunjucks hilft. Neben Node.js ist yarn hierbei wichtig. Basis dieses kleinen Projekts ist [Skeleton-Components](https://github.com/electerious/Skeleton-Components), indem eine kleine Beispielinstallation von Malvid mit Rosid angeboten wird. Ich habe diese Beispielinstallation mit meinen Beispieldateien angereichert.

## Testdateien

Damit ich schnellstmöglich ausreichend Testdateien zur Verfügung habe, habe ich mich bei [Codex UI](https://codexui.com/) bedient. Deren Ziel ist es, möglichst viele neue Module auf Basis der bestehenden Bootstrap-Module zu erzeugen. Der Vorteil hierbei ist, dass die Module alle als isolierte HTML-Dateien vorliegen, deren Dateiendung ich einfach nur in `.njk` änderte, da Malvid von der Verwendung von Nunjucks ausgeht. Ich wollte nicht unnötig konfigurieren. Ausserdem ist es die Stärke eines Systems wie Nunjucks, dass man mit Variablen arbeiten kann und damit Texte und Bilder auslagern kann. Das war ein wichtiges Ziel für mich.

Zusätzlich zur Standardinstallation benötigte ich dann noch Bootstrap, jQuery, popper.js und Font Awesome, die dann alle inkludiert werden mussten. Dafür konnte ich die im Skeleton-Projekt existierenden SCSS-Dateien vollständig entfernen.

## Erster Installationsversuch

Die Dokumentation sieht auf den ersten Blick umfangreich aus, schweigt sich aber an den entscheidenden Stellen aus. Sie ist auch nicht klar formuliert.

Es gibt zwei Arten der Installation: mittels CLI oder API. Es wird nicht klar verständlich erklärt, was der Unterschied sein soll. 

> The API gives you more flexibility and allows you to use Malvid in your existing asset pipeline or toolset.

> Die CLI ist ein Tool was du ausführst und was sich somit zu schwer automatisiert ansprechen lässt. Die API bietet dir alle Funktionen der CLI, ansprechbar als API.

Der erste Versuch scheiterte, denn das Beispiel kam mir schon sehr seltsam vor. Ich wollte schliesslich keine ganze Seite nur für mein Modul schreiben und ich wollte auf der Kommandozeile auch nicht einzelne index.html-Dateien ansteuern. Ich wollte einfach nur einen Ordner (am Besten mit Unterordnern) in einen Styleguide umwandeln lassen, so wie die Videodemonstration das suggerierte. Also war der Weg über die CLI nicht gangbar oder unglücklich beschrieben.

## Zweiter Installationsversuch - erfolgreich

Der Weg über die API wird nicht beschrieben, also nahm ich mir das Skeleton-Projekt und modifizierte es. Mit ein wenig Herumspielen war dann auch klar, wie ich die Pfade zu meinem Bootstrap im node_modules legen musste. Warum dann die Font-Awesome-Quellen doch nicht aus node_modules gezogen werden konnten, ich sie stattdessen kopieren musste, ist mir am Ende egal gewesen. Es ist ein Usecase, der nicht allzu unbedeutend sein dürfte, in der Doku aber nicht erwähnt wird.

> Der node_modules Ordner ist nicht Teil vom Web-Ordner (`src/`). Er ist somit nicht vom Browser erreichbar. Imports von SASS-Dateien oder JS-Dateien (`import x from 'x'`) sind möglich, da diese Imports beim kompilieren aufgelöst werden und nicht vom Browser.

## Ordner erzeugen

In einem ersten Impuls dachte ich, ich könnte einen Ordner erschaffen und darin die zusammengehörigen Module gruppieren. Das brachte nichts. Auf Nachfrage per Twitter wurde ich auf die config.json gestossen. Die Doku war aber auch hier nicht ganz klar. Mein erster Versuch, war die Kreation eines neuen Ordners und die Zuordnung einer config.json zu diesem Ordner. Das war ein Fehlversuch.

Jedes Modul benötigt eine solche config.json, die dann eine Ordner-Zuordnung bekommt. Und dieser Ordner existiert nur virtuell. Auf der Festplatte liegen die Module vollkommen ungeordnet in einer einzigen Ebene herum. Zusätzlich kann man die in der Oberfläche erzeugten Ordner nicht einklappen, sodass der Ordnungseffekt überschaubar ist. Bei über 50 Modulen wird das sowohl auf der Oberfläche als auch auf der Festplatte recht unübersichtlich.

> Du kannst die Dateien in unterschiedliche Ordner packen wenn dies bei der Entwicklung hilfreich ist. Lediglich Malvid nimmt auf die Struktur keine Rücksicht, was dir ermöglicht in der UI eine andere Sortierung vorzunehmen. Wir selbst benutzten hier viel die Suche und den Fuzzy Finder vom Editor was bei vielen Modulen schneller ist als ein- und aufklappen von Ordnern.

## Konfiguration

Die Dokumentation geht immer davon aus, dass ein Modul eine Repräsentanz hat. Doch das ist unrealistisch. Insbesondere bei Buttons, generell aber bei Formularen und auch bei Teasern möchte ich Modifikationen präsentieren. Dies dann immer mit einem neuen Verzeichnis, einer neuen Nunjucks-Datei und einer neuen Konfigurations-, sowie Inhalte-Datei machen zu müssen, ist unattraktiv. Vor allem, wenn identische Templates mehrfach vorgehalten werden müssen. Das ist eine Fehlerquelle.

> In der Tat nicht perfekt. Wir arbeiten hier immer mit Templates die z.B. den gleichen Button mehrfach importieren und immer mit anderen Daten anzeigen. So können wir alle Variationen testen ohne das Template für den Button mehrfach zu schreiben.

Es ist nicht dokumentiert, wie mehrere Instanzen eines Templates dargestellt werden können. Eine Beispielimplementierung diente mir als Anregung. Doch nach vielen Versuchen und Hilfe per Twitter konnte ich zwar eine syntaktisch korrekte JSON-Datei erzeugen, auch das Nunjuck-Template war korrekt. Es wurde aber kein einziger Button erzeugt. 

Hier fehlt es an einem Beispiel und an Informationen in der Doku. Ich kann mir nicht vorstellen, dass das Tool dieses Feature nicht bietet.

> Wäre an der Stelle echt gut, das stimmt!

## Geschwindigkeit

Die Geschwindigkeit ist klasse. In Windeseile wird die Übersicht erzeugt. Die Sass-Generierung muss nicht konfigueriert werden, kommt out-of-the-box.

## WIP-Marker

Eine sehr schöne Idee ist, dass man in der config.json zu jedem Modul den Status "WIP" definieren kann. Er wird dann als Badge am rechten oberen Rand der Moduldarstellung gezeigt. 

## Strukturierung

Alle Inhalte werden in den Ordner `src` gekippt. Es ist keine Strukturierung möglich. Das ist sehr bedauerlich und ein großer Minuspunkt.

> Bei der Strukturierung bist du komplett frei. An der Stelle gibt dir weder Malvid noch Rosid etwas vor, was ein Grund war weshalb wir Malvid gemacht haben. Wie kam es zu dieser Annahme und an welcher Stelle könnten wir das klarer machen?

## Verlinkung externe JS-Dateien

Es ist mir nicht gelungen, die externen JS-Dateien aus dem `node_modules`-Ordner zu ziehen. Ich musste doch manuell die Dateien kopieren und einbinden. Sehr bedauerlich. Auch hierfür fänd ich eine Lösung wichtig und vor allem eine Dokumentation. Es ist mir zudem nicht klar, wie ich einfach feststellen kann, ob die externen Dateien nun geladen wurden oder nicht. Nachdem ich die notwendigen Dateien ins Projekt kopiert hatte, hatte ich nicht den Eindruck, dass sie geladen wurden.

## malvidfile.json

Dieser Test basiert auf einer Testinstallation von Malvid. Es existiert ein Konfigurationsfile für den Development-Server, aber nicht für Malvid. Das ist doppelt bedauerlich. Denn erstens geht aus der Dokumentation leider nicht hervor, **wo** die `malvidfile.json` platziert werden soll (auf der obersten Ebene oder im Verzeichnis `src`) und welche Basis-Konfigarationen Sinn machen können. Ich gehe mal davon aus, dass es Standardwerte gibt.

> Only a single malvidfile.json or malvidfile.js is required in the current working directory when you want to adjust the default behaviour of Malvid.

> Current working directory ist in dem Falle der Ordner von dem du Malvid/Rosid aus startest. Sprich der Ordner mit der `package.json`.

## Die Distribution

Mittels `yarn run compile` kann eine statische Version des Styleguides erstellt werden. Sie landet im Ordner `dist`. Leider kann man die entstandenen Dateien nicht einfach so anklicken, die generierten HTML-Dateien werden dann nicht gefunden. Es ist also immer ein Webserver notwendig. Eine einfache Versendung als ZIP an einen Kunden oder Designer fällt damit aus.

> Das stimmt. Du kannst in dem Falle einen static site server nehmen und die Dateien dorthin laden um den Link zu verschicken: https://surge.sh und https://zeit.co/now eignen sich z.B.. Diesen Link kannst du dann auch up-to-date halten statt öfters neue Dateien verschicken zu müssen. Praktisch für alle im Team. So kann auch der PM immer einen Blick auf den aktuellen Stand werfen.
