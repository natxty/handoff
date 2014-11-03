# Matchmaking

## Overview / General Notes
For general background, refer to the [Advanced Search & Matchmaking Document](https://docs.google.com/a/theidealists.com/document/d/1_81WJnxfdbrHEMtD4nN57bjtaQXqbSkGJWsh39C7EzQ/edit)


## Extra Components
* **Solr**
* **Redis** 

## Process
### Pre-processing
An initial bulk scan/process was (and will be) required to collect & calculate the necessary data and store it in an accessible format.
* **Talent Factors** are [listed here](https://docs.google.com/a/theidealists.com/spreadsheet/ccc?key=0AjVZk7pDeorYdFhCWXFWdDhhbWNlTmNSU2RyRGRNYWc#gid=0).
* These _factors_ are stored in a combination of places: within the **Solr** index (see the _schema.xml_ and the _data-import.xml_ for SQL and calculations) or within key/value pairs in _Redis_.
* There are several datasets or database tables of particular importance to the Matchmaking process- for example, we map talent **skills** to **deliverable_types**, and we categorize **company names** with industry categories (e.g. Coca-Cola -> Food & Beverage). This data needs to be continually refined and expanded in order to increase the accuracy of matchmaking.

### General Matchmaking Process
There's a pretty good set of explanations [in this document](https://docs.google.com/a/theidealists.com/document/d/1BLXyvyLmBpyZuHj4yRKBJuVGXYjmQXrEnnzpVYE0jzo/edit). But here's an overview:

* Client submits a project via `/creative-talent`, just like normal
* The project text and links are scanned for _company mentions_ and _skill mentions_ as well as mapped to a set of skills relevant to the deliverable.
* The system then processes all talent who are signed up to receive notifications for this discipline/deliverable-type: each user is assessed on a set of criteria, and a match % is calculated from a weighted average of all these criteria.
    * Here is the current [weights worksheet])https://docs.google.com/a/theidealists.com/spreadsheets/d/14OFhe48v0Yu8bn59AFJ7GyYejANCYFyvWp5xFphwFf8/edit#gid=0) we were using at the end of development
    * Weights are set in the code via the `Configure::write()` method and can be found in the `app/Config/bootstrap.php` file, lines 750 - 824 of the `hoth` branch of the code.
    * Some of these weights have more complex sub-calculations, for example, [Calculating Job Title/Company Scores](https://docs.google.com/a/theidealists.com/document/d/1t78iUdEOp1a5igTTGN5FphI72UMVvB7Ln7KMuA3nm94/edit)
    * At the time development was paused, we were _tweaking_ these weights and adding filters (e.g. if a Talent had _no skill matches_ they were capped at a 25%). This is still an area for improvement
*  To make the match scores more palatable to users, we then apply a simple scaling function, keeping the max score at 80% and adjusting everything else proportionately. (We leave the last 10% for appraising the Talent pitch).
*  Talent is notified of the Opportunity in the normal method, but their pitch score is included (though only as one of three categories - Good, Fair, or Poor). They do _not_ see the actual number%.
*  These scores are saved in Redis for later lookup.
*  Users who do not receive the initial notification but access the Opportunity on the site have their score calculated on-the-fly (which are also, then, stored in Redis).
*  Scores for all the recipients are available to the Admin within the Admin/Opp Details page ([for example](http://hoth.theidealists.com/admin/dashboard#/admin/opportunities/details/2886)) in the Matchmaking tab.

### Scoring the Pitch
One of the last things we were working on was appraising the Talent Pitch for keywords, and giving an additional 10% of possible boost. Talent's Pitch is scanned for company names, industry mentions, and similar keywords. 

This calculation is described in more detail [here](https://docs.google.com/a/theidealists.com/document/d/19MBYK5AVPHgMpnVIhIrR3QUYZih5MjlRRpCg9W7cKXI/edit).

## Where does this all happen?
A great walk through of the Solr and Redis population can be seen in `/app/Console/Command/ProcessShell.php` in the `updateSolrRedisInfo()` function (line 585, _hoth_ branch). 

Heavy leverage of the **Solr** index for getting/calculating matches takes place primarily in the `app/model/Behavior/MMBehavior.php` file

