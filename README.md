# AWS-solution-Architect-Q-A

1. IAM Cross-Account Access

Scenario:
A company has two AWS accounts: Production and Logging. EC2 instances in Production must write logs to an S3 bucket in the Logging account securely.

Question: Which solution is MOST secure?

A. Create IAM users in Logging and store access keys on EC2
B. Create an IAM role in Logging and allow Production EC2 to assume it
C. Make the S3 bucket public and restrict by IP
D. Share root credentials between accounts

Answer: ✅ B

Explanation:
Using a cross-account IAM role follows least privilege and avoids long-term credentials. A is insecure (access keys). C exposes data publicly. D is catastrophic.

2. KMS Envelope Encryption

Scenario:
A fintech company must encrypt sensitive data before storing it in S3. Encryption keys must rotate annually.

Question: Which is MOST secure and scalable?

A. Client-side encryption using KMS CMK
B. SSE-S3
C. Hardcoded AES key in app
D. Store encryption key in Secrets Manager only

Answer: ✅ A

Explanation:
Client-side encryption with KMS enables full control and automatic key rotation. SSE-S3 lacks fine-grained key control. C is insecure. D doesn’t encrypt the data itself.

3. Security Groups vs NACL

Scenario:
A web app in public subnet must allow HTTP but block a specific malicious IP.

Question: Which is BEST?

A. Modify Security Group to deny IP
B. Modify NACL to deny IP
C. Remove Internet Gateway
D. Use IAM policy

Answer: ✅ B

Explanation:
Security Groups are stateful and allow-only (no explicit deny). NACLs support explicit deny rules, making them correct for blocking specific IPs.

4. Private Database Access

Scenario:
An RDS database must only be accessed by EC2 instances in the same VPC.

Question: Most secure setup?

A. Make RDS public and restrict by SG
B. Place RDS in private subnet + Security Group rule
C. Use NACL only
D. Allow 0.0.0.0/0

Answer: ✅ B

Explanation:
RDS should be in a private subnet with restricted Security Group access. Public access increases attack surface. NACL alone is insufficient.

5. S3 Public Access Prevention

Scenario:
Developers accidentally made S3 buckets public in the past.

Question: How to prevent this organization-wide?

A. IAM deny policy per user
B. S3 Block Public Access at account level
C. Remove Internet Gateway
D. Enable versioning

Answer: ✅ B

Explanation:
S3 Block Public Access at account level overrides individual bucket settings. IAM per user is not scalable. Versioning does not prevent public access.

6. Secrets in Applications

Scenario:
An app running on EC2 needs database credentials securely.

Question: Best solution?

A. Hardcode credentials
B. Store in environment variables
C. Use AWS Secrets Manager + IAM role
D. Store in S3

Answer: ✅ C

Explanation:
Secrets Manager supports rotation and secure retrieval via IAM role. A and B are insecure. S3 is not designed for secret lifecycle management.

7. Enforce MFA for Admins

Scenario:
Security team wants all IAM admin users to use MFA.

Question: Best enforcement?

A. Send reminder email
B. IAM policy denying actions unless MFA present
C. NACL rule
D. Change password policy

Answer: ✅ B

Explanation:
IAM condition aws:MultiFactorAuthPresent enforces MFA at policy level. Emails and password policies don’t enforce MFA usage.

8. Encrypt EBS Volumes Automatically

Scenario:
A company policy requires all new EBS volumes to be encrypted.

Question: Best solution?

A. Manually encrypt each
B. Enable EBS encryption by default
C. Use NACL
D. Encrypt in OS only

Answer: ✅ B

Explanation:
EBS encryption by default ensures all new volumes are encrypted automatically. Manual methods are error-prone.

9. VPC Peering Security

Scenario:
Two VPCs are peered. One should only access a specific subnet in the other.

Question: How to restrict?

A. Route tables only
B. Security Groups + NACL
C. IAM policy
D. Remove peering

Answer: ✅ B

Explanation:
Route tables enable connectivity; Security Groups and NACLs control traffic. IAM doesn’t control network-level communication.

10. Protect Against DDoS

Scenario:
A public application faces frequent DDoS attempts.

Question: Most appropriate AWS-native protection?

A. Enable AWS Shield Standard
B. Increase EC2 size
C. Block all IPs
D. Use NACL only

Answer: ✅ A

Explanation:
AWS Shield Standard provides automatic DDoS protection. Scaling instances doesn’t stop DDoS. NACL alone is insufficient.

🎯 Exam Pattern Insight for You (Nakul)

Notice the pattern:

