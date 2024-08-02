
1. **Which AWS service could an administrator use to securely control access to AWS resources for users?**
   - [ ] AWS CloudFormation
   - [ ] AWS Identity and Access Management (IAM)
   - [ ] AWS Config
   - [ ] AWS CloudTrail

   **Answer:** AWS Identity and Access Management (IAM)

2. **What are characteristics of the security principle to prepare for security events? (Select TWO.)**
   - [ ] Use subnets in only one Availability Zone.
   - [ ] Create processes to isolate incidents and restore operations.
   - [ ] Use different AWS services.
   - [ ] Routinely practice incident response through game days.
   - [ ] Use hard-coded credentials.

   **Answers:** Create processes to isolate incidents and restore operations. Routinely practice incident response through game days.

3. **Which security principle involves monitoring, alerting, and auditing actions and changes to the environment in real time?**
   - [ ] Secure all layers.
   - [ ] Enable traceability.
   - [ ] Protect data in transit and at rest.
   - [ ] Apply the principle of least privilege.

   **Answer:** Enable traceability.

4. **Most AWS Identity and Access Management (IAM) policies are defined and stored in a specific document format. Which document format is most commonly used?**
   - [ ] JavaScript Object Notation (JSON)
   - [ ] Security Assertion Markup Language (SAML)
   - [ ] Extensible Markup Language (XML)
   - [ ] Hypertext Markup Language (HTML)

   **Answer:** JavaScript Object Notation (JSON)

5. **The administrator of a company’s AWS resources creates a standalone policy and attaches it to a principal entity to define their permissions. Which type of policy has the administrator created?**
   - [ ] Permissions boundary
   - [ ] AWS managed policy
   - [ ] Customer managed policy
   - [ ] Service-linked policy

   **Answer:** Customer managed policy

6. **What is a feature of the AWS Identity and Access Management (IAM) service?**
   - [ ] Decentralized management of users and access
   - [ ] Protection against standards-based identity providers
   - [ ] Information assurance obfuscation
   - [ ] Temporary access through the use of roles

   **Answer:** Decentralized management of users and access

7. **An AWS Identity and Access Management (IAM) administrator determines what users need to do, determines the correct set of permissions, and then crafts policies that allow the users to perform only those tasks. Which principle is the administrator adhering to?**
   - [ ] Principle of least privilege
   - [ ] Principle of authentication
   - [ ] Principle of managed policy
   - [ ] Principle of availability

   **Answer:** Principle of least privilege

8. **A company wants to use an automated security assessment tool to improve the security and compliance of applications that they deploy on AWS. Which AWS service would meet their need?**
   - [ ] Amazon GuardDuty
   - [ ] AWS Identity and Access Management (IAM)
   - [ ] Amazon Inspector
   - [ ] Elastic Load Balancing (ELB)

   **Answer:** Amazon Inspector

9. **A company wants to automate management tasks such as collecting system inventory, applying operating system patches, maintaining up-to-date antivirus definitions, and configuring operating systems and applications at scale. Which AWS service would provide these abilities?**
   - [ ] AWS Systems Manager
   - [ ] Elastic Load Balancing (ELB)
   - [ ] AWS Lambda
   - [ ] Amazon Simple Notification Service (Amazon SNS)

   **Answer:** AWS Systems Manager

10. **A company wants to improve their security and compliance tracking without creating additional work or needing additional employees. Which AWS service would help meet the company's needs?**
    - [ ] Amazon Inspector
    - [ ] AWS Shield
    - [ ] Elastic Load Balancing (ELB)
    - [ ] AWS Identity and Access Management (IAM)

    **Answer:** Amazon Inspector

11. **An administrator wants to distribute incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, and IP addresses. The solution must be able to handle the varying load of traffic in a single Availability Zone or across multiple Availability Zones. Which security feature should they implement?**
    - [ ] Subnets
    - [ ] Security groups
    - [ ] Network access control lists (ACLs)
    - [ ] Elastic Load Balancing (ELB)

    **Answer:** Elastic Load Balancing (ELB)

12. **An administrator set up a virtual private cloud (VPC). They want to increase security and create a virtual firewall for their Amazon EC2 instances to control inbound and outbound traffic. Which security feature would meet their needs?**
    - [ ] Subnets
    - [ ] Elastic Load Balancing (ELB)
    - [ ] Security groups
    - [ ] Internet gateway

    **Answer:** Security groups

