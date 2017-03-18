---
title: Einleitung
type: guide
order: 2
---

## Was ist Vue.js?

Vue (/vjuː/ ausgesprochen, wie **view**, Englisch für **Sicht**) ist ein **fortschrittliches Framework** für das erstellen von Nutzeroberflächen. Im Gegensatz zu anderen monolithischen Frameworks ist Vue von Grund auf dafür konzipiert, schrittweise erweiterbar zu sein. Die Kern-Bibliothek von Vue greift ausschließlich in die Oberfläche der Anwendung ein und lässt sich sehr einfach mit anderen Bibliotheken und Projekten erweitern. Auf der anderen Seite ist Vue auch perfekt dafür geeignet, <!-- TODO: sophisticated --> Single-Page-Anwendungen zu gestalten. Dafür bieten sich [moderne Werkzeuge](single-file-components.html) und [unterstützende Bibliotheken](https://github.com/vuejs/awesome-vue#libraries--plugins) an.

Wenn du ein erfahrener Entwickler bist und wissen willst, wie sich Vue im Vergleich mit anderen Bibliotheken bzw. Frameworks schlägt, dann solltest du [diesen Vergleich](comparison.html) in Betracht ziehen.

## Los geht's

<p class="tip">Dieser offizielle Guide setzt gewisse Kenntnisse in HTML, CSS und JavaScript voraus. Wenn du gerade erst mit der Frontend-Programmierung angefangen hast, ist es wahrscheinlich nicht die beste Idee, dich direkt in ein Framework zu stürzen - erarbeite dir lieber die Grundlagen und komme dann zurück! Kenntnisse mit anderen Frameworks ist jedoch nicht von Nöten.</p>

Der einfachste Weg, Vue.js auszuprobieren, ist, sich das [JSFiddle Hello World Beispiel](https://jsfiddle.net/chrisvfritz/50wL7mdz/) anzugucken. Öffne es ruhig in einem neuen Tab und beziehe es immer mal wieder ein, während wir diesen Kurs beschreiten. Du kannst auch einfach deine eigene `.html` Datei mit dem folgenden Inhalt erstellen, um Vue einzubeziehen:

``` html
<script src="https://unpkg.com/vue"></script>
```

Die [Installations-Seite](installation.html) zeigt zusätzliche Optionen auf, Vue zu installieren. Bitte beachte, dass wir Anfängern **nicht** empfehlen, mit `vue-cli` zu beginnen, erst recht nicht, wenn du dich nicht mit Node.js-basierten Entwicklungs-Werkzeugen auskennst.

## Deklaratives Rendern
<!-- TODO: deklarative Rendering -->

Im Kern von Vue.js steckt das System, das deklaratives Rendern des DOMs ermöglicht. Ein kleines Beispiel zeigt dieses Prinzip:

``` html
<div id="app">
  {{ message }}
</div>
```
``` js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```
{% raw %}
<div id="app" class="demo">
  {{ message }}
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
</script>
{% endraw %}

Damit haben wir unsere erste Vue-App erstellt! Das ganze sieht dem einfachen Rendern einer Vorlage ziemlich ähnlich, aber Vue macht unter der Haube noch viel mehr. Die Daten und das DOM sind nun verknüpft, alles **reagiert** jetzt aufeinander. Glaubst du nicht? Öffne doch einfach mal die JavaScript-Konsole deines Browsers (hier, auf dieser Seite) und belege `app.message` mit einem anderen Wert. Entsprechend deiner Änderung solltest du auch die Änderung des Beispiels oben sehen.

Neben dem Einschieben von Text können wir auch Werte an Eigenschaften von Elementen binden. (Um die Auswirkung des Beispiels zu sehen, sollte der DOM-Inspektor benutzt werden, um das sich verändernde Attribut beobachten zu können.)

``` html
<div id="app-2">
  <span v-bind:title="message">
    Bewege deine Maus für ein paar Sekunden über mich,
    um meinen sich dynamisch verändernden Titel zu beobachten.
  </span>
</div>
```
``` js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Du hast die Seite um ' + new Date() + ' geladen'
  }
})
```
{% raw %}
<div id="app-2" class="demo">
  <span v-bind:title="message">
    Bewege deine Maus für ein paar Sekunden über mich,
    um meinen sich dynamisch verändernden Titel zu beobachten.
  </span>
</div>
<script>
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Du hast die Seite um ' + new Date() + ' geladen'
  }
})
</script>
{% endraw %}

Hier sehen wir etwas neues. Das Attribut `v-bind`, dass du siehst, wird eine **Direktive** genannt. Direktiven sind mit dem Präfix `v-` versehen, um anzudeuten, dass sie spezielle, von Vue bereitgestelle Attribute sind. Und wie du vielleicht bereits mitbekommen hast, zeigen sie ein besonderes Verhalten im gerenderten DOM. Hier bedeutet das im Prinzip "setzte das Attribut `title` immer gleich mit der Eigenschaft `message` der Vue-Instanz, erneuere das eine mit dem anderen."

Wenn du in deine JavaScript-Konsole guckst und `app2.message = 'eine neue Nachricht'` eingibst, wirst du sehen, dass sich das verbundene HTML - in diesem Fall das Attribut `title` - erneuert hat.

