---
name: saasufy-components-collections-of-resources
description: Helps you use Saasufy components related to viewing and updating collections/lists of resources such as the collection-viewer, collection-adder, collection-adder-form, collection-deleter and collection-reducer components.
---

## Components

### collection-viewer

Used for rendering collections as lists, tables or other sequences based on a specific view using a template.
Supports pagination by allowing you to specify custom buttons or links to navigate between pages.
Can also perform basic CRUD operations such as deleting or creating records by listening for events from child components.

**Import**

```html
<script src="https://saasufy.com/node_modules/saasufy-components/collection-viewer.js" type="module" defer></script>
```

**Example usage**

```html
<collection-viewer
  collection-type="Chat"
  collection-fields="username,message,createdAt"
  collection-view="recentView"
  collection-view-params=""
  collection-page-size="50"
>
  <template slot="item">
    <div>
      <div>{{Chat.username}}</div>
      <div>{{Chat.message}}</div>
      <div>{{date(Chat.createdAt)}}</div>
    </div>
  </template>

  <template slot="no-item">
    <div>There are no messages.</div>
  </template>

  <template slot="loader">
    <div class="loading-spinner-container">
      <div class="spinning">&#8635;</div>
    </div>
  </template>

  <div slot="viewport" class="chat-viewport"></div>
</collection-viewer>
```

In addition to the slots shown above, the `collection-viewer` also supports the following optional slots:
- `previous-page`: This is typically an `input` element of type `button` for pagination.
- `next-page`: This is typically an `input` element of type `button` for pagination.
- `page-number`: This is typically a `div` element to display the current page number. Alternatively, it can be an `input-provider` element to allow the user to manually type in the page number.
- `first-item`: This should be a `template` element. It can be used to inject an element just above the first record. This can be useful to render the `collection-viewer` as a table with a heading above table rows.
- `last-item`: This should be a `template` element. It can be used to inject an element just after the last record.
- `error`: This can be used to display an error message if the required data cannot be loaded. The `Error` object can be accessed inside the template via the syntax: `$CollectionName.error` (substitute `CollectionName` with your `collection-type` or, if set, your `type-alias` preceded by a dollar sign). For example, the error message for a `Chat` collection will be `$Chat.error.message`.

**Attributes**

- `collection-type` (required): Specifies the type of collection to display. This should match a `Model` available in your Saasufy service.
- `collection-fields` (required): A comma-separated list of fields from the `Model` that you want to display or use. This lets you pick specific pieces of data from your collection to work with. Note that updates made to these fields can cause the `collection-viewer` to re-render itself so you should consider whether or not a field needs to be specified at this level in the component hierarchy or should be delegated to a child component (otherwise it could cause nested `model-input` components to lose focus).
- `collection-view` (required): Determines the view of the collection. This should match one of the `Views` defined in your Saasufy service under that specific `Model`.
- `collection-view-params` (required): Parameters for the view, specified as comma-separated key-value pairs (e.g., key1=value1,key2=value2). These parameters can customize the behavior of the collection view. The keys must match `paramFields` specified in your Saasufy service under the relevant `View`.
- `collection-page-size`: Sets how many items from the collection are displayed at once. This is useful for pagination, allowing users to navigate through large sets of data in chunks.
- `collection-page-offset`: Indicates the current page offset in the collection's data. It’s like telling the viewer which page of data you want to display initially.
- `collection-get-count`: If this attribute is present, the component will get the record count of the target view. The count can be rendered into the template by prefixing the model name with a dollar sign and accessing the `count` property like this: `{{$MyModelName.count}}`.
- `collection-view-primary-fields`: An optional list of field names to specify which specific `collection-view-params` to watch for realtime updates. Fewer primary fields means that the view will be exposed to a broader range of realtime updates but at the cost of performance. It is generally recommended to have just one primary field.
- `collection-disable-realtime`: If this attribute is present, the underlying collection will not listen for changes nor update in realtime. Fields of records that are in view may still update in realtime.
- `auto-reset-page-offset`: If this attribute is present, the `collection-page-offset` will be reset to zero whenever the view params change.
- `type-alias`: Allows you to provide an alternative name for your `Model` to use when injecting values inside the template. This is useful for situations where you may have multiple `collection-viewer` elements and/or `model-viewer` elements of the same type nested inside each other and want to avoid `Model` name clashes in the nested template definitions. For example, if the `type-alias` in the snippet above was set to `SubChat`, then `{{Chat.message}}` would become `{{SubChat.message}}`.
- `hide-error-logs`: A flag which, when present, suppresses error logs from being printed to the console.
- `max-show-loader`: If this attribute is present, your slotted `loader` element will be shown as often as possible; this includes situations where the collection is merely refreshing itself. It is disabled by default.
- `greedy-refresh`: If this attribute exists, the component will refresh itself whenever an attribute is set, even if that attribute's value did not change.

