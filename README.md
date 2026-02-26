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
