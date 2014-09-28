# QA Notes

* clear cache on tatooine when Opp is published
    * also, when opp is auto-generated
    * what is the Caching scheme we use?

* Need to trace out PAMM // Balanced Payment methods
    * View -> Backbone
    * When do we send/get info from Balanced
    * How do we store user info (at Balanced)
        * therefore, how do we retrieve it?

* When you submit a payment - return to Dashboard, doesn't update
    * Where is it stored? Is it not working?
    * actually, this seems to work for recent PAMMs
    * is there a lifespan on Balanced Testing?
    * or some discrepancy from the older code < new code?
    

* `cake IdlBalanced.payment checkPayments` runs at 1pm

* review iPad view(s) with new "Agreements" section in header

* add a visit() to the `generateTestingOpportunity`

* `OpportunitiesController.php` lines 50, `Cache::read('all',
'opportunity.key')` = reading from the cache...
    * actually, seems to be in the variation on tatooine where `function
    afterSave()` (line 275, `Model/Opportunity.php`) ... cycle through
    the groups? ... investigate...
* ^^ turns out to be a FUCKING TYPO... 

* Will still be needing some assets from Adam & Co.:
    * Standard NDA Language
    * Standard contract Language

* When you submit payment info (Client, PAMM 1st screen) and the section
goes blank .. need a spinner or animation there... 

* When the Talent (or 2nd party) clicks the link and arrives on the PAMM
Project Page the 1st time - immediately cycled down to Payment info
(w/error) . not cool

* on `/pamm/dashboard` page, we have "Your Projects" as the title, same
as when a client looks at the "Projects" page (top nav link)
`/members/dashboard`. Clarity for names?

* check BalancedPayments plugin to see getLastPaymentMethod

* PAMM has an added _Behavior_ to help track changes to the
Payment/Milestones, from the installed `Audit Log` plugin
(https://github.com/robwilkerson/CakePHP-Audit-Log-Plugin). 

* PammController.php has, on line 116, a function `getContactName()`
which should probably be `getContractName()` ... 

* The final PAMM approval email has "Need bank info" language in it --
needs to be removed... 

* The client has been invoiced on your behalf --> email seems to have
HTML in its summary (in Mailbox, how about on Gmail?)

* On the "Due in 3 days" the header says "Current Payment Due" howver
the language says "The following payment is due in three days:" .. can
this be made clearer?

* getting a Javascript "Uncaught TypeError: undefined is not a function " when on
PAMM pages- most recently, on a Client's Opp Dashboard, clicking "View
Pitches"

* Important to note that is a milestone is created with a due date less
than 3 days hence, the 3-day alert will not go out. Is this necessary to
make a onSubmit() check?

* clean up process for old/unused/unfinished PAMMs?

* Logging out in Firefox - working/not working?
    * tried on 9/25 --> seemed to work ok


* tomorrow (9/25) -- try to pay 2 at once. 

* seems for the 9/24 ones that I got 2 notifications for ... i must have
hit submit twice, bc this hasn't happened again. We may need to add
a message, or a prevention from double-submitting.

* Don't forget to add info about the Blog to the instructions

* check on the submitted/approved/deposited timing sequence

* also, in the approval emails, HTML parsing? ("Client -&gt; Award -&gt;
PAMM -&gt; submit")

* When filling out Payment/Milestones in PAMM :
    * I can manually enter a past date (waiting on validation...)
        * Let's it go by ... 
        * Seems to create an issue -- 
        * when client looks at Agreement - no Milestones at all!
        * ..and no budget!
    * when my on-the-fly sum() is less than 2000 (error msg, good) but
    when I add a new milestone (that puts the budget > 2000) need to
    clear the error msg

* still sometimes have issues with activating the "deliverables" section
with a mouse-click/enter the field...

* Might need to sort out `/admin/pamm` page - what the different 'Client
Approved' columns mean

---


**Attempting to process 2 payments at once: 9/26/2014 :: Star Murray
(dev+star@theidealists.com).. 9/24 payment + 9/25 payment **

    * sweet ... 9/24 -> PAID; 9/25 -> Pending... interesting...

** Paying an older one: 9/10/2014 to Nkhink Clarkhing: **
    * note: older attempt still shows PENDING
    * 9/10 version shows PENDING now (9/26) .. check back?

