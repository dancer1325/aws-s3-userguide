# Monitoring data security with managed AWS security services<a name="monitoring-data-security"></a>

Several managed AWS security services can help you identify, assess, and monitor security and compliance risks for your Amazon S3 data\. They can also help you protect your data from those risks\. These services include automated detection, monitoring, and protection capabilities that are designed to scale from Amazon S3 resources for a single AWS account to resources for organizations spanning thousands of AWS accounts\.

AWS detection and response services can help you identify potential security misconfigurations, threats, or unexpected behaviors, so that you can quickly respond to potentially unauthorized or malicious activity in your environment\. AWS data protection services can help you monitor and protect your data, accounts, and workloads from unauthorized access\. They can also help you discover sensitive data, such as personally identifiable information \(PII\), in your Amazon S3 data estate\.

To help you identify and evaluate data security and compliance risks, managed AWS security services generate findings to notify you of potential security events or issues with your Amazon S3 data\. The findings provide relevant details that you can use to investigate, assess, and act upon these risks according to your incident\-response workflows and policies\. You can access findings data directly by using each service\. You can also send the data to other applications, services, and systems, such as your security incident and event management system \(SIEM\)\.

To monitor the security of your Amazon S3 data, consider using these managed AWS security services\.

**Amazon GuardDuty**  
Amazon GuardDuty is a threat\-detection service that continuously monitors your AWS accounts and workloads for malicious activity and delivers detailed security findings for visibility and remediation\.  
With the S3 protection feature in GuardDuty, you can configure GuardDuty to analyze AWS CloudTrail management and data events for your Amazon S3 resources\. GuardDuty then monitors those events for malicious and suspicious activity\. To inform the analysis and identify potential security risks, GuardDuty uses threat\-intelligence feeds and machine learning\.  
GuardDuty can monitor different kinds of activity for your Amazon S3 resources\. For example, CloudTrail management events for Amazon S3 include bucket\-level operations, such as `ListBuckets`, `DeleteBucket`, and `PutBucketReplication`\. CloudTrail data events for Amazon S3 include object\-level operations, such as `GetObject`, `ListObjects`, and `PutObject`\. If GuardDuty detects anomalous or potentially malicious activity, it generates a finding to notify you\.  
For more information, see [Amazon S3 Protection in Amazon GuardDuty](https://docs.aws.amazon.com/guardduty/latest/ug/s3-protection.html) in the *Amazon GuardDuty User Guide*\.

**Amazon Detective**  
Amazon Detective simplifies the investigative process and helps you conduct faster, more effective security investigations\. Detective provides prebuilt data aggregations, summaries, and context that can help you analyze and assess the nature and extent of possible security issues\.  
Detective automatically extracts time\-based events, such as API calls from AWS CloudTrail and Amazon VPC Flow Logs for your AWS resources\. It also ingests findings generated by Amazon GuardDuty\. Detective then uses machine learning, statistical analysis, and graph theory to generate visualizations that help you conduct effective security investigations more quickly\.  
These visualizations provide a unified, interactive view of resource behaviors and the interactions between them over time\. You can explore this behavior graph to examine potentially malicious actions, such as failed login attempts or suspicious API calls\. You can also see how these actions affect resources, such as S3 buckets and objects\.  
For more information, see the [Amazon Detective Administration Guide](https://docs.aws.amazon.com/detective/latest/adminguide/what-is-detective.html)\.

**IAM Access Analyzer**  
AWS Identity and Access Management Access Analyzer \(IAM Access Analyzer\) can help you identify resources that are shared with an external entity\. You can also use IAM Access Analyzer to validate IAM policies against policy grammar and best practices, and generate IAM policies based on access activity in your AWS CloudTrail logs\.  
IAM Access Analyzer uses logic\-based reasoning to analyze resource policies in your AWS environment, such as bucket policies\. With IAM Access Analyzer for S3, you're alerted when an S3 bucket is configured to allow access to anyone on the internet or other AWS accounts, including accounts outside your organization\. For example, IAM Access Analyzer for S3 can report that a bucket has read or write access provided through a bucket access control list \(ACL\), a bucket policy, a Multi\-Region Access Point policy, or an access point policy\. For each public or shared bucket, you receive findings that indicate the source and level of public or shared access\. With these findings, you can take immediate and precise corrective action to restore bucket access to what you intended\.  
For more information, see [Reviewing bucket access using IAM Access Analyzer for S3](access-analyzer.md)\.

**Amazon Macie**  
Amazon Macie is a data security service that discovers sensitive data by using machine learning and pattern matching, provides visibility into data security risks, and enables automated protection against those risks\.  
With Macie, you can automate discovery and reporting of sensitive data in your S3 buckets to gain a better understanding of the data that your organization stores in Amazon S3\. To detect sensitive data, you can use built\-in criteria and techniques that Macie provides, custom criteria that you define, or a combination of the two\. If Macie detects sensitive data in an S3 object, Macie generates a finding to notify you\. This finding provides information about the affected bucket and object, the types and number of occurrences of the sensitive data that Macie found, and additional details to facilitate your investigation\.  
Macie also provides statistics and other data that offer insight into the security posture of your Amazon S3 data, and it automatically evaluates and monitors your S3 buckets for security and access control\. If Macie detects a potential issue with the security or privacy of your data, such as a bucket that becomes publicly accessible, Macie generates a finding for you to review and remediate as necessary\.  
For more information, see the [Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/user/what-is-macie.html)\.

**AWS Security Hub**  
AWS Security Hub is a security\-posture management service that performs security best\-practice checks, aggregates alerts and findings from multiple sources into a single format, and enables automated remediation\.  
Security Hub collects and provides security findings data from integrated AWS Partner Network security solutions and AWS services, including Amazon Detective, Amazon GuardDuty, IAM Access Analyzer, and Amazon Macie\. It also generates its own findings by running continuous, automated security checks based on AWS best practices and supported industry standards\.  
Security Hub then correlates and consolidates findings across providers to help you prioritize and process the most significant findings\. It also provides support for custom actions, which you can use to invoke responses or remediation actions for specific classes of findings\.  
With Security Hub, you can assess the security and compliance status of your Amazon S3 resources, and you can do so as part of a broader analysis of your organization's security posture in individual AWS Regions and across multiple Regions\. This includes analyzing security trends and identifying the highest\-priority security issues\. You can also aggregate findings from multiple AWS Regions, and monitor and process aggregated findings data from a single Region\.  
For more information, see [Amazon Simple Storage Service controls](https://docs.aws.amazon.com/securityhub/latest/userguide/s3-controls.html) in the *AWS Security Hub User Guide*\.