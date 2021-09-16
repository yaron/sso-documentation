Our goal is to set up Gsuite and Azure Active directory in such a way that we can login to both Gsuite and Azure with the same azure account without any manual action on the Gsuite side. We also want to be able to provision both users and groups from Azure to Gsuite to minimize administrative overhead.

First thing we did was set up a trial account on Azure (with an AAD P2, because we seemed to need that to be able to assign groups to gsuite instead of only individual users), google workspace and google cloud platform.

We tried to follow this tutorial:
https://docs.microsoft.com/en-us/azure/active-directory/saas-apps/google-apps-tutorial

For this we used the custom domain yarontal.nl
Change the AAD domain to the custom domain and make all test users use that domain instead of the onmicrosoft.com domain.
Keep using userprincipalname as the unique identifier as that is now a valid mail adress under our custom domain.

Use the same domain for the google workspace.

When entering the saml urls on the azure connector, replace the wildcards with our custom domain (yarontal.nl).
When entering the urls in google cloud, use https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0 , the logout url that was provided by the connector gave an error when using it to logout.

We did not use the domain specific issuer (checkmark on the gsuite side), this did not work at first, but could have been due to other misconfigurations. This will need further checking.

When setting up the provisioning from AAD to gsuite, we changed the mapping for groups to use the group description as the source for the email address. This is a dirty fix, but we could not find another way 
to provide an email address in an azure group by other means. Using an office365 group also does not seem to work here, probably because I wasn't able to use our custom domain for it.

When a group is deleted from azure it is not also automatically removed from gsuite. When a user is removed in azure it becomes suspended in gsuite (so no data loss occurs).

Provisioning of users and groups is not instant, but faster than the 40 minutes the interface states.

Users in AAD must be created with a firstname and lastname as those are required in gsuite. Omitting them will generate errors in the provisioning logs stating that those are required fields and the user could not be created.

When a new user is provisioned in gsuite it will have the status "Automatically suspended, Unverified sign-in". This is not a problem, but will have the user give a phone number on the first login and verify it by text message. 
Someone on the internet states that a phone number can only be used to verify up to 4 accounts at the same time, but I haven't tested that.