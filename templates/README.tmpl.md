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

# ${vNAME} &mdash; ${vDESCR}

${vDESCR}

# Installation

$({ [[ $vPRIVATE == false && $vPREFERGLOBAL == true ]] && 
cat <<EOF1; } || { [[ $vPRIVATE == false ]] && cat <<EOF2; }
## Prerequisites

* When installing from the **npm registry**: $(
    (( ${#vOS_ARRAY[@]} == 0 )) && { echo 'all platforms supported by Node.js / io.js'; exit; }
    friendlyList=$(printf '%s\n' ${vOS_ARRAY[@]} | sed 's/^!\{0,1\}\(.*\)$/\1/' | mapKeysToValues ${kOS_NAMES[@]} ${kOS_IDS[@]} | sed 's/^\(.*\)$/**&**/' | toNaturalLanguageList)
    [[ ${vOS_ARRAY[0]} == '!'* ]] && { echo 'all platforms supported by Node.js / io.js **except** '$friendlyList; exit; }
    echo $friendlyList)
* When installing **manually**: ???

## Installation from the npm registry

With [Node.js](http://nodejs.org/) or [io.js](https://iojs.org/) installed, install [the package](https://www.npmjs.com/package/${vNAME}) as follows:

    [sudo] npm install ${vNAME} -g

**Note**:

* Whether you need `sudo` depends on how you installed Node.js / io.js and whether you've [changed permissions later](https://docs.npmjs.com/getting-started/fixing-npm-permissions); if you get an `EACCES` error, try again with `sudo`.
* The `-g` ensures [_global_ installation](https://docs.npmjs.com/getting-started/installing-npm-packages-globally) and is needed to put `${vBIN_FIRST}` in your system's `\$PATH`.

## Manual installation

* Download [the CLI]($(v=${vREPO_URL%.git}; printf %s ${v/\/github.com\//\/raw.githubusercontent.com\/}/stable/bin/${vBIN_FIRST})) as `${vBIN_FIRST}`.
* Make it executable with `chmod +x ${vBIN_FIRST}`.
* Move it or symlink it to a folder in your `\$PATH`, such as `/usr/local/bin` (OSX) or `/usr/bin` (Linux).
EOF1
With [Node.js](http://nodejs.org/) or [io.js](https://iojs.org/) installed, install from the [npm registry](https://www.npmjs.com/package/${vNAME}):

    npm install ${vNAME}

To add this package to a package project of yours as a runtime / dev / optional dependency, append `--save` / `--save-dev` / `--save-optional` - see `npm help install` for details.
EOF2
)

# Usage
$( [[ -n $vBIN_FIRST && $vMAN_ON == true ]] && cat <<EOF

Find concise usage information below; for complete documentation, read the [manual online](doc/${vBIN_FIRST}.md), or, once installed, run `man ${vBIN_FIRST}` (`${vBIN_FIRST} --man` if installed manually).
EOF
)
$( [[ -n $vBIN_FIRST ]] && cat <<EOF

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```nohighlight
\$ ${vBIN_FIRST} --help
```
EOF
)

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# License

## Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have an optional suffix denoting the type of dependency: the *absence* of a suffix denotes a required *run-time* dependency; `(D)` denotes a *development-time-only* dependency, `(O)` an *optional* dependency, and `(P)` a *peer* dependency.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## npm dependencies

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# Changelog
