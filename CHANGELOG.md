# Changelog

Versioning _loosely_ complies with [semantic versioning (semver)](http://semver.org/).
Since this utility is applied only _once_ to a given package - in order to initialize it -
maintaining compatibility is less important. However, larger changes will be reflected
in higher version-number increases.

<!-- NOTE: An entry template is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **[v0.4.1-1](https://github.com/mklement0/make-pkg/compare/v0.4.1-0...v0.4.1-1)** (2015-06-16):
  * [prerelease] Pre-release tag changed to fixed string 'pre' so as to have that track the most recent pre-release.

* **[v0.4.0](https://github.com/mklement0/make-pkg/compare/v0.3.5...v0.4.0)** (2015-06-16):
  * [new] Consistent support for pre-release versions added.
  * [enhancement] New CLI test template added for testing behavior with unknown options.
  * [fix] Minor bug in CLI test templates fixed.

* **[v0.3.5](https://github.com/mklement0/make-pkg/compare/v0.3.4...v0.3.5)** (2015-06-13):
  * [enhancement] When initializing a CLI package, supported-platform information is now added to the installation chapter in the read-me file.
  * [enhancement] `make release` now also opens `README.md` for editing, so as to give you a chance for a final review, and prompts for continuing after.
  * [enhancement] `make release` now aborts if the missing-information placeholder '???' is found in `README.md`.

* **[v0.3.4](https://github.com/mklement0/make-pkg/compare/v0.3.3...v0.3.4)** (2015-06-06):
  * [new] License badge added to `README.md` template for non-npm packages, based on [shields.io](http://shields.io).

* **[v0.3.3](https://github.com/mklement0/make-pkg/compare/v0.3.2...v0.3.3)** (2015-06-03):
  * [doc] Read-me updated to reflect current implementation (badges, `make browse-npm`).

* **[v0.3.2](https://github.com/mklement0/make-pkg/compare/v0.3.1...v0.3.2)** (2015-06-03):
  * [new] npm-license badge added to `README.md` template for npm packages, based on [shields.io](http://shields.io).
  * [change] npm-version badge in `README.md` template for npm packages switched from [badge.fury.io](https://badge.fury.io/) to [shields.io](http://shields.io), because the latter seems like a one-stop source for multiple badges.
  * [enhancement] Small Makefile tweaks.

* **[v0.3.1](https://github.com/mklement0/make-pkg/compare/v0.3.0...v0.3.1)** (2015-06-03):
  * [enhancement] Generated `CHANGELOG.md` files now have version numbers hyperlinked to GitHub for comparing each release to the previous one.
  * [fix] Removed obsolete check from Makefile.

* **v0.3.0** (2015-06-03):
  * [enhancement] Improvements to the versioning workflow: new `make verinfo` task only _lists_ version numbers, whereas `make version` now always _updates_ - either by specifying `VER=...` on the command line, or by _prompting_ the user.
  * [enhancement] TOC placement in generated `README.md` files changed to ensure that badges stay at the top.
  * [fix] npm-registry-installation TOC entry in template for `README.md` fixed, along with corresponding chapter.

* **v0.2.3** (2015-06-02):
  * [fix, enhancement] CLI installation instructions in read-me template fixed & improved.
  * [enhancement] Error-reporting helper function added to CLI test stubs.
  * [doc] Amended this read-me.

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
