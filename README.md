[![npm version](https://img.shields.io/npm/v/make-pkg.svg)](https://npmjs.com/package/make-pkg) [![license](https://img.shields.io/npm/l/make-pkg.svg)](https://github.com/mklement0/make-pkg/blob/master/LICENSE.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

- [make-pkg &mdash; initialize and maintain npm package projects](#make-pkg-&mdash-initialize-and-maintain-npm-package-projects)
- [Package initialization (scaffolding)](#package-initialization-scaffolding)
- [Package release and maintenance](#package-release-and-maintenance)
- [Supported platforms](#supported-platforms)
  - [Additional prerequisites](#additional-prerequisites)
- [Installation](#installation)
- [Usage](#usage)
  - [Example](#example)
  - [Initializing a new package](#initializing-a-new-package)
  - [Command-line syntax](#command-line-syntax)
- [License](#license)
  - [Acknowledgements](#acknowledgements)
  - [npm dependencies](#npm-dependencies)
- [Changelog](#changelog)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# make-pkg &mdash; initialize and maintain npm package projects

`make-pkg` is a **Unix CLI for initializing [npm](https://www.npmjs.com/) package projects and implementing a release and maintenance workflow**.

The projects initialized are backed by a local [Git](http://git-scm.com/) repository with a remote [GitHub](https://github.com) counterpart.

While the **_typical_ use case is to create _npm_ packages**, `make-pkg` can be used to **manage GitHub-based OSS projects in general**.

`make-pkg` is **opinionated**, which allows it to **simplify and automate many tasks**.  
For instance, `make-pkg` is managed by itself, and this read-me was scaffolded by it and is also kept up-to-date by it on every release.

# Package initialization (scaffolding)

* **Initializes the `package.json` package-description file** through a series of prompts, with configurable defaults, similar to, but more fully-featured than `npm init`.
* Initializes **a local Git repository** and the `.gitignore` file.
* Creates a **corresponding remote repository on GitHub** and links the local repository to it.
* Instantiates **templates for various standard files**: `README.md`, `LICENSE.md`, `CHANGELOG.md`
* Creates **stubs** for the **main module file** and/or **CLIs**.
* Creates **stubs for tests**, plus a few standard tests for CLIs.
* For packages intended for publication in the npm registry, adds an **npm-version badge** and **license badge**, courtesy of [shields.io](http://shields.io).
* Installs **a `Makefile` that implements a set of tasks for releasing and ongoing package maintenance**, along with supporting development-dependency packages (which are the same as or a subset of the ones for this utility - see [npm dependencies](#npm-dependencies))

# Package release and maintenance

Note that `make-pkg` itself is no longer in the picture after having initialized a package project; instead, **ongoing tasks are invoked through the standard `make` utility**.
These tasks are defined in file `Makefile`, which can be customized after the fact, if needed; **`make list` lists all top-level tasks**.

* **`make version`** and **`make verinfo`** update and list the **package's version number**, respectively:
    * `make verinfo` lists the current version number from `package.json` as well as the most recently assigned Git version tag; the Git tag is assumed to be in sync with the latest version published in the npm registry, if applicable; the `package.json` version may be ahead, in preparation for a new release.
    * `make version [VER=<new-ver>]` updates the package version number in `package.json` and, if present, in source files:
        * If `VER` is not specified, the new version number is _prompted_ for.
        * The new version number is validated, including to prevent accidental _lowering_ of the version number; however, in exceptional situations you may specify `FORCE=1` on the command line to override.
        * Updates the version number in `package.json`.
        * Updates the version number in source files in the `./bin` and `./lib` subfolders, using string replacement to update `v<major>.<minor>.<patch>` instances of the old version number.
        * `<new-ver>` can either be an explicit `<major>.<minor>.<patch>` version specifier or specify how to *increment* the current version number via the component to increment: `patch`, `minor`, `major`,
  `prepatch`, `preminor`, `premajor`, or `prerelease`; using a `pre`-prefixed specifier on a non-pre-release version number appends `-0` to it.
* **`make update-readme`** updates file **`README.md` to act as a single source of all relevant, current project information**
    * Pulls in the then-current contents of the `LICENSE.md` and `CHANGELOG.md` files.
    * If needed, updates the calendar year in `LICENSE.md`.
    * If applicable, pulls in usage information output by the project's CLI.
    * By default, adds an auto-generated, auto-updating TOC (table of contents) at the top, courtesy of [doctoc](https://github.com/thlorenz/doctoc); see below for how to turn that off.
    * Lists the project's dependencies with links to their respective homepages.
* **`make toggle-toc`** turns inclusion of an **auto-generated, auto-updating TOC in `README.md`** on or off.
    * Shows the current TOC inclusion status and prompts to toggle it; by default, inclusion of a TOC is turned _on_.
    * To change the TOC title, modify property `net_same2u.make_pkg.tocTitle` in file `package.json`.
    * To change the TOC settings for _future_ projects, run `make-pkg -e` and edit the `vTOC_*` settings.
    * To update the TOC on demand (generally not necessary), run `make update-toc`.
* **`make toggle-man`** turns **generating a man page for a package's CLI** on or off.
    * Shows whether the feature is currently on and prompts to toggle it; by default, it is turned _off_.
    * To turn the feature on or off for _future_ CLI projects, run `make-pkg -e` and edit the `vMAN_ON` setting.
    * To update the man page on demand, run `make update-man`; updating is performed automatically during `make release`.
    * To update _and_ view the man page locally, run `make view-man`.
    * Note: For this feature to work, a package's CLI must exhibit very specific behavior: when invoked with `--man-source`, 
      it must output a Markdown-formatted document that [marked-man](https://github.com/kapouer/marked-man) can process;
      the Markdown document is extracted as-is to `./doc/`, and the man-page-format (ROFF) document created by `marked-man` is saved to `./man/`.
      A `man` property in `package.json` points to the ROFF document, which, in the case of global installation (`-g`) of the package, 
      causes `npm` to install the ROFF document as a system-wide man page (except on Windows).
      `make-pkg` itself uses the feature, so you can look at its [CLI](bin/make-pkg) and [package.json file](package.json) to see how it's implemented.
* **`make test`** runs **tests**:
    * Runs the tests defined in subdirectory `./test`; stubs are initially provided, as well as a few standard tests, if the project has a CLI.
    * If the project comprises _only_ a CLI, test library [urchin](https://www.npmjs.com/package/urchin) is used; otherwise, it is [tap](https://www.npmjs.com/package/tap).
* **`make release`** **integrates all of the above tasks** to **publish a release**; if any sub-task fails, the overall task is aborted:
    * Ensures that the active branch is `master` and that there are no untracked files.
    * If the `package.json` version number is already ahead of the latest Git version tag, that version number is offered as the new release's version number by default, but you can opt to change it. If you do, or if the package version number is not ahead, you're prompted for the new version number as described for `make version` above.
    * Runs the tests, as described above; `NOTEST=1` can be appended to the `make release` invocation to skip tests.
    * Adds a date-stamped section for the new version at the beginning of `CHANGELOG.md` and opens it for editing to describe what's changed in the new release. 
    * If applicable and turned on, creates or updates the man page for the project's (main) CLI in `./man` using [marked-man](https://github.com/kapouer/marked-man) - see `make toggle-man` above.
    * Updates `README.md` as described above, opens it in the system's text editor for final review, and prompts for continuing on closing it.
    * Commits, using the new changelog section as the commit message.
    * Creates an annotated Git version tag with the new version number; also (re)creates the lightweight 'stable' tag to mark the most recent stable version, or, analogously, the 'pre' tag for pre-release versions.
    * Pushes the changes and tags to the `master` branch of the remote `origin` GitHub repository.
    * Unless the project is marked as _private_ in `package.json`, offers to publish the new version to the [npm registry](https://www.npmjs.com/).
        * _Pre-release_ versions are published with `--tag pre`, so as to make the latest pre-release version installable on demand with `npm install <pgk>@pre`, while retaining the most recent _release_ version as the default version.
* **`make push`** **pushes changes** to the remote `origin` repository:
    * Initiates a commit, if necessary, but aborts if there are untracked files.
    * On successful commit, pushes changes, including tags, to the branch of the same name in the remote `origin` repository.
* **`make browse`** and **`make browse-npm`** open the project's **GitHub repository** and the **package's page in the npm registry**, respectively, in the **default web browser**.

# Supported platforms

**Linux and OS X**

In principle, any Unix-like platform with `bash` and GNU `make` and otherwise either GNU or BSD utilities.

## Additional prerequisites

* `npm` - as part of a [Node.js](https://nodejs.org/) or [io.js](https://iojs.org/) installation
* if publishing to the [npm registry](https://www.npmjs.com) is desired, an account there
* `git`, a [distributed version-control system](http://git-scm.com/)
* a [GitHub account](https://github.com/)

Note that `bash` is required both for running `make-pkg` itself initially and
later for running the shell commands in `Makefile` via `make` from inside the
projects generated.

# Installation

With [Node.js](http://nodejs.org/) or [io.js](https://iojs.org/) installed, install from the [npm registry](https://www.npmjs.com/package/make-pkg):

    [sudo] npm install -g make-pkg

**Note**:

* Whether you need `sudo` depends on how you installed Node.js / io.js and whether you've [changed permissions later](https://docs.npmjs.com/getting-started/fixing-npm-permissions); if you get an `EACCES` error, try again with `sudo`.
* The `-g` ensures [_global_ installation](https://docs.npmjs.com/getting-started/installing-npm-packages-globally) and is needed to put `make-pkg` in your system's `$PATH`.

# Usage

## Example

The following image shows an example interaction with the series of prompts presented during package initialization:

![Example prompts](doc/images/example-output-prompts.png)

## Initializing a new package

* Create a directory for the new package project.
* `cd` to it.
* Run `make-pkg [<pkg-type>]`
    * [_Experimental_] `<pkg-type>` allows creating a _specific type_ of package:
      * Specifying a type streamlines the initialization process by skipping certain prompts and assuming appropriate default values.
      * Additionally, type-specific prompts may be presented, and the resulting package may have additional features and/or a different internal structure.
      * Conversely, _not_ specifying a type provides the most flexibility for creating a package that contains a JS library and/or a CLI.
      * Types currently supported are:
        * `lib`: a regular Node.js library package without a CLI.
        * `cli`: a CLI-only package, with global installation preferred.
        * `awf`: an [Alfred 2](https://www.alfredapp.com/) [workflow](https://www.alfredapp.com/workflows/) package
      * Refer to the [docs](doc/package-types.md) for details.
    * On first use you'll be prompted to specify required global settings and defaults, such as your GitHub username, your website, and your preferred OSS license.
        * To modify settings later, run `make-pkg -e` from any directory.
    * You'll be guided through a series of prompts to create the `package.json` file.
    * On confirming the intent to create the `package.json` file as presented, the remaining tasks - creating the repositories, instantiating templates, creating stubs - run automatically.
      
**To-dos after the project has been initialized**:

* Flesh out the stub module (in `./lib`) and/or the CLI stub (in `./bin`) with the actual implementation.
  * Note: The specific to-dos may differ for special package types.
* Flesh out the stub tests in `./test`.
* Flesh out `README.md`, making sure to replace instances of "???", the missing-information placeholder.
* Use the `make` tasks as described [above](#package-release-and-maintenance) for releasing and ongoing maintenance.

## Command-line syntax

Find concise usage information below; for more detailed information, read the [manual online](doc/make-pkg.md), or, once installed, run `man make-pkg`.

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```nohighlight
$ make-pkg --help


Initializes an npm-package project in the current directory and  
implements a maintenance and release workflow.
    
    make-pkg [-l] [-f] [<pkg-type>]
    make-pkg -e

    -l            suppresses creation of remote repo on GitHub
    -f            forces running in a non-empty directory
    -e            opens the settings file for editing

    <pkg-type>    [experimental] creates a specific package type:
                  'lib': JS library (only)
                  'cli': CLI (only), global installation preferred
                  'awf': an Alfred 2 workflow

Standard options: --help, --man, --version, --home
```

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# License

Copyright (c) 2015-2016 Michael Klement <mklement0@gmail.com> (http://same2u.net), released under the [MIT license](https://spdx.org/licenses/MIT#licenseText).

## Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have an optional suffix denoting the type of dependency: the *absence* of a suffix denotes a required *run-time* dependency; `(D)` denotes a *development-time-only* dependency, `(O)` an *optional* dependency, and `(P)` a *peer* dependency.


<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## npm dependencies

* [json](https://github.com/trentm/json)
* [doctoc (D)](https://github.com/thlorenz/doctoc)
* [json (D)](https://github.com/trentm/json)
* [marked-man (D)](https://github.com/kapouer/marked-man#readme)
* [replace (D)](https://github.com/harthur/replace)
* [semver (D)](https://github.com/npm/node-semver#readme)
* [tap (D)](https://github.com/isaacs/node-tap)
* [urchin (D)](https://github.com/tlevine/urchin)

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# Changelog

Versioning _loosely_ complies with [semantic versioning (semver)](http://semver.org/).
Since this utility is applied only _once_ to a given package - in order to initialize it -
maintaining compatibility is less important. However, larger changes will be reflected
in higher version-number increases.

<!-- RETAIN THIS COMMENT. An entry template is automatically added each time `make version` is called. Fill in changes afterward. -->

* **[v0.7.2](https://github.com/mklement0/make-pkg/compare/v0.7.1...v0.7.2)** (2016-04-21):
  * [fix] Makefile template: npm registry saved-credentials detection code 
          updated to account for how newer npm versions save them
          (`//registry.npmjs.org/:_authToken=`).

* **[v0.7.1](https://github.com/mklement0/make-pkg/compare/v0.7.0...v0.7.1)** (2015-11-08):
  * [doc] Tweaks to the read-me templates.

* **[v0.7.0](https://github.com/mklement0/make-pkg/compare/v0.6.3...v0.7.0)** (2015-11-08):
  * [behavioral change] The package's directory name is now _invariably_ used as 
    the npm package name.
  * [enhancement] A package name is now checked for being a legal npm-registry package
    name: if not, a warning is given at the first prompt, and, unless a _private_ package
    is being created, initialization will abort.
  * [enhancement, experimental] Support for package types added, both to streamline
    regular package initialization and to add support for specialized packages such as 
    Alfred 2 workflows.

* **[v0.6.3](https://github.com/mklement0/make-pkg/compare/v0.6.2...v0.6.3)** (2015-10-28):
  * [enhancement] An attempt to run tests now bows out gracefully with a status
    messsage (and without error) if the `./test` subdir. is empty (save for hidden items),
    _before_ attempting to invoke `tap` or `urchin`.

* **[v0.6.2](https://github.com/mklement0/make-pkg/compare/v0.6.1...v0.6.2)** (2015-10-28):
  * [enhancement] `README.md`: Added custom badge that marks a project initially as not ready for release;
    to be removed when appropriat later; similarly, for npm-package projects, 
    the npm-version badge is now initially commented out, to be uncommented once the project is published to 
    the npm registry.  
    Streamlined the license badges to always use a custom badge with the license's SPDX ID, which
    also makes it work with npm-package projects not yet published to the registry.
  * [dev] Makefile robustness improved.

* **[v0.6.1](https://github.com/mklement0/make-pkg/compare/v0.6.0...v0.6.1)** (2015-09-15):
  * [enhancement] New Makefile task `make view-man` updates the man page and views it locally with `man`.
  * [fix] Outdated status message at the end of package initialization corrected.
  * [doc] Read-me and man-page improvements.

* **[v0.6.0](https://github.com/mklement0/make-pkg/compare/v0.5.6...v0.6.0)** (2015-09-15):
  * [enhancement] New feature: optional, off-by-default support for creating and installing a man page
    for a package's main CLI - see description of the `make toggle-man` Makefile task in file README.md.
  * [behavioral change] Makefile task `toc` renamed to `toggle-doc`; also, toggling
    no longer attempts to remove an existing TOC on turning off, and no longer automatically adds one on turning on.
  * [fix] `make release` now once again adds the version-specific changelog entries to the Git commit message.
  * [doc] `make-pkg` now comes with a man page; `make-pkg -h` now just outputs concise usage info.

* **[v0.5.6](https://github.com/mklement0/make-pkg/compare/v0.5.5...v0.5.6)** (2015-09-14):
  * [fix] A preconfigured `.gitignore` file is now copied to a new package folder, as it always should have been.

* **[v0.5.5](https://github.com/mklement0/make-pkg/compare/v0.5.4...v0.5.5)** (2015-08-03):
  * [fix] Read-me template now uses ` ```nohighlight ` to fence CLI usage output, which is what the `make update-readme` and `make release` tasks expect.

* **[v0.5.4](https://github.com/mklement0/make-pkg/compare/v0.5.3...v0.5.4)** (2015-07-08):
  * [enhancement] Read-me template improved so as to ensure that CLI usage information is printed without syntax highlighting.

* **[v0.5.3](https://github.com/mklement0/make-pkg/compare/v0.5.2...v0.5.3)** (2015-06-29):
  * [enhancement] Test stub improved.

* **[v0.5.2](https://github.com/mklement0/make-pkg/compare/v0.5.1...v0.5.2)** (2015-06-29):
  * [enhancement] CLI and main-module templates improved.

* **[v0.5.1](https://github.com/mklement0/make-pkg/compare/v0.5.0...v0.5.1)** (2015-06-22):
  * [enhancement] CLI test template for unknown options added; existing templates improved.

* **[v0.5.0](https://github.com/mklement0/make-pkg/compare/v0.4.0...v0.5.0)** (2015-06-16):
  * [change] Pre-release npm tag changed to fixed string 'pre' so as to have that track the most recent pre-release.
  * [change] Pre-release Git tag analogously changed to 'pre'.
  * [fix] `README.md` template is now again correctly expanded in Bash 4.x.
  * [enhancement] New test for test prerequisites added. 
  * [dev] Tests improved to detect corrupt of `README.md` template on expansion.

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
