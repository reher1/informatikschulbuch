# OER Lehrbücher mit Markdown und Git erstellen
*[Quelle: Artikel von Julian Dorn](https://blog.wi-wissen.de/post/oer-lehrbuecher-mit-markdown-und-git-erstellen), 19. Januar 2019*

In diesem Artikel erkläre ich wie ich mit überschaubaren Aufwand meie Lehrbücher auf [buch.informatik.cc](http://buch.informatik.cc/) erstelle.

Open Educational Resources sind freie Lern- und Lehrmaterialien, welche in Deutschland zumeist unter einer [Creative Commons](https://de.wikipedia.org/wiki/Creative_Commons) Lizenz veröffentlicht werden. Dafür gibt es verschiedene Bausteine :

* BY - Der Autor muss genannt werden
* NC - Nicht kommerziell. Kann je nach Auslegung private Schulen ausschließen und verhindert auf jeden Fall den Druck in einem Schulbuch
* SA - Ein neu entstandenes Werk muss unter den gleichen Bedingungen weitergegeben werden.
* ND - Damit dürfte das Werk nicht verändert werden, das ist in sofern ungünstig, da jede/r LehrerIn weiß, dass fremdes Unterrichtsmaterial / Lehrbücher zwar sehr gut sein können, aber irgendwie doch nicht perfekt auf dein eigenen Unterricht angepasst werden.

In diesem Artikel möchte ich zeigen, wie ich OER Schulbücher schreibe und mit dieser Methode aktiv dabei helfe, dass gemeinsam daran gearbeitet werden kann und gleichzeitig jeder seine persönliche Variante erstellen kann.

## Das Lehrbuch als Webseite

Lehrbücher als Webseiten zu erstellen ist deutlich besser als ein PDF zu exportieren:

- Das Lehrbuch lässt sich auf dem Smartphone und dem Rechner gut lesen.
- Videos und andere Inhalte wie etwa [H5P](https://h5p.org/) lassen sich gut einbinden.

So kann das dann etwa aussehen:

![Bildbeschreibung](https://blog.wi-wissen.de/bl-content/uploads/html-css-kurs.jpg)

## Das Lehrbuch unkompliziert mit Markdown schreiben

Webseiten werden mit HTML&CSS dargestellt. Hier werden wir [docsify](https://docsify.js.org/#/) und Markdown nutzen, um schnell eine schicke Webseite zu erstellen:

Wollen wir in Markdown etwa folgenden Text schreiben

![Bildbeschreibung](https://blog.wi-wissen.de/bl-content/uploads/k%C3%BChe.jpg)

So müssen wir folgendes eingeben:

```markdown
# Kühe mögen Bäume
Ein Huhn kann *keine* Kürbisse essen, da es kein **Wasservogel** ist.

Mehr spannendes Wissen zu Kühen bei [Wikipedia](https://de.wikipedia.org/wiki/Kuh)
```

Eine gute Übersicht aller Befehle findet sich [hier bei GitHub](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

Als Editor kann ich [Typora](https://typora.io/) sehr empfehlen. Hier müssen fast keine Befehle gelernt werden und es fühlt sich fast so an, wie in Word zu schreiben. Alternativ reicht ein Texteditor oder etwa eine Oberfläche wie [HackMD](https://hackmd.okfn.de/).

Mit Markdown schreiben wir also einzelne Artikel, aber wie wird da nun eine Webseite draus? Ganz einfach, wir lassen unseren Text mit [docsify](https://docsify.js.org/#/) im Browser rendern.

Dazu legen wir eine HTML-Datei an und binden darin docsify ein:

```html
<html>
<head>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <meta charset="UTF-8">
  <link rel="stylesheet" href="//unpkg.com/docsify/themes/vue.css">
</head>
<body>
  <div id="app"></div>
  <script>
    window.$docsify = {
      name: 'HTML-CSS-Kurs',
      homepage: 'index.md',
      auto2top: true,
      loadSidebar: true,
      subMaxLevel: 2
    }
  </script>
  <script src="//unpkg.com/docsify/lib/docsify.min.js"></script>
</body>
</html>
```

In der Markdown-Datei `_sidebar.md` legen wir eine Liste mit allen Artikeln an. Hieraus entsteht die linke Navigationsleiste. [Hier ein Beispiel](https://github.com/wi-wissen/informatikschulbuch/blob/master/html-css/_sidebar.md).

**Aussehen**

Das Aussehen ist durch CSS bestimmt, das kannst du selbst anpassen oder dir [hier](https://docsify.js.org/#/themes) eine fertige Vorlage nehmen. Ich persönlich finde den Standard sehr gelungen. Er entspricht vermutlich dem Vorbild [VueJS](https://vuejs.org/), welches eine enorme Verbreitung hat.

**Suche, Zeitstempel und Edit on Github**

[Hier](https://github.com/wi-wissen/informatikschulbuch/blob/master/html-css/index.html#L43) findest du ein vollständiges Beispiel mit all diesen Möglichkeiten. Beachte bitte, dass ich zwei weitere JavaScripts eingebunden habe, die du auch benötigst.

**Hilfe-, Aufgaben- und Fehlerboxen einbauen.**

Ich habe (vermutlich nicht sehr elegant) die Markdownsyntax erweitert:

```javascript
markdown: {
    smartypants: true,
    renderer: {
      paragraph: function (text) {
        var result;
        if (/^!>/.test(text)) {
          result = helper('tip', text);
        } else if (/^\?>/.test(text)) {
          result = helper('warn', text);
        } else if (/^s>/.test(text)) {
          result = helper('alert alert-success', text);
        } else if (/^i>/.test(text)) {
          result = helper('alert alert-info', text);
        } else if (/^w>/.test(text)) {
          result = helper('alert alert-warning', text);
        } else if (/^d>/.test(text)) {
          result = helper('alert alert-danger', text);
        } else if (/^t>/.test(text)) {
          result = helper('alert alert-task', text);
        } else {
          result = "" + text + "";
        }
        return result;
        function helper(className, content) {
          return ("" + (content.slice(5).trim()) + "")
        }
      }
    }
  },
```

Schreibe ich jetzt etwa `w>` wird der anschließende Absatz mit der CSS-Klase `alert alert-warning` versehen. Diese habe ich in [vue-boxes.css](https://github.com/wi-wissen/informatikschulbuch/blob/master/html-css/css/vue-boxes.css) etwa wie folgt definiert:

```css
/* warning*/
.markdown-section .alert-warning  {
    border-left: 4px solid #FFAB66;
  }
  .markdown-section .alert-warning:before {
    background-color: #FFAB66;
    color: #fff;
    content: '!';
  }
  .markdown-section .alert-warning code {
    background-color: #efefef;
  }
  .markdown-section .alert-warning em {
    color: #34495e;
}
```

## Das Lehrbuch auf einem Git-Server verwalten

Dein erstes Buch soll hochgeladen werden, aber du hast keinen eigenen Webspace? Dann kannst du dir ein kostenfreies Konto bei [GitHub](https://github.com/) anlegen. Installiere dir noch [GitHub Desktop](https://desktop.github.com/) und du musst kaum `git`-Befehle lernen.

Ich habe all meine Lehrbücher in ein [Repository](https://github.com/wi-wissen/informatikschulbuch) gepackt. Dieses habe ich dann über [GitHub Pages](https://pages.github.com/) als Webseite bereitgestellt. Achte hier unbedingt darauf auch wie ich die `.nojekyll` -Datei angelegt zu haben. Sonst wird GitHub versuchen dein Markdown zu rendern.

Deine Webseite ist dann unter `nutzername.github.io/projekt` zu erreichen. Du kannst aber auch nach der [Anleitung](https://help.github.com/articles/setting-up-a-custom-subdomain/) eine eigene Domain aufschalten. Ich habe das etwa mit `buch.informatik.cc` gemacht.

GitHub ist nicht nur super zum Ausliefern der Webseite, sondern auch zum gemeinsamen Arbeiten:

![Bildbeschreibung](https://blog.wi-wissen.de/bl-content/uploads/github.png)

- Code - Hier kannst du den gesamten Quelltext durchstöbern, als wäre er auf deiner Festplatte.
- Issues - Zeige einen Fehler im Lehrbuch an, den du nicht selbst beheben kannst.
- Watch - Bleibe über Änderungen in anderen Repositorys auf dem laufenden.
- Fork - Erstelle auf Grundlage eines bestehenden Lehrbuchs dein eigenes.
- Pull Request - Gib dem Autor des originalen Buchs deine Änderungen zurück. So könnt ihr perfekt zusammenarbeiten und beide erhalten ein tolles Lehrbuch.
