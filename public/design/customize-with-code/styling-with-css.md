
Kinde pages and the widget are styled using standard CSS custom properties, collectively managed in 'settings'. Settings provide a simple way for you to customize the end-user experience, as you can override the [CSS custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties) using your own CSS code.

For example, you can:

- Set the page’s base text color with `--kinde-base-color`.
- Set the primary button’s background color with `--kinde-button-primary-background-color`.
- Set the color of a form field’s invalid message with `--kinde-control-associated-text-invalid-message-color` (or use `--kinde-shared-color-invalid` for a more global approach).

## Hosting your CSS and other asset files

We know you will want to use assets like stylesheets, fonts and images. Kinde does not currently offer hosting for these static assets and you will need to host them yourself.

We recommend hosting them on servers that share the same domain as your custom domain. For example, if you are using the custom domain `auth.myapp.com` you could host your external fonts, images and CSS files at `assets.myapp.com` and they would be accessible in your code.

## Define settings in your CSS

Kinde settings are standard CSS custom properties, making them easy to integrate into your custom CSS. You can also map your own design tokens or custom properties to Kinde settings for added consistency.

To define settings in your CSS, scope them to the [`:root` pseudo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/:root):

```css
:root {
  --kinde-base-background-color: hsl(0 100% 50% / 50%);
  --kinde-base-color: hsl(50 80% 40%);
  --kinde-base-font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue",
    Arial, sans-serif;
  --kinde-base-font-weight: 700;
  --kinde-button-primary-background-color: var(--kinde-base-color);
  --kinde-button-primary-color: hsl(0, 0%, 100%);
  --kinde-alert-banner-error-background-color: hsl(0, 100%, 27%);
  --kinde-alert-banner-error-color: hsl(0, 0%, 100%);
}
```

### Apply your styles

To apply custom styles, you need to load your custom CSS **after** the Kinde required CSS in the HTML `<head>`. This ensures your styles will apply on top of the default styles.

### Include the Kinde required CSS

Kinde’s required CSS provides the default styles for the widget and defines all Kinde settings. Without it, the widget will not function correctly.

You can include it using the `getKindeRequiredCSS()` helper method from the `@kinde/infrastructure` package:

```js
import {getKindeRequiredCSS} from "@kinde/infrastructure";
```

Once imported, call this function inside the HTML `<head>` of your page to load the necessary styles.

For example, to load your custom CSS after the Kinde required CSS using an external stylesheet:

```html
<html>
  <head>
    {getKindeRequiredCSS()}
    <link rel="stylesheet" href="/custom_styles.css" />
  </head>
</html>
```

## Available settings

