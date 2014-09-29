### ACL ###

We use CakePHP's "baked in" ACL modules, and you can find the data in the database tables `acos`, `aros` and `aros_acos`.

But for ease of administration, we use the `AclExtras` [CakePHP Plugin](https://github.com/markstory/acl_extras).

Currently, the roles are fairly simple:

Role Name     | Group ID        | Notes
------------- | -------------   |----------
Admin         | 1               | _can't publish opportunity_
Client        | 2               | 
Talent        | 3               | 
Publisher     | 4               | _CAN publish opportunity_


On deployments, we run `update_acl=True` to make sure now code/components are added to the access list structure.




