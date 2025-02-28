#### Microsoft SSO

##### References

  - <https://learn.microsoft.com/en-us/azure/active-directory/saas-apps/aws-single-sign-on-tutorial>
  - <https://www.youtube.com/watch?v=WSd0POCqklY>

##### Note:

When following the above youtube tutorial and creating a non-gallery SSO
app, we encountered an issue that was due to duplicate users
in AWS. Therefore, we need to ensure that existing groups don’t have users with the
same email in Microsoft. It may be best to use the “AWS IAM
Identity Center” application from the Microsoft enterprise app gallery.
The following instructions still apply except for the initial bullet
point for app creation in Microsoft section

##### Prerequisites

1. Identify AWS groups with the appropriate target permission sets /
  account assignments
2. Ensure those same group names exist within Microsoft
    - Owners / members should be set appropriately
3. Create backup IAM admin user with access to identity center in AWS
  in case changes need to be reverted

##### In AWS

1. Change IdP to external
2. Download metadata file

##### In Microsoft

1. Create new enterprise app (non-gallery app)
2. In single sign-on section, select SAML
3. Upload metadata file from AWS
4. Save the SAML config
5. Download federation metadata xml

##### In AWS

1. Upload metadata file from Microsoft
2. Finish changing identity source
3. Enable automatic provisioning
4. Copy SCIM endpoint / access token for use in Microsoft

##### In Microsoft

1. In Provisioning section, set mode to automatic
2. Set tenant url to SCIM endpoint, secret token to access token (from
  the previous AWS provisioning setup screen)
3. Test connection
4. Save provisioning config
5. Make sure to assign groups to enterprise app
6. Then start provisioning
