# Postmark #

The site sends emails via the [Postmark service](https://postmarkapp.com/). This is a _Transactional Email Service_ so we don't send our newsletters or batch communications through this (rather, we use _MailChimp_). 

That said, we do send out 1000 - 3000 emails per Opportunity published, and these are routed through **Postmark**.

Some messaging is handled by the SHIFT transport, and I am pretty sure at one point there was an overlap in code, so some normal mail operations were coded into SHIFT code... but looking now, I cannot remember which.

[SHIFT is described elsewhere](shift.md).

Details for email setup are initiated in `Config/email.php` but you'll find the main plugin/components code here:

```bash
Plugin/Postmark/Lib/Network/Email/PostmarkTransport.php
Plugin/Shift/Network/Email/ShiftTransport.php
Controller/Component/PostmarkComponent.php
Controller/Component/ShiftcoComponent.php
```

Emails are logged comprehensively (see **Logging the emails** on the [Publishing an Opportunity page](publishing_opp.md)).

Most emails are also logged to the `Notifications` table, and are viewable from there, and in the admin.

Email templates contain a small _tracking pixel_ which helps record "opens", but this is a simple device and prone to error. 

**Postmark** just released an _open tracking_ addition, and opens can be seen in the Postmark _Activity_ admin; however, we have not tapped into this for our own email tracking. Yet.