# 💎📦 skpm - Sketch Plugin Manager

A utility to build, publish, install and manage [sketch](https://www.sketchapp.com/) plugins.

## Installation

```bash
npm install -g skpm
```

_The `npm` command-line tool is bundled with Node.js. If you have it installed, then you already have npm too. If not, go download [Node.js](https://nodejs.org/en/download/)._

## Usage

### I'm a sketch user

#### Installing a plugin

To install a sketch plugin:

```bash
skpm install name-of-the-plugin
```

#### Uninstalling a plugin

To uninstall a sketch plugin:

```bash
skpm uninstall name-of-the-plugin
```

### I'm a plugin developer

#### Scaffold the architecture of a new plugin

To interactively create the architecture to start developing a new plugin (see the [sketch documentation](http://developer.sketchapp.com/introduction/plugin-bundles/) for more information):

```bash
skpm init
```

This will create:
* a `package.json` file
* a `src` folder with:
  * a `manifest.json` file
  * a `command.js` file for an example of command
* a `.gitignore` file if non-existent

The `package.json` must contain 3 specific fields:
* `main`: pointing to your `.sketchplugin`
* `manifest`: pointing to your `manifest.json` (`src/manifest.json` by default)
* `repository`: pointing to your github repository

#### Build the plugin

To transpile the JavaScript to CocoaScript and copy the `manifest.json` to the `.sketchplugin`:
```bash
skpm build
```

Additionally, some fields from the `package.json` will be set in the `manifest.json` (if not present):
* version
* name
* description
* homepage

#### Publish the plugin on the registry

To publish a new version of the plugin to the registry:
```bash
skpm publish <newversion> | major | minor | patch | premajor | preminor | prepatch | prerelease
```

The exact order of execution is as follows:
* Run the `preversion` script
* Bump `version` in package.json as requested (`patch`, `minor`, `major`, etc).
* Run the `version` script
* Commit and tag
* Run the `postversion` script.
* Run the `build` script
* Zip the folder specified in the `main` field
* Create a draft release on GitHub
* Upload the zip to GitHub
* Publish the release
* Remove the zip
* Notify the registry

## License

MIT
