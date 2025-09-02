
Style hooks are [HTML data attributes](https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Solve_HTML_problems/Use_data_attributes) that can be targeted using the [CSS attribute selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors), allowing you to apply styles directly to the Kinde widget in your custom CSS.

<Aside>

Style hooks only apply to styles in the Kinde widget, and not the full page design.

</Aside>

They give you full control over styling widget components and layouts. Unlike CSS classes, which are used internally, style hooks are dedicated to external (custom) styling. This separation ensures your custom styles remain stable, even if internal styles change.

Style hooks are useful when a setting doesn’t exist for a specific style, such as adding a shadow effect to the **Button** component.

For example:

```css
[data-kinde-button] {
  box-shadow: 0 1px 3px rgba(12, 12, 12, 0.09);
}
```

## Define style hooks in your CSS

Style hooks allow you to apply styles directly in your custom CSS.

For example, add a shadow effect to the **Button** component when hovered:

```css
[data-kinde-button]:hover {
  box-shadow: 0 1px 3px rgba(12, 12, 12, 0.09);
}
```

<Aside>

For your custom styles to apply correctly, make sure they load **after** the Kinde-required CSS. See [Including the Kinde required CSS](/design/customize-with-code/styling-with-css/).

</Aside>

## Available style hooks

Nearly all elements in the widget have a style hook applied, allowing you to style it as needed. The best way to see which hooks are available is by using [DevTools](/design/customize-with-code/inspect-design-devtools/).

Here’s an example of a form field with its style hooks:

```html
<div
  class="kinde-form-field kinde-form-field-variant-select-text
  data-kinde-form-field="true
  data-kinde-form-field-variant="select-text
>
  <label class="kinde-control-label" data-kinde-control-label="true" for="credentials_email">
    Email
  </label>
  <input
    class="kinde-control-select-text
    data-kinde-control-select-text="true
    data-kinde-control-select-text-variant="text
    id="credentials_email
    name="p_email
    required="
    kui-input-persist="true
    autocapitalize="off
    autocomplete="email
    spellcheck="false
    type="email
    inputmode="email
  />
</div>
```

And here’s the HTML showing only the style hooks:

```html
<div data-kinde-form-field="true" data-kinde-form-field-variant="select-text">
  <label data-kinde-control-label="true"> Email </label>
  <input data-kinde-control-select-text="true" data-kinde-control-select-text-variant="text" />
</div>
```

## When style hooks aren’t available

In rare cases, an element may not have a style hook. This doesn’t mean it can’t be styled—just that a hook isn’t needed.

In these cases, it's safe to use an attribute selector or an element (type) selector, but never a class selector, because these attributes and elements are standardized in HTML and won't change.

For example, if you wanted to style a specific text input type, like “email”, “url”, “password”, etc, which belongs to the **Control select text** component, you can safely do that by targeting the “type” attribute:

```css
[data-kinde-control-select-text][type="email"] {
  /* Your styles */
}
```

Or if you wanted to target a select list’s `<option>` element, you can safely do that via a type selector:

```css
[data-kinde-control-select-text] option {
  /* Your styles */
}
```

## Apply theme styles

<Aside>

Light and dark themes are managed in Kinde. See [Design brand experience defaults](/design/brand/global-brand-defaults/) for more details.

</Aside>

Theme styles can be applied using the theme hook (`data-kinde-theme`), an HTML data attribute assigned to the `<html>` element.

To include it, use the `getKindeThemeCode()` helper from the `@kinde/infrastructure` package:

```js
import {getKindeThemeCode} from "@kinde/infrastructure";
```

Then, apply it inside the `<html>` tag:

```html
<html data-kinde-theme="{getKindeThemeCode()}"></html>
```

The theme hook has three possible values based on the applied theme in Kinde:

- “light” (default)
- “dark”
- “user-preference”

Since “light” is the default theme, no extra setup is required—styles can be assigned to style hooks as usual.

### Apply dark mode styles

To apply dark mode styles to a component or layout, use the `[data-kinde-theme='dark']` selector.

Example: Dark mode styles for the **Card** component

