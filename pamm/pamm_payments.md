# PAMM #
## Payments ##

Processing of payments in PAMM happens with a combination of files and methods. The basis of the the payment page comes from the `/View/Themed/Bootstrap3/Pamm/payment.ctp` file, but the majority of the action takes place in the _Backbone.js_ files located in `/webroot/scripts/backbone/view/user.balanced.js` and a few others.

### Milestone Payments ###   

When a client is ready to make a payment, they will proceed to the specific payment page for that PAMM (e.g. `/pamm/payment/{opportunity-slug}#pay-{milestone id}`). THe client will toggle the first column (toggling the class `pay-row`) for each and every payment they wish to process. 

### Payment Method Details ###

If the client has a payment method or methods from a previous PAMM (which we look up, via the Balanced API, using the client's email), the most recently used method will be rendered on page load. If not, the client must enter and have Balanaced confirm the credit card/bank account. An existing payment method can be changed at this point as well.

### Submit // Processing ###

The `user.balanced.js` file maps the "Use this method" button/event to the `successModel` function. Basically, this creates an object ,taking each `.pay-row` id and a few other parameters, and passes it to  _tasks_, the processes all the tasks. 

