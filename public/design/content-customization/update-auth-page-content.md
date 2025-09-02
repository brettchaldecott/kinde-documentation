
For maximum extensibility, page content has been decoupled from the page itself. 

This lets you update the copy on most screens, which you can do via the **Page content** section of the admin area (see below) or via API (coming soon).

### Update page content

If your application is built for [multiple languages](https://docs.kinde.com/design/pages/set-language-for-pages/), select these first in Kinde. 

<Aside type="warning">

Make sure you hit save each time you finish updating a language or page. If you switch between without saving, you may lose changes. 

</Aside>

1. In Kinde, go to **Design > Page content**.
2. Select the language you want (only applicable if multiple languages are configured). 
3. Select the page you want to edit. The list of content shown corresponds with page elements such as headings, field labels, helper text, error messages, etc.
4. Make the changes to the content. If you want, you can use text variables, explained below.
5. When you’ve finished, select **Save**. The save only applies to the current language and page selected. You need to select **Save** again if you edit in different languages.
6. Check changes by selecting **Preview**. You'll only see an accurate preview if the environment is fully connected, otherwise you may see errors.
7. Repeat from step 2 for each language and page you want to edit.

### Text variables for page content

Variables are used to stand in for actual values that you want to appear on pages. They are a way of automating content. For example, if you use the `${email_address}` variable, the user’s email address will be shown.

Variables can be used on pages as follows. 

**Sign in confirm code page**

`${email_address}` - shows the full email address of the user

`${email_address_obfuscated}` - shows only a part of the user's email

**Sign up confirm code page**

`${email_address}`

### Include ‘escape route’ URL in auth errors

When auth errors appear, you want to give users a way to navigate out of them. To provide an ‘escape route’ URL in these situations:

1. In Kinde, go to **Settings > Applications** and select **View details** on your application.
2. Enter your website URL into the **Application homepage URI** and **Application login URI** fields.
3. Select **Save**.