If question says “most secure” → Think IAM roles, private subnets, KMS, Block Public Access

If question says “block specific IP” → Think NACL

If question says “avoid credentials” → Think IAM role

This thinking pattern alone gives you +8–10 marks.

Reply with “Next 10” and we’ll move into:
Resilient Architectures + some tricky high-availability traps.


11. Multi-AZ High Availability (RDS)

Scenario:
An ecommerce application uses Amazon RDS. The company requires automatic failover if the primary DB instance fails.

Question: Which solution is MOST resilient?

A. Create Read Replica in same AZ
B. Enable Multi-AZ deployment
C. Daily manual backups
D. Snapshot every hour

Answer: ✅ B

Explanation:
Multi-AZ provides synchronous replication and automatic failover. Read replicas are for scaling reads, not HA. Snapshots and backups do not provide automatic failover.

12. EC2 Auto Scaling Failure Recovery

Scenario:
An application runs on EC2 behind an Application Load Balancer. Instances sometimes fail health checks.

Question: What ensures automatic replacement?

A. CloudWatch alarm only
B. Auto Scaling Group with health checks
C. Increase instance size
D. Manual restart

Answer: ✅ B

Explanation:
Auto Scaling Groups detect unhealthy instances and replace them automatically. CloudWatch alone does not replace instances.

13. Cross-Region Disaster Recovery

Scenario:
A company needs a disaster recovery setup with minimal cost. RTO can be several hours.

Question: Best strategy?

A. Multi-site active-active
B. Warm standby
C. Backup & Restore
D. Pilot light

Answer: ✅ C

Explanation:
Backup & Restore is lowest cost and acceptable when RTO is hours. Warm standby and pilot light are faster but cost more.

14. Route 53 Failover Routing

Scenario:
A primary web server is in us-east-1. A secondary server in us-west-2 must take over if primary fails.

Question: Which Route 53 routing policy?

A. Weighted
B. Latency
C. Failover
D. Simple

Answer: ✅ C

Explanation:
Failover routing uses health checks to redirect traffic when primary fails. Weighted and latency are not failover mechanisms.

15. Stateless Application Design

Scenario:
A company wants to scale web servers horizontally.

Question: What design supports resilience?

A. Store sessions locally on EC2
B. Store sessions in ElastiCache
C. Single large EC2 instance
D. Store sessions in instance memory

Answer: ✅ B

Explanation:
Externalizing session storage (ElastiCache) allows stateless scaling. Local storage prevents failover and scaling.

16. S3 Cross-Region Replication

Scenario:
Critical compliance data must be stored in two AWS Regions automatically.

Question: Best solution?

A. Manual copy
B. S3 Cross-Region Replication
C. Snapshot EC2
D. Lifecycle rule only

Answer: ✅ B

Explanation:
CRR automatically replicates objects across Regions. Lifecycle policies don’t replicate.

17. Application Tier Isolation

Scenario:
A 3-tier app must remain available even if one AZ fails.

Question: Best architecture?

A. All tiers in one AZ
B. Web tier multi-AZ only
C. Deploy all tiers across multiple AZs
D. Single large server

Answer: ✅ C

Explanation:
True resilience requires multi-AZ for every tier. Single-AZ deployment creates single point of failure.

18. DynamoDB High Availability

Scenario:
A gaming app uses DynamoDB and must survive Region failure.

Question: Which feature ensures resilience?

A. DynamoDB DAX
B. Global Tables
C. Read replicas
D. EBS snapshot

Answer: ✅ B

Explanation:
DynamoDB Global Tables replicate across Regions for active-active resilience. DAX is caching only.

19. ELB Health Checks

Scenario:
Users complain that failed EC2 instances still receive traffic.

Question: What should be configured?

A. Route table updates
B. ELB health checks properly configured
C. IAM roles
D. Larger instance size

Answer: ✅ B

Explanation:
Load balancers route traffic only to healthy targets. Misconfigured health checks cause unhealthy instances to still receive traffic.

20. EFS Resilience

Scenario:
Multiple EC2 instances across AZs need shared file storage.

Question: Which service provides resilient shared storage?

A. EBS
B. Instance store
C. EFS
D. S3 One Zone

Answer: ✅ C

Explanation:
EFS is multi-AZ and designed for shared access. EBS is AZ-specific. Instance store is ephemeral.

🎯 Exam Trap Patterns (Very Important)

When exam says:

“Automatic failover” → Multi-AZ

“Low cost DR” → Backup & Restore

“Cross-region active-active” → Global Tables

“Horizontal scaling” → Stateless + Auto Scaling