### collection-adder

A basic form component for inserting data into collections.
This component is a simplified version of the `collection-adder-form` component as it doesn't require input elements to be slotted into it.

**Import**

```html
<script src="https://saasufy.com/node_modules/saasufy-components/collection-adder.js" type="module" defer></script>
```

**Example usage**

```html
<collection-adder
  collection-type="Product"
  collection-fields="name,brand,qty"
  model-values="categoryName=electronics,isNew:boolean=true"
  submit-button-label="Create"
  trim-spaces
></collection-adder>
```

**Attributes**

- `collection-type` (required): Specifies the type of collection to add the resource to when the form is submitted. This should match a `Model` available in your Saasufy service.
- `collection-fields`: A comma-separated list of fields from the `Model` to display as input elements inside the form for the user to fill in. Each field name in this list can optionally be followed by an input element type after a `:` character. For example `collection-fields="qty:number, size:select(small,medium,large)` will create one input element with `type="number"` and one with `type=select` with options `small`, `medium` or `large`. To make a select field optional, you can prefix the list of options with a comma; e.g. `size:select(,small,medium,large)`. Supported input types include: `text`, `select`, `number`, `checkbox`, `radio`, `textarea`, `file` and `text-select`.
- `model-values`: An optional list of key-value pairs in the format `field1=value1,field2=value2` to add to the newly created resource alongside the values collected from the user via the form. For non-string values, the type should be provided in the format `fieldName:type=value`; supported types are `string`, `number` and `boolean`. Unlike `collection-fields` which are rendered as input elements in the form, `model-values` are not shown to the user.
- `computable-model-values`: If this attribute exists on the element, then the `model-values` attribute will be re-computed before the form is submitted; this may be useful if the `model-values` attribute contains expressions which need to be updated before submitting.
- `field-labels`: Allows you to specify custom input labels by mapping field names to more user-friendly labels in the format `fieldName1='Field Name 1',fieldName2='Field Name 2'`.
- `submit-button-label`: Text to display on the submit button. If not specified, defaults to `Submit`.
- `hide-submit-button`: Adding this attribute will hide the submit button from the form.
- `success-message`: A message to show the user if the resource has been successfully added to the collection after submitting the form.
- `autocapitalize`: Can be set to `off` to disable auto-capitalization of the first input character on mobile devices.
- `autocorrect`: Can be set to `off` to disable auto-correction of input on mobile devices.
- `show-field-errors`: If this attribute exists on the element, then granular error messages will be shown above each corresponding input element instead of a single form-level error message.
- `trim-spaces`: If this attribute exists on the element, then leading and trailing spaces will be trimmed from each input element's value before submitting the form.
- `consumers`: This allows you to connect this `collection-adder` to other elements on your page. It should be used in conjunction with the `provider-template` attribute. The `consumers` attribute takes a list of selectors with optional attributes to target in the format `element-selector:attribute-name`. For example `.my-input:value` will find all elements with a `my-input` class and update their `value` attributes with the output of the `collection-adder` component on submit. You can specify multiple selectors separated by commas such as `.my-input,my-div`; in this case, because attribute names are not specified, values will be injected into the `value` attribute (for input elements) or into the `innerHTML` property (for other kinds of elements). The default attribute/property depends on the element type.
- `success-consumers`: Same as the `consumers` attribute but the target elements are only updated if insertion was a success.
- `error-consumers`: Same as the `consumers` attribute but the target elements are only updated if insertion was a failure.
- `provider-template`: A template string within which the `{{value}}` can be injected before passing to consumers. In the case of the `collection-adder` component, the value is an object with either a `resource` property (on successful insert) or `error` (on failure). On success, the `resource` object will be the object which was added to the collection. On failure, the `error` object will be a JavaScript `Error` object with a `message` property.

### collection-adder-form

A flexible form component for inserting data into collections.
This component is similar to the `collection-adder` component except that it requires input elements (and others) to be slotted into it.

**Import**

```html
<script src="https://saasufy.com/node_modules/saasufy-components/collection-adder-form.js" type="module" defer></script>
```

**Example usage**

