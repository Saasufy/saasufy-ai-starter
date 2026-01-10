---
name: saasufy-components-authentication
description: Helps you use Saasufy components related to authentication such as the log-in-form, log-out, oauth-link and oauth-handler components.
---

## Components

### log-in-form

A form component which allows end-users to authenticate themselves into your app using the Saasufy blockchain or any Capitalisk-based blockchain.
On success, the socket of the form's `socket-provider` will be set to the authenticated state; this will then cause appropriate Saasufy components which share this `socket-provider` to reload themselves.
Once a user is authenticated, they will be able to access restricted data (as specified on the Saasufy `Model -> Access` page).
It's also possible to specify a `success-location-hash` attribute to trigger a client-side redirect upon successful authentication.
The change in the location hash can then be detected by an `app-router` to switch to a different page upon successful login.

**Import**

```html
<script src="https://saasufy.com/node_modules/saasufy-components/log-in-form.js" type="module" defer></script>
```

**Example usage**

```html
<log-in-form
  hostname="capitalisk.com"
  port="443"
  network-symbol="clsk"
  chain-module-name="capitalisk_chain"
  secure="true"
></log-in-form>
```

**Attributes**

- `hostname` (required): The hostname of the blockchain node to match authentication details against.
- `port` (required): The port of the blockchain node to match authentication details against. Should be 80 for HTTP/WS or 443 for HTTPS/WSS.
- `network-symbol` (required): The symbol of the target blockchain.
- `chain-module-name` (required): The module name used by the target blockchain.
- `secure` (required): Should be `true` or `false` depending on whether or not the node is exposed over HTTP/WS or HTTPS/WSS.
- `auth-timeout`: The maximum number of milliseconds to wait for authentication to complete. Defaults to 10000 (10 seconds).
- `success-location-hash`: If authentication is successful, the browser's `location.hash` will be set to this value. It can be used to redirect the user to their dashboard, for example.
- `greedy-refresh`: If this attribute exists, the component will refresh itself whenever an attribute is set, even if that attribute's value did not change.

### log-out

A component which can be placed inside a `socket-provider` to deauthenticate the socket (e.g. on click).

**Import**

```html
<script src="https://saasufy.com/node_modules/saasufy-components/log-out.js" type="module" defer></script>
```

**Example usage**

```html
<log-out onclick="logOut()"><a href="javascript:void(0)">Log out</a></log-out>
```

### oauth-link

A link to initiate an OAuth flow to authenticate a user as part of log in. Can support a range of different OAuth providers.

**Import**

```html
<script src="https://saasufy.com/node_modules/saasufy-components/oauth-link.js" type="module" defer></script>
```

**Example usage**

```html
<oauth-link class="log-in-oauth-section" provider="github" client-id="dabb51f8af31a0bc53bf">
  <template slot="item">
    <a href="https://github.com/login/oauth/authorize?client_id={{oauth.clientId}}&state={{oauth.state}}">
      Log in with GitHub
    </a>
  </template>
  <div slot="viewport"></div>
</oauth-link>
```

**Attributes**

- `provider` (required): The name of the provider. It must match the provider name specified on the `Authentication` page of your Saasufy control panel and also the value provided to the `oauth-handler` component at the end of the OAuth flow.
- `client-id` (required): This is the client ID from the third-party OAuth provider.
- `state-size`: This allows you to control the size (in bytes) of the random state string which is passed to the OAuth provider as part of the OAuth flow. Defaults to 20.
- `state-storage-key`: The state string is stored in the browser's `sessionStorage` under this key. Defaults to `oauth.state`. It must match the value provided to the `oauth-handler` component at the end of the OAuth flow.
- `use-local-storage`: By default, the state is stored inside sessionStorage. If this property is set, then the state will be stored inside localStorage instead. This is useful for sharing the state across different tabs. If set, this attribute should also be set on the related `oauth-handler` component.

### oauth-handler

This component handles the final stage of OAuth and then redirects the user to the relevant page/URL if successful.
When using an OAuth provider, the callback URL which you register with the provider must lead the user back to a page which contains this `oauth-handler` component.

**Import**

```html
<script src="https://saasufy.com/node_modules/saasufy-components/oauth-handler.js" type="module" defer></script>
```

**Example usage**

```html
<oauth-handler provider="github" success-location-hash="/chat"></oauth-handler>
```

**Attributes**

- `provider` (required): The name of the provider. It must match the provider name specified on the `Authentication` page of your Saasufy control panel and also the value provided to the `oauth-link` component at the start of the OAuth flow.
- `success-location-hash`: If authentication is successful, the browser's `location.hash` will be set to this value. It can be used to redirect the user to their dashboard, for example.
- `success-url`: If authentication is successful, the browser's `location.href` will be set to this value. It can be used to redirect the user to their dashboard, for example.
- `extra-data`: This attribute can be used to pass additional data to the OAuth `getAccessTokenURL` endpoint. For Google OAuth, for example, an additional `redirect_uri` field is required, so it should be set using `extra-data="redirect_uri=http://localhost.com:8000/"` (substitute the URI with your own).
- `navigate-event-path`: If authentication is successful and a path is set via this attribute, the component will emit a `navigate` event which will bubble up the component hierarchy and can be used by a parent component to perform the success redirection. This approach can be used as an alternative to `success-location-hash` or `success-url` for doing the final redirect.
- `state-storage-key`: The state string is stored in the browser's `sessionStorage` under this key. Defaults to `oauth.state`. It must match the value provided to the `oauth-link` component at the start of the OAuth flow.
- `code-param-name`: The name of the query parameter which holds the `code` as provided by the OAuth provider within the OAuth callback URL. Defaults to `code`.
- `state-param-name`: The name of the query parameter which holds the `state` as provided by the OAuth provider within the OAuth callback URL. Defaults to `state`.
- `auth-timeout`: The number of milliseconds to wait for authentication to complete before timing out. Defaults to 10000 (10 seconds).
- `use-local-storage`: By default, the state is retrieved from sessionStorage. If this property is set, then the state will be retrieved from localStorage instead. This is useful for sharing the state across different tabs. If set, this attribute should also be set on the related `oauth-link` component.
