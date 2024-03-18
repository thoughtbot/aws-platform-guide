## AWS Managed Services

thoughtbot recommends the following AWS services in its platform:

<div class="table-wrap">

| **Service**     | [**SOC**](https://aws.amazon.com/compliance/services-in-scope/SOC/) | [**HITRUST CSF**](https://aws.amazon.com/compliance/services-in-scope/HITRUST-CSF/) | [**HIPAA BAA**](https://aws.amazon.com/compliance/hipaa-eligible-services-reference/) | [**GDPR**](https://aws.amazon.com/compliance/gdpr-center/) |
| --------------- | ------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| EC2             | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| RDS Postgres    | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| OpenSearch      | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| EKS             | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| KMS             | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| CloudWatch      | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| CloudWatch Logs | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| Secrets Manager | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| Config          | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| Route 53        | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| ECR             | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| S3              | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| CloudTrail      | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| DynamoDB        | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| ELB             | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| ACM             | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| SNS             | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |
| SQS             | ![(tick)](images/icons/emoticons/check.png)                         | ![(tick)](images/icons/emoticons/check.png)                                         | ![(tick)](images/icons/emoticons/check.png)                                           | ![(tick)](images/icons/emoticons/check.png)                |

</div>

You should be familiar with the [AWS Shared Responsibility
model](https://aws.amazon.com/compliance/shared-responsibility-model/).
You can learn more on AWS’s [compliance
page](https://aws.amazon.com/compliance/).

AWS also has further security documentation for its services:

<div class="table-wrap">

<table>
<thead>
<tr class="header">
<th><p><strong>AWS Service</strong></p></th>
<th><p><strong>Security Documentation</strong></p></th>
<th><p><strong>Notes</strong></p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><a href="http://aws.amazon.com/solutions/aws-landing-zone/" class="external-link">AWS Landing Zone</a></p></td>
<td><p><a href="https://docs.aws.amazon.com/whitepapers/latest/nhs-cloud-security-guidance-using-aws/overall-security-governance---aws-landing-zones.html" class="external-link">https://docs.aws.amazon.com/whitepapers/latest/nhs-cloud-security-guidance-using-aws/overall-security-governance---aws-landing-zones.html</a></p></td>
<td><p>Relevant to most of the Principles covered by the Good Practice Guide, a Landing Zone is a solution available from AWS that automatically creates an environment consisting of a set of related AWS accounts configured in such a way as to establish security (and cost-related) guardrails for AWS usage by a wide variety of teams with minimum friction. The environment includes the foundations of identity management, logging and monitoring, governance, security, and network design, the specifics of which may be implemented using decisions made in examining each of the principles covered in the overall security governance document.</p></td>
</tr>
<tr class="even">
<td><p>AWS Control Tower</p></td>
<td><p><a href="https://docs.aws.amazon.com/controltower/latest/userguide/security.html" class="external-link">https://docs.aws.amazon.com/controltower/latest/userguide/security.html</a></p></td>
<td><p>AWS Control Tower is a well-architected service that can help your organization meet your compliance needs with controls and best practices. Additionally, third-party auditors assess the security and compliance of a number of the services you can use in your landing zone as a part of multiple AWS compliance programs. These include SOC, PCI, FedRAMP, HIPAA, and others.</p>
<p>Your compliance responsibility when using AWS Control Tower is determined by the sensitivity of your data, your company’s compliance objectives, and applicable laws and regulations.</p></td>
</tr>
<tr class="odd">
<td><p><a href="https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html" class="external-link">AWS Config</a></p></td>
<td><p><a href="https://docs.aws.amazon.com/config/latest/developerguide/security.html" class="external-link">https://docs.aws.amazon.com/config/latest/developerguide/security.html</a></p>
<p>Templates for conformance packs (selected few, there are many available). Provide example mappings of controls to implementation.</p>
<p><a href="https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-cis_top_20.html" class="external-link">https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-cis_top_20.html</a></p>
<p><a href="https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-nist-csf.html" class="external-link">https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-nist-csf.html</a></p>
<p><a href="https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-hipaa_security.html" class="external-link">https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-hipaa_security.html</a></p>
<p><a href="https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-fedramp-moderate.html" class="external-link">https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-fedramp-moderate.html</a></p>
<p><a href="https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-fedramp-low.html" class="external-link">https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-fedramp-low.html</a></p></td>
<td><p>Included within the Landing Zone solution, this service tracks configuration settings of AWS resources over time against a desired-state baseline, and raises alerts (and optionally triggers remedial action) when changes are detected.</p>
<p>The service also enables configuration to be audited, in order to demonstrate compliance (or otherwise) against a baseline. See the <a href="https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html" class="external-link">AWS Config Developer Guide</a> for a detailed description of how to use it.</p>
<p>Recommended best practice guidelines:</p>
<ul>
<li><p>Leverage tagging for AWS Config, which makes is easier to manage, search for, and filter resources.</p></li>
<li><p>Confirm your <a href="https://docs.aws.amazon.com/config/latest/developerguide/manage-delivery-channel.html" class="external-link">delivery channels</a> have been properly set, and once confirmed, verify that AWS Config is <a href="https://docs.aws.amazon.com/config/latest/developerguide/stop-start-recorder.html" class="external-link">recording properly</a>.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>AWS Secrets Manager</p></td>
<td><p><a href="https://docs.aws.amazon.com/secretsmanager/latest/userguide/security.html" class="external-link">https://docs.aws.amazon.com/secretsmanager/latest/userguide/security.html</a></p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>AWS CloudTrail</p></td>
<td><p><a href="https://docs.aws.amazon.com/awscloudtrail/latest/userguide/best-practices-security.html" class="external-link">https://docs.aws.amazon.com/awscloudtrail/latest/userguide/best-practices-security.html</a></p></td>
<td></td>
</tr>
<tr class="even">
<td><p>AWS EKS</p></td>
<td><p><a href="https://docs.aws.amazon.com/eks/latest/userguide/security.html" class="external-link">https://docs.aws.amazon.com/eks/latest/userguide/security.html</a></p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>AWS CloudFormation</p></td>
<td><p><a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/security.html" class="external-link">https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/security.html</a></p></td>
<td></td>
</tr>
<tr class="even">
<td><p>AWS S3</p></td>
<td><p><a href="https://aws.amazon.com/s3/security/" class="external-link">https://aws.amazon.com/s3/security/</a></p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>AWS IAM Identity Center (SSO)</p></td>
<td><p><a href="https://docs.aws.amazon.com/singlesignon/latest/userguide/security.html" class="external-link">https://docs.aws.amazon.com/singlesignon/latest/userguide/security.html</a></p></td>
<td></td>
</tr>
<tr class="even">
<td><p>AWS Managed Prometheus</p></td>
<td><p><a href="https://aws.amazon.com/prometheus/" class="external-link">https://aws.amazon.com/prometheus/</a></p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>AWS Managed Grafana</p></td>
<td><p><a href="https://aws.amazon.com/grafana/" class="external-link">https://aws.amazon.com/grafana/</a></p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

## GitHub

thoughtbot also recommends the use of GitHub for source control and
CI/CD workflows. GitHub supports compliance with a number of frameworks,
including GDPR, SOC 1/2, and FEDRAMP. You can learn more at GitHub’s
[security page](https://github.com/security).

## Procedures and Practices

thoughtbot implements the following procedures and practice to help with
compliance:

  - Integrate cloud access with single sign on

  - Separate workflows by development life cycle

  - Encrypt all data at rest and in transit

  - Unique customer controlled encryption keys for each data store

  - Network isolation for data stores and backend services

  - Organization-wide AWS backup policies

  - Organization-wide AWS security policies

  - Organization-wide AWS config controls

  - Enforce SDLC workflows using CI/CD

  - Automated vulnerability scans for infrastructure and application
    dependencies

  - Encrypted logs with archives

  - Audit logs for infrastructure access and changes
