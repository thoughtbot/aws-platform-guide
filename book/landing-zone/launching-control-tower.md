
## Launching Control Tower


<div class="panel" style="background-color: #FFFAE6;border-width: 1px;">

<div class="panelContent" style="background-color: #FFFAE6;">

This is an advanced topic for platform engineers.

</div>

</div>

### AWS Landing Zone

The Landing Zone is the start URL for users to access the organization's
AWS accounts.

thoughtbot uses [Control Tower](https://aws.amazon.com/controltower/) to
build a Landing Zone implementing security best practices and reliable
workload isolation. This provides a quick starting point for a
multi-account setup while still allowing for significant customization
and expansion later.

Rather than managing individual IAM users, we use [AWS
SSO](https://aws.amazon.com/single-sign-on/) to manage users centrally
and integrate with existing identity stores like a Google or Microsoft
user directory.

We use [Customizations for Control
Tower](https://aws.amazon.com/solutions/implementations/customizations-for-aws-control-tower/)
to configure [account
baselines](https://docs.aws.amazon.com/controltower/latest/userguide/terminology.html)
and deploy [service control
policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html).

### Getting Started

<div class="confluence-information-macro confluence-information-macro-note">

<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"></span>

<div class="confluence-information-macro-body">

Before you can launch your landing zone, you will need to make sure you
have completed all the [prerequisite
steps](#landing-zone-prerequisites).

</div>

</div>

1.  Allow IAM administrators to manage billing following the [IAM
    billing
    guide](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/control-access-billing.html#ControllingAccessWebsite-Activate).

2.  <span class="inline-comment-marker" data-ref="cf0b31b9-f8d3-44f7-bf2a-5f1dcefa227e">Create
    an IAM user following the Control Tower</span> [IAM user
    guide](https://docs.aws.amazon.com/controltower/latest/userguide/setting-up.html#setting-up-iam).
    Create a user with programmatic and console access. Save your
    password and access key.

3.  Write down your AWS account ID.

4.  Sign out of the root user account and sign in as the newly-created
    IAM user.

5.  <span class="inline-comment-marker" data-ref="9283b9a5-03dd-4c8c-a2a4-ef6f2638c34a">Enable
    Cost Management by following the
    </span>[<span class="inline-comment-marker" data-ref="9283b9a5-03dd-4c8c-a2a4-ef6f2638c34a">AWS
    Cost
    Management</span>](https://docs.aws.amazon.com/cost-management/latest/userguide/billing-getting-started.html)<span class="inline-comment-marker" data-ref="9283b9a5-03dd-4c8c-a2a4-ef6f2638c34a">
    guide. This will allow you to see your costs broken down by service,
    tags, and account.</span>

6.  Deploy your landing zone (steps below follow the Control Tower
    [getting started
    guide](https://docs.aws.amazon.com/controltower/latest/userguide/getting-started-with-control-tower.html))

    1.  Create shared account emails for `logs` and `audit` accounts:
        [AWS
        Guide](https://docs.aws.amazon.com/controltower/latest/userguide/step-one.html)

    2.  Configure and launch your Landing Zone: [AWS
        Guide](https://docs.aws.amazon.com/controltower/latest/userguide/step-two.html)

    3.  Review and set up the landing zone: [AWS
        Guide](https://docs.aws.amazon.com/controltower/latest/userguide/review-and-set-up.html)

        1.  You can use the default names for OUs and foundational
            accounts.

        2.  <span class="inline-comment-marker" data-ref="db00ac43-732f-448c-9234-f3b64a660485">Enable
            encryption settings using KMS. You will need to create a new
            symmetric, single-region key. When prompted for an alias,
            enter
            </span>`control-tower`<span class="inline-comment-marker" data-ref="db00ac43-732f-448c-9234-f3b64a660485">.</span>

            1.  Edit the policy for the KMS key you created to grant
                [permissions that allow AWS
                CloudTrail](https://docs.aws.amazon.com/controltower/latest/userguide/getting-started-with-control-tower.html#kms-key-policy-update)
                and AWS Config to use the chosen KMS key. You can use
                the following statement:

            2.

                ```
                {
                  "Sid": "CloudTrail and AWS Config to encrypt/decrypt logs",
                  "Effect": "Allow",
                  "Principal": {
                    "Service": [
                      "cloudtrail.amazonaws.com",
                      "config.amazonaws.com"
                    ]
                  },
                  "Action": [
                    "kms:GenerateDataKey",
                    "kms:Decrypt"
                  ],
                  "Resource": "*"
                }
                ```

        3.  Select the key you created and launch your landing zone. If
            you receive quota-related errors during this step for a
            freshly-created account, wait at least an hour and try
            again. It may also take several hours for your landing zone
            to be fully deployed. While your landing zone is deploying,
            you will receive an email requesting email confirmation. You
            must confirm your email before the landing zone will
            complete successfully.

7.  Congratulations\! You now have an AWS Landing Zone managed by
    Control Tower.

You are now ready to launch [Customizations for Control
Tower](#launch-customizations-for-control-tower).
