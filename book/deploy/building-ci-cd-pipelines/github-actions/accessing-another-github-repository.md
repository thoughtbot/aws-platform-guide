
#### Accessing another GitHub repository

If for any reason, your workflow needs to access another GitHub
repository different than its source, for example:

1.  Pulling application code from CodeBuild, or

2.  Pulling manifests from a separate repo than the application code.

In these cases, the CI/CD workflow will require a Personal Access Token
with the correct permissions set as a secret in the workflow source.

##### To generate a personal access token (PAT)

1.  Sign in as the CI GitHub user (stored in GitHub)

2.  Access `Settings` via the user picture dropdown in the upper right
    corner

3.  From the left navigation menu, access `<> Developer settings`(all
    the way at the bottom at time of writing)

4.  From the left navigation menu in `Developer settings`, access
    Personal Access Token
    
    1.  If presented with a dropdown, select `Tokens (classic)`

5.  You may now generate a new PAT with the `Generate new token` button
    above the list of PATs already in existence

6.  In the popup:
    
    1.  Notate the purpose of the token
    
    2.  Choose its expiration timeframe
    
    3.  Ensure that the token has the correct permissions by selecting
        the checkbox next to `repo`

![image 20221025 141717](./images/image-20221025-141717.png)

d. Click the `Generate token` green button at the bottom

<div class="confluence-information-macro confluence-information-macro-note">

<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"></span>

<div class="confluence-information-macro-body">

Make sure you copy the value of the PAT, you will not be able to
retrieve it again once you leave this page.

</div>

</div>

##### Setting the token secret

1.  In the repo that is running the CI/CD workflow, navigate to
    `Settings` with the horizontal navigation menu

2.  From the left navigation menu, go to Security \> Secrets \> Actions
    
    1.  ![image 20221025 142406](./images/image-20221025-142406.png)

3.  Create a `New repository secret` with the green button at the top

4.  Fill in the name and value of your token

5.  Once saved, you’ll be able to retrieve the value with
    `secrets.NAME_OF_SECRET`
