<!-- Remove, once published/released. -->
![project status - not ready for release](https://img.shields.io/badge/status-not_ready_for_release-red.svg)

$([[ $vPRIVATE != 'true' ]] && cat <<EOF1 || cat <<EOF2
<!-- Uncomment first link, once published to the npm registry. -->
<!-- [![npm version](https://img.shields.io/npm/v/${vNAME}.svg)](https://npmjs.com/package/${vNAME}) --> [![license](https://img.shields.io/badge/license-${vLIC_SPDX_ID//-/--}-blue.svg)](${vREPO_URL%.git}/blob/master/LICENSE.md)
EOF1
[![license](https://img.shields.io/badge/license-${vLIC_SPDX_ID//-/--}-blue.svg)](${vREPO_URL%.git}/blob/master/LICENSE.md)
EOF2
)

<!-- START doctoc -->
<!-- END doctoc -->

# ${vNAME}: ${vDESCR}

**${vName}** is an [Alfred 2](http://www.alfredapp.com) [workflow](https://www.alfredapp.com/workflows/)

# Installation

## Prerequisites

 * OS X
 * [Alfred 2](http://alfredapp.com) with its paid [Power Pack](https://www.alfredapp.com/powerpack/) add-on.

$( [[ $vPRIVATE == false ]] && cat <<EOF1
## Installation from the npm registry

 <sup>Note: Even if you don't use Node.js itself: its package manager, `npm`, works across platforms and is easy to install; try  
 [`curl -L http://git.io/n-install | bash`](https://github.com/mklement0/n-install)</sup>

With [Node.js](http://nodejs.org/) installed, install [the package](https://www.npmjs.com/package/${vNAME}) as follows:

    [sudo] npm install -g ${vNAME}

**Note**:

* Whether you need `sudo` depends on how you installed Node.js and whether you've [changed permissions later](https://docs.npmjs.com/getting-started/fixing-npm-permissions); if you get an `EACCES` error, try again with `sudo`.
* Alfred 2 will prompt you to import the workflow - select a category (optional), and confirm.
* After importing, proceed with [customization](#customization) below.
EOF1
)

## Manual installation

* **Click [here](https://github.com/mklement0/${vNAME}/blob/stable/archive/${vAWF_BUNDLEID}.alfredworkflow?raw=true)** to download the installer.
* Open the downloaded file: Alfred 2 will prompt you to import the workflow - select a category (optional), and confirm.
* After importing, proceed with [customization](#customization) below.

# Customization

Note:

* Custom hotkeys and modified keywords are _retained_ on reinstallation / upgrade.
* To customize a workflow again later, click on Alfred's menu-bar extras icon,
(the bowler hat symbol), select `Preferences...`, switch to the `Workflows` tab
at the top, then locate the workflow in the list on the left.

## Hotkeys (keyboard shortcuts)

Alfred initially installs a workflow without actual hotkeys, so you must
assign them manually:

* Double-click the `Hotkey` element(s) of interest, click on the `Hotkey:` input field,
and press the desired key combination.

## Keywords

Optionally, you may customize the keyword(s) a workflow comes with:

* Double-click the `Keyword` or `Script Filter` element(s) of interest and
modify the value of the `Keyword:` input field.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# License

## Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have an optional suffix denoting the type of dependency: the *absence* of a suffix denotes a required *run-time* dependency; `(D)` denotes a *development-time-only* dependency, `(O)` an *optional* dependency, and `(P)` a *peer* dependency.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## npm dependencies

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# Changelog
