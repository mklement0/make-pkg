# Changelog

Versioning _loosely_ complies with [semantic versioning (semver)](http://semver.org/).
Since this utility is applied only _once_ to a given package - in order to initialize it -
maintaining compatibility is less important. However, larger changes will be reflected
in higher version-number increases.

<!-- NOTE: An entry template is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **v0.2.2** (2015-06-01):
  * [new] `make browse` opens the project's GitHub repository in the default browser.
  * [enhancement] npm-registry installation instructions in generated `README.md` files improved.
  * [doc] npm-registry installation instructions in `README.md` improved.

* **v0.2.1** (2015-05-31):
  * [new] For packages intended for publication in the npm registry, adds an [npm version badge](http://badge.fury.io/).
  * [fix] Typo in `templates/README.tmpl.md`.
  
* **v0.2.0** (2015-05-31):
  * [new] By default, an auto-generated, auto-updating TOC (table of contents) is now placed at the top of `README.md`; behavior is configurable.
  * [doc] Tweaked `README.md` TOC formatting to render as intended on npmjs.com.

* **v0.1.0** (2015-05-31):
  * Initial release.
