<p align="center">
    <img src="https://image.ibb.co/nR99Wy/svg_resolveunpkg_github.png"/>
</p>

[![Build Status](https://travis-ci.org/wix-incubator/resolve-unpkg.svg)](https://travis-ci.org/wix-incubator/resolve-unpkg) [![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)

The **Resolve-Unpkg** library helps you to keep the unpkg links up to date by patching the unpkg URLs with the package version installed in your environment. The **Resolve-Unpkg** may be used in command line and in JavaScript imports.

> **TLDR**
>
> In CLI:
> ```bash
>    npm i --save-dev resolve-unpkg
>    resolve-unpkg path-to-optional-config.json
> ```
>
> In JS code:
> ```javascript
>    import scripts from '!resolve-unpkg?unpkgPrefix=unpkg.com&versionPlaceholder=a.b.c&onlyByVersionPlaceholder=true!scripts.json';
> ```

<!-- TOC -->

- [Import](#import)
- [Configuration](#configuration)
- [Usage in CLI](#usage-in-cli)
- [Usage in JS](#usage-in-js)

<!-- /TOC -->

# Import

To add **Resolve-Unpkg**, run in the project root folder:

`npm i --save-dev resolve-unpkg`

This will add the **Resolve-Unpkg** as a dependency to the *package.json* and enable you to run resolve-unpkg command and us it with the Webpack loader.

# Configuration

By default, Resolve-Unpkg processes URLs with unpkg.com prefix in the *index.html* file and replaces the *x.x.x* version placeholders with the actual version of the packages that are installed in your local environment.

To customize the default configuration, use the following options:

| Property         | Type       | Required | Description                              | CLI | Loader |
| ---------------- | ---------- | :------: | ---------------------------------------- | :-: | :----: |
| `unpkgPrefix`    | `string`   |   No     | The prefix of the unpkg URLs (default: `'unpkg.com'`) | Yes | Yes |
| `versionPlaceholder`| `string`   |   No     | A version number placeholder to be replaced when occurs in the position of package version (default: `'x.x.x'`).  | Yes | Yes |
| `onlyByVersionPlaceholder`  | `boolean`   |   No     |  A flag that may limit the update to those URLs that contain the versionPlaceholder (e.g. x.x.x); when set to `true`, the exact package version numbers, like 1.9.1, are ignored (default: `true`).     | Yes | Yes |
| `files`    | `string[]`   |   No     | Path to file(s) to update/resolve URLs in (default: `['index.html']`) | Yes | No |
| `dist`    | `string`   |   No     | Folder where the updated file(s) should be saved (default: overwrite original) | Yes | No |

# Usage in CLI

With Resolve-Unpkg, you can automatically update the package versions that are mentioned in the unpkg URLs in your project files, like *index.html*.

> The package versions used are those installed in your local environment.

To launch unpkg links update via CLI, run:

`resolve-unpkg [optional-config.json]`

**Sample Resolve-Unpkg configuration file**

```
{
  "unpkgPrefix": "/unpkg",
  "files": ["index.vm", "index.ejs"],
  "dist": "dist/"
}
```

As a result, the unpkg URLs will be updated as illustrated in the example below.

Let's consider the following file (`index.html`) :

```html
<html>
    <head>
        <link rel="stylesheet" href="https://unpkg.com/spectrum-colorpicker@1.6.0/spectrum.css">
    </head>
    <body>
        <script src="https://unpkg.com/jquery@x.x.x/dist/jquery.min.js"></script>
    </body>
</html>
``` 

If the version of the `jquery` installed in the local environment is `'2.21'`, and `spectrum-colorpicker` is `'1.6.1'`, and we run the `resolve-unpkg` with default configuration parameters, the original `index.html` file will be overwritten with this one:

```html
<html>
    <head>
        <link rel="stylesheet" href="https://unpkg.com/spectrum-colorpicker@1.6.0/spectrum.css">
    </head>
    <body>
        <script src="https://unpkg.com/jquery@2.2.1/dist/jquery.min.js"></script>
    </body>
</html>
```

# Usage in JS

For the unpkg links that are loaded into your JS code from the JSON file, use **resolve-unpkg** loader to inject the updated versions into the URLs on loading. To do so, update the script import and provide the necessary values in the Resolve-Unpkg loader parameter:

```javscript
...
import scripts from '!resolve-unpkg?unpkgPrefix=unpkg.com&versionPlaceholder=a.b.c&onlyByVersionPlaceholder=true!scripts.json';
...
``` 
