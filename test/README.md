
* Tests require `bash` and [`expect`](http://expect.sourceforge.net/) to run.

* Tests cover package *creation*, but do not exercise the Makefile tasks
  once a package has been created - that would require a lot more effort.

* For testing package creation, a *static* settings file that uses feature
  defaults is currently used, which means that off-by-default features such as
  enabling man-page creation (setting `vMAN_ON`) are not exercised.
