## AWS Accounts

When possible, thoughtbot uses a multi-account architecture. thoughtbot
uses the standard Control Tower [shared
accounts](https://docs.aws.amazon.com/controltower/latest/userguide/how-control-tower-works.html#what-shared):

- Management
- Log Archive
- Audit

We also use some extra accounts.

![A diagram of the accounts and what they can access](./images/accounts.png)

<table>
  <thead>
    <tr class="header">
      <th><p><strong>Account</strong></p></th>
      <th></th>
      <th><p><strong>Access</strong></p></th>
      <th><p><strong>Diagram Notes</strong></p></th>
    </tr>
  </thead>
  <tbody>
    <tr class="odd">
      <td><p>Management</p></td>
      <td></td>
      <td>
        <ul>
          <li>Control Tower</li>
          <li>Organization</li>
          <li>Customizations</li>
          <li>Identity Center</li>
        </ul>
      </td>
      <td><p>Customizations flow to operations, sandbox, production, and network</p></td>
    </tr>
    <tr class="even">
      <td><p>Identity</p></td>
      <td><p>This account is the delegated administrator for the Identity Center instance and manages permission sets and account assignments.</p></td>
      <td>
        <ul>
          <li>Identity Center</li>
        </ul>
      </td>
      <td>
        <p>Identity Center in the Management account delegates to permission sets and account assignments in the Identity account.</p>
      </td>
    </tr>
    <tr class="odd">
      <td><p>Operations</p></td>
      <td>
        <p>This account contains CI/CD pipelines for building and deploying applications, as well as any centralized application monitoring tools like Grafana and ElasticSearch. IAM roles for entry point access are also deployed to this account.</p>
        <p>The goal for this account is to allow developers to perform most of their tasks, such as deploying and debugging applications, without logging into any account besides the Operations account.</p>
      </td>
      <td>
        <ul>
          <li>CloudTrail</li>
          <li>CI/CD</li>
          <li>Docker Images</li>
          <li>Prometheus</li>
        </ul>
      </td>
      <td>
        <p>Baseline flows to sandbox, production, and network.</p>
        <p>CI/CD flows to docker images and clusters in production and sandbox.</p>
        <p>Cloudtrail flows to sandbox, production, network, and log archive.</p></td>
      </tr>
      <tr class="even">
        <td><p>Network</p></td>
        <td>
          <p>This account contains centralized DNS and networking resources including:</p>
          <p>Route 53 hosted zones for root domains</p>
          <p>SES domain identities</p>
          <p>If using a hub-and-spoke network architecture, the hub resources</p></td>
        <td>
          <ul>
            <li>Baseline</li>
            <li>CloudTrail</li>
            <li>Email Identities</li>
            <li>Subdomain Root Domain</li>
          </ul>
        </td>
        <td><p>Root domain connects to subdomains in production and sandbox.</p></td>
      </tr>
      <tr class="odd">
        <td><p>Audit</p></td>
        <td></td>
        <td>
          <ul>
            <li>AWS Config</li>
            <li>Notifications</li>
          </ul>
        </td>
        <td></td>
      </tr>
      <tr class="even">
        <td><p>Log Archive</p></td>
        <td></td>
        <td>
          <ul>
            <li>CloudTrail Archive</li>
          </ul>
        </td>
        <td></td>
      </tr>
      <tr class="odd">
        <td><p>Production</p></td>
        <td>
          <p>This workload account contains resources for running production workloads, including:</p>
          <p>An EKS cluster running the Flightdeck platform</p>
          <p>Any databases required for running applications on the platform</p>
          <p>S3 buckets and other cloud resources for running applications</p>
          <p>This account will also contain a Route 53 hosted domain for its workloads. Nameservers for the subdomain can be delegated from the Network account, and aliases can be added in the root hosted zone for public addresses.</p>
        </td>
        <td>
          <ul>
            <li>Baseline</li>
            <li>Cluster</li>
            <li>Subdomain</li>
            <li>CloudTrail</li>
          </ul>
        </td>
        <td></td>
      </tr>
      <tr class="even">
        <td><p>Sandbox</p></td>
        <td><p>This account mirrors the production account, but runs development and staging workloads. This account will typically contain resources that are identical in shape to production but smaller in size.</p></td>
        <td>
          <ul>
            <li>Baseline</li>
            <li>Cluster</li>
            <li>Subdomain</li>
            <li>CloudTrail</li>
          </ul>
        </td>
        <td></td>
      </tr>
      <tr class="odd">
        <td><p>Backup</p></td>
        <td><p>Contains isolated backup vaults of workload data. Access to this account is strictly locked down to prevent deletion of backups in the event of a Ransomware attack.</p></td>
        <td>
          <ul>
            <li>Control Tower</li>
            <li>Customizations</li>
          </ul>
        </td>
        <td><p>Sandbox and production accounts connect to the vaults in the Backup account.</p></td>
      </tr>
    </tbody>
</table>
