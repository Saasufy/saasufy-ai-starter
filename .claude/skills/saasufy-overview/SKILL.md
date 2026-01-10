---
name: saasufy-overview
description: Provides an overview of how to use the main Saasufy features and components.
---

# Saasufy Examples

Build real-time web applications using Saasufy components - a declarative web component framework for creating data-driven apps with WebSocket connectivity.

## Core Concepts

### Component Architecture
- **socket-provider**: Top-level component connecting to Saasufy service via WebSocket
- **socket-consumer**: Any component that uses Saasufy data (must be inside socket-provider)
- **Templating**: Use `{{expression}}` for safe HTML-escaped values, `{{{expression}}}` for raw HTML
- **Slots**: Components use `slot="item"` for templates and `slot="viewport"` for rendered output

### Application Structure
```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://saasufy.com/node_modules/saasufy-components/socket.js" type="module" defer></script>
  <!-- Include only the component scripts you need -->
</head>
<body>
  <socket-provider url="wss://saasufy.com/YOUR_SERVICE_ID/socketcluster/">
    <!-- Your app components go here -->
  </socket-provider>
</body>
</html>
```

### Service WebSocket API URL

The Service API endpoint depends on your deployed Saasufy service.
This URL is stored inside the `.saasufy-service-url` file at the root of the project. If it is not provided, the user should deploy their service and provide it to you; you should then add it to that file (replacing the default one which is a dummy URL containing the path `/sidxxxx/`).

## Key Components

### Data Display
- **collection-viewer**: Display collections as lists/tables with pagination and real-time updates
- **collection-reducer**: Combine/aggregate collection data (e.g., for dropdowns)
- **model-viewer**: Display a single model instance
- **model-text**: Display a single field (read-only)
- **model-input**: Display and edit a single field with real-time sync

### Data Modification
- **collection-adder**: Simple form to insert records (auto-generated inputs)
- **collection-adder-form**: Flexible form with custom slotted inputs
- **collection-deleter**: Delete records with optional confirmation

### User Input
- **input-provider**: Input component that provides data to other components
- **input-combiner**: Combine multiple input-provider outputs
- **input-transformer**: Transform input-provider data before passing to consumers

### Navigation & Auth
- **app-router**: Client-side routing with route parameters and auth redirects
- **log-in-form**: Blockchain-based authentication
- **oauth-link** + **oauth-handler**: OAuth authentication flow

### UI Components
- **confirm-modal**: Confirmation dialogs
- **render-group**: Ensure components render together
- **if-group**: Conditional rendering
- **switch-group**: Multiple conditional cases

## Best Practices

### 1. Component Loading
Only include script tags for components you actually use to minimize bandwidth:
```html
<!-- Essential -->
<script src="https://saasufy.com/node_modules/saasufy-components/socket.js" type="module" defer></script>

<!-- Add only what you need -->
<script src="https://saasufy.com/node_modules/saasufy-components/collection-viewer.js" type="module" defer></script>
<script src="https://saasufy.com/node_modules/saasufy-components/model-input.js" type="module" defer></script>
```

### 2. Security
- Use `{{value}}` (double brackets) by default - auto-escapes HTML to prevent XSS
- Only use `{{{value}}}` (triple brackets) for trusted, pre-sanitized HTML
- Never inject untrusted user input with triple brackets or nested expressions

### 3. Template Variables
Inside component templates, access model data by type name:
- `{{Product.name}}` - field from Product model
- `{{$Product.count}}` - collection metadata (with `collection-get-count` attribute)
- `{{productName}}` - route parameter from app-router
- Use `type-alias` attribute to avoid name clashes in nested components

### 4. Real-time Updates
- Collections auto-update in real-time by default
- Use `collection-disable-realtime` to disable if needed
- Use `collection-view-primary-fields` to optimize real-time subscriptions
- Use `enable-rebound` on model-input when cross-client sync needed

### 5. Performance
- Use `collection-page-size` for pagination on large datasets
- Use `fields-slice-to` or `slice-to` to limit field size
- Minimize `collection-view-primary-fields` for better performance
- Use `debounce-delay` on inputs to batch rapid updates

## Common Patterns

