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
