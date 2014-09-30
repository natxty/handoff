# General Overview and Notes #

This document is _not exhaustive_ nor makes any claim to be comprehensive. It is a generalized overview and a selection of highlights- the most important, difficult, or simply non-obvious elements are included to speed up on-boarding and code assimilation.

In general, the site and code follow MVC and CakePHP conventions, which will provide the foundation for site functionality- and make it much easier to understand and, if necessary, troubleshoot.

Some important points, in no particular order:

### CakePHP Version
* Site code is CakePHP 2.5.2

### PHP Composer
PAMM (Project Agreement/Payment Scheduling/Payment Processing) branches (`feature/pamm` and `tatooine`) use `Composer` for plugin management. Therefore, newer plugins might be found in the `PluginComposer` or `VendorComposer` directories on those branches (not in use on `master`/production branch). Older plugins will be found in the `Plugin` and `Vendor` directories

### CakePHP Themes
`theidealists.com` uses CakePHP's Themes. Most, but not all, the View templates are located in `View/Themed/Bootstrap3/`.
    * Sometimes, if there's no matching view file in the `View/Themed/...` directory, the system will fall back on the `View/...` core to find a match.

### General File Mapping
When trying to find a piece of code, follow these general steps:

* Look in `Config/routes.php` to make sure the URL/action/parameters aren't mapped to a non-obvious controller/action
* Then, proceed along paths consistent with CakePHP's MVC architecture. A Controller's function will be in `View/{Controller Name}/{function}.ctp` by default unless something else is explicitly called - or if the function returns JSON for AJAX calls only.
* Usually, this base view file will have a number of included `elements`, which are easy to trace, and an assortment of javascript and/or backbone.js files- which is not so easy. 
    * In this document, I've tried to point out the most important files/functions that have heavy Backbone.js layers
    * It was on my late-2014 agenda to refactor the template system, as it is fragmented and needs cohesion and unification.
 