# Admin #

Late Winter/Spring, 2014, we did a large upgrade of the admin. To save time, we purchased a template from the [WrapBootstrap service](http://wrapboostrap.com) and built out tools and pages.

## Views ##
In accordance with CakePHP conventions, most of the admin-related functions are located within the separate controllers. For example, `Opportunity`-esqe materials will be in the `OpportunitiesController.php` controller, with the simple prepend `admin_` (e.g. `admin_index()`). 

Therefore, mapping Controller function to View isn't quite as easy as usual.

## Dashboard ##
It is important to note that to generate some of the stats and charts, a new controller was created (`DashboardController.php`) to perform the queries and process the data. This generally maps to the `View/Themed/Boostrap3/Dashboard/admin_index.ctp` file. 

The purchased template comes with a load of javascript libraries and dashboard-scripts. These are found, separated, in `webroot/js/admin/*`

## Datatables ##
`jQuery` `datatables` plugin is used to present most of the data in these views. While most of the implementation is standard, some of the filters and sorts are processed is an abstracted way, and needs review/consideration: 

Consider looking at [the admin opportunity index](http://theidealists.com/admin/dashboard#/admin/notifications/opportunities).

Fields in the rendered dataTable, like `Status` in the `dataTable` declaration of `webroot/js/admin/admin.opportunity.index.js` (lines 174 - 195), make an AJAX POST to `admin/dashboard/opportunity` which is handled by `admin_opportunity()` in the `DashboardController.php`. 

This function calls on another class function `sanitizeTableRequest($modelName)` which does some Model work _and also_ handles some of the request data (`$this->request->data`), significant portions of which are passed from the dataTables object on the requesting page.

That sounds complicated, but the takeway is this: _sometimes it isn't obvious from looking at the Controller functions what data is being processed_, and you can find out more by examining the dataTables data that originates the request. 

## Search ##
Generally speaking, the "quick search" that appears on top of the [Admin Users](http://theidealists.com/admin/dashboard#/admin/users) pages queries the MySQL database using the CakePHP ORM.

The _Basic Search_ and _Advanced Search_ located on the [Members > Search](http://theidealists.com/admin/dashboard#/admin/users/advanced_search) page are hooked up with our **Solr** instance and query via our `IdlSolr` plugin methods.

...