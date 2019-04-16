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

## Baseline security best practices


## Identity Federation

* https://aws.amazon.com/blogs/security/how-to-set-up-federated-single-sign-on-to-aws-using-google-apps/
* http://federationworkshopreinvent2016.s3-website-us-east-1.amazonaws.com/

## WAF

* https://github.com/awslabs/aws-well-architected-labs/tree/master/Security
* https://aws.amazon.com/blogs/aws/prepare-for-the-owasp-top-10-web-application-vulnerabilities-using-aws-waf-and-our-new-white-paper/
* https://github.com/aws-samples/aws-waf-sample/tree/master/waf-owasp-top-10