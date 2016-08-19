# Migrating from JSCS

In April 2016, we [announced](http://eslint.org/blog/2016/04/welcoming-jscs-to-eslint) that the JSCS project was shutting down and the JSCS team would be joining the ESLint team. This guide is intended to help those who are using JSCS to migrate their settings and projects to use ESLint. We've tried to automate as much of the conversion as possible, but there are some manual changes that are needed.

## Terminology

Before beginning the process of migrating to ESLint, it's helpful to understand some of the terminology that ESLint uses and how it relates to terminology that JSCS uses.

* **Configuration File** - In JSCS, the configuration file is `.jscsrc`. In ESLint, the configuration file can be `.eslintrc.json`, `.eslintrc.yml`, `.eslintrc.yaml`, or `.eslintrc.js` (there is also a deprecated `.eslintrc` file format).
* **Presets** - In JSCS, there were numerous predefined configurations shipped directly within JSCS. ESLint doesn't ship with predefined configurations, however, ESLint does support shareable configs. Shareable configs are configurations that are published on their own to npm and there are shareable configs available for almost all of the JSCS presets (see the "Converting Presets" section below). Additionally, the "preset" option in a configuration file is the equivalent of the ESLint "extends" option.



## Convert Configuration Files Using Polyjuice

[Polyjuice](https://github.com/brenolf/polyjuice) is a utility for converting JSCS (and JSHint) configuration files into ESLint configuration files automatically. It understands the equivalent rules from each utility and will automatically output an ESLint configuration file that is a good approximation of your existing JSCS file.

To install Polyjuice:

```
$ npm install -g polyjuice
```

To convert your configuration file, pass in the location of your `.jscs` file using the `--jscs` flag:

```
$ polyjuice --jscs .jscsrc > .eslintrc
```

This creates a `.eslintrc` with the equivalent rules from `.jscsrc`.

If you have multiple `.jscsrc` files, you can pass them all and Polyjuice will combine them into one `.eslintrc` file:

```
$ polyjuice --jscs .jscsrc ./foo/.jscsrc> .eslintrc
```

**Note:** If you'd like to create a new configuration file from scratch, you can run `eslint --init`, which will guide you through the process of creating a new configuration file.

## Converting Presets

There are shareable configs available for most JSCS presets. The equivalent shareable configs for each JSCS preset are listed in the following table:

| **JSCS Preset** | **ESLint Shareable Config** |
|-----------------|-----------------------------|
| `airbnb`        | [`eslint-config-airbnb`](https://npmjs.com/package/eslint-config-airbnb) |
| `crockford`        | (not available) |
| `google`        | [`eslint-config-google`](https://npmjs.com/package/eslint-onfig-google) |
| `grunt`        | [`eslint-config-grunt`](https://npmjs.com/package/eslint-config-grunt) |
| `idiomatic`        | [`eslint-config-idiomatic`](https://npmjs.com/package/eslint-config-idiomatic) |
| `jquery`        | [`eslint-config-jquery`](https://npmjs.com/package/eslint-config-jquery) |
| `mdcs`        | (not available) |
| `node-style-guide`        | [`eslint-config-node-style-guide`](https://npmjs.com/package/eslint-config-node-style-guide) |
| `wikimedia`        | [`eslint-config-wikimedia`](https://npmjs.com/package/eslint-config-wikimedia) |
| `wordpress`        | [`eslint-config-wordpress`](https://npmjs.com/package/eslint-config-wordpress) |

As an example, suppose that you are using the `airbnb` preset, so your `.jscsrc` file looks like this:

```json
{
    "preset": "airbnb"
}
```

In order to get the same functionality in ESLint, you would first need to install the `eslint-config-airbnb` shareable config package:

```
$ npm install eslint-config-airbnb --save-dev
```

And then you would modify your configuration file like this:

```json
{
    "extends": "airbnb"
}
```

ESLint sees `"airbnb"` and will look for `eslint-config-airbnb` (to save you some typing).