```html
<collection-adder-form
  collection-type="Product"
  model-values="categoryName=electronics,isNew:boolean=true"
  trim-spaces
>
  <div slot="message"></div>
  <input type="text" name="name" placeholder="Name">
  <input type="text" name="brand" placeholder="Brand">
  <input type="number" name="qty" placeholder="Quantity">
  <input type="submit" value="Add new product">
</collection-adder-form>
```

**Attributes of collection-adder-form**

- `collection-type` (required): Specifies the type of collection to add the resource to when the form is submitted. This should match a `Model` available in your Saasufy service.
- `model-values`: An optional list of key-value pairs in the format `field1=value1,field2=value2` to add to the newly created resource alongside the values collected from the user via the form. For non-string values, the type should be provided in the format `fieldName:type=value`; supported types are `string`, `number` and `boolean`. Unlike `collection-fields` which are rendered as input elements in the form, `model-values` are not shown to the user.
- `computable-model-values`: If this attribute exists on the element, then the `model-values` attribute will be re-computed before the form is submitted; this may be useful if the `model-values` attribute contains expressions which need to be updated before submitting.
- `success-message`: A message to show the user if the resource has been successfully added to the collection after submitting the form.
- `show-field-errors`: If this attribute exists on the element, then granular error messages will be shown above each corresponding input element instead of a single form-level error message.
- `trim-spaces`: If this attribute exists on the element, then leading and trailing spaces will be trimmed from each input element's value before submitting the form.
- `auto-reset-hidden-inputs`: If this attribute exists, then `hidden` input elements which are inside the `collection-adder-form` will be cleared along with other input fields. By default, `hidden` input elements are not cleared when the form is reset.
- `disable-submit-on-enter-key`: If this attribute exists, then the form will not submit when the enter key is pressed.
- `greedy-refresh`: If this attribute exists, the component will refresh itself whenever an attribute is set, even if that attribute's value did not change.
- `consumers`: This allows you to connect this `collection-adder-form` to other elements on your page. It should be used in conjunction with the `provider-template` attribute. The `consumers` attribute takes a list of selectors with optional attributes to target in the format `element-selector:attribute-name`. For example `.my-input:value` will find all elements with a `my-input` class and update their `value` attributes with the output of the `collection-adder-form` component on submit. You can specify multiple selectors separated by commas such as `.my-input,my-div`; in this case, because attribute names are not specified, values will be injected into the `value` attribute (for input elements) or into the `innerHTML` property (for other kinds of elements). The default attribute/property depends on the element type.
- `success-consumers`: Same as the `consumers` attribute but the target elements are only updated if insertion was a success.
- `error-consumers`: Same as the `consumers` attribute but the target elements are only updated if insertion was a failure.
- `provider-template`: A template string within which the `{{value}}` can be injected before passing to consumers. In the case of the `collection-adder-form` component, the value is an object with either a `resource` property (on successful insert) or `error` (on failure). On success, the `resource` object will be the object which was added to the collection. On failure, the `error` object will be a JavaScript `Error` object with a `message` property.

**Attributes of slotted input, select, textarea... elements**

- `name` (required): This must correspond to a field name on the model specified via the `collection-type` attribute of the `collection-adder-form`.
- `output-type`: An optional type to cast the value of the input element into when the form is submitted.

### collection-deleter

A component which can be placed anywhere inside a `collection-viewer` component to delete a specific item from a collection as a result of a user action (e.g. on click).
It supports either immediate deletion or deletion upon confirmation; in the latter case, the parent `collection-viewer` must have a `confirm-modal` component slotted into its `modal` slot.

**Import**

```html
<script src="https://saasufy.com/node_modules/saasufy-components/collection-deleter.js" type="module" defer></script>
```

**Example usage**

```html
<!-- Must be placed somewhere inside a collection-viewer component, typically inside the slotted item template. -->
<collection-deleter model-id="{{Product.id}}" onclick="deleteItem()">&#x2715;</collection-deleter>
```

OR (with confirmation step)

```html
<!-- Must be placed somewhere inside a collection-viewer component, typically inside the slotted item template. -->
<collection-deleter model-id="{{Product.id}}" confirm-message="Are you sure you want to delete the {{Product.name}} product?" onclick="confirmDeleteItem()">&#x2715;</collection-deleter>
```

**Attributes**

