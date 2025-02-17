# Installation

You can use Shoelace via CDN or by installing it locally. You can also [cherry pick](#cherry-picking) individual components for faster load times.

If you're using a framework, make sure to check out the pages for [React](/frameworks/react), [Vue](/frameworks/vue), and [Angular](/frameworks/angular).

## CDN Installation (Easiest)

The easiest way to install Shoelace is with the CDN. Just add the following tags to your page to get all components and the default light theme.

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@shoelace-style/shoelace@%VERSION%/dist/themes/light.css">
<script type="module" src="https://cdn.jsdelivr.net/npm/@shoelace-style/shoelace@%VERSION%/dist/shoelace.js"></script>
```

?> If you're only using a handful of components, it will be more efficient to [cherry pick](#cherry-picking) the ones you need.

### Dark Theme

If you prefer to use the dark theme instead, use this. Note the `sl-theme-dark` class on the `<html>` element. [Learn more about the Dark Theme.](/getting-started/themes#dark-theme)

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@shoelace-style/shoelace@%VERSION%/dist/themes/dark.css">
<script type="module" src="https://cdn.jsdelivr.net/npm/@shoelace-style/shoelace@%VERSION%/dist/shoelace.js"></script>
```

### Light & Dark Theme

If you want to load the light or dark theme based on the user's `prefers-color-scheme` setting, use this. The `media` attributes ensure that only the user's preferred theme stylesheet loads and the `onload` attribute sets the appropriate [theme class](/getting-started/themes) on the `<html>` element.

```html
<link rel="stylesheet" media="(prefers-color-scheme:light)" href="https://cdn.jsdelivr.net/npm/@shoelace-style/shoelace@%VERSION%/dist/themes/light.css">
<link rel="stylesheet" media="(prefers-color-scheme:dark)"
      href="https://cdn.jsdelivr.net/npm/@shoelace-style/shoelace@%VERSION%/dist/themes/dark.css"
      onload="document.documentElement.classList.add('sl-theme-dark');">
<script type="module" src="https://cdn.jsdelivr.net/npm/@shoelace-style/shoelace@%VERSION%/dist/shoelace.js"></script>
```

Now you can [start using Shoelace!](/getting-started/usage)

## Local Installation

If you don't want to use the CDN, you can install Shoelace locally with the following command.

```bash
npm install @shoelace-style/shoelace
```

It's up to you to make the source files available to your app. One way to do this is to create a route in your app called `/shoelace` that serves static files from `node_modules/@shoelace-style/shoelace`.

Once you've done that, add the following tags to your page. Make sure to update `href` and `src` so they point to the route you created.

```html
<link rel="stylesheet" href="/shoelace/dist/themes/light.css">
<script type="module" src="/shoelace/dist/shoelace.js"></script>
```

Alternatively, [you can use a bundler](#bundling).

?> For clarity, the docs will usually show imports from `@shoelace-style/shoelace`. If you're not using a module resolver or bundler, you'll need to adjust these paths to point to the folder Shoelace is in.

## Setting the Base Path

Some components rely on assets (icons, images, etc.) and Shoelace needs to know where they're located. For convenience, Shoelace will try to auto-detect the correct location based on the script you've loaded it from. This assumes assets are colocated with `shoelace.js` and will "just work" for most users.

However, if you're [cherry picking](#cherry-picking) or [bundling](#bundling) Shoelace, you'll need to set the base path. You can do this one of two ways.

```html
<!-- Option 1: the data-shoelace attribute -->
<script src="bundle.js" data-shoelace="/path/to/shoelace"></script>

<!-- Option 2: the setBasePath() method -->
<script src="bundle.js"></script>
<script type="module">
  import { setBasePath } from '@shoelace-style/shoelace/dist/utilities/base-path.js';
  setBasePath('/path/to/shoelace');
</script>
```

?> The library also exports a `getBasePath()` method you can use to reference assets.

## Cherry Picking

The previous approach is the _easiest_ way to load Shoelace, but easy isn't always efficient. You'll incur the full size of the library even if you only use a handful of components. This is convenient for prototyping or if you're using most of the components, but it may result in longer load times in production. To improve this, you can cherry pick the components you need.

Cherry picking can be done from your local install or [directly from the CDN](https://cdn.jsdelivr.net/npm/@shoelace-style/shoelace@%VERSION%/). This will limit the number of files the browser has to download and reduce the amount of bytes being transferred. The disadvantage is that you need to load component manually.

Here's an example that loads only the button component. Again, if you're not using a module resolver, you'll need to adjust the path to point to the folder Shoelace is in.

```html
<link rel="stylesheet" href="@shoelace-style/shoelace/dist/themes/light.css">

<script type="module" data-shoelace="/path/to/shoelace">
  import '@shoelace-style/shoelace/dist/components/button/button.js';

  // <sl-button> is ready to use!
</script>
```

You can copy and paste the code to import a component from the "Importing" section of the component's documentation. Note that some components have dependencies that are automatically imported when you cherry pick. If a component has dependencies, they will be listed in the "Dependencies" section of its docs.

!> Never cherry pick components or utilities from `shoelace.js` as this will cause the browser to load the entire library. Instead, cherry pick from specific modules as shown above.

!> You will see files named `chunk.[hash].js` in the `chunks` directory. Never import these files directly, as they are generated and change from version to version.

## Bundling

Shoelace is distributed as a collection of standard ES modules that [all modern browsers can understand](https://caniuse.com/es6-module). However, importing a lot of modules can result in a lot of HTTP requests and potentially longer load times. Using a CDN can alleviate this, but some users may wish to further optimize their imports with a bundler.

To use Shoelace with a bundler, first install Shoelace along with your bundler of choice.

```bash
npm install @shoelace-style/shoelace
```

Now it's time to configure your bundler. Configurations vary for each tool, but here are some examples to help you get started.

- [Example webpack config](https://github.com/shoelace-style/webpack-example/blob/master/webpack.config.js)
- [Example Rollup config](https://github.com/shoelace-style/rollup-example/blob/master/rollup.config.js)

Once your bundler is configured, you'll be able to import Shoelace components and utilities.

```js
import '@shoelace-style/shoelace/dist/themes/light.css';
import '@shoelace-style/shoelace/dist/components/button/button.js';
import '@shoelace-style/shoelace/dist/components/icon/icon.js';
import '@shoelace-style/shoelace/dist/components/input/input.js';
import '@shoelace-style/shoelace/dist/components/rating/rating.js';
import { setBasePath } from '@shoelace-style/shoelace/dist/utilities/base-path.js';

// Set the base path to the folder you copied Shoelace's assets to
setBasePath('/dist/shoelace');

// <sl-button>, <sl-icon>, <sl-input>, and <sl-rating> are ready to use!
```

!> Component modules include side effects for registration purposes. Because of this, importing directly from `@shoelace-style/shoelace` may result in a larger bundle size than necessary. For optimal tree shaking, always cherry pick, i.e. import components and utilities from their respective files, as shown above.