13. **What is a characteristic of Amazon S3 server-side encryption with AWS Key Management Service (AWS KMS) keys (SSE-KMS)?**
    - [ ] The keys and algorithms are known only to the customer.
    - [ ] The application encrypts data before sending it to AWS.
    - [ ] The customer manages the keys.
    - [ ] The customer can audit usage of the key.

    **Answer:** The customer can audit usage of the key.

14. **A cloud security administrator wants to protect data in transit. Which practice would best accomplish this?**
    - [ ] Automate identification and classification of the data.
    - [ ] Identify the data within your workload.
    - [ ] Use Secure Sockets Layer (SSL) endpoints over Transport Layer Security (TLS). This method is known as HTTPS.
    - [ ] Separate workloads by using separate accounts.

    **Answer:** Use Secure Sockets Layer (SSL) endpoints over Transport Layer Security (TLS). This method is known as HTTPS.

15. **Which service uses machine learning to automatically discover, classify, and protect sensitive data in AWS?**
    - [ ] Amazon Macie
    - [ ] AWS Key Management Service (AWS KMS)
    - [ ] AWS Secrets Manager
    - [ ] AWS Certificate Manager (ACM)

    **Answer:** Amazon Macie

16. **A company wants to establish a secure solution for its workforce and machine identities that require secrets, such as passwords to third-party applications, with automatic rotation. Which AWS service is the best solution for the company to store and use secrets securely?**
    - [ ] AWS Secrets Manager
    - [ ] AWS Certificate Manager Private Certificate Authority
    - [ ] Amazon Macie
    - [ ] AWS Key Management Service (AWS KMS)

    **Answer:** AWS Secrets Manager

17. **A company needs to store sensitive documents in an Amazon S3 bucket. The documents must be encrypted at rest at their destination, and the company wants to manage the underlying keys that are used for encryption. Which method could they use for this purpose?**
    - [ ] Server-side encryption with customer-provided keys
    - [ ] Server-side encryption with Amazon S3 managed keys
    - [ ] Client-side encryption
    - [ ] Client-side encryption with Amazon Macie

    **Answer:** Server-side encryption with Amazon S3 managed keys

18. **Elastic Load Balancing (ELB) captures and compresses logs. Where does ELB store these logs?**
    - [ ] Virtual private cloud (VPC)
    - [ ] Amazon EC2 instance
    - [ ] Amazon DynamoDB database
    - [ ] Amazon S3 bucket

    **Answer:** Amazon S3 bucket

19. **Which service can be configured to provide logging about storage use?**
    - [ ] Elastic Load Balancing (ELB)
    - [ ] Amazon VPC
    - [ ] AWS CloudTrail
    - [ ] Amazon S3

    **Answer:** Amazon S3

20. **Which types of information does Amazon CloudWatch collect monitoring and operational data as? (Select THREE.)**
    - [ ] Metrics
    - [ ] Incidents
    - [ ] Events
    - [ ] Logs
    - [ ] Authentications
    - [ ] Objects

    **Answers:** Metrics, Events, Logs

21. **An auditor is reviewing logs during a security audit. What is NOT a common log element that would be available to them?**
    - [ ] Date and time of the event
    - [ ] Origin or source of the event
    - [ ] Name of the resource owner
    - [ ] Identity of the resources that were accessed

    **Answer:** Name of the resource owner

22. **An administrator wants to run continuous assessment checks on their resources. The purpose is to verify that these resources comply with their own security policies, industry best practices, and compliance standards. Which AWS service would meet their need?**
    - [ ] AWS CloudTrail
    - [ ] AWS Config
    - [ ] Amazon CloudWatch
    - [ ] AWS CloudFormation

    **Answer:** AWS Config

23. **What is an industry best practice for handling incidents?**
    - [ ] Identify key personnel, external resources, and tools only after the incident has occurred.
    - [ ] Limit incident response rehearsals to avoid complacency.
    - [ ] Pre-provision access and tools.
    - [ ] Avoid using automated containment capabilities.

    **Answer:** Pre-provision access and tools.

24. **A cloud administrator wants the ability to automatically discover and scan Amazon EC2 instances for vulnerabilities. Which service would provide that capability?**
    - [ ] Amazon GuardDuty
    - [ ] Amazon Inspector
    - [ ] AWS Config
    - [ ] Amazon CloudWatch

    **Answer:**

 Amazon Inspector

25. **Which phase of incident response involves investigation and diagnosis?**
    - [ ] Discovery and recognition phase
    - [ ] Categorization and notification phase
    - [ ] Resolution and recovery phase
    - [ ] Planning and implementation phase

    **Answer:** Discovery and recognition phase
