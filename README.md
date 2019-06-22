
# Table of Contents

1.  [AWS](#org0c17809)
    1.  [Resources](#orgd1ee834)
    2.  [Installing CLI](#orgeb9cd3b)
    3.  [SQS vs SNS](#orge7d6b3c)
    4.  [SQS](#org94e08d0)
2.  [AWS Architect Associate](#org7f9f5b5)
    1.  [Cross Account AMI](#org0aba91f)
    2.  [EC2](#orgb4fa150)
    3.  [ASG](#org44e967d)
    4.  [RDS](#orgce53a46)
    5.  [Aurora](#orga815661)
    6.  [Kinesis](#org2244807)
        1.  [Streams](#org156f4ef)
        2.  [Shards](#org5e4ad41)
    7.  [ElastiCache for Solutions Architects](#org311eabe)
    8.  [S3](#orgeaad785)
        1.  [S3 Encryption for Objects](#orge85921f)
3.  [Terms](#orgc6f8f34)
        1.  [Record Sets](#org4a50899):DNS:ROUTE54:
4.  [Exam Questions](#org9e3f10c)
    1.  [Question](#org01e4e20)
        1.  [Answer](#org24c8e8f)
    2.  [Question](#org0aaddac)
        1.  [Explanation](#org10af1d6)
    3.  [Question 3:](#orgb6946f3)
    4.  [Question 4](#org0d50271)
5.  [Popular exam topics](#org8fc7dc0)
    1.  [CORS](#org346d894)



<a id="org0c17809"></a>

# AWS


<a id="orgd1ee834"></a>

## Resources

[auto scaling cheat sheet](https://tutorialsdojo.com/aws-cheat-sheet-aws-auto-scaling/)
[Create apps using serverless architecture](https://serverless.com/)

-   Use cloudguru for exams
-   Use linux academy for hands one
-   akamai


<a id="orgeb9cd3b"></a>

## Installing CLI

[AWS CLI install instructions](https://docs.aws.amazon.com/cli/latest/userguide/install-macos.html#install-bundle-macos)

You can install python 3, but you have to specify the path when installing

    brew install python
    PY_PATH=$(which python3)
      
    curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
    unzip awscli-bundle.zip
    sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
    
    sudo $PY_PATH awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws

Or if you already are using pip:

    pip3 install awscli --upgrade --user

If you use pip you will need to add the aws path to your $PATH
That should be something like:

    $HOME/Library/Python/<python-version>/bin

Then configure the cli using your creds.

    aws configure


<a id="orge7d6b3c"></a>

## SQS vs SNS

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">&#xa0;</th>
<th scope="col" class="org-left">SQS (Simple Queuing Service)</th>
<th scope="col" class="org-left">SNS (Simple Notification Service)</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">Type</td>
<td class="org-left">Queue</td>
<td class="org-left">Topic (Pub/Sub system)</td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">Message consumption</td>
<td class="org-left">Pull</td>
<td class="org-left">Push</td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">Use Case</td>
<td class="org-left">Decoupling applictions</td>
<td class="org-left">Fanout</td>
</tr>


<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">Parallel asych processsing</td>
<td class="org-left">Processing same message different ways</td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">Persistence</td>
<td class="org-left">Configurable</td>
<td class="org-left">None</td>
</tr>


<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">Persisted for some time</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">Consumer Type</td>
<td class="org-left">Consumers are supposed to be identical.</td>
<td class="org-left">Consumers can process messages in different ways</td>
</tr>
</tbody>
</table>


<a id="org94e08d0"></a>

## SQS

Amazon Simple Queue Service (SQS) is a highly scalable distributed message
queuing service provided by Amazon.

There are two types of queues

-   **Standard:** Nearly unlimited throughput with best-effort ordering. At-least-once delivery (You might get same message twice.)

-   **FIFO:** Limited number of transactions per second (TPS). See Amazon SQS FIFO developer

guide for more information on limits. Order in which messages are sent/received
is strictly preserved Exactly-once delivery


<a id="org7f9f5b5"></a>

# AWS Architect Associate


<a id="org0aba91f"></a>

## Cross Account AMI

-   Sharing an AMI to other AWS accounts is possible.

-   The owner of the AMI retains ownership of that AMI
    -   you are the owner of the target AMI in your account.

-   To copy an AMI that was shared with, the owner of the
    source AMI must grant you read permissions for the **storage that backs the AMI**,
    either the associated EBS snapshot (for an Amazon EBS-backed AMI) or an
    associated S3 bucket (for an instance store-backed AMI).

-   Limits:

-   Encrypted AMIS shared to your account cannot be copied.

-   You can, instead, if the underlying snapshot and encryption key were shared with you,
    you can copy the snapshot while re- encrypting it with a key of your own.You
    own the copied snapshot, and can register it as a new AMI.

-   You can't copy an AMI with an associated **billingProduct** code that was shared
    with you from another account.This includes Windows AMIs and AMIs from the AWS
    Marketplace.To copy a shared AMI with a **billingProduct** code, launch an EC2
    instance in your account using the shared AMI and then create an AMI from the
    instance.

<https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/CopyingAMIs.html>


<a id="orgb4fa150"></a>

## EC2

EC2 instances are billed by the second, t2.micro is free tier

On Linux / Mac we use SSH, on Windows we use Putty

SSH is on port 22, lock down the security group to your IP

Timeout issues => Security groups issues

Permission issues on the SSH key => run “chmod 0400”

Security Groups can reference other Security Groups instead of IP
ranges (very popular exam question)

Know the difference between Private, Public and Elastic IP

You can customize an EC2 instance at boot time using EC2 User Data

Know the 4 EC2 launch modes:

-   On demand
-   Reserved
-   Spot instances
-   Dedicated Hosts
-   Know the basic instance types: R,C,M,I,G,T2/T3

-   You can create AMIs to pre-install software on your EC2 => faster boot
-   AMI can be copied across regions and accounts
-   EC2 instances can be started in placement groups:
    -   Cluster
    -   Spread


<a id="org44e967d"></a>

## ASG

 ASG Default Termination Policy (simplified version):
. Find the AZ which has the most number of instances
. If there are multiple instances in the AZ to choose from, delete the one with the oldest launch configuration

 ASG tries the balance the number of instances across AZ by default
vailability zone A
 v1 v2 v1 v2
 Auto Scaling group
vailability zone B
 v1 v1 v1
    © Stephane Maarek
 ASG for Solutions Architects Scaling Cooldowns

-   The cooldown period helps to ensure that your Auto Scaling group doesn't
    launch or terminate additional instances before the previous scaling activity
    takes effect.

In addition to default cooldown for Auto Scaling group, we can create cooldowns that apply to a specific simple scaling policy

A scaling-specific cooldown period overrides the default cool down period.

One common use for scaling-specific cool downs is with a scale-in policy
a policy that terminates instances based on a specific criteria or metric.
Because this policy terminates instances, Amazon EC2 Auto Scaling needs less
time to determine whether to terminate additional instances.

If the default cooldown period of 300 seconds is too long, you can reduce
costs by applying a scaling-specific cooldown period of 180 seconds to the
scale-in policy.

 If your application is scaling up and down multiple times each hour,modifythe
 Auto Scaling Groups cool-down timers and the CloudWatch Alarm Period that
 triggers the scale in
ttps://docs.aws.amazon.com/autoscaling/ec2/userguide/Cooldown.html


<a id="orgce53a46"></a>

## RDS

-   Read replicas are used for SELECT (=read) only kind of statements (not INSERT, UPDATE, DELETE)

-   Amazon RDS supports Transparent Data Encryption for DB encryption:
-   Oracle or SQL Server DB instance only
-   TDE can be used on top of KMS – may affect performance

-   IAM Authentication (versus traditional username / password):
    -   Works for MySQL, PostgreSQL
    -   Lifespan of an authentication token is 15 minutes (short-lived)
    -   Tokens are generated by AWS credentials
    -   SSL must be used when connecting to the database
    -   Easy to use EC2 Instance Roles to connect to the RDS database


<a id="orga815661"></a>

## Aurora

-   Can use IAM authentication for Aurora MySQL and PostgresSQL
    -   Aurora Global databases span multiple regions and enable DR • One Primary Region
    -   One DR Region
    -   The DR region can be used for lower latency reads
    -   < 1 second replica lag on average
-   If not using Global Databases, you can create cross-region Read Replicas
-   But the FAQ recommends you use Global Databases instead


<a id="org2244807"></a>

## Kinesis


<a id="org156f4ef"></a>

### Streams

-   Divided into partitions
-   Can retain data from one day up to seven days
-   Can have multiple consumers
-   Real-time processing
-   Immutable data


<a id="org5e4ad41"></a>

### Shards

-   a stream has many shards
-   1 MB/s or 1000 messages at write **per shard**
-   2 MB/s at **read per shard**
-   Billed per shard
-   Users can have unlimited shards
-   Messages are ordered per shard


<a id="org311eabe"></a>

## ElastiCache for Solutions Architects

-   Security:
    -   Redis support Redis AUTH (username / password)
    -   SSL in-flight encryption must be enabled and used
    -   Memcached support SASL authentication (advanced)
    -   None of the caches support IAM authentication
    -   IAM policies on ElastiCache are only used for AWS API-level security

-   Patterns for ElastiCache:
    -   Lazy Loading: all the read data is cached, data can become stale in cache
    -   Write Through: Adds or update data in the cache when written to a DB (no
        stale data)
    -   Session Store: store temporary session data in a cache (using TTL features)


<a id="orgeaad785"></a>

## S3


<a id="orge85921f"></a>

### S3 Encryption for Objects

1.  SSE-S3

    -   AWS S3 manages keys
    -   server-side encryption
    -   AES-256
    -   Must set header: “x-amz-server-side-encryption": "AES256"

2.  SSE-KMS

    -   encryption using keys handled & managed by KMS
    -   KMS Advantages: **user control + audit trail\***
    -   Object is encrypted server side
    -   Must set header: “x-amz-server-side-encryption": ”aws:kms"

3.  SSE-C

    -   server-side encryption
    -   customer manages keys (not in aws)
    -   HTTPS must be used
    -   Encryption key must provided in HTTP headers, for every HTTP request made

4.  Client Side Encryption

    • Client library such as the Amazon S3 Encryption Client
    • Clients must encrypt data themselves before sending to S3
    • Clients must decrypt data themselves when retrieving from S3
    • Customer fully manages the keys and encryption cycle


<a id="orgc6f8f34"></a>

# Terms

-   **<a id="org158c22b">Cross Orgin Resoure Sharing</a> <a id="org366d51e">CORS</a>:** allows you to limit the number
    of websites that can request your files in S3.
    
    (Lowers cost)
-   **KMS:** Amazon's Key Management System

-   **Route 54:** Amazon Route 53 provides highly available and scalable Domain Name System (DNS), domain
    name registration, and health-checking web services. With Amazon Route 53, you can create and manage
    your public DNS records.
    
    Akamai is more widely used.
    
    [faqs](https://aws.amazon.com/route53/faqs/)

-   **<a id="org4a97e18">record set</a>:** 


<a id="org4a50899"></a>

### Record Sets     :DNS:ROUTE54:

Some common record sets: [link](https://ns1.com/resources/dns-types-records-servers-and-queries)

-   **Address Mapping record (A Record):** AKA DNS host record, stores a hostname and its corresponding IPv4 address.

-   **Canonical Name record (CNAME Record):** used to alias a hostname to another hostname.
    When a DNS client requests a record that contains a CNAME, which points to
    another hostname, the DNS resolution process is repeated with the new
       hostname.

-   **Certificate record (CERT Record):** stores encryption certificates—PKIX, SPKI, PGP, and so on

-   **IP Version 6 Address record (AAAA Record):** stores a hostname and its corresponding IPv6 address.

-   **Mail exchanger record (MX Record):** specifies an SMTP email server for the domain, used to route
    outgoing emails to an email server.

-   **Name Server records (NS Record):** specifies that a DNS Zone, such as “example.com” is delegated to
    a specific Authoritative Name Server, and provides the address of the name server.

-   **Reverse-lookup Pointer records (PTR Record):** allows a DNS resolver to provide an IP address and r
    eceive a hostname (reverse DNS lookup).

-   **Service Location (SRV Record):** a service location record, like MX but for other communication protocols

-   **Text Record (TXT Record):** typically carries machine-readable data such as opportunistic encryption, sen
    der policy framework, DKIM, DMARC, etc.

-   Start of Authority (SOA Record) ::this record appears at the beginning of a DNS zone file, and indicates
    the Authoritative Name Server for the current DNS zone, contact details for the domain administrator,
    domain serial number, and information on how frequently DNS information for this zone should be refreshed.


<a id="org9e3f10c"></a>

# Exam Questions


<a id="org01e4e20"></a>

## Question

A tech company has a CRM application hosted on an Auto Scaling group of
On-Demand EC2 instances. The application is extensively used during office hours
from 9 in the morning till 5 in the afternoon. Their users are complaining that
the performance of the application is slow during the start of the day but then
works normally after a couple of hours.

Which of the following can be done to ensure that the application works properly
at the beginning of the day?


<a id="org24c8e8f"></a>

### Answer

Scaling based on a schedule allows you to scale your application in response to
predictable load changes. For example, every week the traffic to your web
application starts to increase on Wednesday, remains high on Thursday, and
starts to decrease on Friday. You can plan your scaling activities based on the
predictable traffic patterns of your web application.

&#x2014; There was a picture here -&#x2014;

To configure your Auto Scaling group to scale based on a schedule, you create a
scheduled action. The scheduled action tells Amazon EC2 Auto Scaling to perform
a scaling action at specified times. To create a scheduled scaling action, you
specify the start time when the scaling action should take effect, and the new
minimum, maximum, and desired sizes for the scaling action. At the specified
time, Amazon EC2 Auto Scaling updates the group with the values for minimum,
maximum, and desired size specified by the scaling action. You can create
scheduled actions for scaling one time only or for scaling on a recurring
schedule.

Option 3 is the correct answer. You need to configure a Scheduled scaling
policy. This will ensure that the instances are already scaled up and ready
before the start of the day since this is when the application is used the most.

Options 1 and 2 are incorrect because although this is a valid solution, it is
**still better** to configure a Scheduled scaling policy as you already know the
exact peak hours of your application. By the time either the CPU or Memory hits
a peak, the application already has performance issues, so you need to ensure
the scaling is done beforehand using a Scheduled scaling policy.

Option 4 is incorrect. Although the Application load balancer can also balance
the traffic, it cannot increase the instances based on demand.


<a id="org0aaddac"></a>

## Question

As the Solutions Architect of the company, which of the following should you do
to meet the above requirement?

You are deploying an Interactive Voice Response (IVR) telephony system in your
cloud architecture that interacts with callers, gathers information, and routes
calls to the appropriate recipients in your company. The system will be composed
of an Auto Scaling group of EC2 instances, an Application Load Balancer, and an
RDS instance in a Multi-AZ Deployments configuration. To protect the
confidential data of your customers, you have to ensure that your RDS database
can only be accessed using the profile credentials specific to your EC2
instances via an authentication token.

As the Solutions Architect of the company, which of the following should you do
to meet the above requirement?


<a id="org10af1d6"></a>

### Explanation

You can authenticate to your DB instance using AWS Identity and Access
Management (IAM) database authentication. IAM database authentication works with
MySQL and PostgreSQL. With this authentication method, you don't need to use a
password when you connect to a DB instance. Instead, you use an authentication
token.

An authentication token is a unique string of characters that Amazon RDS
generates on request. Authentication tokens are generated using AWS Signature
Version 4. Each token has a lifetime of 15 minutes. You don't need to store user
credentials in the database, because authentication is managed externally using
IAM. You can also still use standard database authentication.

![img](https://udemy-images.s3.amazonaws.com/redactor/raw/2019-01-13_07-04-06-a2157247b0fa129795001208504fcb51.png)

IAM database authentication provides the following benefits:

Network traffic to and from the database is encrypted using Secure Sockets Layer
(SSL).

You can use IAM to centrally manage access to your database resources, instead
of managing access individually on each DB instance.

For applications running on Amazon EC2, you can use profile credentials specific
to your EC2 instance to access your database instead of a password, for greater
security

Hence, Option 1 is the correct answer based on the above reference.

Option 2 is incorrect because an SSL connection is not using an authentication
token from IAM. Although configuring SSL to your application can improve the
security of your data in flight, it is still not a suitable option to use in
this scenario.

Option 3 is incorrect because although you can create and assign an IAM Role to
your EC2 instances, you still need to configure your RDS to use IAM DB
Authentication.

Option 4 is incorrect because you have to use IAM DB Authentication for this
scenario, and not a combination of an IAM and STS. Although STS is used to send
temporary tokens for authentication, this is not a compatible use case for RDS.


<a id="orgb6946f3"></a>

## Question 3:

You founded a tech startup that provides online training and software
development courses to various students across the globe. Your team has
developed an online portal in AWS where the students can log into and access the
courses they are subscribed to.

Since you are in the early phases of the startup and the funding is still hard
to come by, which service can help you manage the budgets for all your AWS
resources?


<a id="org0d50271"></a>

## Question 4

Explanation

AWS Budgets gives you the ability to set custom budgets that alert you when your
costs or usage exceed (or are forecasted to exceed) your budgeted amount.

Budgets can be tracked at the monthly, quarterly, or yearly level, and you can
customize the start and end dates. You can further refine your budget to track
costs associated with multiple dimensions, such as AWS service, linked account,
tag, and others. Budget alerts can be sent via email and/or Amazon Simple
Notification Service (SNS) topic.

You can also use AWS Budgets to set a custom reservation utilization target and
receive alerts when your utilization drops below the threshold you define. RI
utilization alerts support Amazon EC2, Amazon RDS, Amazon Redshift, and Amazon
ElastiCache reservations.

Budgets can be created and tracked from the AWS Budgets dashboard or via the
Budgets API.

Option 1 is incorrect because the Cost Explorer only helps you visualize and
manage your AWS costs and usages over time. It offers a set of reports you can
view data with for up to the last 13 months, forecast how much you're likely to
spend for the next three months, and get recommendations for what Reserved
Instances to purchase. You use Cost Explorer to identify areas that need further
inquiry and see trends to understand your costs.

Option 2 is incorrect because Cost Allocation Tags only eases the organization
of your resource costs on your cost allocation report, to make it easier for you
to categorize and track your AWS costs.

Option 4 is incorrect because the payment history option only provides a
location where you can view the monthly invoices you receive from AWS. If your
account isn't past due, the Payment History page shows only previous invoices
and payment status.

Reference:

<https://aws.amazon.com/aws-cost-management/aws-budgets/>

Check out this AWS Billing and Cost Management Cheat Sheet:

<https://tutorialsdojo.com/aws-cheat-sheet-aws-billing-and-cost-management/>


<a id="org8fc7dc0"></a>

# Popular exam topics


<a id="org346d894"></a>

## CORS

