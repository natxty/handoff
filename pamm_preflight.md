# PAMM Preflight
## Requirements/Tasks for Launch

[JIRA Board Link](https://theidealists.atlassian.net/secure/RapidBoard.jspa?rapidView=1&selectedIssue=NEW-2835&sprint=69) to list of tasks, from UI edits to _Critical_ ones


**Critical Fixes/Tests**

* FIX: Manually Adding Past Dates in Milestone Error
* FIX: Messaging in Final Approved Email
* FIX: Prevent user from double-submitting Payment
* ADD: Standard Language to NDA and Terms section
* TEST: Switch Balanced API keys to Live Marketplace, test with small denomination transactions (e.g. $1 Milestone)
    * Test multiple cards & bank accounts
    * Catch any unusual errors

**Important Code Prep**

* Revert `develop` branch to pre-Matchmaking (`hoth`) integration
* merge `tatooine` into `develop`
* regression testing/etc.

**Final Steps**

* after regression/fixing:
* create new release branch (`release/3.6`)
* merge `develop` -> `release/3.6`
* push `release/3.6` to remote repo
* (Jenkins will auto-deploy to staging)
* _Final Visual Check_
* merge `release/3/6` -> `master`
* push `master` to remote repo
* (Jenkins will auto-deploy to production)