Memorize these mappings. They repeat constantly.


21. High Read Traffic on Database

Scenario:
An ecommerce app experiences heavy read traffic on its RDS MySQL database. Write traffic is moderate.

Question: Which solution improves performance MOST efficiently?

A. Increase instance size
B. Add RDS Read Replicas
C. Enable Multi-AZ
D. Move to EC2 database

Answer: ✅ B

Explanation:
Read Replicas scale read workloads. Multi-AZ is for HA, not performance scaling. Vertical scaling (A) is limited and costly.

22. Global Static Content Performance

Scenario:
A media company serves static content globally from S3. Users report high latency internationally.

Question: Most performant solution?

A. Increase S3 storage class
B. Use CloudFront distribution
C. Use EBS instead
D. Move S3 to single region only

Answer: ✅ B

Explanation:
CloudFront caches content at edge locations worldwide, reducing latency. Storage class doesn’t affect global performance.

23. DynamoDB Read Latency

Scenario:
A mobile app using DynamoDB experiences high read latency during peak hours.

Question: What improves performance MOST?

A. Increase RCU
B. Add DynamoDB DAX
C. Enable Multi-AZ
D. Use EBS

Answer: ✅ B

Explanation:
DAX provides in-memory caching for microsecond latency reads. Increasing RCU helps throughput but not cache-level latency improvements.

24. High Write Throughput in DynamoDB

Scenario:
A gaming leaderboard requires extremely high write throughput.

Question: Best configuration?

A. On-demand capacity mode
B. Provisioned capacity with auto scaling
C. Store data in S3
D. Use EFS

Answer: ✅ B

Explanation:
Provisioned with auto scaling ensures predictable high throughput. On-demand is flexible but can be costly and less optimized for sustained heavy load.

25. Reduce Database Load

Scenario:
A frequently accessed product catalog rarely changes.

Question: Most performant solution?

A. Increase DB size
B. Add ElastiCache Redis
C. Use Multi-AZ
D. Snapshot database

Answer: ✅ B

Explanation:
ElastiCache offloads read pressure from DB, significantly improving response times. Multi-AZ doesn’t reduce load.

26. Large File Upload Performance

Scenario:
Users globally upload large files to S3.

Question: Best way to improve upload performance?

A. Enable S3 Transfer Acceleration
B. Use Glacier
C. Move to EBS
D. Reduce file size manually

Answer: ✅ A

Explanation:
Transfer Acceleration uses CloudFront edge network to speed up uploads globally. Glacier is archival only.

27. Auto Scaling Based on Demand

Scenario:
Traffic spikes unpredictably during promotions.

Question: Best scaling policy?

A. Scheduled scaling only
B. Target tracking scaling policy
C. Manual scaling
D. Increase instance size permanently

Answer: ✅ B

Explanation:
Target tracking automatically adjusts capacity based on metrics like CPU utilization. Scheduled scaling won’t handle unpredictable spikes.

28. High IOPS Requirement

Scenario:
A financial app requires extremely high IOPS for database storage.

Question: Best storage option?

A. General Purpose gp2
B. Provisioned IOPS SSD (io2)
C. S3 Standard
D. EFS

Answer: ✅ B

Explanation:
Provisioned IOPS SSD (io2) delivers consistent high IOPS performance. gp2 cannot guarantee high consistent throughput.

29. Global API Performance

Scenario:
An API hosted behind ALB in us-east-1 serves global users.

Question: Best way to reduce global latency?

A. Route 53 latency routing only
B. Deploy in single region
C. Use CloudFront in front of ALB
D. Increase EC2 size

Answer: ✅ C

Explanation:
CloudFront reduces latency by caching at edge and optimizing TCP connections. Larger instances don’t reduce network distance latency.

30. S3 Request Performance Optimization

Scenario:
An analytics company stores millions of objects in a single S3 bucket and experiences request throttling.

Question: Best solution?

A. Use random object key prefixes
B. Reduce object count
C. Change storage class
D. Move to EBS

Answer: ✅ A

Explanation:
S3 scales automatically but spreading requests across prefixes improves parallelism. Storage class doesn’t affect request scaling.

🎯 Performance Exam Patterns

If question says:

“Reduce read latency” → DAX / ElastiCache

“Global performance” → CloudFront

“High IOPS” → io2

“Scale reads in RDS” → Read Replicas

“Upload faster globally” → Transfer Acceleration

These appear again and again.

Reply “Next 10” and we’ll move into:

💰 Cost-Optimized Architectures (Spot, Savings Plans, Storage classes, lifecycle policies — very exam heavy).

