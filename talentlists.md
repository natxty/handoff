# Talentlists #
Talentlists make up the bulk of post-publishing activity. After Talent _pitches_ for an Opportunity, their pitch and user info/references are saved to several `talentlist_*` tables. 

## Main Data Structures ##
* **talentlists**: core table for the _Talentlist_ as relates to _Opportunity_. Includes client info and stats. Is referenced by all the following tables.
* **talentlist_awards**: If an oportunity is awarded to a Talent, it is recorded here. Because an Opportunity can have multiple awardees, this is a separate table.
* **talentlist_members**: Users who pitched for an Opportunity are recorded in this table, with their specific pitch content.
* **talentlist_member_comments**: Admins and clients used to comment on Talentlist members, and those comments were stored here. Not used anymore.
* **talentlist_member_links**: URLs entered on a Talent pitch stored here.

### Code

Beyond the typical controller and view files, most of the action takes place in the Backbone.js files `webroot/scripts/backbone/pitch.js`, `webroot/scripts/backbone/pitch.list.js`, `webroot/scripts/backbone/pitch.list.item.js`.


