# Members Applications #

## Code ##
Mosst of this page (`/members/apply` on the site) follows CakePHP architecture ... in `UsersController.php` the function in question is `apply($id=null)`, and the main View template is `View/Themed/Bootstrap3/Users/apply.ctp`, though you will note a large number of included elements, e.g. :

```php
<p class="form-legend">Are you applying as an individual or on behalf of a company?</p>
<?php echo $this->element('Users/Edit/rowCompany'); ?>
<?php echo $this->element('Users/Edit/rowCountry'); ?>
<?php echo $this->element('Users/Edit/rowSlug'); ?>
<?php echo $this->element('Users/Edit/rowPrivate'); ?>
```

Most of the javascript is located within the included elements or in the `webroot/js/profile.js` file. 

## Portfolio Assets ##
A few different forms, with specific actions:
* Uploaded images (uploaded to S3, filename/path saved in database)
* Youtube/Vimeo links (processed by the `AssetController.php` and a custom component or two) ... id/hash is stored in our database
* Images from other services (we've tapped into the Behance & Dribbble APIs)
    * using the Talent's unique URL (we parse for the handle/id) - we can import a batch of images from another service
    * These are then stored on _our_ S3 storage, as the first (_uploaded_) type listed above

## User Application Overview ##
Much of this form is self-explanatory (e.g. Profile Image will upload a profile image, and CSS is used to create the circular border). 

Some of the areas that might need a word or two of explanation:

#### Idealists Profile Link ####
This is a unique "slug" for a user's URL (e.g. `http://theidealists.com/pugdesign`). When the Talent enters a value, there's an AJAX call on-the-fly to check if the slug is unique. 

#### Disciplines, Skills, and Industry Experiences ####
An inidividual Talent can pick up to 3 **Disciplines** (Design, Video/Photo, PR/Social, Writing, Web/Tech, Experiential); a Company can pick up to 6. These are used primarily as _Notification Settings_ - that is, when Opportunities are posted, they will only be sent to Talent who have selected the matching Discipline. 

**Skills** are a free-form text input with an auto-complete and a _jQuery tag-it_ library attached. Skills are stored in the database, and a large number of them are marked as _suggestions_ - which means they will be present in the autocomplete drop-down if a user enters a matching initial 2-char set. 

There are a number of regex patterns also stored in the _skills_ database table - as "bad words". A match with one of these regex will produce a "Bad Word!" alert. e.g.

```mysql
+----+------------------------+----------+------------+
| id | name                   | bad_word | suggestion |
+----+------------------------+----------+------------+
|  1 | \bahole                |        1 |          0 |
|  2 | \banus                 |        1 |          0 |
|  3 | \bas+h[o0]le?s?        |        1 |          0 |
|  4 | \bas{2,}\b             |        1 |          0 |
```


#### Budget Slider ####
A highly customized slider, most of the slider functions are located in the above-mentiond `profile.js` file. This is used to set the range of budgets for which a Talent will receive notifications.

