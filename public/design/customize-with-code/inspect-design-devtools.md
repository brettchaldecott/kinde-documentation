
It's helpful to be able to inspect the Kinde widget components and layouts, using DevTools.

This lets you:

- view [settings](/design/customize-with-code/styling-with-css/) applied to a component or layout, especially when they aren’t defined under `:root`.
- get the [style hook](/design/customize-with-code/style-with-style-hooks/) of an element.

You can learn more about DevTools in browser documentation. Here's a few common examples:

- [Apple Safari](https://developer.apple.com/documentation/safari-developer-tools)
- [Google Chrome](https://developer.chrome.com/docs/devtools?hl=en)
- [Microsoft Edge](https://learn.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/landing/)
- [Mozilla Firefox](https://firefox-source-docs.mozilla.org/devtools-user/)

## Step 1: Open DevTools

Most browsers follow a similar approach for opening DevTools. Here’s a quick guide.

**Right-click method:**

1. Right-click anywhere on the page.
2. From the context menu, select **"Inspect"** or **"Inspect Element"**.

**Keyboard shortcut:**

- `Ctrl + Shift + I` (Windows/Linux).
- `Cmd + Option + I` (macOS).

**Menu navigation:**

1. Open your browser’s menu (three dots or lines in the top-right corner).
2. Select **More tools** **>** **Developer Tools/Web Developer Tools**.

In Safari, go to the top menu bar and select **Develop > Show Web Inspector**.

If you don’t see the **Develop** menu item, you’ll need to enable it:

1. In the top menu bar, go to **Safari > Settings**.
2. Open the **Advanced** tab.
3. Check **Show features for web developers**.

## Step 2: Locate in HTML

Once DevTools is open:

1. On the page, right-click the element you want to inspect.
2. From the context menu, select **Inspect** or **Inspect Element**.
3. The element will be highlighted and focused to in the **Elements** (or **Inspector**) tab.
4. All widget components and layouts have style hooks on them: `data-kinde-[…]`.
5. Click the arrow (▸) icon next to the element to expand its child elements.

## Step 3: View settings

Once a component or layout is located in the HTML:

1. On the page, right-click the element you want to inspect.
2. From the context menu, select **Inspect** or **Inspect Element**.
3. In the **Styles** (or **Rules**) panel, the element’s CSS rules will be displayed, revealing the settings applied to it.
4. Look for `--kinde-[…]` instances to identify the settings specific to the component or layout.

   If a `--kinde-[…]` setting is explicitly defined, clicking on it will navigate to its definition at the document root, where its value is set. Most browsers also display the setting’s value in a tooltip when hovered.

   If the setting is not defined and relies on a fallback, it will appear inactive and cannot be clicked.

5. To view state-specific settings:
   1. In the **Elements** (or **Inspector**) tab, right-click the element (some browsers may include a menu button next to the element for this action).
   2. From the context menu, select **"Force state"** (or **“Change Pseudo-class”** or **“Forced Pseudo-Classes,”** depending on the browser).
   3. Choose the desired state from the list.