Most styles (individual CSS properties) have a corresponding setting that can be defined or overridden in your custom CSS. For a full list of available settings, see [Types of settings](#types-of-settings).

For example, here’s the CSS for the **Alert banner** component:

```css
.kinde-alert-banner {
  background-color: var(--kinde-alert-banner-error-background-color);
  border: var(--kinde-alert-banner-border-width, 0.0625rem)
    var(--kinde-alert-banner-border-style, solid) var(--kinde-alert-banner-error-border-color);
  border-radius: var(--kinde-alert-banner-border-radius, 0.5rem);
  color: var(--kinde-alert-banner-error-color);
  display: var(--kinde-alert-banner-display, flex);
  gap: var(--kinde-alert-banner-gap, 1rem);
  padding: var(--kinde-alert-banner-padding, 1.5rem);
}
```

### Non-customizable styles

Not all styles can be overridden using settings. Some styles are intentionally hardcoded in Kinde to ensure functionality, usability, and accessibility.

There are two main categories of non-customizable styles:

**Functional styles** - Styles that are necessary for proper functionality and accessibility. These prevent the UI from breaking and ensure a smooth user experience. Examples include:

- Preventing interactions using [`pointer-events`](https://developer.mozilla.org/en-US/docs/Web/CSS/pointer-events), such as disabling an element with `pointer-events: none;`.
- Enforcing correct mouse cursors with [`cursor`](https://developer.mozilla.org/en-US/docs/Web/CSS/cursor), such as `cursor: not-allowed;` for disabled elements.
- Managing visibility with [`display`](https://developer.mozilla.org/en-US/docs/Web/CSS/display) to correctly show or hide elements.
- Applying accessibility-related styles, such as visually hiding elements while keeping them screen-reader accessible or ensuring compliance with user preferences like "reduced motion.”

**Expected styles** – Styles that are widely expected and do not typically require customization. Examples include:

- The **Card** component’s “is-content-center-aligned” modifier applies `text-align: center;` because the modifier specifically exists for centering the content. Allowing other alignments like "left" or "right" would contradict its purpose.
- The **Button** component removes underlines from links (`<a>`) using `text-decoration-line: none;`. This follows best practices, as underlined text generally indicates a standard hyperlink, not a button.

## Settings fallback values

We explicitly define settings in the following cases:

- **Color-related settings** - For managing light and dark mode with dynamic values.
- **Reusability** - When a setting is referenced in multiple places.

For settings that aren’t explicitly defined, styles are applied using [CSS custom property fallback values](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties#custom_property_fallback_values).

For example, the **Button** component’s font size appears as:

```css
font-size: var(--kinde-button-font-size, 1rem);
```

This means:

- If `--kinde-button-font-size` is undefined, the fallback value (`1rem`) will be used.
- If you define `--kinde-button-font-size`, your value will override the fallback.

<Aside>

Settings that use fallback values won’t appear in the document root (`:root`) when inspecting in DevTools. If a setting isn’t visible under `:root`, [inspect the specific element instead](/design/customize-with-code/inspect-design-devtools/).

</Aside>

## Styling via the Kinde designer

You can control page elements and content (including elements of the widget) directly within Kinde, in the Kinde designer. See [Design brand experience defaults](/design/brand/global-brand-defaults/) for more details. Styles set this way act as fallbacks where something is undefined in your code.

You can take a hybrid approach and define styles in Kinde as well as in code-defined settings. In fact, we recommend setting defaults via the Kinde designer, so that fallbacks exist for core elements. For example, in the widget’s CSS, the page background color is defined like this:

```css
background-color: var(--kinde-base-background-color, var(--kinde-designer-base-background-color));
```

This means:

- `--kinde-base-background-color` is not defined by default. Instead, Kinde uses the fallback: `--kinde-designer-base-background-color`, which comes from the Kinde designer.
- To override this style, you can either update it in Kinde designer or define `--kinde-base-background-color` in your custom CSS.

<Aside type="warning">

Do not assign values to Kinde designer settings (`--kinde-designer-[...]`) in your custom CSS. These act as non-consumable fallbacks. Instead, control the style either in Kinde Designer or by using the associated default setting.

</Aside>

## Types of settings

Settings are categorized into distinct types based on their purpose and scope. Here’s an overview of the available types.

### Shared settings: `--kinde-shared-[…]`

These settings are shared across the widget CSS to minimize redundancy. They serve as global fallbacks, applied only when a locally scoped setting is not defined.

| Setting                                                                                                                                                      | Description                                                                                                                                            | Applies to                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| `--kinde-shared-color-disabled-background` / `--kinde-shared-color-disabled-background-dark`                                                                 | Sets the background color for disabled elements                                                                                                        | Button component, Control checkable component, Control select text component                                             |
| `--kinde-shared-color-disabled-text` / `--kinde-shared-color-disabled-text`                                                                                  | Sets the text color for disabled elements                                                                                                              | Button component, Text button component                                                                                  |
| `--kinde-shared-color-invalid` / `--kinde-shared-color-invalid-dark`                                                                                         | Sets the color for invalid elements. Applied to borders or text for elements failing form validation (via `:user-invalid` or `[aria-invalid='true']`). | Composite field component, Control associated text component, Control checkable component, Control select text component |
| `--kinde-shared-color-text-caption` / `--kinde-shared-color-text-caption-dark`                                                                               | Sets the text color for captioned elements, using a lighter shade of body copy                                                                         | Choice separator component, Control associated text component                                                            |
| `--kinde-shared-color-text-label` / `--kinde-shared-color-text-label-dark`                                                                                   | Sets the text color for labeled text elements (e.g., `<label>` or `<dt>`), using a darker shade of body copy                                           | Control label component, Key value pair component                                                                        |
| `--kinde-shared-font-small-size` / `--kinde-shared-font-small-letter-spacing` / `--kinde-shared-font-small-line-height` / `--kinde-shared-font-small-weight` | Sets the font styles for text smaller than the body copy                                                                                               | Base `small` element, Disclaimer component, Control associated text component                                            |

### Base settings: `--kinde-base-[…]`

These settings define foundational styles, such as fonts and colors, that apply globally to the page. They are typically scoped to the `body` element and a few other key global elements.

| Setting                                                                | Description                                                                                                                                                                                                                                                                                                                | Default                                                                                  | Applies to                                |
| ---------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------- |
| `--kinde-base-scroll-behavior`                                         | Sets the page’s scroll behavior using the CSS [`scroll-behavior`](https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-behavior) property.                                                                                                                                                                              | “smooth”                                                                                 | `html` element                            |
| `--kinde-base-block-size`                                              | Sets the page’s width or height using the CSS [`block-size`](https://developer.mozilla.org/en-US/docs/Web/CSS/block-size) property, depending on the page’s [writing mode](https://developer.mozilla.org/en-US/docs/Web/CSS/writing-mode).                                                                                 | `100%`                                                                                   | `html` and `body` elements                |
| `--kinde-base-accent-color` / `--kinde-base-accent-color-dark`         | Sets the accent color for specific form controls using the CSS [`accent-color`](https://developer.mozilla.org/en-US/docs/Web/CSS/accent-color) property. **Note:** Matches the “checked” background color of the Control checkable component                                                                               | Light: `#0f0f0f`                                                                         |
| Dark: `#0054f0`                                                        | `body` element                                                                                                                                                                                                                                                                                                             |
| `--kinde-base-background-color` / `--kinde-base-background-color-dark` | Sets the page’s background color using the CSS [`background-color`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-color) property. **Note:** Falls back to [Kinde's admin brand settings](https://docs.kinde.com/design/pages/design-your-welcome-pages/#set-brand-colors-for-page-elements) if not defined. | Light: `#ffffff`                                                                         |
| Dark: `#0f0f0f`                                                        | `body` element                                                                                                                                                                                                                                                                                                             |
| `--kinde-base-color` / `--kinde-base-color-dark`                       | Sets the page’s text color using the CSS [`color`](https://developer.mozilla.org/en-US/docs/Web/CSS/color) property                                                                                                                                                                                                        | Light: `#2b2b2b`                                                                         |
| Dark: `#dbdbdb`                                                        | `body` element                                                                                                                                                                                                                                                                                                             |
| `--kinde-base-font-family`                                             | Sets the page’s font family using the CSS [`font-family`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family) property                                                                                                                                                                                           | “Inter, 'CustomSystemFontStack’”                                                         | `body` element                            |
| `--kinde-base-font-kerning`                                            | Sets the page’s font kerning using the CSS [`font-kerning`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-kerning) property                                                                                                                                                                                        | “normal”                                                                                 | `body` element                            |
| `--kinde-base-font-size`                                               | Sets the page’s font size using the CSS [`font-size`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size) property. Note: All other rem-based settings are derived from this value.                                                                                                                                | `1rem`                                                                                   | `body` element                            |
| `--kinde-base-moz-osx-font-smoothing`                                  | Sets the page’s font smooth using the non-standard Firefox CSS [`-moz-osx-font-smoothing`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-smooth) property                                                                                                                                                          | “grayscale”                                                                              | `body` element                            |
| `--kinde-base-webkit-font-smoothing`                                   | Sets the page’s font smooth using the non-standard WebKit CSS [`-webkit-font-smoothing`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-smooth) property                                                                                                                                                            | “antialiased”                                                                            | `body` element                            |
| `--kinde-base-font-synthesis`                                          | Sets the page’s font synthesis using the CSS [`font-synthesis`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-synthesis) property                                                                                                                                                                                  | “none”                                                                                   | `body` element                            |
| `--kinde-base-font-variant-ligatures`                                  | Sets the page’s font ligatures and contextual forms using the CSS [`font-variant-ligatures`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-variant-ligatures) property                                                                                                                                             | “common-ligatures contextual”                                                            | `body` element                            |
| `--kinde-base-font-variant-numeric`                                    | Sets the page’s font glyphs for numbers, fractions, and ordinal markers using the CSS [`font-variant-numeric`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-variant-numeric) property                                                                                                                             | “oldstyle-nums proportional-nums”                                                        | `body` element                            |
| `--kinde-base-font-weight`                                             | Sets the page’s font weight using the CSS [`font-weight`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-weight) property                                                                                                                                                                                           | `400`                                                                                    | `body` element                            |
| `--kinde-base-letter-spacing`                                          | Sets the page’s letter spacing using the CSS [`letter-spacing`](https://developer.mozilla.org/en-US/docs/Web/CSS/letter-spacing) property                                                                                                                                                                                  | `-.005em`                                                                                | `body` element                            |
| `--kinde-base-line-height`                                             | Sets the page’s line height using the CSS [`line-height`](https://developer.mozilla.org/en-US/docs/Web/CSS/line-height) property                                                                                                                                                                                           | `1.5`                                                                                    | `body` element                            |
| `--kinde-base-overflow-wrap`                                           | Sets the page’s text overflow using the CSS [`overflow-wrap`](https://developer.mozilla.org/en-US/docs/Web/CSS/overflow-wrap) property                                                                                                                                                                                     | “break-word”                                                                             | `body` element                            |
| `--kinde-base-tab-size`                                                | Sets the page’s tab character width using the CSS [`tab-size`](https://developer.mozilla.org/en-US/docs/Web/CSS/tab-size) property                                                                                                                                                                                         | `4`                                                                                      | `body` element                            |
| `--kinde-base-focus-border-radius`                                     | Sets the border radius of the page’s focus indicator using the CSS [`border-radius`](https://developer.mozilla.org/en-US/docs/Web/CSS/border-radius) property. Note: If the focused element has a custom radius, the focus indicator will match it.                                                                        | `.125rem`                                                                                | `:focus-visible`                          |
| `--kinde-base-focus-outline-width`                                     | Sets the outline width of the page’s focus indicator using the CSS [`outline-width`](https://developer.mozilla.org/en-US/docs/Web/CSS/outline-width) property                                                                                                                                                              | `.125rem`                                                                                | `:focus-visible`                          |
| `--kinde-base-focus-outline-style`                                     | Sets the outline style of the page’s focus indicator using the CSS [`outline-style`](https://developer.mozilla.org/en-US/docs/Web/CSS/outline-style) property                                                                                                                                                              | “solid”                                                                                  | `:focus-visible`                          |
| `--kinde-base-focus-outline-color`                                     | Sets the outline color of the page’s focus indicator using the CSS [`outline-color`](https://developer.mozilla.org/en-US/docs/Web/CSS/outline-color) property                                                                                                                                                              | Light: `#0054f0`                                                                         |
| Dark: `#94b9ff`                                                        | `:focus-visible`                                                                                                                                                                                                                                                                                                           |
| `--kinde-base-focus-outline-offset`                                    | Sets the outline offset of the page’s focus indicator using the CSS [`outline-offset`](https://developer.mozilla.org/en-US/docs/Web/CSS/outline-offset) property                                                                                                                                                           | `.125rem`                                                                                | `:focus-visible`                          |
| `--kinde-base-focus-transition`                                        | Sets the transition of the page’s focus indicator using the CSS [`transition`](https://developer.mozilla.org/en-US/docs/Web/CSS/transition) property.                                                                                                                                                                      | “outline-offset 150ms cubic-bezier(.25, 0, .1, 1)”                                       | `:focus-visible`                          |
| `--kinde-base-font-family-mono`                                        | Sets the font family for code-related elements using the CSS [`font-family`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family) property                                                                                                                                                                        | “ui-monospace, SFMono-Regular, ‘SF Mono’, Consolas, ‘Liberation Mono’, Menlo, monospace” | `code`, `kbd`, `samp`, and `pre` elements |
| `--kinde-base-font-kerning-mono`                                       | Sets the font kerning for code-related elements using the CSS [`font-kerning`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-kerning) property                                                                                                                                                                     | “none”                                                                                   | `code`, `kbd`, `samp`, and `pre` elements |
| `--kinde-base-font-size-mono`                                          | Sets the font size for code-related elements using the CSS [`font-size`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size) property                                                                                                                                                                              | `--kinde-base-font-size`                                                                 | `code`, `kbd`, `samp`, and `pre` elements |
| `--kinde-base-font-variant-ligatures-mono`                             | Sets the font ligatures and contextual forms for code-related elements using the CSS [`font-variant-ligatures`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-variant-ligatures) property                                                                                                                          | “contextual”                                                                             | `code`, `kbd`, `samp`, and `pre` elements |
| `--kinde-base-font-variant-numeric-mono`                               | Sets the font glyphs for numbers, fractions, and ordinal markers for code-related elements using the CSS [`font-variant-numeric`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-variant-numeric) property                                                                                                          | “lining-nums tabular-nums slashed-zero”                                                  | `code`, `kbd`, `samp`, and `pre` elements |
| `--kinde-base-font-weight-mono`                                        | Sets the font weight for code-related elements using the CSS [`font-weight`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-weight) property                                                                                                                                                                        | `500`                                                                                    | `code`, `kbd`, `samp`, and `pre` elements |
| `--kinde-base-letter-spacing-mono`                                     | Sets the letter spacing for code-related elements using the CSS [`letter-spacing`](https://developer.mozilla.org/en-US/docs/Web/CSS/letter-spacing) property                                                                                                                                                               | “normal”                                                                                 | `code`, `kbd`, `samp`, and `pre` elements |
| `--kinde-base-line-height-mono`                                        | Sets the line height for code-related elements using the CSS [`line-height`](https://developer.mozilla.org/en-US/docs/Web/CSS/line-height) property                                                                                                                                                                        | `1.5`                                                                                    | `code`, `kbd`, `samp`, and `pre` elements |
| `--kinde-base-pre-overflow-x`                                          | Sets the `pre` element’s horizontal overflow using the CSS [`overflow-x`](https://developer.mozilla.org/en-US/docs/Web/CSS/overflow-x) property                                                                                                                                                                            | “auto”                                                                                   | `pre` element                             |
| `--kinde-base-strong-font-weight`                                      | Sets the font weight for emphasized text elements using the CSS [`font-weight`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-weight) property                                                                                                                                                                     | `500`                                                                                    | `b` and `strong` elements                 |

### Component settings: `--kinde-[component_name]-[…]`

These settings are specific to self-contained UI patterns within the widget, such as buttons, cards, and form controls. Components are the default unit of styling, so they do not require an explicit `component` type indicator.

Component settings are numerous, as they represent the primary method for styling the widget. To explore them, use DevTools. See [Inspect using DevTools](/design/customize-with-code/inspect-design-devtools/) for instructions.

### Layout settings: `--kinde-layout-[…]`

These settings are specific to layouts within the widget. Unlike components, which define specific UI patterns, layouts determine the arrangement of components—such as spacing between them—to ensure a consistent structure across the widget.

Like component settings, layout settings are numerous. To explore them, use DevTools. See [Inspect using DevTools](/design/customize-with-code/inspect-design-devtools/) for instructions.

## Naming structures

Settings follow a consistent naming structure to make them easy to identify and understand.

Most settings are structured as follows.

This sets the background color of the page using the CSS `background-color` property:

```
--kinde-base-background-color
  |     |    |
  |     |    └── Property (background-color)
  |     └── Type (base)
  └── Prefix (kinde)
```

Here’s a more complex setting. This sets the background color in dark mode for the page using the CSS `background-color` property.

```
--kinde-base-background-color-dark
  |     |    |                |
  |     |    |                └── Dark mode suffix
  |     |    └── Property (background-color)
  |     └── Type (base)
  └── Prefix (kinde)
```

This sets the border width for the “primary” variant of the **Button** component using the CSS `border-width` property.

```
--kinde-button-primary-border-width
  |     |      |       |
  |     |      |       └── Property (border-width)
  |     |      └── Variant (primary)
  |     └── Type (component)
  └── Prefix (kinde)
```

This sets the text-decoration-line style (underline, overline, etc.) of the **Text link** component when hovered using the CSS `text-decoration-line` property.

```
--kinde-text-link-text-decoration-line-is-inline-hover
  |     |         |                    |         |
  |     |         |                    |         └── State (hover)
  |     |         |                    └── Modifier (is-inline)
  |     |         └── Property (text-decoration-line)
  |     └── Type (component)
  └── Prefix (kinde)
```

### Prefixes

All settings start with the `--kinde` prefix to ensure uniqueness and prevent conflicts with custom-defined CSS.

### Type

The second part indicates the [type of setting](#types-of-settings).

### [Variant/Element] / Property / Modifier / State

Each setting includes a combination of **[variant/element]**, **property**, **modifier**, and **state**, depending on the level of specificity required. Every setting always includes a **property**.

#### [Variant/Element]

**Variant** - Certain components, such as the **Button** component, have predefined variants. Variants define distinct styles or versions of a component. For example, the background colors for the **Button** component’s variants are defined as: - **Primary variant:** `--kinde-button-primary-background-color` - **Secondary variant:** `--kinde-button-secondary-background-color` - **Uncontained variant:** `--kinde-button-uncontained-background-color`
**Element** - For components with child elements, element-specific identifiers are used to distinguish settings that apply to the component's root element versus its child elements. For example, here are some settings for the **Key value pair** component’s "key" element (the child `<dt>` element): - `--kinde-key-value-pair-key-color` - `--kinde-key-value-pair-key-font-weight`

    Some identifiers can be more generic. For example, list structures (e.g., `<li>` elements) use the generic **"item"** identifier:

    - `--kinde-key-value-pair-item-margin-block-end`

    Another common generic identifier is **"child"**, representing unnamed child elements typically targeted with the [universal selector (`*`) CSS selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Universal_selectors):

    - `--kinde-fallback-action-child-display`

<Aside>

"Variant" and "Element" can coexist when a component has settings for a child element that are scoped to a specific variant. The variant identifier always comes before the element identifier.

For example, the setting that controls the size of the **Composite field** component’s nested select element when in the "phone-number" variant is defined as: `--kinde-composite-field-phone-number-control-select-inline-size`

</Aside>

#### Property

Each setting corresponds to a standard CSS property. For example, settings related to text color, such as `--kinde-base-color`, match the associated `color` property. This makes the settings intuitive and easy to understand.

<Aside>

An exception to this applies to settings related to element spacing. Instead of using specific CSS properties (like `margin-block-end`), a generic “spacing” term is used. This helps identify the common concept of spacing across multiple properties (e.g., `margin`, `gap`).

Spacing refers to the whitespace **external** to an element, while **internal** spacing is defined with the `padding` property.

For example:

- `--kinde-control-label-spacing` defines spacing after the **Control label** component.

More specific variants are:

- **“spacing-content”** – Defines spacing between child elements, or the “content” (e.g., `--kinde-card-spacing-content`).
- **“spacing-[element_name]”** - Defines spacing for a specific child element (e.g., `--kinde-layout-widget-spacing-footer`).

</Aside>

#### Modifier

Sometimes, styles include modifiers, which are small adjustments to the default style. For example, the **Button** component includes the “is-content-width” modifier: `--kinde-button-inline-size-is-content-width`, which adjusts the button's width to match its content, as opposed to using the default width based on its container.

#### State

For components with interactive elements, such as buttons, links, and form controls, state-specific settings apply when the element is in a particular state. For example, the states for the **Text link** component’s text color are defined as:

- **Active state:** `-kinde-text-link-color-active`
- **Focus state:** `-kinde-text-link-color-focus`
- **Hover state:** `-kinde-text-link-color-hover`
- **Visited state:** `-kinde-text-link-color-visited`

States are defined using [CSS pseudo-classes](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes). Common states include:

- [**Active** (`:active`)](https://developer.mozilla.org/en-US/docs/Web/CSS/:active) – Styles applied when the element is being activated, such as when a user clicks, taps, or presses a key that triggers the element.
- [**Focus** (`:focus-visible`)](https://developer.mozilla.org/en-US/docs/Web/CSS/:focus-visible) – Styles applied when the element receives keyboard or programmatic focus.

    <Aside>

  **Kinde uses `:focus-visible` exclusively instead of `:focus` to provide a better user experience.**

  `:focus-visible` ensures that focus indicators appear only when necessary—typically during keyboard or other non-mouse interactions. In contrast, `:focus` applies whenever an element gains focus, including mouse clicks and taps, which can lead to visual confusion and make the UI harder to interpret.

    </Aside>

- [**Hover** (`:hover`)](https://developer.mozilla.org/en-US/docs/Web/CSS/:hover) – Styles applied when the element is hovered by a mouse pointer.

### Dark mode suffix

Settings for defining the widget’s dark mode colors include the “dark” term appended to the end of the setting. For example, the **Alert banner** component’s dark mode settings are defined as:

- **Background color:** `--kinde-alert-banner-error-background-color-dark`
- **Border color:** `--kinde-alert-banner-error-border-color-dark`
- **Text color:** `-kinde-alert-banner-error-color-dark`

<Aside>

Light and dark themes are managed in Kinde. See [Design brand experience defaults](/design/brand/global-brand-defaults/) for more details.

For theme styling to work in your repo, see HTML data attributes [(Style hooks) > Apply theme styles](/design/customize-with-code/style-with-style-hooks/).

</Aside>

### Shared settings

Shared settings use a unique naming structure because they apply to broader styling aspects rather than specific components or elements. Unlike other settings, they do not follow the **[Variant/Element] / Property / Modifier / State** structure.

Instead, these settings are categorized by general UI styling aspects. Currently, the main categories are:

- **“Color”:** `--kinde-shared-color-[…]`
- **“Font”:** `--kinde-shared-font-[…]`

After the category, the remaining structure varies depending on the styling need. For example, state-based color settings follow this format:

- `--kinde-shared-color-[state]-[element_part]`

**Examples:**

- `--kinde-shared-color-disabled-background` – Sets the background color for disabled elements.
- `--kinde-shared-color-disabled-text` – Sets the text color for disabled elements.
- `--kinde-shared-color-invalid` – Sets the color of all parts of an element affected by an “invalid” state, such as background, border, or text.
