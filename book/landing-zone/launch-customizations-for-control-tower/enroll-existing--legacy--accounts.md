<div class="confluence-information-macro confluence-information-macro-note">

<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"></span>

<div class="confluence-information-macro-body">

This page is a work in progress.

</div>

</div>

If the AWS Organization for which you are setting up Control
Tower/Landing Zone contains legacy accounts that you wish to enroll to
be managed by Control Tower, follow the steps below:

1.  Before deploying Customizations for Control Tower, manually create
    the `AWSControlTowerExecution` role by following the Step 2 in [this
    guide](https://docs.aws.amazon.com/controltower/latest/userguide/enroll-manually.html).
    In a Control Tower-initialized account, this role is created by AWS
    automatically, and is [required for Control Tower to manage any
    account](https://docs.aws.amazon.com/controltower/latest/userguide/roles-how.html).
    Legacy accounts do not have it.

2.  Add the legacy account configs to `accounts.yaml` in the
    landing-zone repo, with values for `AccountName` and `AccountEmail`
    that match current account details.
