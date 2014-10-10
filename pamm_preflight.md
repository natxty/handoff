# PAMM Preflight
## Requirements/Tasks for Launch

**Critical Fixes/Tests**

* FIX: Manually Adding Past Dates in Milestone Error
* FIX: Messaging in Final Approved Email
* FIX: Prevent user from double-submitting Payment
* ADD: Standard Language to NDA and Terms section
* TEST: Switch Balanced API keys to Live Marketplace, test with small denomination transactions (e.g. $1 Milestone)
    * Test multiple cards & bank accounts
    * Catch any unusual errors

**Important Code Prep**

* Revert `develop` branch to pre-Matchmaking (`hoth`) integratin
* merge `tatooine` into `develop`
* regression testing/etc.


