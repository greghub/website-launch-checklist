# Website Launch Checklist
Things to double-check before launching a website

Before launching the website ensure that:

- [All images have "Alt" tags](#all-images-have-alt-tags)
- [Website is allowed for indexing](#website-is-allowed-for-indexing)
- [Social Media meta tags are present](#social-media-meta-tags-are-present)
- [Favicon is present](#favicon-is-present)
- [Progressive Web App meta tags are present](#progressive-web-app-meta-tags-are-present)
- [Analytics tool is tracking only in the production environment](#analytics-tool-is-tracking-only-in-the-production-environment)
- [Document has title and meta description](#document-has-title-and-meta-description)
- [External links have rel="noopener"](#external-links-have-relnoopener)
- [Remove unused CSS](#remove-unused-css)
- [Efficient cache policy and compression is used on static assets](efficient-cache-policy-and-compression-is-used-on-static-assets)

## All images have "Alt" tags
An alt tag ("alt attribute" or "alt description") is an attribute applied to an image which serves as an invisible description of the image. 

It describes the contents of an image and used by screen readers to read aloud to blind users.
It is also used by search engines since they cannot interpret images, they rely on the alt tag description. Using alt tags on images positively impacts search engine ranking, thus it is good for SEO.
Alt text is also displayed when the images fail to load.

Alt tag has to describe the image contents and it is recommended to be up to 125 characters long.

Example:
```html
<img src="paul_mccartney.jpg" alt="Paul McCartney performing in the East Room of the White House">
```

## Website is allowed for indexing

If you have a "staging" or "dev" environment then chances are you are blocking staging from indexing. 

If you want your website to appear in search results, that search engine shall "index" your website. Search engines have automated "bots" that visit webpages, "crawl" the content and store it the search engine's index. That will allow the search engine to later retrieve most relevant search results.

Before launching make sure that the version that will go live allows indexing of your website. Ensure you have no such meta tag in your HTML:

```html
<meta name="robots" content="noindex, nofollow"> <!-- make sure you remove this if you want your website to be indexed by search engines
```

Also, check your robots.txt file. If you want to *allow* all pages of your website to be indexed, then your robots.txt shall contain this:

```
User-agent: *
Disallow:
```

One more place to check is Apache/Nginx configurations.

## Social Media meta tags are present

When a website is shared to Facebook or Twitter it displays a thumbnail, a title, and a description:

![Twitter Meta Tag](./images/twitter-meta-tags.png)

If you want your website to be displayed with a correct thumbnail, title and description you have to add meta tags required by Facebook and Twitter.

```html
<meta property="og:title" content="Title">
<meta property="og:description" content="Description">
<meta property="og:image" content="Image URL">
<meta property="og:url" content="URL of the page/article/post">

<meta name="twitter:title" content="Title">
<meta name="twitter:description" content="Description">
<meta name="twitter:image" content="Image URL">
<meta name="twitter:card" content="summary_large_image"> <!-- how the card is displayed -->
```

You can test how your website will look when shared to Facebook or Twitter using these tools:

[Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/sharing/)

[Twitter Card Validator](https://cards-dev.twitter.com/validator)

## Favicon is present

Favicon is the small icon near the website title in the browser tab.

![Favicon Example](./images/twitter-favicon.png)

It makes the website easy to identify when many tabs are open, viewing browser history and bookmarks. Some search engines, such as DuckDuckGo displays the favicon near the URL in the search result. Apart from improving the usability, it can help catch user's attention in the search results, so we can call it an indirect SEO technique.

```html
<link rel="shortcut icon" type="image/png" href="/favicon.png">
```

## Progressive Web App meta tags are present

Users can save a webpage to their home screen on mobile devices. That creates an icon for the website, just like the app icon, tapping that icon will open the website in the browser.

Developers are given some control to make the website experience closer to a native app experience on mobile. For example, by default, iOS will set the screenshot of the website as an icon. But you can set a custom icon design using the Apple meta tags.

```html
<link rel="apple-touch-icon" href="touch-icon-iphone.png">
<link rel="apple-touch-icon" sizes="152x152" href="touch-icon-ipad.png">
<link rel="apple-touch-icon" sizes="180x180" href="touch-icon-iphone-retina.png">
<link rel="apple-touch-icon" sizes="167x167" href="touch-icon-ipad-retina.png">
<link rel="apple-touch-startup-image" href="launch.png"> <!-- Splash screen image (image that is displayed when the website is buing opened) -->
<meta name="apple-mobile-web-app-status-bar-style" content="default"> <!-- Status bar style -->
<meta name="apple-mobile-web-app-title" content="My Website"> <!-- title of the website --> 
```

Android will use the "apple-touch-icon" value or the favicon (if the meta tag is not present) to create a home screen icon.

## Analytics tool is tracking only in the production environment

If the analytics tool you use does not have a filter for the environment, then you will be polluting the analytics of your website with results from testing in a non-production environment. You can add the filter in the analytics tool or conditionally embed the code only on the production environment.

## Document has title and meta description

Title tag specifies the title of the website. The meta description tag contains a short description of the page

```html
<title>Page Title</title>
<meta name="description" content="A short description of what this page is about">
```

The title is what displayed in the search engine results, displayed in the browser tab and the social network cards when sharing (if a separate title is not provided for the social network). 

The description is displayed in the search results as well. It is not directly used in the ranking algorithm, so it doesn't affect chances of appearing in the search result, but it makes it more likely that users will click on your website in the results. That will increase the click-through-rate (CTR) of your page for Google, that means that Google will consider it a good result and will rank higher in future search results.

## External links have rel="noopener"

If you have links to external links on your webpage, especially the ones that open in a new tab or window, you should use `rel="noopener"`.

```html
<a href="http://example.com" target="_blank" rel="noopener">Some other site</a>
```

There are security and performance benefits in doing so. Without it, the external page can access your window object using `window.opener`. You can learn more about what vulnerabilities are solved by using the `noopener` here: [About rel=noopener](https://mathiasbynens.github.io/rel-noopener/)

The other website may also run on the same process as your current page, so it the external page is running some heavy JavaScript it will negatively affect your website performance. `noopener` prevents that as well. 

Read more about it here: [The performance benefits of rel=noopener](https://jakearchibald.com/2016/performance-benefits-of-rel-noopener/)

## Remove unused CSS

As you make a lot of changes to your website or when you use a lot of external libraries or CSS frameworks then chances are that your CSS file contains a lot of styles that are not used by your pages. For example, you may be using one theme of some plugin, but the CSS for the other themes just stays in your CSS file unused and the stylesheet file size grows.

You can remove all the unused styles using a tool called [PurgeCSS](https://github.com/FullHuman/purgecss). You can use it with CLI or use it in Webpack, Gulp, etc. It will analyze your pages, match the selectors used on pages with the ones in the CSS files and remove the unused styles. On a recent project of mine, that used Tailwind CSS framework, there were, of course, a lot of utilities that I didn't use. PurgeCSS decreased the size of my app.css file from 214KiB to 45.6Kib.

Be careful though, that if a plugin dynamically creates elements on the page, PurgeCSS will not detect the styles of that element. But you can whitelist selectors by passing the selectors or selector patterns in the config, and you can also whitelist selector by using a special comment to whitelist specific rules. Read more about [PurgeCSS Whitelisting](https://www.purgecss.com/whitelisting).

## Efficient cache policy and compression is used on static assets

When the browser requests a resource, the server can instruct the browser for how long it can store, or "cache", the resource. The next time the resource is required it can use the local copy. It will greatly improve speed and reduce loads on the server. You can configure your server to return a header, telling how long the asset shall be cached:

```
Cache-Control: max-age=31557600 // 31557600 seconds is 356 days
```

You shall set the max-age based on how often your assets change.

You can also configure your server to use compression, such as Gzip compression, which will let the resources transferred faster. Compressing CSS files with gzip saves around 50-70% of the file size. 