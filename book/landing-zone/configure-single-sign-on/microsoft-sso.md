
#### Microsoft SSO

##### References:

  - <https://learn.microsoft.com/en-us/azure/active-directory/saas-apps/aws-single-sign-on-tutorial>

  - <https://www.youtube.com/watch?v=WSd0POCqklY>

##### Note:

When following the above youtube tutorial and creating a non-gallery SSO
app, we encountered an issue that I believe was due to duplicate users
in AWS (we need to ensure that existing groups don’t have users with the
same email in Microsoft), but it may be best to use the “AWS IAM
Identity Center” application from the Microsoft enterprise app gallery.
The following instructions still apply except for the initial bullet
point for app creation in Microsoft section

##### Prerequisites:

  - Identify AWS groups with the appropriate target permission sets /
    account assignments

  - Ensure those same group names exist within Microsoft
    
      - owners / members should be set appropriately

  - Create backup IAM admin user with access to identity center in AWS
    in case changes need to be reverted

##### In AWS:

  - change IdP to external
    
      - download metadata file

##### In Microsoft:

  - create new enterprise app (non-gallery app)

  - in single sign-on section, select SAML
    
      - upload metadata file from AWS

  - save the SAML config

  - Download federation metadata xml

##### In AWS:

  - upload metadata file from Microsoft

  - finish changing identity source

  - enable automatic provisioning
    
      - copy SCIM endpoint / access token for use in Microsoft

##### In Microsoft:

  - in Provisioning section, set mode to automatic

  - set tenant url to SCIM endpoint, secret token to access token (from
    the previous AWS provisioning setup screen)

  - test connection

  - save provisioning config

  - make sure to assign groups to enterprise app

  - then start provisioning
