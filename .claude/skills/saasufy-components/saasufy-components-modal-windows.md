---
name: saasufy-components-modal-windows
description: Helps you use Saasufy components related to modal windows and dialog boxes such as the confirm-modal and overlay-modal components.
---

## Components

### confirm-modal

A modal component to prompt the user for confirmation before performing sensitive operations. Can be slotted into a `collection-viewer` component.

**Import**

```html
<script src="https://saasufy.com/node_modules/saasufy-components/confirm-modal.js" type="module" defer></script>
```

**Example usage**

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

  <div slot="viewport"></div>

  <!-- The confirm-modal element must be specified here with slot="modal" to prompt the user for confirmation -->
  <confirm-modal slot="modal" title="Delete confirmation" message="" confirm-button-label="Delete"></confirm-modal>
</collection-viewer>
```

**Attributes**

- `title`: The text to show in the modal's title bar.
- `message`: The text to show as the modal's main content.
- `confirm-button-label`: The text to use as the confirm button label.
- `cancel-button-label`: The text to use as the cancel button label.

### overlay-modal

A general purpose modal component.
