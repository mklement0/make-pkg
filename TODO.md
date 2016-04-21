
* Ensure awf installation up front (offer it before the first prompt)
* Ask for bundle ID first.
  * If it exists among installed workflows, make sure it is not a symlink - abort, if so.
* Using awf, get description from an existing workflow by that name and use it as the edit-buffer default, prefixed with "An Alfred workflow" or "Alfred workflow: ", which we can strip out when writing back to the workflow's info.plist?
* If an existing workflow is going to be moved, print a clear warning.
* If the user confirms package creation, move the existing workflow silently.

* Publish strlength.awf to the Alfred forums, and explain how it was created to introduce make-pkg-based workflows.
