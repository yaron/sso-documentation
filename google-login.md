Our goal is to set up Gsuite and Azure in such a way that we can use our Gsuite account to login to azure. We don't want to have multiple accounts, but authorization to Azure resources can be handled on Azure itself.

Make sure you don't have the gsuite domain as a custom domain in Azure AD. If you do you will be unable to save the SAML settings and get a cryptic error.
In the google admin console go to security->settings->SSO with Google as SAML IdP. Download the metadata file.
In azure AD go to external identities->all id-providers and add a new SAML provider (the google provider is for normal google accounts, not gsuite).
Enter the gsuite domain as the idp's domain and let AD parse the metadata file.
You can now add external users to AD and they will get a redirect to the google login form if they try to login. There is no need for them to have a seperate microsoft account.
There is no way for google to do provisioning of users or send group information to azure as this is only SAML. Only authentication, authorization is done in azure AD.