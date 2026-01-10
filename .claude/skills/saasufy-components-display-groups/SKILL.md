---
name: saasufy-components-display-groups
description: Helps you use Saasufy components related to grouping and displaying other components; for example to delay rendering until all components within a group have been loaded or to group multiple components into one. Components include the render-group, if-group, switch-group and collection-adder-group.
---

## Components

### render-group

A component which can be used to guarantee that certain children components are always rendered at the same time (once they are loaded) to provide a smooth user experience.

**Import**

```html
<script src="https://saasufy.com/node_modules/saasufy-components/render-group.js" type="module" defer></script>
```

**Example usage**

```html
<render-group wait-for="1,2,3">
  <model-input
    type="text"
    load-id="1"
    model-type="Product"
    model-id="{{productId}}"
    model-field="name"
  ></model-input>
  <model-input
    type="text"
    load-id="2"
    model-type="Product"
    model-id="{{productId}}"
    model-field="desc"
  ></model-input>
  <model-input
    type="number"
    load-id="3"
    model-type="Product"
    model-id="{{productId}}"
    model-field="qty"
  ></model-input>
</render-group>
```

**Attributes**

- `wait-for` (required): A list of components to wait for before rendering based on their `load-id` attributes.
The `render-group` will only render its content once all the components specified via this attribute have finished loading their data.

### if-group

A component which exposes a `show-content` property which can be set to true or false to show or hide its content.
It's intended to be placed inside a `model-viewer`, `collection-viewer` or `collection-reducer` component such that the true/false value of the `show-content` attribute can be computed using a template `{{expression}}` placeholder. This component requires a template and a viewport to be slotted in. The content of the template will not be processed unless the `show-content` condition is met.

**Import**

```html
<script src="https://saasufy.com/node_modules/saasufy-components/if-group.js" type="module" defer></script>
```

**Example usage**

```html
<if-group show-content="{{!!Product.isVisible}}">
  <template slot="content">
    <div>Name: {{Product.name}}</div>
    <div>Description: {{Product.desc}}</div>
    <div>Quantity: {{Product.qty}}</div>
  </template>
  <div slot="viewport"></div>
</if-group>
```

### switch-group

A component which exposes a `show-cases` property which can be set to key-value pairs in the format `key1=true,key2=false`. It can be used to conditionally display multiple slotted elements based on multiple conditions. It helps to keep HTML clean when the conditions are complex.
This element is intended to be placed inside a `model-viewer`, `collection-viewer` or `collection-reducer` component such that the true/false values of the `show-cases` attribute can be computed using template `{{expression}}` placeholders. This component requires one or more templates and a viewport to be slotted in. The content of the templates will not be processed unless the `show-cases` condition is met. All templates must have a `slot="content"` attribute and a name attribute in the format `name="key1"` where the value `key1` corresponds to the key specified inside the `show-cases` attribute of the `switch-group`.

**Import**

```html
<script src="https://saasufy.com/node_modules/saasufy-components/switch-group.js" type="module" defer></script>
```

**Example usage**

```html
<switch-group show-cases="image={{!!Section.imageURL}},text={{!!Section.text}}">
  <template slot="content" name="image">
    <img src="{{Section.imageURL}}" alt="{{Section.imageDesc}}" />
  </template>
  <template slot="content" name="text">
    <div>{{Section.text}}</div>
  </template>
  <div slot="viewport"></div>
</switch-group>
```

### collection-adder-group

A component which can be used to group together multiple `collection-adder` components. It can be used to insert multiple records into a collection via a single button click.

**Import**

```html
<script src="https://saasufy.com/node_modules/saasufy-components/collection-adder-group.js" type="module" defer></script>
```

**Example usage**

```html
<!--
  This collection-adder-group adds multiple pre-defined records into a Product
  collection using a single button click. This example assumes that the name
  field is the only required field in the Product model.
-->
<collection-adder-group>
  <collection-adder
    slot="collection-adder"
    collection-type="Product"
    model-values="name=Flour"
    hide-submit-button
  ></collection-adder>
  <collection-adder
    slot="collection-adder"
    collection-type="Product"
    model-values="name=Egg"
    hide-submit-button
  ></collection-adder>
  <collection-adder
    slot="collection-adder"
    collection-type="Product"
    model-values="name=Milk"
    hide-submit-button
  ></collection-adder>
  <div slot="error-container"></div>
  <input slot="submit-button" type="button" value="Add pancake ingredients">
</collection-adder-group>
```
