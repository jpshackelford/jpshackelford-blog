# jpshackelford.info blog site

This blog is built with love and WASM. We use [Bartholomew](https://github.com/fermyon/bartholomew) for the blog engine and [Spin](https://spin.fermyon.dev) as the supporting framework. 

This example has diverged from the [base template](https://github.com/fermyon/bartholomew-site-template) somewhat in that we use
[Pico.css](https://picocss.com) instead of [bootstrap](https://getbootstrap.com). 

## Installation

To use Bartholomew, install [Spin](https://spin.fermyon.dev).

For MacOS:

Use [Homebrew](https://brew.sh) by first adding a tap and then installing spin.
```console
$ brew tap fermyon/tap
```
```console
$ brew install fermyon/tap/spin
```

For Linux:

The installer script installs Spin along with a starter set of language templates and plugins:

```console
$ curl -fsSL https://developer.fermyon.com/downloads/install.sh | bash
```
Once you have run the installer script, it is highly recommended to add Spin to a folder, which is on your path, e.g.:
```console
$ sudo mv spin /usr/local/bin/
```
See the complete [install instructions](https://developer.fermyon.com/spin/v2/install) for source builds and other platforms.

## Edit & View Changes

To start your website, run the following command from this directory:

```console
$ spin watch --skip-build

Serving HTTP on address http://127.0.0.1:3000
Available Routes:
  bartholomew: http://127.0.0.1:3000 (wildcard)
  fileserver: http://127.0.0.1:3000/static (wildcard)
```

Point your web browser to `http://localhost:3000/` to preview the blog site. The [watch](https://developer.fermyon.com/spin/v2/running-apps#monitoring-applications-for-changes) command will pick up changes as you save them so you only need to refresh the browser, not restart spin.

## Directory Structure:

- `config/site.toml`: The main [configuration file](https://developer.fermyon.com/bartholomew/configuration#configuration-using-toml) for the site.
- `content/`: [Markdown](https://developer.fermyon.com/bartholomew/markdown) files for blog content.
- `scripts/` (advanced): [Rhai](https://developer.fermyon.com/bartholomew/scripting) scripts.
- `spin.toml`: The [configuration file](https://developer.fermyon.com/spin/v2/writing-apps#writing-an-application-manifest) for the Spin application.
- `static/`: Static assets like images, CSS, and downloads.
- `templates/`: Handlebars [templates](https://developer.fermyon.com/bartholomew/templates) 
- `shortcodes/`: [shortcodes](https://bartholomew.fermyon.dev/shortcodes)  

Point your web browser to `http://localhost:3000/` to see the blog site.

## About the License

Content in the following directory trees are licensed under CC0 (see LICENSE for details). To the greatest extent possible, you are free to use this content however you want.  

```
/config
/scripts
/shortcodes
/static/css
/templates
```
No license is granted for content in the following directory trees 
(and all others) as this material is copyright John-Mason Shackelford and all rights are reserved.
```
/content
/static/media
```