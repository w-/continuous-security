# Continuous Security

The following is a list of quick wins you can implement within your AWS account(s) to improve your security posture and protect your infrastructure.

## Baseline security best practices

* The [AWS Well Architected Framework](https://aws.amazon.com/architecture/well-architected/) is a collection of best-practices across 5 core pillars (one of them is security) to help teams run efficient and secure services in the cloud. 
* There is also now the [AWS Well-Architected Tool](https://aws.amazon.com/well-architected-tool/) which is a series of questions across those 5 areas (in my experience it takes about 2hrs to go through all 5 areas completely) to help you self-assess how well you're implementing suggested best-practice. At the end you'll get a report that lets you quickly identify where the gaps are and come to a decision among your team on whether you should prioritize addressing them or whether they're acceptable risks given the stage of your product/business.
* The [AWS Well-Architected Labs (Security)](https://github.com/awslabs/aws-well-architected-labs/tree/master/Security) are a good place to look for inspiration on how to address some potential security related gaps in best practice. They're being regularly added to and at the time of writing included solutions such as locking down the `root` account, securing S3 buckets via CloudFront, implementing a Web Application Firewall, setting up granular user/group/role access, automatically detecting new non-compliant infrastructure, and setting permission boundaries to limit the scope of new users/groups/roles people can create.

## GuardDuty

Amazon GuardDuty is a threat detection service that continuously monitors for malicious activity and unauthorized behavior to protect your AWS accounts and workloads.

![GuardDuty Visualizations](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2018/09/05/visualize-GD-16.png)

* Whether you're GuardDuty already or want to get started with it I've found the [Visualizing Amazon GuardDuty Findings](https://aws.amazon.com/blogs/security/visualizing-amazon-guardduty-findings/) example the one people get the most value from implementing. It will walk you through setting up GuardDuty, build out a pipeline to stream the events into another source, and ultimately build out the beautiful dashboard you see above.

* Once you've got GuardDuty setup you need to make sure your team has timely visibility of the issues so they can act on them. Keat from Propeller Aero wrote [this neat setup to publish them GuardDuty findings into a Slack room](https://github.com/keattang/guard-duty-to-slack-lambda).

* https://github.com/aws-samples/amazon-guardduty-hands-on/

* https://github.com/aws-samples/amazon-guardduty-multiaccount-scripts

- Quick test of if GuardDuty is working
https://github.com/awslabs/amazon-guardduty-tester


## Securing The Root Account

Do not use the Root account for day-to-day activity. Setup IAM Users with appropriate permissions

- Enable MFA on Root Account User
https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html#id_root-user_manage_mfa
For virtual, review Authy vs Google Authenticator
For larger organization, hardware mfa makes sense.

- Monitoring Root User Activity
https://aws.amazon.com/blogs/mt/monitor-and-notify-on-aws-account-root-user-activity/

- Verify the root email account and phone number with AWS Support. Open a support ticket
This is the last resort that Amazon will use to allow account recovery


## AWS Security Hub

needs AWS Config running  
https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-settingup.html

- Enable Guard Duty  
- Enable CIS Compliance Check  
- Automatically add child accounts
https://github.com/awslabs/aws-securityhub-multiaccount-scripts

- Manage Access to Security Hub
  - Create a group for security related users  
https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-access.html
  


## AWS Config
- Additional Custom checks  
- See lifetime config changes to resources  
- Manage Access to AWS Config  
  - Create a group for security related users  
  https://docs.aws.amazon.com/config/latest/developerguide/example-policies.html



## Cloudtrail 
### Best Practice
Make sure Cloudtrail is enabled for all regions across all accounts.

Setup cloudtrail logs in a separate child account with extremely restricted permissions

Make sure no one has access to modify logs

### Activities
- Auditable account of all changes to infrastructure (can now be accomplished from AWS Config as well)

- Make sure log file validation is enabled to validate log integrity
https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-log-file-validation-intro.html

- Useful approaches to investgating Cloudtrail logs
https://aws.amazon.com/blogs/big-data/aws-cloudtrail-and-amazon-athena-dive-deep-to-analyze-security-compliance-and-operational-activity/

https://medium.com/starting-up-security/investigating-cloudtrail-logs-c2ecdf578911

- partioning cloudtrail logs
https://medium.com/@alsmola/partitioning-cloudtrail-logs-in-athena-29add93ee070

have a lambda that runs on cron to automatically parition the athena table each day. (see last line in docs)
https://docs.aws.amazon.com/athena/latest/ug/partitions.html

**Can we get automatic partitioning by defining it at create??**
https://gist.github.com/alsmola/db87d3ab1ade8e88da52650e719897c5


## Amazon Inspector

- Basic Testing of network
https://aws.amazon.com/blogs/security/amazon-inspector-assess-network-exposure-ec2-instances-aws-network-reachability-assessments/

- Testing Continuous Deployment of AMIs
https://aws.amazon.com/blogs/security/how-to-set-up-continuous-golden-ami-vulnerability-assessments-with-amazon-inspector/


## Web Identity Federation

Control access to console via web federation.

* https://aws.amazon.com/blogs/security/how-to-set-up-federated-single-sign-on-to-aws-using-google-apps/
* http://federationworkshopreinvent2016.s3-website-us-east-1.amazonaws.com/


## WAF

* https://github.com/awslabs/aws-well-architected-labs/tree/master/Security
* https://aws.amazon.com/blogs/aws/prepare-for-the-owasp-top-10-web-application-vulnerabilities-using-aws-waf-and-our-new-white-paper/
* https://github.com/aws-samples/aws-waf-sample/tree/master/waf-owasp-top-10


# Appendix

## Securing ElasticSearch / Kibana
https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-ac.html

https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-cognito-auth.html
