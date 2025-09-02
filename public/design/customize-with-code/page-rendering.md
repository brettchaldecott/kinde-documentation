
Part of page rendering is understanding how Kinde treats the code you add.

By default Kinde will swap out the content of the `<body>` tag in a single DOM replacement when navigating between pages. This provides single-page-application (SPA) behavior, with all the performance and security benefits of a server-rendered page.

We use the `<body>` element as we don't know the structure of your code. This does mean there is a risk of collisions with scripts that do something with the `body` (e.g. Google Font Loader or third-party browser extensions) which can produce very weird and hard to debug errors in production.

We recommend that you designate a specific entry point for Kinde by placing the `data-kinde-root` attribute on a wrapper element.

This has the added advantage of minimizing the amount of the UI being swapped out, which means less network requests and a better end user experience.

Here's an example:

```jsx
<header>
   <Logo>
</header>

// our header and logo won't change so exclude from re-rendering
<main data-kinde-root="true">
		<h1>{heading}</h1>
		<p>{description}</p>
		{getKindeWidget()}
</main>

// our footer won't change so exclude from re-rendering
<footer>
	// some content here
</footer>
```

This means that only critical page items are being re-rendered.
