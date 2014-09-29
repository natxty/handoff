# Admin #

Late Winter/Spring, 2014, we did a large upgrade of the admin. To save time, we purchased a template from the [WrapBootstrap service](http://wrapboostrap.com) and built out tools and pages.

## Views ##
In accordance with CakePHP conventions, most of the admin-related functions are located within the separate controllers. For example, `Opportunity`-esqe materials will be in the `OpportunitiesController.php` controller, with the simple prepend `admin_` (e.g. `admin_index()`). 

Therefore, mapping Controller function to View isn't quite as easy as usual.

## Dashboard ##
It is important to note that to generate some of the stats and charts, a new controller was created (`DashboardController.php`) to perform the queries and process the data. This generally maps to the `View/Themed/Boostrap3/Dashboard/admin_index.ctp` file. 

The purchased template comes with a load of javascript libraries and dashboard-scripts. These are found, separated, in `webroot/js/admin/*`

## Datatables ##

## Search ##


        * Views
            * Dashboard
            * Opportunities
                - Overviews/Tables
                - Single
            * Users
                - Overviews
                - Individual
            * Search
                - DB query vs. Solr
        * Template // datatables // javascript