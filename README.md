# \<shared-properties-mixin\>

[![CircleCI](https://circleci.com/gh/DavidEspinola/shared-properties-mixin/tree/master.svg?style=svg)](https://circleci.com/gh/DavidEspinola/shared-properties-mixin/tree/master)
[![Published on webcomponents.org](https://img.shields.io/badge/webcomponents.org-published-blue.svg)](https://www.webcomponents.org/element/DavidEspinola/shared-properties-mixin)


Polymer mixin to enable property syncronization between different component instances through a transparent, zero-configuration shared store.

It extends PolymerElement's array and object mutation methods providing syncronization across all instances attached to the same store.

## Install the Polymer-CLI

First, make sure you have the [Polymer CLI](https://www.npmjs.com/package/polymer-cli) installed. Then run `polymer serve` to serve your element locally.

## Viewing Your Element

```
$ polymer serve
```

## Running Tests

```
$ polymer test
```

Your application is already set up to be tested via [web-component-tester](https://github.com/Polymer/web-component-tester). Run `polymer test` to run your application's test suite locally.

## How to use it

Share a property between two instances of the same component:

<!---
```
<custom-element-demo>
  <template>
    <script src="../webcomponentsjs/webcomponents-lite.js"></script>
    <link rel="import" href="../polymer/polymer.html">
    <link rel="import" href="shared-properties-mixin.html">
    <style>
      demo-component{
        display: inline-block;
        padding: 10px;
        margin: 5px;
        border: 1px solid lightgrey;
      }
    </style>
    <next-code-block></next-code-block>
  </template>
</custom-element-demo>
```
-->
```html
<demo-component></demo-component>
<demo-component></demo-component>
<script>
  class DemoComponent extends SharedPropertiesMixin(Polymer.Element){
    static get properties(){
      return {
        prop: { 
          type: 'Number',
          value: 0,
          sharedStoreKey: 'commonProp' // <- The only config needed
        }
      }
    }
    static get template() {
      return Polymer.html`
        <h3>[[prop]]</h3>
        <button on-click="_increment">Increment</button>`;
    }
    _increment(){
      this.prop++;
    }
  }

  window.customElements.define('demo-component', DemoComponent);
</script>
```

Share a property between two instances of different components:

<!---
```
<custom-element-demo>
  <template>
    <script src="../webcomponentsjs/webcomponents-lite.js"></script>
    <link rel="import" href="../polymer/polymer.html">
    <link rel="import" href="shared-properties-mixin.html">
    <style>
      demo-component2, demo-component3{
        display: inline-block;
        padding: 10px;
        margin: 5px;
        border: 1px solid lightgrey;
      }
    </style>
    <next-code-block></next-code-block>
  </template>
</custom-element-demo>
```
-->
```html
<demo-component2></demo-component2>
<demo-component3></demo-component3>

<script>
  class DemoComponent2 extends SharedPropertiesMixin(Polymer.Element){
    static get properties(){
      return {
        prop2: { 
          type: 'Number',
          value: 0,
          sharedStoreKey: 'foo' 
        }
      }
    }
    static get template() {
      return Polymer.html`
        <p>         
          [[prop2]] <button on-click="_increment">Increment!</button>
        </p>`;
    }
    _increment(){
      this.prop2++;
    }
  }

  class DemoComponent3 extends SharedPropertiesMixin(Polymer.Element){
    static get properties(){
      return {
        prop3: { 
          type: 'Number',
          value: 1, // <- Ignored because it have been previously defined
          sharedStoreKey: 'foo' // <- Same storeKey in different property
        }
      }
    }
    static get template() {
      return Polymer.html`
        <p>[[prop3]]</p>
        <button on-click="_increment">Increment!!</button>`;
    }
    _increment(){
      this.prop3++;
    }
  }

  window.customElements.define('demo-component2', DemoComponent2);
  window.customElements.define('demo-component3', DemoComponent3);
</script>
```

Dynamically share a new property:

<!---
```
<custom-element-demo>
  <template>
    <script src="../webcomponentsjs/webcomponents-lite.js"></script>
    <link rel="import" href="../polymer/polymer.html">
    <link rel="import" href="shared-properties-mixin.html">
    <style>
      demo-component4{
        display: inline-block;
        padding: 10px;
        margin: 5px;
        border: 1px solid lightgrey;
      }
    </style>
    <next-code-block></next-code-block>
  </template>
</custom-element-demo>
```
-->
```html
<demo-component4></demo-component4>
<demo-component4></demo-component4>

<script>
  class DemoComponent4 extends SharedPropertiesMixin(Polymer.Element){
    constructor(){
      super();
      this.demo = 0;
      this.setSharedProperty('demo', 'dynamicallyAdded', 1);
    }
    static get template() {
      return Polymer.html`
        <p>         
          [[demo]] <button on-click="_increment">Increment!</button>
        </p>`;
    }
    _increment(){
      this.demo++;
    }
  }

  window.customElements.define('demo-component4', DemoComponent4);
</script>
```
