
By default, all Kinde user-facing screens (for example the sign up and sign in screens) are in US English. However, you can make your end-user experience more international by selecting multiple languages for these screens as well.

## What gets translated?

Only the user facing pages are translated. This includes, sign up, sign in, verification code, confirmations, one-time code emails. It is not yet possible to select a different language for the whole Kinde platform.

To change the language displayed on sign up and request access pages, you need to [customize the relevant page](/design/brand/global-brand-defaults/).

## Supported languages

Kinde’s language support offering is growing all the time.

Here’s just a sample of what we have so far: English (en / en-US / en-AU / en-GB), Dutch, German, Polish, Hebrew, Italian, French, Malay, Spanish, Russian, Portuguese (Brazilian), Norwegian, Swedish.

We aim to eventually support [all these languages](https://github.com/kinde-oss/kinde-translations#language-codes).

Feel free to submit a pull request via [GitHub](https://github.com/kinde-oss/kinde-translations) to request or contribute a specific language translation.

## Choose languages

Kinde will display the relevant language on authentication screens, when the user’s regional settings are detected.

1. Go to **Settings > Languages**.
2. In the **Supported languages** section, select the checkboxes of all the languages you want to support.

Includes support for right to left (RTL) languages.

## Change the default language

The default language is the fallback language if a user’s region is not detected, or if the detected language is not selected as supported in Kinde.

1. Go to **Settings > Languages**.
2. In the **Default language** dropdown, choose the language you would like to display as standard.

## How Kinde displays languages

For user-facing authentication screens, Kinde gives display priority using language detection, as follows:

1. The language supplied in the auth url ^
2. The preferred language selected in the user’s browser ^
3. The default language you have selected in Kinde
4. US English as a last resort, if a translation is not available

^ If you have chosen not to support a language, we will try the next available option.

Through language matching, Kinde automatically works out the closest base language to the requested language and will use this if supported by your app.

## Translation of content in the Kinde widget

Translations inside of the Kinde widget are taken care of by Kinde. We will grab the copy from the relevant route and language from your [page content settings](/design/content-customization/update-auth-page-content/) and apply them to the widget. 

## Customize text on authentication pages

You can customize the text on various pages in the auth flow. See [Update page content for the auth flow](/design/content-customization/update-auth-page-content/) for more information.

Kinde provides relevant localized content within the `context` object that is passed into the Page default function as mentioned above. Localizations and placeholder replacements are handled for you at run time. 

For example:

```jsx
const {content} = event.context.widget;

<img src={<img_url>} alt={content.logoAlt} />

<title>{content.pageTitle}</title>

<h1>{content.heading}</h1>

<p>{content.description}</p>
```