- `model-id` (required): Specifies the ID of the resource to delete from the parent collection when this component is activated. This can be achieved by invoking either the `deleteItem()` or `confirmDeleteItem()` function from inside an event handler. The example above shows how to achieve deletion via the `onclick` event. You can either invoke `deleteItem()` to delete the resource immediately or you can invoke `confirmDeleteItem()` to require additional confirmation prior to deletion.
- `onclick` (required): The logic to execute to delete the item. Should be either `deleteItem()` or `confirmDeleteItem()`.
- `confirm-message`: The confirmation message to show the user when this component's `confirmDeleteItem()` function is invoked.
- `model-field`: If specified, this attribute can be used to delete a single field of a model instance, instead of the whole instance.

If `confirmDeleteItem()` is used, then the parent `collection-viewer` must have a `confirm-modal` element slotted into it as shown here:

```html
<collection-viewer
  collection-type="Product"
  collection-fields="name,qty"
  collection-view="alphabeticalView"
  collection-view-params=""
  collection-page-size="50"
>
  <template slot="item">
    <div>
      <div>{{Product.name}}</div>
      <collection-deleter model-id="{{Product.id}}" confirm-message="Are you sure you want to delete the {{Product.name}} product?" onclick="confirmDeleteItem()">&#x2715;</collection-deleter>
    </div>
  </template>

  <div slot="viewport" class="chat-viewport"></div>

  <!-- The confirm-modal element must be specified here with slot="modal" to prompt the user for confirmation -->
  <confirm-modal slot="modal" title="Delete confirmation" message="" confirm-button-label="Delete"></confirm-modal>
</collection-viewer>
```

### collection-reducer

Similar to the `collection-viewer` component but designed to render collections in combined format. For example, to combine values from multiple records into a single item.
A common use case is to extract and join values to pass to other child components via their attributes.

**Import**

```html
<script src="https://saasufy.com/node_modules/saasufy-components/collection-reducer.js" type="module" defer></script>
```

**Example usage**

```html
<!--
  Here, the collection-reducer fetches all available categories and joins their name fields
  to use as options for a model-input select element.
  This allows the user to choose among available category names to update the
  current product's categoryName field.
  You should assume that {{productId}} comes from a parent component (e.g. app-router).
-->
<collection-reducer
  collection-type="Category"
  collection-fields="name"
  collection-view="alphabeticalView"
  collection-page-size="100"
  collection-view-params=""
>
  <template slot="item">
    <div class="form">
      <label class="label-container">
        <div>Product category</div>
        <model-input
          type="select"
          options="{{joinFields(Category, 'name')}}"
          model-type="Product"
          default-value="accountId"
          model-id="{{productId}}"
          model-field="categoryName"
        ></model-input>
      </label>
    </div>
  </template>
  <div slot="viewport"></div>
</collection-reducer>
```

Unlike with `collection-viewer` or `model-viewer`, the template variable references not a single model instance but an array of model instances.
In the example above, the name of the first element can be accessed with `{{Category[0].name}}`.

**Attributes**

- `collection-type` (required): Specifies the type of collection to use. This should match a `Model` available in your Saasufy service.
- `collection-fields` (required): A comma-separated list of fields from the `Model` that you want to use. This lets you pick specific pieces of data from your collection to work with.
- `collection-view` (required): Determines the view of the collection. This should match one of the `Views` defined in your Saasufy service under that specific `Model`.
- `collection-view-params` (required): Parameters for the view, specified as comma-separated key-value pairs (e.g., key1=value1,key2=value2). These parameters can customize the behavior of the collection view. The keys must match `paramFields` specified in your Saasufy service under the relevant `View`.
- `collection-page-size`: Sets how many items from the collection are displayed at once. This is useful for pagination, allowing users to navigate through large sets of data in chunks.
- `collection-page-offset`: Indicates the current page offset in the collection's data. It’s like telling the viewer which page of data you want to display initially.
- `collection-get-count`: If this attribute is present, the component will get the record count of the target view. The count can be rendered into the template by prefixing the model name with a dollar sign and accessing the `count` property like this: `{{$MyModelName.count}}`.
- `type-alias`: Allows you to provide an alternative name for your `Model` to use when injecting values inside the template. This is useful for situations where you may have multiple `collection-viewer` elements and/or `model-viewer` elements of the same type nested inside each other and want to avoid `Model` name clashes in the nested template definitions. For example, if the `type-alias` in the snippet above was set to `SubChat`, then `{{Chat.message}}` would become `{{SubChat.message}}`.
- `hide-error-logs`: A flag which, when present, suppresses error logs from being printed to the console.
- `greedy-refresh`: If this attribute exists, the component will refresh itself whenever an attribute is set, even if that attribute's value did not change.
