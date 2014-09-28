# Opportuntity > Talent Email Lifecycle #

[Lifecycle Testing Document](https://docs.google.com/a/theidealists.com/spreadsheet/ccc?key=0AjVZk7pDeorYdHE4d1lxQ2JxdWdIbU9nMUp2MjF3c0E#gid=1) which gives a nice overview of the emails and criteria.

## Cron Job
These are regularly executed via a `cron` job on the `idlrack` server.

```
seneca@idlrack:~$ sudo crontab -l -u www-data
#MAILTO=nathaniel@theidealists.com

30 8,21 * * * backup_path="/tmp/idl-`/bin/date +\%Y\%m\%d\%T`" && mysqldump -uidluser -pz00m_thief_0 idl > $backup_path && /usr/bin/s3cmd --config=/root/s3cmd.conf put $backup_path s3://idl-dbs/ && rm $backup_path

#run daily at 3:00pm PST
0 15 * * * APPLICATION_SERVER="theidealists.com" && /var/www/prod.theidealists.com/public/app/Console/cake email sendOpportunityToTalentDaily -app /var/www/prod.theidealists.com/public/app

# run sitemap at 11:53pm PST
53 11 * * * export HTTP_HOST="theidealists.com" && /var/www/prod.theidealists.com/public/app/Console/cake sitemap generate -app /var/www/prod.theidealists.com/public/app

#run weekly at 5:00pm EST/2:00pm PST
0 14 * * 5 APPLICATION_SERVER="theidealists.com" && /var/www/prod.theidealists.com/public/app/Console/cake email sendOpportunityToTalentWeekly -app /var/www/prod.theidealists.com/public/app

#run reminders at 11:05am EST/8:05am PST
5 8 * * * /var/www/prod.theidealists.com/public/app/Console/cake process sendReminders -app /var/www/prod.theidealists.com/public/app

#run client at 3:15pm PST
0 13 * * * export APPLICATION_SERVER="theidealists.com" && /var/www/prod.theidealists.com/public/app/Console/cake email sendClient -app /var/www/prod.theidealists.com/public/app   >> /var/www/prod.theidealists.com/public/app/tmp/logs/client-emails.log 2>> /var/www/prod.theidealists.com/public/app/tmp/logs/client-emails.log &```
```

## Email Shell and Tasks

* The cake EmailShell is located in the `/app/Console/Command/` directory. 
* Initial/Basic processing occurs there, and then individual email tasks are executed from there. 
* Individual tasks (e.g. `sendNoAwards()`) are located in the `/app/Console/Command/Tasks/` directory.
* Individual tasks will contain template information. Templates for the emails are generally found in '/View/Emails/html/email-3/' directory
	* _Note_:  