### Pattern: List with Add/Delete
```html
<collection-viewer
  collection-type="Product"
  collection-fields="name,price,qty"
  collection-view="alphabeticalView"
  collection-view-params=""
  collection-page-size="20"
>
  <template slot="item">
    <div class="product-row">
      <span>{{Product.name}} - ${{Product.price}}</span>
      <collection-deleter
        model-id="{{Product.id}}"
        confirm-message="Delete {{Product.name}}?"
        onclick="confirmDeleteItem()">✕</collection-deleter>
    </div>
  </template>

  <template slot="no-item">
    <div>No products found.</div>
  </template>

  <div slot="viewport"></div>

  <confirm-modal slot="modal" title="Confirm Delete" confirm-button-label="Delete"></confirm-modal>
</collection-viewer>

<collection-adder
  collection-type="Product"
  collection-fields="name,price:number,qty:number"
  submit-button-label="Add Product"
></collection-adder>
```

### Pattern: Search/Filter
```html
<input-provider
  type="text"
  consumers=".product-viewer:collection-view-params"
  provider-template="query='name contains (?i){{value}}'"
  placeholder="Search products..."
  debounce-delay="500"
></input-provider>

<collection-viewer
  class="product-viewer"
  collection-type="Product"
  collection-fields="name,price"
  collection-view="searchView"
  collection-view-params=""
>
  <!-- templates here -->
</collection-viewer>
```

### Pattern: Multi-Page App
```html
<app-router default-page="/products">
  <template slot="page" route-path="/products">
    <h1>Products</h1>
    <!-- collection-viewer here -->
  </template>

  <template slot="page" route-path="/product/:productId">
    <h1>Product Details</h1>
    <model-viewer
      model-type="Product"
      model-id="{{productId}}"
      model-fields="name,desc,price,qty"
    >
      <template slot="item">
        <div>{{Product.name}}</div>
        <div>{{Product.desc}}</div>
      </template>
      <div slot="viewport"></div>
    </model-viewer>
  </template>

  <div slot="viewport"></div>
</app-router>
```

### Pattern: Edit Form
```html
<model-viewer
  model-type="Product"
  model-id="abc-123"
  model-fields="name,desc,price"
>
  <template slot="item">
    <div class="form">
      <label>
        Name:
        <model-input
          type="text"
          model-type="Product"
          model-id="{{Product.id}}"
          model-field="name"
        ></model-input>
      </label>

      <label>
        Price:
        <model-input
          type="number"
          model-type="Product"
          model-id="{{Product.id}}"
          model-field="price"
        ></model-input>
      </label>
    </div>
  </template>
  <div slot="viewport"></div>
</model-viewer>
```

## Utility Functions

### Template Utilities (use in {{}} expressions)
- `uuid.v4()` - Generate random UUID
- `computeId('val1', 'val2')` - Generate deterministic UUID from values
- `url('text')` - URL-encode text
- `lowerCase('TEXT')` / `upperCase('text')` / `capitalize('text')` - Case conversion
- `trim('  text  ')` - Remove leading/trailing spaces
- `fallback(value1, value2, 'default')` - First non-null value
- `date(timestamp)` - Format UNIX timestamp as date
- `safeString(text)` - Escape HTML characters
- `joinFields(arrayOfObjects, 'fieldName')` - Extract and join field values

### JavaScript Utilities (import from utils.js)
```javascript
import { getRecords, getRecord, generateRecords } from 'https://saasufy.com/node_modules/saasufy-components/utils.js';

let socket = document.querySelector('socket-provider').getSocket();

// Fetch multiple records
let products = await getRecords({
  socket,
  type: 'Product',
  viewName: 'alphabeticalView',
  viewParams: { category: 'electronics' },
  fields: ['name', 'price'],
  pageSize: 50
});

// Stream records efficiently (generator)
for await (let product of generateRecords({ socket, type: 'Product', viewName: 'allView' })) {
  console.log(product.name);
}
```

## Special Model Fields

Expose via Saasufy control panel by creating matching field:
- `id` (String) - Always available, UUID v4
- `createdBy` (String) - Account ID that created record
- `updatedBy` (String) - Account ID that last modified
- `createdByIp` (String) - IP address of creator
- `createdAt` (Number) - UNIX timestamp of creation
- `updatedAt` (Number) - UNIX timestamp of last update

