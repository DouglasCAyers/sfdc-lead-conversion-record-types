Overview
========

As of this writing, the Salesforce Lead Conversion process does not
allow users to pick the record types for the converted accounts/contacts/opportunities.

Salesforce instead uses the 'default' record type for those objects of the user's profile
who is performing the conversion. This assumption is not always valid, particularly
for users that have multiple record types available to them or that the ones they
use aren't actually flagged as 'default' on their profile (for example, the user's profile
has the master record type as default but the record types they actually use are granted
through permission sets).

This trigger is a workaround that leverages a custom setting to provide administrators
a way to provide simple one-to-one record type conversion mappings while still using the out-of-the-box
lead conversion page.

Simply put, this trigger looks up a xref from the custom setting based on the lead's record type
then for each converted object (account, contact, opportunity) it updates their record types to
the values specified in the custom setting.

On the **Salesforce Success Community**, the ability to choose a record type is a highly requested feature,
and I hope that this workaround will eventually not be necessary:
* https://success.salesforce.com/ideaView?id=08730000000Bra8AAC
* https://success.salesforce.com/answers?id=90630000000gvCcAAI

Read the Salesforce Docs on **Lead Conversion Process** to learn more:
* https://help.salesforce.com/HTViewHelpDoc?id=leads_notes.htm


Getting Started
===============

To implement this workaround in your salesforce org you will need to create a custom setting and deploy
the apex classes in this repository. The steps are very easy to do, and I'll outline the basic steps below.

1. Create a [Custom Setting](http://help.salesforce.com/apex/HTViewHelpDoc?id=cs_about.htm&language=en_US)
2. Create an [Apex Class](http://help.salesforce.com/apex/HTViewHelpDoc?id=code_define_package.htm&language=en_US)
3. Create an [Apex Trigger](http://help.salesforce.com/apex/HTViewHelpDoc?id=code_define_trigger.htm&language=en_US)
4. Create an [Apex Test](http://help.salesforce.com/apex/HTViewHelpDoc?id=code_run_tests.htm&language=en_US)


Create Custom Setting
---------------------

The custom setting is our cross-reference mapping between Lead record types and Account, Contact, and Opportunity
record types that we want to leverage during lead conversion process. The **name** field of the custom setting
will hold our Lead record type names and the **three fields** on the custom setting will hold the names of the
actual record types we want to use for those sobjects during lead conversion.

![custom settings](/images/custom_settings.png)


Manage Custom Setting
---------------------

Once the custom setting is defined, from its detail page click the **Manage** button to add some mappings.
You will create a new instance of the custom setting for each of your Lead record types. If you don't use
Opportunities then that field can be left blank. It is only used by the code if the lead converted into an opp.

![manage settings](/images/manage_custom_settings.png)


Create Apex Class, Trigger, and Test
------------------------------------

The metadata for the helper class, its test, and the lead trigger are in this repository.
You may deploy them to your developer or sandbox org however you are most comfortable then
promote it to your production org when you see fit. There are lots of resources online and at
http://developer.force.com on how to perform these steps so I won't go over them here.
