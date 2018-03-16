# \<shared-properties-mixin\>

Polymer mixin to enable property syncronization between different component instances through a transparent, zero-configuration shared store

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

## Demo

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