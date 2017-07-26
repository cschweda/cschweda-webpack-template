[![npm version](https://badge.fury.io/js/cschweda-webpack-template.svg)](https://badge.fury.io/js/cschweda-webpack-template)

# Custom Webpack Template

**This is a fork of the excellent [html-webpack-template](https://github.com/jaketrent/html-webpack-template) with some minor deletions and additions**

## Added:
- `buildInfo`: Information about developer name, build time/date, and GitHub repo
- `postscripts`: Scripts to appear directly before the closing body tag, after the Webpack chunks.
- `headscripts`: Scripts to appear directly before the closing head tag.

## Deleted:
- Google Analytics info. (In my case, I inject the GA info via vue-router.)

For usage info, please see below.

---


This is a template for the [webpack](http://webpack.github.io/) plugin [html-webpack-plugin](https://www.npmjs.com/package/html-webpack-plugin).
It has a few extra features than the [default template](https://github.com/jantimon/html-webpack-plugin/blob/master/default_index.ejs)
which will hopefully make it less likely that you'll have to create your own `index.html` file in your webpack project.

Templates for the html-webpack-plugin are implemented using [underscore templates](http://underscorejs.org/#template)
(previously, in 2.x, [blueimp templates](https://github.com/blueimp/JavaScript-Templates)). You can write your own as
well.


## Installation

Install the template in your project with npm:

```shell
$ npm install cschweda-webpack-template --save-dev
```

## Basic Usage

There are a couple required parameters:

- `inject`: Set to `false`. Controls asset addition to the template. This template takes care of that.
- `template`: Specify this module's `index.ejs` file.

And some other optional:
- `appMountId`: The `<div>` element id on which you plan to mount a JavaScript app.
- `appMountIds`: An array of application element ids.
- `baseHref`: Adjust the URL for relative URLs in the document ([MDN](https://developer.mozilla.org/en/docs/Web/HTML/Element/base)).
- `devServer`: Insert the webpack-dev-server hot reload script at this host:port/path; e.g., http://localhost:3000.
- `lang`: String identifying your content language
- `links`: Array of `<link>` elements.
  - If an array element is a string, the value is assigned to the `href` attribute and the `rel` attribute is set to
    `"stylesheet"`;
  - If an array element is an object, the object's properties and values are used as the attribute names and values,
    respectively.
- `meta`: Array of objects containing key value pairs to be included as meta tags.
- `mobile`: Sets appropriate meta tag for page scaling.
- `inlineManifestWebpackName`: For use with [inline-manifest-webpack-plugin](https://www.npmjs.com/package/inline-manifest-webpack-plugin).
- `scripts`: Array of external script imports to include on page.
  - If an array element is a string, the value is assigned to the `src` attribute and the `type` attribute is set to
    `"text/javascript"`;
  - If an array element is an object, the object's properties and values are used as the attribute names and values,
    respectively.
- `window`: Object that defines data you need to bootstrap a JavaScript app.
- `buildInfo`: Meta info about build (Contact name, GitHub repo, time/date, etc.)
- `postscripts`: Array of scripts to appear at the bottom of the page, after the Webpack chunks.
- `headScripts`: Array of scripts to appear at the bottom of the head section, just before the closed ```</head>``` tag.

Plus any [html-webpack-plugin config options](https://github.com/ampedandwired/html-webpack-plugin#configuration)
otherwise available.

### Example

Here's an example webpack config illustrating how to use these options in your `webpack.config.js`:

```js
{
  // ...
  plugins: [
    new HtmlWebpackPlugin({
      // Required
      inject: false,
      template: require('cschweda-webpack-template'),
      // template: 'node_modules/cschweda-webpack-template/index.ejs',

      // Optional
      appMountId: 'app',
      baseHref: 'http://example.com/awesome',
      devServer: 'http://localhost:8080',
      meta: [
        {
          name: 'description',
          content: 'A better default template for html-webpack-plugin.'
        }
      ],
      mobile: true,
      lang: 'en-US',
      links: [
        'https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css',
        'https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css'
      ],
      inlineManifestWebpackName: 'webpackManifest',
      scripts: [
        'http://example.com/somescript.js',
        {
          src: '/myModule.js',
          type: 'module'
        }
      ],
      headscripts: [
        'https://fontawesome.js'
      ],
      postscripts: [
        'https://code.jquery.com/jquery-3.2.1.slim.js',
        'https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js'
      ],
      title: 'My Webpack App',
      buildInfo: [
        {
          Build: moment().tz("America/Chicago").format("dddd, MMMM Do YYYY, h:mm:ss a"),
          // npm i moment-timezone --save-dev
          // let moment = require('moment-timezone')
          GitHub: 'https://github.com/GitHubName/GitHubProject',
          Contact: 'myname@somewhere.com'
        }
      ],
      window: {
        env: {
          apiHost: 'http://myapi.com/api/v1'
        }
      }

      // And any other config options from html-webpack-plugin:
      // https://github.com/ampedandwired/html-webpack-plugin#configuration
    })
  ]
}
```
