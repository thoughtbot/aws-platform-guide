## Launch Customizations for Control Tower

::: caution
This is an advanced topic for platform engineers.
:::

::: info
You should use the [landing zone template
repository](https://github.com/thoughtbot/aws-landing-zone-template) to
set up your landing zone by clicking the "Use this template" button and creating the repository in GitHub from there.
:::

1.  Accept the invitation to join AWS Identity Center that was sent to
    the management account email.
2.  If there are legacy accounts to enroll, see [Enroll Existing (Legacy) Accounts](#enroll-existing-legacy-accounts).
3.  From your Landing Zone template, run the `bin/deploy` script to launch Customizations for Control Tower.
4.  Follow the prompts from the self-guided installation to configure your landing zone.

You are now ready to [set up your single sign on identity provider](#configure-single-sign-on).
