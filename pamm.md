# PAMM OVERVIEW

_Note_: For an overview of the critical steps to get PAMM live, see [this document](pamm_preflight.md)

The name PAMM stands of _Project Agreement/Money Maker_, which started as a joke and stuck. You could probably vary that with _Project Agreement/Milestones/Management_ or something similar.

The _PAMM_ process begins directly after a _Client_ awards a _Talent_ from within the _Talentlist_ interface. It is, essentially, a guided process for establishing milestones, payments, deliverables, and terms between the two parties; as such, it does allow and require both parties to "approve" the final agreement. To get to this point, the _Project Agreement_ can pass through both parties' hands a number of times, as each- during their turn- can edit the agreement.

The entire business process has been mapped out in the original Dropbox folder (link); I will take this time to point out some of the non-obvious aspects.

## Audit Logs and Listeners

### Audits for Milestones
PAMM takes advantage of some more advanced CakePHP functionality and a very nice background plugin- taking the latter first, we have implemented [Rob Wilkerson's Audit Log Plugin](https://github.com/robwilkerson/CakePHP-Audit-Log-Plugin) to handle the changes in the Milestones (dates and associated payment amounts). 

The plugin operates invisibly, and is tied to the `Milestones` within the `Model/Milestone.php` file:

```
public $actsAs = array(
        'AuditLog.Auditable'=>array(
            'ignore' => array('balanced_status_credit', 'balanced_status_debit', 'date_edited',),
            'habtm'  => array( 'Pamm' )
        )
    );
```

And a similar declaration in the `Model/Pamm.php` model file. 

You'll find the changes are recorded in the `audits` and the `audit_deltas` tables in the database.

```
mysql> show tables;
..
audit_deltas
audits
...
milestones
```

### Pamm Listener

Utilizing CakePHP's [Event Manager Framework](http://api.cakephp.org/2.3/class-CakeEventManager.html) we built a `Listener` to tie into the "Talentlist Award" event, and initiate the entire PAMM process. You can review the `PammListener` in the `app/Event/PammListener.php` file.

### Backbone Layer

Like most the other major site components, most of the action for PAMM takes place in the Backbone.js files. 

These can be located in the `app/webroot/scripts/backbone/` directory, and will have the term `pamm.` prepending the file's purpose. For example:

```
pamm.dashboard.js
pamm.edit.agreement.js
pamm.edit.agreement.user.js
pamm.edit.budget.js
{etc}
```

These files sometimes reference other required or autoloaded files, so look in the `autoloaded` and `init` directories as well.

The backbone layer pretty much controls the interactive flow, from adding/editing/deleting milestones all the way to processing the payment information. 

Bank information is processed via Balanced Payments (we do not store any financial information on the server), and there are AJAX-access handlers that are used (e.g. `/pamm/user_payment`) to create the user/Balanced account, etc. These can be seen in the Backbone.js code in the various functions and submitHandlers.

### Emails
Most of the communication for this takes place via email. For example, when the _client_ finishes the form and submits/approves... an email is sent to the _talent_, alerting him/her that they've been awarded, and the Agreement is ready to be reviewed. 

Likewise, when the _talent_ comes to the site, and edits the Agreement, an email alert is sent to the _client_ indicating that the _talent_ has edited the Agreement and the _client_ must return to review, edit, and/or approve.

These email processes and templates are initiated in the `Controller/PammController.php` file but are really processed in the `Console/Command/EmailShell.php` file and, most specifically, in the specific task script in the `Console/Command/Task/` directory.


### Approval
To finalize the Project Agreement, both parties must "approve". Approvals are stored as datetime stamps in the `pamms` table:
```
| date_client_approved | datetime     |
| date_talent_approved | datetime     |
```

### Payments

See the [PAMM Payments](pamm_payments.md) page for an overview of the payment processing.


