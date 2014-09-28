# General Overview and Notes #

This document is _not exhaustive_ nor makes any claim to be comprehensive. It is a generalized overview and a selection of highlights- the most important, difficult, or simply non-obvious elements are included to speed up on-boarding and code assimilation.

In general, the site and code follow MVC and CakePHP conventions, which will provide the foundation for site functionality- and make it much easier to understand and, if necessary, troubleshoot.

Some important points, in no particular order:

* Site code is CakePHP 2.5.2
* PAMM (Project Agreement/Payment Scheduling/Payment Processing) branches (`feature/pamm` and `tatooine`) use `Composer` for plugin management. Therefore, newer plugins might be found in the `PluginComposer` or `VendorComposer` directories on those branches (not in use on `master`/production branch). Older plugins will be found in the `Plugin` and `Vendor` directories
*  