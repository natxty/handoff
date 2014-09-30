# Project Calculator / SOW #

## SOW Topics ##
* Base Structure
* Data
* Payment Pages & Artifacts
* Drafts
* Publishing an Opportunity

Tied to the `Opportunity` Model/Controller, the interactive form was originally called a _Statement Of Work Calculator_, or _SOW_ for short. The name has stuck internally, even though the client-facing form now reads "Project Calculator". These titles will be used interchangeably.

This was originally a much simpler form. 

### Base Structure

The main code block for the `/creative-talent` page is in the `post()` function in the `OpportunitiesController.php` file. Here you will find the processing for the final submission/save, as well as the data prep for the View. 

The associated view template, `View/Themed/Bootstrap3/Opportunities/post.ctp` is basically a _stem_, and has a list of included elements:

```php
<?php echo $this->AssetCompress->script('post.js',array('inline' => false));?>
<?php echo $this->element('/Opportunities/main'); ?>
<?php echo $this->element('/Opportunities/preview'); ?>
<?php echo $this->element('/Opportunities/olark'); ?>
```

That said, the majority of the action takes place in the Backbone.js files:

```
webroot/js/opportunity-backbone.js
webroot/js/opportnity-functions.js
```

### Data
The questions, dependent answers, and pricing data/modifiers are all stored in a series of tables (and there are corresponding Models):

```mysql
deliverable_type_answers
deliverable_type_questions 
deliverable_types
deliverable_types_questions
deliverables
deliverables_disciplines
```

In a basic sense, it's a "tree" formation, with a set of main parent questions, several levels of "child" questions that will render based on specific answers to the "parent" questions. It's only 3-4 levels deep, and ultimately the user chooses an answer that has a price associated with it.

Top-level data is loaded on page load. Later data is loaded asynchronously as the user makes selections. 

### Payment and Payment_Proceed Pages
These are essentially artifacts of when we charged to post a project (hence the names). We've retained them to complete the process of creating a _Client Account_, so here is where the user will enter name/email/etc in order to complete the submission.

### Drafts
A user can elect to save their SOW as a Draft... this essentially saves the Opportunity to the `opportunities` table, with the `is_draft` column = 1. The user is sent an email with a hash/link to the Draft.

Admins can jump in and edit a draft, and re-save as Draft or complete the submission. It is important to make sure that the Admin enters client information, so the Talentlist and subsequent notifications can be generated.

### Publishing
When a user submits a finished Opportunity, it has an _unpublished_ status. A **Publisher** admin must review and publish. _Note: not all admins have publishing capabilities_. 

When a Publisher does review/publish, a certain amount of information is saved (datetime, etc) and then a shell command is executed to process/send out notifications. See [Publishing an Opportunity](publishing_opp.md) for more details.