```css
[data-kinde-theme="dark"] [data-kinde-card] {
  /* Your styles */
}
```

### Apply user preference styles

The **user-preference** theme allows users to inherit their system theme settings. To apply styles only when the system is set to dark mode, use the [`prefers-color-scheme: dark`](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme) media query inside the theme selector.

Example: User preference styles for the **Card** component

```css
[data-kinde-theme="user-preference"] [data-kinde-card] {
  @media screen and (prefers-color-scheme: dark) {
    /* Your styles */
  }
}
```

### Combine dark mode and user preference

When supporting both dark mode and user preference, repeat the dark mode styles inside the `@media` query:

```css
[data-kinde-theme="dark"] [data-kinde-card] {
  /* Your styles */
}

[data-kinde-theme="user-preference"] [data-kinde-card] {
  @media screen and (prefers-color-scheme: dark) {
    /* Your styles */
  }
}
```

## Style hook naming structure

Style hooks, like settings, follow a consistent naming structure to make them easy to identify and understand.

### Root element

A component or layout’s root element follows this structure, as shown in the **Text link** component:

```
data-kinde-text-link
|    |     |
|    |     └── Component/layout name (text link)
|    └── Prefix (kinde)
└── HTML-defined prefix (data)
```

Example HTML:

```html
<a class="kinde-text-link" data-kinde-text-link="true" href="[url]"> Sign in </a>
```

Since components are the default unit of styling, layouts include a `layout` identifier in their style hook to distinguish them from components:

```
data-kinde-layout-button-group
|    |     |      |
|    |     |      └── Component/layout name (button group)
|    |     └── Layout identifier
|    └── Prefix (kinde)
└── HTML-defined prefix (data)
```

### Variant

Component or layout variants follow this structure, as shown in the **Button** component:

```
data-kinde-button-variant="primary
|    |     |      |
|    |     |      └── Variant (primary)
|    |     └── Component/layout name (button)
|    └── Prefix (kinde)
└── HTML-defined prefix (data)
```

Example HTML:

```html
<button
  class="kinde-button kinde-button-variant-primary
  data-kinde-button="true
  data-kinde-button-variant="primary
  type="submit
>
  <span class="kinde-button-text" data-kinde-button-text="true"> Save </span>
</button>
```

### Modifier

Component or layout modifiers follow this structure, as shown in the **Text link** component:

```
data-kinde-text-link-is-external
|    |     |         |
|    |     |         └── Modifier (is-external)
|    |     └── Component/layout name (text link)
|    └── Prefix (kinde)
└── HTML-defined prefix (data)
```

Example HTML:

```html
<a
  class="kinde-text-link
  data-kinde-text-link="true
  data-kinde-text-link-is-external="true
  href="[url]
  target="_blank
  rel="noopener noreferrer
>
  Terms of use
</a>
```

A component or layout can have multiple modifiers:

```html
<a
  class="kinde-text-link kinde-text-link-is-inline
  data-kinde-text-link="true
  data-kinde-text-link-is-external="true
  data-kinde-text-link-is-inline="true
  href="[url]
  target="_blank
  rel="noopener noreferrer
>
  Terms of use
</a>
```

### Child elements

Child elements of components or layouts follow this structure, as shown in the **Recovery codes** component:

```
data-kinde-recovery-codes-code
|    |     |              |
|    |     |              └── Element name (code)
|    |     └── Component/layout name (recovery codes)
|    └── Prefix (kinde)
└── HTML-defined prefix (data)
```

Example HTML:

```html
<ul class="kinde-recovery-codes" data-kinde-recovery-codes="true">
  <li data-kinde-recovery-codes-item="true">
    <code data-kinde-recovery-codes-code="true">4EA4356E3</code>
  </li>
  <li data-kinde-recovery-codes-item="true">
    <code data-kinde-recovery-codes-code="true">954602631</code>
  </li>
  <li data-kinde-recovery-codes-item="true">
    <code data-kinde-recovery-codes-code="true">A2C3C6D43</code>
  </li>
  <!-- [More codes…] -->
</ul>
```

List structures (e.g., `<li>` elements) use the generic `item` identifier for consistency.
