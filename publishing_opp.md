# Publishing an Opportunity #  

### Publishers only ###
After a client submits an opportunity, the record is saved un an `unpublished` status and is only viewable by admins. After review (and possible editing) a _publisher_-enabled admin (`group_id = 4`) can publish from either the site admin or from the detail view itself. Normal admins can view, and edit, but not publish. Then:

### Cake Console ###  
After some pre-processing, _publishing_ will execute a shell command for a cake console action, using ` $this->dispatchShell('email','sendOpportunity',array('id'=>strval(intval($id))));`
 
This is a background process to query for users who fit the publishing criteria and send them individual emails through the `Postmark API`

#### Logging the emails ####
Comprehensive logging occurs on this action, due to the variety of issues that can arise with email sends, 3rd Party APIs, and the like. A running log is kept of the console outputs (`client-emails.log` and `console-emails.log`), and each individual error or non-success response is logged individually. A quick section of the logs folder:

	# /var/www/prod.theidealists.com/public/app/tmp/logs   
	-rw-rw-r--   1 www-data www-data    20685 Sep 26 13:00 client-emails.log  
	-rw-r--r--   1 www-data www-data   203403 Sep 26 13:47 console-email.log
	-rw-r--r--   1 www-data www-data  7140849 Sep 26 14:56 console-process.log
	...
	drwxrwxrwx   4 www-data www-data     4096 Aug 26 07:55 email
	...

In the `email` directory seen above, there are subfolders for the `STD` (stdout) and `errors`

	drwxr-xr-x 3 www-data www-data  4096 Aug 26 07:55 errors
	drwxrwxrwx 2 www-data www-data 20480 Sep 26 13:47 STD

You can look through these to see individual responses and errors returned from the Postmark API.