## Bedingungen und Schleifen

Es ist ziemlich einfach, die Sichtbarkeit eines Elements zu steuern:

``` html
<div id="app-3">
  <p v-if="seen">Jetzt siehst du mich</p>
</div>
```

``` js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

{% raw %}
<div id="app-3" class="demo">
  <span v-if="seen">Jetzt siehst du mich</span>
</div>
<script>
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
</script>
{% endraw %}

Wenn du `app3.seen = false` in der Konsole eingibst, sollte diese Nachricht verschwinden.
Dieses Beispiel zeigt, dass wir Daten nicht nur mit Text und Attributen verknüpfen konnen, sondern auch direkt mit der **Struktur** des DOMs. Des weiteren bietet Vue ein System an Animationen für Übergänge an, das automatisch diese [Effekte anwenden kann](transitions.html), wenn die Elemente von Vue eingefügt/erneuert/entfernt werden.

Es gibt noch einige andere Direktiven, jede mit ihrer eigenen speziellen Funktionsweise. Die `v-for` Direktive zum Beispiel kann genutzt werden, um eine Liste mit Inhalten aus einem Array darzustellen.

``` html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
``` js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'JavaScript lernen' },
      { text: 'Vue lernen' },
      { text: 'Irgendwas geniales daraus machen' }
    ]
  }
})
```
{% raw %}
<div id="app-4" class="demo">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
<script>
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'JavaScript lernen' },
      { text: 'Vue lernen' },
      { text: 'Irgendwas geniales daraus machen' }
    ]
  }
})
</script>
{% endraw %}

Wenn du `app4.todos.push({ text: 'Neues Element' })` in der Konsole eingibst, so solltest du ein neues Element in der Liste sehen.

## Mit Nutzereingaben umgehen

To let users interact with your app, we can use the `v-on` directive to attach event listeners that invoke methods on our Vue instances:

``` html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
```
``` js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
{% raw %}
<div id="app-5" class="demo">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
<script>
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Note in the method we simply update the state of our app without touching the DOM - all DOM manipulations are handled by Vue, and the code you write is focused on the underlying logic.

Vue also provides the `v-model` directive that makes two-way binding between form input and app state a breeze:

``` html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
``` js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```
{% raw %}
<div id="app-6" class="demo">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
<script>
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
</script>
{% endraw %}

## Composing with Components

The component system is another important concept in Vue, because it's an abstraction that allows us to build large-scale applications composed of small, self-contained, and often reusable components. If we think about it, almost any type of application interface can be abstracted into a tree of components:

![Component Tree](/images/components.png)

In Vue, a component is essentially a Vue instance with pre-defined options. Registering a component in Vue is straightforward:

``` js
// Define a new component called todo-item
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})
```

Now you can compose it in another component's template:

``` html
<ol>
  <!-- Create an instance of the todo-item component -->
  <todo-item></todo-item>
</ol>
```

But this would render the same text for every todo, which is not super interesting. We should be able to pass data from the parent scope into child components. Let's modify the component definition to make it accept a [prop](components.html#Props):

``` js
Vue.component('todo-item', {
  // The todo-item component now accepts a
  // "prop", which is like a custom attribute.
  // This prop is called todo.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

Now we can pass the todo into each repeated component using `v-bind`:

``` html
<div id="app-7">
  <ol>
    <!-- Now we provide each todo-item with the todo object    -->
    <!-- it's representing, so that its content can be dynamic -->
    <todo-item v-for="item in groceryList" v-bind:todo="item"></todo-item>
  </ol>
</div>
```
``` js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { text: 'Vegetables' },
      { text: 'Cheese' },
      { text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
```
{% raw %}
<div id="app-7" class="demo">
  <ol>
    <todo-item v-for="item in groceryList" v-bind:todo="item"></todo-item>
  </ol>
</div>
<script>
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { text: 'Vegetables' },
      { text: 'Cheese' },
      { text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
</script>
{% endraw %}

This is just a contrived example, but we have managed to separate our app into two smaller units, and the child is reasonably well-decoupled from the parent via the props interface. We can now further improve our `<todo-item>` component with more complex template and logic without affecting the parent app.

In a large application, it is necessary to divide the whole app into components to make development manageable. We will talk a lot more about components [later in the guide](components.html), but here's an (imaginary) example of what an app's template might look like with components:

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

### Relation to Custom Elements

You may have noticed that Vue components are very similar to **Custom Elements**, which are part of the [Web Components Spec](http://www.w3.org/wiki/WebComponents/). That's because Vue's component syntax is loosely modeled after the spec. For example, Vue components implement the [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) and the `is` special attribute. However, there are a few key differences:

1. The Web Components Spec is still in draft status, and is not natively implemented in every browser. In comparison, Vue components don't require any polyfills and work consistently in all supported browsers (IE9 and above). When needed, Vue components can also be wrapped inside a native custom element.

2. Vue components provide important features that are not available in plain custom elements, most notably cross-component data flow, custom event communication and build tool integrations.

## Ready for More?

We've just briefly introduced the most basic features of Vue.js core - the rest of this guide will cover them and other advanced features with much finer details, so make sure to read through it all!
