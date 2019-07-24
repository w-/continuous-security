# Continuous Security

The following is a list of quick wins you can implement within your AWS account(s) to improve your security posture and protect your infrastructure.

- [Baseline security best practices](#baseline-security-best-practices)
- [AWS re](#inforce-2019--security-best-practices-the-well-architected-way-(sdd318))
- [GuardDuty](#guardduty)
- [Securing The Root Account](#securing-the-root-account)
- [AWS Security Hub](#aws-security-hub)
- [AWS Config](#aws-config)
- [Cloudtrail ](#cloudtrail-)
  - [Best Practice](#best-practice)
  - [Activities](#activities)
- [Amazon Inspector](#amazon-inspector)
- [Static Analysis of Containers](#static-analysis-of-containers)
- [Web Identity Federation](#web-identity-federation)
- [WAF](#waf)
- [Appendix](#appendix)
  - [Securing ElasticSearch / Kibana](#securing-elasticsearch-/-kibana)



## Baseline security best practices

* The [AWS Well Architected Framework](https://aws.amazon.com/architecture/well-architected/) is a collection of best-practices across 5 core pillars (one of them is security) to help teams run efficient and secure services in the cloud. 
* There is also now the [AWS Well-Architected Tool](https://aws.amazon.com/well-architected-tool/) which is a series of questions across those 5 areas (in my experience it takes about 2hrs to go through all 5 areas completely) to help you self-assess how well you're implementing suggested best-practice. At the end you'll get a report that lets you quickly identify where the gaps are and come to a decision among your team on whether you should prioritize addressing them or whether they're acceptable risks given the stage of your product/business.
* The [AWS Well-Architected Labs (Security)](https://github.com/awslabs/aws-well-architected-labs/tree/master/Security) are a good place to look for inspiration on how to address some potential security related gaps in best practice. They're being regularly added to and at the time of writing included solutions such as locking down the `root` account, securing S3 buckets via CloudFront, implementing a Web Application Firewall, setting up granular user/group/role access, automatically detecting new non-compliant infrastructure, and setting permission boundaries to limit the scope of new users/groups/roles people can create.

## AWS re:Inforce 2019  Security Best Practices the Well-Architected Way (SDD318)
This is a great video on latest Security Best Practices.  

https://www.youtube.com/watch?v=u6BCVkXkPnM

Of particular note is the preparation of a playbook which you can combine with GuardDuty and Cloudtrail auditing above to enable quick targeted incident response.

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

- Lab with many best practices
https://github.com/awslabs/aws-well-architected-labs/tree/master/Security/100_AWS_Account_and_Root_User
  - Review Credential report
  - Configure Account Security Challenge Questions  
    place answers in a physical secure location accessible only to key personnel in organization
  - Configure Account Alternate Contacts
  - Remove Your AWS Account Root User Access Keys

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
Per region service. So need to enable for each region and each account.
https://docs.aws.amazon.com/config/latest/developerguide/config-concepts.html#multi-account-multi-region-data-aggregation

All regions and accounts can be viewed centrally in one AWS Config "instance"   
https://docs.aws.amazon.com/config/latest/developerguide/aggregate-data.html


- Additional Custom checks  
  - most common is checking S3 buckets for public read / write permissions  
  
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

  - have a lambda that runs on cron to automatically parition the athena table each day. (see last line in docs)
    https://docs.aws.amazon.com/athena/latest/ug/partitions.html

**Can we get automatic partitioning by defining it at create??**  (open question)  
https://gist.github.com/alsmola/db87d3ab1ade8e88da52650e719897c5
 
- visualizing Cloudtrail logs in a kibana dashboard  
https://medium.com/@marcusrosen_98470/real-time-log-streaming-with-cloudtrail-and-cloudwatch-logs-3389c4cc5ef4

There are two ways as described above:
**batch** Cloudtrail -> write log to S3 -> S3 event triggers lambda -> Lambda writes data to elasticsearch

**streaming** Cloudtrail -> Cloudwatch Log -> cloudwatch log destination -> Kinesis -> Lambda writes to elastic search

Once in Elasticsearch, we can build visualizations based on the useful queries above. some inspiration for dashboards
https://github.com/adcreare/traildash2  
https://app.logz.io/#/dashboard/apps (search for cloudtrail)




## Amazon Inspector

(not currently available in Singapore)
- Basic Testing of network
https://aws.amazon.com/blogs/security/amazon-inspector-assess-network-exposure-ec2-instances-aws-network-reachability-assessments/


( can be accomplished by copying AMIs to another region and testing there)
- Testing Continuous Deployment of AMIs
https://aws.amazon.com/blogs/security/how-to-set-up-continuous-golden-ami-vulnerability-assessments-with-amazon-inspector/


## Static Analysis of Containers

Staticly analyze containers against known CVEs.

https://github.com/aws-samples/amazon-ecs-mythicalmysfits-workshop/tree/master/workshop-2
specifically Lab 4  
https://github.com/aws-samples/amazon-ecs-mythicalmysfits-workshop/blob/master/workshop-2/Lab-4


## Web Identity Federation

Control access to console via web federation.

* https://aws.amazon.com/blogs/security/how-to-set-up-federated-single-sign-on-to-aws-using-google-apps/
* http://federationworkshopreinvent2016.s3-website-us-east-1.amazonaws.com/


## WAF

* https://github.com/awslabs/aws-well-architected-labs/tree/master/Security
* https://aws.amazon.com/blogs/aws/prepare-for-the-owasp-top-10-web-application-vulnerabilities-using-aws-waf-and-our-new-white-paper/
* https://github.com/aws-samples/aws-waf-sample/tree/master/waf-owasp-top-10


## Appendix

### Securing ElasticSearch / Kibana
https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-ac.html

https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-cognito-auth.html


