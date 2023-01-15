# NPM License Checker

## Table of Contents

- [NPM License Checker](#npm-license-checker)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Changes](#changes)
    - [Version 3.1.0](#version-310)
    - [Version 3.0.1](#version-301)
    - [Version 3.0.0](#version-300)
  - [All options in alphabetical order:](#all-options-in-alphabetical-order)
  - [Exclusions](#exclusions)
  - [Examples](#examples)
  - [Custom format](#custom-format)
  - [Requiring](#requiring)
  - [Debugging](#debugging)
  - [How Licenses are Found](#how-licenses-are-found)
  - [Related information sources on the internet](#related-information-sources-on-the-internet)

<a name="introduction"/>

## Introduction
_This is a fork of rseidelsohn' [license-checker-rseidelsohn](https://github.com/RSeidelsohn/license-checker-rseidelsohn#readme) - It mainly exists to exlude README which rarely has license files

**Please notice:** Version 1.2.2 is the last version working fine on node v12. From Version 2 on, you will need at least Node v14 to run this NPM license checker. Thanks to @daniel-schulz for pointing this out!

Ever needed to see all the license info for a module and its dependencies?

It's this easy:

```shell
npm install -g npm-license-checker-alt

mkdir foo
cd foo
npm install yui-lint
npm-license-checker-alt
```

You should see something like this:

```
├─ cli@0.4.3
│  ├─ repository: http://github.com/chriso/cli
│  └─ licenses: MIT
├─ glob@3.1.14
│  ├─ repository: https://github.com/isaacs/node-glob
│  └─ licenses: UNKNOWN
├─ graceful-fs@1.1.14
│  ├─ repository: https://github.com/isaacs/node-graceful-fs
│  └─ licenses: UNKNOWN
├─ inherits@1.0.0
│  ├─ repository: https://github.com/isaacs/inherits
│  └─ licenses: UNKNOWN
├─ jshint@0.9.1
│  └─ licenses: MIT
├─ lru-cache@1.0.6
│  ├─ repository: https://github.com/isaacs/node-lru-cache
│  └─ licenses: MIT
├─ lru-cache@2.0.4
│  ├─ repository: https://github.com/isaacs/node-lru-cache
│  └─ licenses: MIT
├─ minimatch@0.0.5
│  ├─ repository: https://github.com/isaacs/minimatch
│  └─ licenses: MIT
├─ minimatch@0.2.9
│  ├─ repository: https://github.com/isaacs/minimatch
│  └─ licenses: MIT
├─ sigmund@1.0.0
│  ├─ repository: https://github.com/isaacs/sigmund
│  └─ licenses: UNKNOWN
└─ yui-lint@0.1.1
   ├─ licenses: BSD
   └─ repository: http://github.com/yui/yui-lint
```

An asterisk next to a license name means that it was deduced from
an other file than package.json (README, LICENSE, COPYING, ...)
You could see something like this:

```
└─ debug@2.0.0
   ├─ repository: https://github.com/visionmedia/debug
   └─ licenses: MIT*
```

<a name="changes"/>

## Changes

### Version 1.0.1

First real version

<a name="all_options_in_alphabetical_order"/>

## All options in alphabetical order:

-   `--angularCli` is just a synonym for `--plainVertical`
-   `--csv` output in csv format.
-   `--csvComponentPrefix` prefix column for component in csv format.
-   `--customPath` to add a custom Format file in JSON
-   `--development` only show development dependencies.
-   `--direct` look for direct dependencies only
-   `--excludeLicenses [list]` exclude modules which licenses are in the comma-separated list from the output
-   `--excludePackages [list]` restrict output to the packages (either "package@fullversion" or "package@majorversion" or only "package") not in the semicolon-seperated list
-   `--excludePackagesStartingWith [list]` exclude modules which names start with the comma-separated list from the output (useful for excluding modules from a specific vendor and such). Example: `--excludePackagesStartingWith "webpack;@types;@babel"`
-   `--excludePrivatePackages` restrict output to not include any package marked as private
-   `--failOn [list]` fail (exit with code 1) on the first occurrence of the licenses of the semicolon-separated list
-   `--files [path]` copy all license files to path and rename them to `module-name`@`version`-LICENSE.txt.
-   `--includeLicenses [list]` include only modules which licenses are in the comma-separated list from the output
-   `--includePackages [list]` restrict output to the packages (either "package@fullversion" or "package@majorversion" or only "package") in the semicolon-seperated list
-   `--json` output in json format.
-   `--limitAttributes [list]` limit the attributes to be output.
-   `--markdown` output in markdown format.
-   `--nopeer` skip peer dependencies in output.
-   `--onlyAllow [list]` fail (exit with codexclusionse 1) on the first occurrence of the licenses not in the semicolon-seperated list
-   `--onlyunknown` only list packages with unknown or guessed licenses.
-   `--out [filepath]` write the data to a specific file.
-   `--plainVertical` output license info in plain vertical format like [Angular CLI does](https://angular.io/3rdpartylicenses.txt)
-   `--production` only show production dependencies.
-   `--relativeLicensePath` output the location of the license files as relative paths
-   `--relativeModulePath` output the location of the module files as relative paths
-   `--start [filepath]` path of the initial json to look for
-   `--summary` output a summary of the license usage',
-   `--unknown` report guessed licenses as unknown licenses.
-   `--version` The current version
-   `--help` The text you are reading right now :)

<a name="exclusions"/>

## Exclusions

A list of licenses is the simplest way to describe what you want to exclude.

You can use valid [SPDX identifiers](https://spdx.org/licenses/).
You can use valid SPDX expressions like `MIT OR X11`.
You can use non-valid SPDX identifiers, like `Public Domain`, since `npm` does
support some license strings that are not SPDX identifiers.

<a name="examples"/>

## Examples

```
npm-license-checker-alt --json > /path/to/licenses.json
npm-license-checker-alt --csv --out /path/to/licenses.csv
npm-license-checker-alt --unknown
npm-license-checker-alt --customPath customFormatExample.json
npm-license-checker-alt --excludeLicenses 'MIT, MIT OR X11, BSD, ISC'
npm-license-checker-alt --includePackages 'react@16.3.0;react-dom@16.3.0;lodash@4.3.1'
npm-license-checker-alt --excludePackages 'internal-1;internal-2'
npm-license-checker-alt --onlyunknown
```

<a name="custom_format"/>

## Custom format

The `--customPath` option can be used with CSV to specify the columns. Note that
the first column, `module_name`, will always be used.

When used with JSON format, it will add the specified items to the usual ones.

The available items are the following:

-   copyright
-   description
-   email
-   licenseFile
-   licenseModified
-   licenses
-   licenseText
-   name
-   publisher
-   repository
-   url
-   version

You can also give default values for each item.
See an example in [customFormatExample.json](customFormatExample.json).

<a name="requiring"/>

## Requiring

```js
var checker = require('npm-license-checker-alt');

checker.init(
    {
        start: '/path/to/start/looking',
    },
    // eslint-disable-next-line no-unused-vars
    function (err, packages) {
        if (err) {
            //Handle error
        } else {
            //The sorted package data
            //as an Object
        }
    },
);
```

<a name="debugging"/>

## Debugging

license-checker uses [debug](https://www.npmjs.com/package/debug) for internal logging. There’s two internal markers:

-   `npm-license-checker-alt:error` for errors
-   `npm-license-checker-alt:log` for non-errors

Set the `DEBUG` environment variable to one of these to see debug output:

```shell
$ export DEBUG=npm-license-checker-alt*; npm-license-checker-alt
scanning ./yui-lint
├─ cli@0.4.3
│  ├─ repository: http://github.com/chriso/cli
│  └─ licenses: MIT
# ...
```

<a name="all_options_in_alphabetical_order"/>

## How Licenses are Found

We walk through the `node_modules` directory with the [`read-installed-packages`](https://www.npmjs.org/package/read-installed-packages) module. Once we gathered a list of modules we walk through them and look at all of their `package.json`'s, We try to identify the license with the [`spdx`](https://www.npmjs.com/package/spdx) module to see if it has a valid SPDX license attached. If that fails, we then look into the module for the following files: `LICENSE`, `LICENCE`, `COPYING`, 

If one of the those files are found (in that order) we will attempt to parse the license data from it with a list of known license texts. This will be shown with the `*` next to the name of the license to show that we "guessed" at it.

<a name="related_information_sources_on_the_internet"/>

## Related information sources on the internet

-   [ChooseALicense.com](https://choosealicense.com/) - aims at helping you in choosing an open source license for your project
-   [TLDRLegal.com](https://tldrlegal.com/) - aims at exlaining complicated legal details of software licenses in easy to understand english
