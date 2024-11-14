# Using Amazon S3 storage classes<a name="storage-class-intro"></a>

* 👀storage class / EACH object | Amazon S3 👀
  * range of allowed ones
  * high durability
  * criteria to choose one
    * use case scenario
    * performance access requirements

**Topics**
+ [Storage classes for frequently accessed objects](#sc-freq-data-access)
+ [Storage class for automatically optimizing data with changing or unknown access patterns](#sc-dynamic-data-access)
+ [Storage classes for infrequently accessed objects](#sc-infreq-data-access)
+ [Storage classes for archiving objects](#sc-glacier)
+ [Storage class for Amazon S3 on Outposts](#s3-outposts)
+ [Comparing the Amazon S3 storage classes](#sc-compare)
+ [Setting the storage class of an object](#sc-howtoset)

## Storage classes for frequently accessed objects<a name="sc-freq-data-access"></a>

* TODO:
For performance\-sensitive use cases \(those that require millisecond access time\) and frequently accessed data, Amazon S3 provides the following storage classes:
+ **S3 Standard** – The default storage class\. If you don't specify the storage class when you upload an object, Amazon S3 assigns the S3 Standard storage class\.
+ **Reduced Redundancy** – The Reduced Redundancy Storage \(RRS\) storage class is designed for noncritical, reproducible data that can be stored with less redundancy than the S3 Standard storage class\.
**Important**  
We recommend not using this storage class\. The S3 Standard storage class is more cost\-effective\. 

  For durability, RRS objects have an average annual expected loss of 0\.01 percent of objects\. If an RRS object is lost, when requests are made to that object, Amazon S3 returns a 405 error\.

## Storage class for automatically optimizing data with changing or unknown access patterns<a name="sc-dynamic-data-access"></a>

**S3 Intelligent\-Tiering** is an Amazon S3 storage class that's designed to optimize storage costs by automatically moving data to the most cost\-effective access tier, without performance impact or operational overhead\. S3 Intelligent\-Tiering is the only cloud storage class that delivers automatic cost savings by moving data on a granular object level between access tiers when access patterns change\. S3 Intelligent\-Tiering is the ideal storage class when you want to optimize storage costs for data that has unknown or changing access patterns\. There are no retrieval fees for S3 Intelligent\-Tiering\. 

For a small monthly object monitoring and automation fee, S3 Intelligent\-Tiering monitors access patterns and automatically moves objects that have not been accessed to lower\-cost access tiers\. S3 Intelligent\-Tiering delivers automatic storage cost savings in two low\-latency and high\-throughput access tiers\. For data that can be accessed asynchronously, you can choose to activate automatic archiving capabilities within the S3 Intelligent\-Tiering storage class\. S3 Intelligent\-Tiering is designed for 99\.9% availability and 99\.999999999% durability\.

S3 Intelligent\-Tiering automatically stores objects in three access tiers: 
+ **Frequent Access** – Objects that are uploaded or transitioned to S3 Intelligent\-Tiering are automatically stored in the Frequent Access tier\.
+ **Infrequent Access** – S3 Intelligent\-Tiering moves objects that have not been accessed in 30 consecutive days to the Infrequent Access tier\.
+ **Archive Instant Access** – With S3 Intelligent\-Tiering, any existing objects that have not been accessed for 90 consecutive days are automatically moved to the Archive Instant Access tier\. 

In addition to these three tiers, S3 Intelligent\-Tiering offers two optional archive access tiers: 
+ **Archive Access** – S3 Intelligent\-Tiering provides you with the option to activate the Archive Access tier for data that can be accessed asynchronously\. After activation, the Archive Access tier automatically archives objects that have not been accessed for a minimum of 90 consecutive days\.
+ **Deep Archive Access** – S3 Intelligent\-Tiering provides you with the option to activate the Deep Archive Access tier for data that can be accessed asynchronously\. After activation, the Deep Archive Access tier automatically archives objects that have not been accessed for a minimum of 180 consecutive days\.

**Note**  
Only activate the Archive Access tier for 90 days if you want to bypass the Archive Instant Access tier\. The Archive Access tier delivers slightly lower\-cost storage with minute\-to\-hour retrieval times\. The Archive Instant Access tier delivers millisecond access and high\-throughput performance\.
Activate the Archive Access and Deep Archive Access tiers only if your objects can be accessed asynchronously by your application\. If the object that you are retrieving is stored in the Archive Access or Deep Archive Access tiers, first restore the object by using `RestoreObject`\.

You can [move newly created data to S3 Intelligent\-Tiering](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-intelligent-tiering.html#moving-data-to-int-tiering), setting it as your default storage class\. You can also choose to activate one or both of the archive access tiers by using the [https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketIntelligentTieringConfiguration.html](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketIntelligentTieringConfiguration.html) API operation, the AWS CLI, or the Amazon S3 console\. For more information about using S3 Intelligent\-Tiering and activating the archive access tiers, see [ Using S3 Intelligent\-Tiering](using-intelligent-tiering.md)\.

To access archived objects later, you first need to restore them\. For more information, see [ Restoring objects from the S3 Intelligent\-Tiering Archive Access and Deep Archive Access tiers](intelligent-tiering-managing.md#restore-data-from-int-tier-archive)\.

**Note**  
If the size of an object is less than 128 KB, it is not monitored and not eligible for auto\-tiering\. Smaller objects are always stored in the Frequent Access tier\. For more information about S3 Intelligent\-Tiering, see [S3 Intelligent\-Tiering access tiers](intelligent-tiering-overview.md#intel-tiering-tier-definition)\.

## Storage classes for infrequently accessed objects<a name="sc-infreq-data-access"></a>

The **S3 Standard\-IA** and **S3 One Zone\-IA** storage classes are designed for long\-lived and infrequently accessed data\. \(IA stands for *infrequent access*\.\) S3 Standard\-IA and S3 One Zone\-IA objects are available for millisecond access \(similar to the S3 Standard storage class\)\. Amazon S3 charges a retrieval fee for these objects, so they are most suitable for infrequently accessed data\. For pricing information, see [Amazon S3 pricing](https://aws.amazon.com/s3/pricing/)\. 

For example, you might choose the S3 Standard\-IA and S3 One Zone\-IA storage classes to do the following:
+ For storing backups\. 
+ For older data that is accessed infrequently, but that still requires millisecond access\. For example, when you upload data, you might choose the S3 Standard storage class, and use lifecycle configuration to tell Amazon S3 to transition the objects to the S3 Standard\-IA or S3 One Zone\-IA class\.

  For more information about lifecycle management, see [Managing your storage lifecycle](object-lifecycle-mgmt.md)\.

**Note**  
The S3 Standard\-IA and S3 One Zone\-IA storage classes are suitable for objects larger than 128 KB that you plan to store for at least 30 days\. If an object is less than 128 KB, Amazon S3 charges you for 128 KB\. If you delete an object before the end of the 30\-day minimum storage duration period, you are charged for 30 days\. For pricing information, see [Amazon S3 pricing](https://aws.amazon.com/s3/pricing/)\.

These storage classes differ as follows:
+ **S3 Standard\-IA** – Amazon S3 stores the object data redundantly across multiple geographically separated Availability Zones \(similar to the S3 Standard storage class\)\. S3 Standard\-IA objects are resilient to the loss of an Availability Zone\. This storage class offers greater availability and resiliency than the S3 One Zone\-IA class\. 
+ **S3 One Zone\-IA** – Amazon S3 stores the object data in only one Availability Zone, which makes it less expensive than S3 Standard\-IA\. However, the data is not resilient to the physical loss of the Availability Zone resulting from disasters, such as earthquakes and floods\. The S3 One Zone\-IA storage class is as durable as S3 Standard\-IA, but it is less available and less resilient\. For a comparison of storage class durability and availability, see [Comparing the Amazon S3 storage classes](#sc-compare) at the end of this section\. For pricing information, see [Amazon S3 pricing](https://aws.amazon.com/s3/pricing/)\. 

We recommend the following:
+ **S3 Standard\-IA** – Use for your primary or only copy of data that can't be re\-created\. 
+ **S3 One Zone\-IA** – Use if you can re\-create the data if the Availability Zone fails, and for object replicas when configuring S3 Cross\-Region Replication \(CRR\)\. 

## Storage classes for archiving objects<a name="sc-glacier"></a>

The **S3 Glacier Instant Retrieval**, **S3 Glacier Flexible Retrieval**, and **S3 Glacier Deep Archive** storage classes are designed for low\-cost data archiving\. These storage classes offer the same durability and resiliency as the S3 Standard and S3 Standard\-IA storage classes\. For a comparison of storage class durability and availability, see [Comparing the Amazon S3 storage classes](#sc-compare)\.

These storage classes differ as follows:
+ **S3 Glacier Instant Retrieval** – Use for archiving data that is rarely accessed and requires milliseconds retrieval\. Data stored in the S3 Glacier Instant Retrieval storage class offers a cost savings compared to the S3 Standard\-IA storage class, with the same latency and throughput performance as the S3 Standard\-IA storage class\. S3 Glacier Instant Retrieval has higher data access costs than S3 Standard\-IA\. 

  For pricing information, see [Amazon S3 pricing](https://aws.amazon.com/s3/pricing/)\.
+ **S3 Glacier Flexible Retrieval** – Use for archives where portions of the data might need to be retrieved in minutes\. Data stored in the S3 Glacier Flexible Retrieval storage class has a minimum storage duration period of 90 days and can be accessed in as little as 1\-5 minutes by using an expedited retrieval\. The retrieval time is flexible, and you can request free bulk retrievals in up to 5\-12 hours\. If you delete, overwrite, or transition the object to a different storage class before the 90\-day minimum, you are charged for 90 days\. Amazon S3 supports restore requests at a rate of up to 1,000 transactions per second, per AWS account for S3 Glacier Flexible Retrieval\.

  For pricing information, see [Amazon S3 pricing](https://aws.amazon.com/s3/pricing/)\. 
+ **S3 Glacier Deep Archive** – Use for archiving data that rarely needs to be accessed\. Data stored in the S3 Glacier Deep Archive storage class has a minimum storage duration period of 180 days and a default retrieval time of 12 hours\. If you delete, overwrite, or transition the object to a different storage class before the 180\-day minimum, you are charged for 180 days\. Amazon S3 supports restore requests at a rate of up to 1,000 transactions per second, per AWS account for S3 Glacier Deep Archive\.

  For pricing information, see [Amazon S3 pricing](https://aws.amazon.com/s3/pricing/)\. 

  S3 Glacier Deep Archive is the lowest\-cost storage option in AWS\. Storage costs for S3 Glacier Deep Archive are less expensive than using the S3 Glacier Flexible Retrieval storage class\. You can reduce S3 Glacier Deep Archive retrieval costs by using bulk retrieval, which returns data within 48 hours\. 

### Retrieving archived objects<a name="sc-glacier-restore"></a>

You can set the storage class of an object to S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive in the same ways that you do for the other storage classes as described in the section [Setting the storage class of an object](#sc-howtoset)\. However, S3 Glacier Flexible Retrieval and S3 Glacier Deep Archive objects are not available for real\-time access\. You must first restore S3 Glacier Flexible Retrieval and S3 Glacier Deep Archive objects before you can access them\. \(S3 Standard, Reduced Redundancy Storage \(RRS\), S3 Standard\-IA, S3 One Zone\-IA, S3 Glacier Instant Retrieval, and S3 Intelligent\-Tiering objects are available for anytime access\.\) For more information about retrieving archived objects, see [Restoring an archived object](restoring-objects.md)\.

**Important**  
When you choose the S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive storage class, your objects remain in Amazon S3\. You can't access them directly through the separate Amazon S3 Glacier service\. 

To learn more about the Amazon S3 Glacier service, see the [Amazon S3 Glacier Developer Guide](https://docs.aws.amazon.com/amazonglacier/latest/dev/)\.

### Getting started with S3 Glacier storage classes<a name="sc-glacier-tutorial"></a>

To learn more about how to use Amazon S3 Glacier storage classes, see [Tutorial: Getting started using the Amazon S3 Glacier storage classes](http://aws.amazon.com/getting-started/hands-on/getting-started-using-amazon-s3-glacier-storage-classes/?ref=docs_gateway/amazons3/storage-class-intro.html)\.

## Storage class for Amazon S3 on Outposts<a name="s3-outposts"></a>

With Amazon S3 on Outposts, you can create S3 buckets on your AWS Outposts resources and store and retrieve objects on\-premises for applications that require local data access, local data processing, and data residency\. You can use the same APIs and features on AWS Outposts as you do on Amazon S3, including access policies, encryption, and tagging\. You can use S3 on Outposts through the AWS Management Console, AWS CLI, AWS SDKs, or REST API\.

S3 on Outposts provides a new storage class, S3 Outposts \(`OUTPOSTS`\)\. The S3 Outposts storage class is available only for objects stored in buckets on Outposts\. If you try to use this storage class with an S3 bucket in an AWS Region, an `InvalidStorageClass` error occurs\. In addition, if you try to use other S3 storage classes with objects stored in S3 on Outposts buckets, the same error occurs\. 

Objects stored in the S3 Outposts \(`OUTPOSTS`\) storage class are always encrypted by using server\-side encryption with Amazon S3 managed encryption keys \(SSE\-S3\)\. For more information, see [Protecting data using server\-side encryption with Amazon S3 managed encryption keys \(SSE\-S3\)](UsingServerSideEncryption.md)\. 

You can also explicitly choose to encrypt objects stored in the S3 Outposts storage class by using server\-side encryption with customer\-provided encryption keys \(SSE\-C\)\. For more information, see [Using server\-side encryption with customer\-provided keys \(SSE\-C\)](ServerSideEncryptionCustomerKeys.md)\. 

**Note**  
S3 on Outposts doesn't support server\-side encryption with AWS Key Management Service \(AWS KMS\) keys \(SSE\-KMS\.

For more information about S3 on Outposts, see [What is Amazon S3 on Outposts?](S3onOutposts.md)

## Comparing the Amazon S3 storage classes<a name="sc-compare"></a>

The following table compares the storage classes, including their availability, durability, minimum storage duration, and other considerations\.




****  

| Storage class | Designed for | Durability \(designed for\) | Availability \(designed for\) | Availability Zones | Min storage duration | Min billable object size | Other considerations  | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
|  S3 Standard  |  Frequently accessed data \(more than once a month\) with millisecond access  |  99\.999999999%   |  99\.99%  |  >= 3  |  None  |  None  |  None  | 
|  S3 Standard\-IA  |  Long\-lived, infrequently accessed data \(once a month\) with millisecond access  |  99\.999999999%   |  99\.9%  |  >= 3  |  30 days  |  128 KB  |  Per\-GB retrieval fees apply\.   | 
|  S3 Intelligent\-Tiering  |  Data with unknown, changing, or unpredictable access patterns  |  99\.999999999%  |  99\.9%  |  >= 3  |  None  |  None  |  Monitoring and automation fees per object apply\. No retrieval fees\.  | 
|  S3 One Zone\-IA  |  Recreatable, infrequently accessed data \(once a month\) with millisecond access  |  99\.999999999%   |  99\.5%  |  1  |  30 days  |  128 KB  |  Per\-GB retrieval fees apply\. Not resilient to the loss of the Availability Zone\.  | 
|  S3 Glacier Instant Retrieval  | Long\-lived, archive data accessed once a quarter with millisecond access | 99\.999999999%  |  99\.9%  |  >= 3  |  90 days  |  128 KB  | Per\-GB retrieval fees apply\. | 
|  S3 Glacier Flexible Retrieval  | Long\-lived archive data accessed once a year with retrieval times of minutes to hours | 99\.999999999%  |  99\.99% \(after you restore objects\)  |  >= 3  |  90 days  |  40 KB\*  | Per\-GB retrieval fees apply\. You must first restore archived objects before you can access them\. For information, see [Restoring an archived object](restoring-objects.md)\. | 
|  S3 Glacier Deep Archive  | Long\-lived archive data accessed less than once a year with retrieval times of hours | 99\.999999999%  |  99\.99% \(after you restore objects\)  |  >= 3  |  180 days  |  40 KB\*\*  | Per\-GB retrieval fees apply\. You must first restore archived objects before you can access them\. For information, see [Restoring an archived object](restoring-objects.md)\. | 
|  RRS \(not recommended\)  |  Noncritical, frequently accessed data with millisecond access  |  99\.99%   |  99\.99%  |  >= 3  |  None  |  None  |  None  | 

\* S3 Glacier Flexible Retrieval requires 40 KB of additional metadata for each archived object\. This includes 32 KB of metadata charged at the S3 Glacier Flexible Retrieval rate \(required to identify and retrieve your data\), and an additional 8 KB data charged at the S3 Standard rate\. The S3 Standard rate is required to maintain the user\-defined name and metadata for objects archived to S3 Glacier Flexible Retrieval\. For more information about storage classes, see [Amazon S3 storage classes](https://aws.amazon.com/s3/storage-classes/)\.

\*\* S3 Glacier Deep Archive requires 40 KB of additional metadata for each archived objects\. This includes 32 KB of metadata charged at the S3 Glacier Deep Archive rate \(required to identify and retrieve your data\), and an additional 8 KB data charged at the S3 Standard rate\. The S3 Standard rate is required to maintain the user\-defined name and metadata for objects archived to Amazon S3 Glacier Deep Archive\. For more information about storage classes, see [Amazon S3 storage classes](https://aws.amazon.com/s3/storage-classes/)\.

Be aware that all of the storage classes except for S3 One Zone\-IA are designed to be resilient to the physical loss of an Availability Zone resulting from disasters\. Also, consider costs, in addition to the performance requirements of your application scenario\. For storage class pricing, see [Amazon S3 pricing](https://aws.amazon.com/s3/pricing/)\.

## Setting the storage class of an object<a name="sc-howtoset"></a>

To set and update object storage classes, you can use the Amazon S3 console, AWS SDKs, or the AWS Command Line Interface \(AWS CLI\)\. All of these approaches use the Amazon S3 APIs to send requests to Amazon S3\.

Amazon S3 API operations support setting \(or updating\) the storage class of objects as follows:
+ When creating a new object, you can specify its storage class\. For example, when creating objects by using the [`PUT` Object](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectPUT.html), [`POST` Object](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectPOST.html), and [Initiate Multipart Upload](https://docs.aws.amazon.com/AmazonS3/latest/API/mpUploadInitiate.html) API operations, you add the `x-amz-storage-class` request header to specify a storage class\. If you don't add this header, Amazon S3 uses S3 Standard, the default storage class\.
+ You can also change the storage class of an object that is already stored in Amazon S3 to any other storage class by making a copy of the object by using the [`PUT` Object \- Copy](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectCOPY.html) API operation\. However, you can't use [`PUT` Object \- Copy](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectCOPY.html) to copy objects that are stored in the S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive storage classes\. You also can't transition from S3 One Zone\-IA to S3 Glacier Instant Retrieval\.

  You copy the object in the same bucket by using the same key name and specifying the request headers as follows:
  + Set the `x-amz-metadata-directive` header to `COPY`\.
  + Set the `x-amz-storage-class` header to the storage class that you want to use\. 

  In a versioning\-enabled bucket, you can't change the storage class of a specific version of an object\. When you copy the object, Amazon S3 gives it a new version ID\.
+ You can change an object's storage class using the Amazon S3 console if the object size is less than 160GB\. If larger, we recommend adding an S3 Lifecycle configuration to change the object's storage class\.
+ You can direct Amazon S3 to change the storage class of objects by adding an S3 Lifecycle configuration to a bucket\. For more information, see [Managing your storage lifecycle](object-lifecycle-mgmt.md)\.
+ When setting up a replication configuration, you can set the storage class for replicated objects to any other storage class\. However, you can't replicate objects that are stored in the S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive storage classes\. For more information, see [Replication configuration](replication-add-config.md)\.

### Restricting access policy permissions to a specific storage class<a name="restricting-storage-class"></a>

When you grant access policy permissions for Amazon S3 operations, you can use the `s3:x-amz-storage-class` condition key to restrict which storage class to use when storing uploaded objects\. For example, when you grant the `s3:PutObject` permission, you can restrict object uploads to a specific storage class\. For an example policy, see [Example 5: Restricting object uploads to objects with a specific storage class](amazon-s3-policy-keys.md#example-storage-class-condition-key)\. 

For more information about using conditions in policies and a complete list of Amazon S3 condition keys, see the following topics:
+ [Actions, resources, and condition keys for Amazon S3](list_amazons3.md)
+ [Amazon S3 condition key examples](amazon-s3-policy-keys.md)