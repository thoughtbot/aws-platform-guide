1.  Create 1Password for credentials (To be completed by the Client)

2.  If not already available, create an external Identity provider for
    SSO and store credentials in 1Password. Sample AWS supported options
    include [Azure
    AD](https://docs.aws.amazon.com/singlesignon/latest/userguide/azure-ad-idp.html),
    [CyberArk](https://docs.aws.amazon.com/singlesignon/latest/userguide/cyberark-idp.html),
    [JumpCloud](https://docs.aws.amazon.com/singlesignon/latest/userguide/jumpcloud-idp.html),
    [Okta](https://docs.aws.amazon.com/singlesignon/latest/userguide/okta-idp.html),
    [Onelogin](https://docs.aws.amazon.com/singlesignon/latest/userguide/onelogin-idp.html),
    [Ping
    Identity](https://docs.aws.amazon.com/singlesignon/latest/userguide/pingidentity.html).
    Once a preferred SSO provider is chosen, the SSO management account
    should be created by the client.  
    Note, the [AWS default
    SSO](https://docs.aws.amazon.com/singlesignon/latest/userguide/manage-your-identity-source-sso.html)
    option could also be chosen for a start pending when the client is
    willing to subscribe to an external identity provider.

3.  Create AWS root account and store root credentials in 1Password. If
    possible, account should be created using a group email address, e.g
    aws-management@example.com.

<div class="confluence-information-macro confluence-information-macro-information">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"></span>

<div class="confluence-information-macro-body">

The Google Group must be set up to allow anyone on the web to post to
the group.

`General` → `Who Can Post` → `Anyone on the web`

Otherwise, the verification email from AWS will not go through. If your
group settings do allow anyone to post, but you still do not see the AWS
email, check under Conversations \> Pending in Google Groups.

</div>

</div>

4.  Create the following group emails to be used for other dependency
    accounts in AWS (To be completed by the Client), to use the below
    email address naming convention, the `ACCOUNT_EMAIL_PREFIX` in your
    landing-zone configuration file should be `aws-`;
    
    1.  aws-management@example.com
    
    2.  aws-identity@example.com
    
    3.  aws-audit@example.com
    
    4.  aws-backup@example.com
    
    5.  aws-report@example.com
    
    6.  aws-log-archive@example.com
    
    7.  aws-network@example.com
    
    8.  aws-operations@example.com
    
    9.  aws-sandbox@example.com
    
    10. aws-production@example.com
    
    11. sso-management@example.com

5.  Create a Github organisation (To be completed by the Client).

6.  Create [necessary
    repositories](../../conventions-and-expectations/repository-conventions.md)
    on GitHub.

7.  Login to AWS and [enable
    MFA](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html#enable-virt-mfa-for-root)
    on the root account for AWS, then you can [link the MFA
    to 1Password](https://support.1password.com/one-time-passwords/).
