# make-pkg(1) - initialize and maintain npm package projects

## SYNOPSIS
 Initializes an npm-package project in the current directory and
 implements a maintenance and release workflow.
    
    make-pkg [-l] [-f]
    make-pkg -e

    -l            suppresses creation of remote repo on GitHub
    -f            forces running in a non-empty directory
    -e            opens the settings file for editing

 Standard options: `--help`, `--man`, `--version`, `--home`

## DESCRIPTION
 Initializes a GitHub-based npm-package project in the current directory and  
 implements a maintenance and release workflow based on Makefile tasks.
 
 To create a new package project:

 * create a directory for it
 * change to that directory
 * run `make-pkg` there.


 On first run you'll be prompted for settings, including creation of a GitHub  
 authorization token.

 In addition to initializing package-configuration file 'package.json'  
 through a series of prompts, this utility:

  * initializes a local Git repository, adds a .gitignore file, and defines  
    the remote 'origin' repo.
  * creates a matching remote repository on GitHub.
  * instantiates several template files so as to provide a starting point for  
    a read-me file, license file, ...
  * installs stubs for the main module, the CLI, if applicable, and tests.  
  * installs npm dev dependencies for managing the new package.
  * installs a Makefile with a set of tasks for releasing and maintaining the  
    package.


 Run `make list` in a package folder to see a list of all Makefile tasks.

 While this utility is typically used to create packages for publishing at  
 the npm registry (https://www.npmjs.com/), it can be used to manage  
 GitHub-based OSS projects in general.

 For more information, visit https://github.com/mklement0/make-pkg

## OPTIONS
 * `-l`  
  Local processing only: no attempt is made to create a matching  
  repository on GitHub.

 * `-f`  
  Forces execution in the current directory, even if it is not empty.  
  Note that existing files, including a Git repository, are left untouched,  
  'package.json' being the only exception.

 * `-e`  
    Opens this utility's settings file, `~/.make-pkg-rc`, for editing.

## STANDARD OPTIONS
 All standard options provide information only.

 * `-h, --help`  
    Prints the contents of the synopsis chapter to stdout for quick reference.

 * `--man`  
    Displays this manual page, which is a helpful alternative to using `man`, 
    if the manual page isn't installed.

 * `--version`  
    Prints version information.
    
 * `--home`  
    Opens this utility's home page in the system's default web browser.

## LICENSE
 For license information and more, visit this utility's home page by running  
 `make-pkg --home`.