## File Hosting

1. Create string field with `blob` constraint in Saasufy
2. Upload via dashboard or model-input with `type="file"`
3. Access via: `https://saasufy.com/SERVICE_ID/files/MODEL/RECORD_ID/FIELD`

Example:
```html
<img src="https://saasufy.com/sid7999/files/Image/58f2051c-14bc-4518-8816-ff387cfdd57e/data" alt="Product" />
```

## Events

### Collection Events (bubble up from collection-viewer)
```javascript
element.addEventListener('collectionCreate', (e) => {
  console.log('Created:', e.detail.type, e.detail.id);
});

element.addEventListener('collectionUpdate', (e) => {
  console.log('Updated:', e.detail.type, e.detail.id);
});

element.addEventListener('collectionDelete', (e) => {
  console.log('Deleted:', e.detail.type, e.detail.id);
});
```

## Development Workflow

1. **Design Schema**: Define Models, Fields, Views in Saasufy control panel
2. **Deploy Service**: Get WebSocket URL (wss://saasufy.com/YOUR_ID/socketcluster/)
3. **Create HTML**: Build app with Saasufy components
4. **Test Locally**: Serve HTML file (python -m http.server 8000)
5. **Configure Access**: Set field-level permissions in Saasufy control panel
6. **Add Auth**: Implement log-in-form or oauth-link/oauth-handler if needed

## Reference

Full documentation available at: `/home/jon/Work/saasufy-ai-starter/saasufy-components.md`

## Implementation Guidelines

When building Saasufy apps:

1. **Start Simple**: Begin with socket-provider and one component
2. **Required Attributes**: Always provide required attributes (marked in docs)
3. **Service ID**: User must provide their Saasufy service WebSocket URL
4. **View Setup**: Ensure Views are configured in Saasufy control panel
5. **Field Access**: Check field permissions in Saasufy control panel if data won't load
6. **Debug**: Check browser console for Saasufy connection errors
7. **Single HTML File**: Keep apps as single .html files when possible for simplicity
8. **Styling**: Optionally use `<link href="./base-styles.css" rel="stylesheet" />`

## Example: Complete Todo App

```html
<!DOCTYPE html>
<html>
<head>
  <title>Todo App</title>
  <script src="https://saasufy.com/node_modules/saasufy-components/socket.js" type="module" defer></script>
  <script src="https://saasufy.com/node_modules/saasufy-components/collection-viewer.js" type="module" defer></script>
  <script src="https://saasufy.com/node_modules/saasufy-components/collection-adder.js" type="module" defer></script>
  <script src="https://saasufy.com/node_modules/saasufy-components/collection-deleter.js" type="module" defer></script>
  <script src="https://saasufy.com/node_modules/saasufy-components/confirm-modal.js" type="module" defer></script>
  <link href="https://saasufy.com/node_modules/saasufy-components/styles.css" rel="stylesheet" />
</head>
<body>
  <socket-provider url="wss://saasufy.com/YOUR_SERVICE_ID/socketcluster/">
    <h1>My Todos</h1>

    <collection-adder
      collection-type="Todo"
      collection-fields="task,completed:checkbox"
      submit-button-label="Add Todo"
    ></collection-adder>

    <collection-viewer
      collection-type="Todo"
      collection-fields="task,completed"
      collection-view="recentView"
      collection-view-params=""
      collection-page-size="50"
    >
      <template slot="item">
        <div style="display: flex; gap: 10px; padding: 5px;">
          <span>{{Todo.task}}</span>
          <span>{{Todo.completed ? '✓' : '○'}}</span>
          <collection-deleter
            model-id="{{Todo.id}}"
            confirm-message="Delete this todo?"
            onclick="confirmDeleteItem()">✕</collection-deleter>
        </div>
      </template>

      <template slot="no-item">
        <div>No todos yet. Add one above!</div>
      </template>

      <div slot="viewport"></div>

      <confirm-modal slot="modal" title="Confirm" confirm-button-label="Delete"></confirm-modal>
    </collection-viewer>
  </socket-provider>
</body>
</html>
```
