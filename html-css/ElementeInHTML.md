# Elemente in HTML

In HTML werden die Bereiche des Textes mithilfe von **Tags** voneinander abgegrenzt:

```html
<h1>Die Tiere des Meeres</h1>
```

Im diesem Beispiel wird eine Überschrift ausgezeichnet. Dabei markiert `<h1>` den Beginn der Überschrift und wird somit **Starttag** genannt. `</h1>` markiert das Ende des Tags und ist somit der **Endtag**.

Sicher ist dir aufgefallen, dass sich Start- und Endtag nur durch den `/` unterscheiden. Dadurch behälst du ganz leicht den Überblick.  Die spitzen Klammern `<>` mit allem was darin steht, werden nicht in der Webseite zu sehen sein und dienen nur als Orientierung für den Browser.

t> [Aufgabe](https://eule27.de/t/sdFpf)

Es gibt einzelne Fälle, da möchte man zwar ein durch Start- und Endtag markiertes Element aufschreiben, hat aber keinen Text, der ausgegeben werden soll. In diesem Fall könntest du folgendes schreiben:

```html
<br></br>
```

Das geht zwar, ist aber den Entwicklern zu lang. Daher gibt es hier eine Kurzschreibweise:

```html
<br />
```

t> [Aufgabe](https://eule27.de/t/DdsUg)

i> br ist die Abkürzung für *break and return*
