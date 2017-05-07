---
title: Klasse und Stil Bindungen
type: guide
order: 6
---

Ein gemeinsamer Bedarf an Datenbindung ist die Manipulation der Klassenliste
eines Elements und seiner Inline-Styles. Da sie beide Attribute sind, können
wir `v-bind` benutzen, um sie zu behandeln: Wir müssen nur einen endgültigen
String mit unseren Ausdrücken berechnen. Allerdings ist die Einmischung mit
String-Verkettung ärgerlich und fehleranfällig. Aus diesem Grund bietet Vue
spezielle Verbesserungen, wenn `v-bind` mit `class` und `style` verwendet wird.
Zusätzlich zu Strings können die Ausdrücke auch Objekte oder Arrays auswerten.

## Bindung von HTML-Klassen

### Objekt-Syntax

Wir können ein Objekt an `v-bind:class` übergeben, um die Klassen dynamisch zu
wechseln:

``` html
<div v-bind:class="{ active: isActive }"></div>
```

Die obige Syntax bedeutet, dass die Gegenwart der `active` Klasse durch die
[truthiness](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) der
Dateneigenschaft `isActive` bestimmt wird. Sie können mehrere Klassen
umschalten, indem Sie weitere Felder im Objekt haben. Darüber hinaus kann die
`v-bind:class`-Richtlinie auch mit dem einfachen `class` Attribut koexistieren.
 Also, angesichts der folgenden Vorlage:

``` html
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

Und die folgenden Daten:

``` js
data: {
  isActive: true,
  hasError: false
}
```

Es wird machen:

``` html
<div class="static active"></div>
```

Wenn `isActive` oder `hasError` Änderungen hat, wird die Klassenliste entsprechend aktualisiert. Wenn zum Beispiel `hasError` `true` wird, wird die Klassenliste `"static active text-danger"`.

Das gebundene Objekt muss nicht inline sein:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

Dies wird das gleiche Ergebnis machen. Wir können auch an eine [computed property](computed.html) binden, die ein Objekt zurückgibt. Dies ist ein gemeinsames und kraftvolles Muster:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal',
    }
  }
}
```

### Array-Syntax

Wir können ein Array an `v-bind:class` übergeben, um eine Liste der Klassen anzuwenden:

``` html
<div v-bind:class="[activeClass, errorClass]">
```
``` js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

Was wird machen:

``` html
<div class="active text-danger"></div>
```

Wenn du gern auch eine Klasse in der Liste bedingt hast, kannst du es mit einem ternären Ausdruck machen:

``` html
<div v-bind:class="[isActive ? activeClass : '', errorClass]">
```

Dies gilt immer `errorClass`, wird aber nur `activeClass` angewendet, wenn `isActive` `true` ist.

Allerdings kann dies ein bisschen ausführlich sein, wenn man mehrere bedingte Klassen hat. Aus diesem Grund ist es auch möglich, die Objektsyntax in der Array-Syntax zu verwenden:

``` html
<div v-bind:class="[{ active: isActive }, errorClass]">
```

### Mit Komponenten

> In diesem Abschnitt wird die Kenntnis von [Vue Components](components.html) angenommen. Fühlen Sie sich frei, es zu überspringen und kommen später zurück.

Wenn Sie das `class`-Attribut auf einer benutzerdefinierten Komponente verwenden, werden diese Klassen dem Stammelement der Komponente hinzugefügt. Bestehende Klassen auf diesem Element werden nicht überschrieben.

Zum Beispiel, wenn Sie diese Komponente erklären:

``` js
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```

Fügen Sie dann einige Klassen hinzu, wenn Sie es verwenden:

``` html
<my-component class="baz boo"></my-component>
```

Das gerenderte HTML wird sein:

``` html
<p class="foo bar baz boo">Hi</p>
```

Gleiches gilt für Klassenbindungen:

``` html
<my-component v-bind:class="{ active: isActive }"></my-component>
```

Wenn `isActive` wahrheitsgemäß ist, wird das gerenderte HTML:

``` html
<p class="foo bar active">Hi</p>
```

## Einbindung von Inline-Styles

### Objekt-Syntax

Die Objekt-Syntax für `v-bind:style` ist ziemlich einfach - es sieht fast wie CSS aus, außer es ist ein JavaScript-Objekt. Sie können entweder camelCase oder kebab-case (verwenden Sie Anführungszeichen mit kebab-case) für die CSS-Eigenschaftsnamen verwenden:

``` html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
``` js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

Es ist oft eine gute Idee, an ein Style-Objekt direkt zu binden, so dass die Vorlage sauberer ist:

``` html
<div v-bind:style="styleObject"></div>
```
``` js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

Auch hier wird die Objektsyntax häufig in Verbindung mit berechneten Eigenschaften verwendet, die Objekte zurückgeben.

### Array-Syntax

Die Array-Syntax für `v-bind:style` erlaubt Ihnen, mehrere Style-Objekte auf das gleiche Element anzuwenden:

``` html
<div v-bind:style="[baseStyles, overridingStyles]">
```

### Auto-Präfixierung

Wenn Sie eine CSS-Eigenschaft verwenden, die Vendor-Präfixe in `v-bind:style` erfordert, z. B. `transform`, wird Vue automatisch die entsprechenden Präfixe auf die angewandten Styles ermitteln und hinzufügen.