Next 10

Good. 🔥 Now we move into Design Cost-Optimized Architectures (very scoring domain if you know the patterns).

Exam trick:
If the question says “most cost-effective” → eliminate high-availability overkill, eliminate overprovisioning, think lifecycle, Spot, Savings Plans.

✅ SAA-C03 High-Yield Questions (31–40)
31. Steady-State Compute Workload

Scenario:
A company runs predictable EC2 workloads 24/7 for the next 3 years.

Question: Most cost-effective option?

A. On-Demand Instances
B. Spot Instances
C. Compute Savings Plan
D. Dedicated Hosts

Answer: ✅ C

Explanation:
Savings Plans provide major discounts for predictable long-term workloads. Spot is interruptible. On-Demand is most expensive long term.

32. Batch Processing Jobs

Scenario:
A company runs nightly batch processing that can tolerate interruptions.

Question: Most cost-efficient solution?

A. On-Demand
B. Reserved Instances
C. Spot Instances
D. Dedicated Instances

Answer: ✅ C

Explanation:
Spot Instances are ideal for fault-tolerant workloads. They offer up to 90% discount. Reserved is better for steady workloads.

33. S3 Data Access Pattern Change

Scenario:
Application data is accessed frequently for 30 days, then rarely.

Question: Most cost-optimized solution?

A. Keep in S3 Standard
B. Move manually
C. S3 Lifecycle rule to Standard-IA
D. Use EBS

Answer: ✅ C

Explanation:
Lifecycle policies automatically transition objects to cheaper tiers. Manual processes are error-prone.

34. Archival Data (Compliance)

Scenario:
Data must be retained for 7 years for compliance but rarely accessed.

Question: Most cost-effective storage?

A. S3 Standard
B. S3 Glacier Deep Archive
C. EFS
D. gp3 EBS

Answer: ✅ B

Explanation:
Glacier Deep Archive is lowest-cost storage for long-term retention. Other options are significantly more expensive.

35. Dev/Test Environment

Scenario:
Developers use EC2 during office hours only.

Question: Most cost-effective solution?

A. Run 24/7
B. Stop/start instances with automation
C. Use Dedicated Hosts
D. Use larger instance

Answer: ✅ B

Explanation:
Stopping instances when not needed saves compute charges. Dedicated hosts increase cost.

36. Underutilized EC2 Instances

Scenario:
CloudWatch shows CPU consistently below 10%.

Question: Best cost optimization?

A. Terminate all
B. Right-size instances
C. Increase instance size
D. Enable Multi-AZ

Answer: ✅ B

Explanation:
Rightsizing reduces cost while maintaining performance. Increasing size worsens waste.

37. Mixed Workload with Baseline Usage

Scenario:
A startup has baseline EC2 usage plus unpredictable spikes.

Question: Most cost-effective strategy?

A. All On-Demand
B. All Reserved
C. Savings Plan for baseline + Spot for spikes
D. Dedicated Hosts

Answer: ✅ C

Explanation:
Combine Savings Plans for steady load and Spot for variable load. This hybrid approach maximizes savings.

38. S3 Small Objects Retrieval Pattern Unknown

Scenario:
A company doesn’t know future access patterns of stored objects.

Question: Best cost-optimized storage class?

A. S3 Standard
B. S3 Intelligent-Tiering
C. Glacier
D. EBS

Answer: ✅ B

Explanation:
Intelligent-Tiering automatically moves objects between tiers based on usage without performance impact.

39. RDS Cost Reduction

Scenario:
A production RDS instance is underutilized during nights.

Question: Most cost-effective approach?

A. Delete database nightly
B. Switch to smaller instance
C. Move to Multi-AZ
D. Increase storage

Answer: ✅ B

Explanation:
Rightsizing reduces cost without affecting availability. You cannot stop production RDS like EC2.

40. Data Transfer Cost Optimization

Scenario:
Large data transfer costs between EC2 and S3 in same region.

Question: Most cost-effective architecture?

A. Use NAT Gateway
B. Use VPC Gateway Endpoint for S3
C. Use Internet Gateway
D. Use Elastic IP

Answer: ✅ B

Explanation:
VPC Gateway Endpoints allow private connection to S3 without internet and reduce data transfer charges.

🎯 Cost Optimization Exam Patterns

If exam says:

“7 years retention” → Glacier Deep Archive

“Unpredictable access” → Intelligent-Tiering

“Interruptible workload” → Spot

“Predictable 1–3 years” → Savings Plan

“Private access to S3” → VPC Endpoint

Memorize these mapping rules.


