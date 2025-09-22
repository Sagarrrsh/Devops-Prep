# AWS Interview Preparation Guide for Beginners

## Table of Contents
1. [What is AWS?](#what-is-aws)
2. [Core AWS Services](#core-aws-services)
3. [Key Concepts](#key-concepts)
4. [Common Interview Questions](#common-interview-questions)
5. [Hands-on Experience Tips](#hands-on-experience-tips)
6. [Additional Resources](#additional-resources)

## What is AWS?

Amazon Web Services (AWS) is a comprehensive cloud computing platform provided by Amazon. It offers over 200 services including computing power, storage, databases, networking, analytics, machine learning, and more.

### Key Benefits of AWS:
- **Cost-effective**: Pay only for what you use
- **Scalable**: Easily scale up or down based on demand
- **Reliable**: 99.99% uptime SLA
- **Secure**: Enterprise-grade security features
- **Global**: Available in multiple regions worldwide

## All AWS Services by Category

### 1. Compute Services

#### Amazon EC2 (Elastic Compute Cloud)
- **What it is**: Virtual servers in the cloud
- **Use cases**: Web servers, application servers, development environments
- **Key features**: 
  - Various instance types (t2.micro, m5.large, etc.)
  - Auto Scaling
  - Elastic Load Balancer integration
- **Interview tip**: Know the difference between instance types and when to use each

#### AWS Lambda
- **What it is**: Serverless compute service
- **Use cases**: Event-driven applications, microservices
- **Key features**:
  - No server management
  - Pay per execution
  - Automatic scaling
  - Supports multiple programming languages

#### Amazon ECS (Elastic Container Service)
- **What it is**: Container orchestration service
- **Use cases**: Running Docker containers at scale
- **Key features**: Integration with EC2, Fargate, Application Load Balancer

#### Amazon EKS (Elastic Kubernetes Service)
- **What it is**: Managed Kubernetes service
- **Use cases**: Container orchestration using Kubernetes
- **Key features**: Fully managed Kubernetes control plane

#### AWS Fargate
- **What it is**: Serverless compute for containers
- **Use cases**: Running containers without managing servers
- **Key features**: Pay per task, automatic scaling

#### AWS Batch
- **What it is**: Batch job processing service
- **Use cases**: Large-scale parallel workloads
- **Key features**: Job queues, compute environments

#### AWS Elastic Beanstalk
- **What it is**: Platform-as-a-Service for web applications
- **Use cases**: Easy deployment of web apps
- **Key features**: Supports Java, .NET, PHP, Node.js, Python, Ruby, Go

#### AWS App Runner
- **What it is**: Containerized web applications service
- **Use cases**: Deploy containerized apps quickly
- **Key features**: Automatic scaling, load balancing

#### Amazon Lightsail
- **What it is**: Simplified cloud platform
- **Use cases**: Simple web applications, development environments
- **Key features**: Predictable pricing, easy setup

### 2. Storage Services

#### Amazon S3 (Simple Storage Service)
- **What it is**: Object storage service
- **Use cases**: Static website hosting, data backup, content distribution
- **Storage classes**:
  - S3 Standard: Frequently accessed data
  - S3 Standard-IA: Infrequently accessed data
  - S3 One Zone-IA: Less frequently accessed, single AZ
  - S3 Glacier Instant Retrieval: Archive with instant access
  - S3 Glacier Flexible Retrieval: Archive with retrieval times
  - S3 Glacier Deep Archive: Lowest cost archive
  - S3 Intelligent-Tiering: Automatic cost optimization
- **Key concepts**: Buckets, Objects, Versioning, Lifecycle policies

#### Amazon EBS (Elastic Block Store)
- **What it is**: Block storage for EC2 instances
- **Volume types**:
  - gp3/gp2: General purpose SSD
  - io2/io1: Provisioned IOPS SSD
  - st1: Throughput optimized HDD
  - sc1: Cold HDD
- **Key features**: Snapshots, encryption, Multi-Attach

#### Amazon EFS (Elastic File System)
- **What it is**: Managed NFS file system
- **Use cases**: Shared storage across multiple EC2 instances
- **Key features**: Automatic scaling, POSIX-compliant

#### AWS FSx
- **What it is**: Fully managed file systems
- **Types**: FSx for Windows File Server, FSx for Lustre, FSx for NetApp ONTAP, FSx for OpenZFS
- **Use cases**: High-performance computing, enterprise applications

#### AWS Storage Gateway
- **What it is**: Hybrid cloud storage service
- **Types**: File Gateway, Volume Gateway, Tape Gateway
- **Use cases**: Connecting on-premises to cloud storage

#### AWS Backup
- **What it is**: Centralized backup service
- **Use cases**: Backup across AWS services
- **Key features**: Cross-region backup, compliance

### 3. Database Services

#### Amazon RDS (Relational Database Service)
- **What it is**: Managed relational database service
- **Supported engines**: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Amazon Aurora
- **Key features**: Automated backups, Multi-AZ deployments, Read replicas

#### Amazon Aurora
- **What it is**: Cloud-native relational database
- **Key features**: 5x faster than MySQL, 3x faster than PostgreSQL
- **Types**: Aurora MySQL, Aurora PostgreSQL

#### Amazon DynamoDB
- **What it is**: NoSQL database service
- **Key features**: 
  - Single-digit millisecond latency
  - Automatic scaling
  - Global tables for multi-region replication

#### Amazon DocumentDB
- **What it is**: MongoDB-compatible document database
- **Use cases**: Content management, catalogs, user profiles
- **Key features**: Fully managed, scalable

#### Amazon Neptune
- **What it is**: Graph database service
- **Use cases**: Social networks, recommendation engines, fraud detection
- **Key features**: Supports Apache TinkerPop Gremlin and W3C's SPARQL

#### Amazon Keyspaces
- **What it is**: Managed Apache Cassandra service
- **Use cases**: High-scale applications
- **Key features**: Serverless, pay-per-request

#### Amazon QLDB (Quantum Ledger Database)
- **What it is**: Immutable ledger database
- **Use cases**: Financial records, supply chain, registrations
- **Key features**: Cryptographically verifiable, immutable

#### Amazon Timestream
- **What it is**: Time series database
- **Use cases**: IoT applications, operational metrics
- **Key features**: Built for time series data, automatic scaling

#### Amazon ElastiCache
- **What it is**: In-memory caching service
- **Types**: Redis, Memcached
- **Use cases**: Session management, real-time analytics

#### Amazon Redshift
- **What it is**: Data warehouse service
- **Use cases**: Business intelligence, analytics
- **Key features**: Columnar storage, massively parallel processing

#### AWS Database Migration Service (DMS)
- **What it is**: Database migration service
- **Use cases**: Migrate databases to AWS
- **Key features**: Minimal downtime, continuous replication

### 4. Networking & Content Delivery

#### Amazon VPC (Virtual Private Cloud)
- **What it is**: Virtual network in the cloud
- **Components**:
  - Subnets (Public and Private)
  - Internet Gateway
  - NAT Gateway/Instance
  - Route Tables
  - Security Groups
  - NACLs (Network Access Control Lists)

#### Amazon CloudFront
- **What it is**: Content Delivery Network (CDN)
- **Use cases**: Faster content delivery, reduced latency
- **Key features**: Edge locations, Origin servers, Cache behaviors

#### Elastic Load Balancing (ELB)
- **Types**: 
  - Application Load Balancer (Layer 7)
  - Network Load Balancer (Layer 4)
  - Gateway Load Balancer
  - Classic Load Balancer (deprecated)
- **Use cases**: Distribute traffic across multiple targets

#### Amazon Route 53
- **What it is**: DNS web service
- **Key features**: Domain registration, DNS routing, health checking
- **Routing policies**: Simple, weighted, latency-based, failover, geolocation

#### AWS Direct Connect
- **What it is**: Dedicated network connection to AWS
- **Use cases**: Hybrid cloud, consistent network performance
- **Key features**: Reduced bandwidth costs, dedicated connection

#### Amazon API Gateway
- **What it is**: Managed API service
- **Use cases**: RESTful APIs, WebSocket APIs
- **Key features**: Authentication, throttling, monitoring

#### AWS Transit Gateway
- **What it is**: Network transit hub
- **Use cases**: Connect multiple VPCs and on-premises networks
- **Key features**: Centralized connectivity, routing

#### AWS PrivateLink
- **What it is**: Private connectivity between VPCs and services
- **Use cases**: Secure service access
- **Key features**: No internet gateway, NAT, or firewall needed

#### Amazon CloudMap
- **What it is**: Service discovery for cloud resources
- **Use cases**: Microservices architecture
- **Key features**: DNS-based service discovery

### 5. Developer Tools

#### AWS CodeCommit
- **What it is**: Git-based source control service
- **Use cases**: Store and version code
- **Key features**: Encryption at rest and in transit

#### AWS CodeBuild
- **What it is**: Continuous integration service
- **Use cases**: Compile source code, run tests, produce packages
- **Key features**: Pay per minute, scales automatically

#### AWS CodeDeploy
- **What it is**: Automated deployment service
- **Use cases**: Deploy to EC2, on-premises, Lambda, ECS
- **Key features**: Blue/green deployments, rollbacks

#### AWS CodePipeline
- **What it is**: Continuous delivery service
- **Use cases**: Automate release pipelines
- **Key features**: Visual workflow, integrations

#### AWS CodeStar
- **What it is**: Development project management
- **Use cases**: Unified development environment
- **Key features**: Project templates, team collaboration

#### AWS X-Ray
- **What it is**: Application performance monitoring
- **Use cases**: Trace requests, analyze performance
- **Key features**: Service map, latency analysis

#### AWS Cloud9
- **What it is**: Cloud-based IDE
- **Use cases**: Code development in the cloud
- **Key features**: Real-time collaboration, terminal access

#### AWS CodeArtifact
- **What it is**: Artifact repository service
- **Use cases**: Store and share software packages
- **Key features**: Supports multiple package formats

#### AWS CodeGuru
- **What it is**: ML-powered code review service
- **Use cases**: Code quality improvement, performance optimization
- **Key features**: Automated code reviews, performance recommendations

### 6. Security, Identity & Compliance

#### AWS IAM (Identity and Access Management)
- **What it is**: Access management service
- **Components**: Users, Groups, Roles, Policies
- **Key features**: Fine-grained permissions, MFA

#### AWS Cognito
- **What it is**: User identity and data synchronization
- **Use cases**: Mobile and web app authentication
- **Key features**: User pools, identity pools, social login

#### AWS KMS (Key Management Service)
- **What it is**: Managed encryption keys
- **Use cases**: Encrypt data across AWS services
- **Key features**: Hardware security modules, audit trails

#### AWS CloudHSM
- **What it is**: Hardware security modules in the cloud
- **Use cases**: FIPS 140-2 Level 3 compliance
- **Key features**: Single-tenant key storage

#### AWS Certificate Manager (ACM)
- **What it is**: SSL/TLS certificate management
- **Use cases**: Secure communications
- **Key features**: Free SSL certificates, automatic renewal

#### AWS WAF (Web Application Firewall)
- **What it is**: Web application protection
- **Use cases**: Protect against common web exploits
- **Key features**: Custom rules, rate limiting

#### AWS Shield
- **What it is**: DDoS protection
- **Types**: Shield Standard (free), Shield Advanced (paid)
- **Key features**: Always-on detection, automatic mitigation

#### AWS GuardDuty
- **What it is**: Threat detection service
- **Use cases**: Identify malicious activity
- **Key features**: Machine learning, threat intelligence

#### AWS Inspector
- **What it is**: Application security assessment
- **Use cases**: Identify security vulnerabilities
- **Key features**: Agent-based assessment, compliance reports

#### AWS Macie
- **What it is**: Data security and privacy service
- **Use cases**: Discover and protect sensitive data
- **Key features**: Machine learning, data classification

#### AWS Security Hub
- **What it is**: Central security findings management
- **Use cases**: Aggregated security findings
- **Key features**: Compliance dashboards, automated remediation

#### AWS CloudTrail
- **What it is**: API call logging service
- **Use cases**: Audit and compliance
- **Key features**: Event history, log file encryption

#### AWS Config
- **What it is**: Resource configuration tracking
- **Use cases**: Compliance monitoring, change management
- **Key features**: Configuration history, rules evaluation

#### AWS Secrets Manager
- **What it is**: Secrets management service
- **Use cases**: Store database credentials, API keys
- **Key features**: Automatic rotation, encryption

#### AWS Systems Manager
- **What it is**: Operational data and automation
- **Use cases**: Patch management, configuration management
- **Key features**: Parameter Store, Session Manager

### 7. Analytics

#### Amazon Redshift
- **What it is**: Data warehouse service
- **Use cases**: Business intelligence, analytics
- **Key features**: Columnar storage, massively parallel processing

#### Amazon EMR (Elastic MapReduce)
- **What it is**: Big data processing service
- **Use cases**: Apache Spark, Hadoop, Presto workloads
- **Key features**: Managed cluster, auto-scaling

#### Amazon Kinesis
- **What it is**: Real-time data streaming
- **Services**: 
  - Kinesis Data Streams: Real-time data streaming
  - Kinesis Data Firehose: Data delivery to destinations
  - Kinesis Data Analytics: Real-time analytics
  - Kinesis Video Streams: Video streaming

#### AWS Glue
- **What it is**: ETL (Extract, Transform, Load) service
- **Use cases**: Data preparation, data cataloging
- **Key features**: Serverless, data catalog

#### Amazon Athena
- **What it is**: Interactive query service
- **Use cases**: Query data in S3 using SQL
- **Key features**: Serverless, pay per query

#### Amazon QuickSight
- **What it is**: Business intelligence service
- **Use cases**: Data visualization, dashboards
- **Key features**: Machine learning insights, mobile access

#### AWS Data Pipeline
- **What it is**: Data workflow orchestration
- **Use cases**: Move and transform data
- **Key features**: Fault tolerance, retry logic

#### Amazon CloudSearch
- **What it is**: Managed search service
- **Use cases**: Add search functionality to applications
- **Key features**: Auto-scaling, multiple languages

#### Amazon Elasticsearch Service (now OpenSearch)
- **What it is**: Search and analytics engine
- **Use cases**: Log analytics, real-time monitoring
- **Key features**: Kibana integration, security features

#### AWS Lake Formation
- **What it is**: Data lake service
- **Use cases**: Build and manage data lakes
- **Key features**: Data cataloging, security, governance

### 8. Machine Learning & AI

#### Amazon SageMaker
- **What it is**: Machine learning platform
- **Use cases**: Build, train, deploy ML models
- **Key features**: Jupyter notebooks, model hosting

#### Amazon Rekognition
- **What it is**: Image and video analysis
- **Use cases**: Object detection, facial recognition
- **Key features**: Celebrity recognition, content moderation

#### Amazon Polly
- **What it is**: Text-to-speech service
- **Use cases**: Voice applications, accessibility
- **Key features**: Multiple languages, SSML support

#### Amazon Transcribe
- **What it is**: Speech-to-text service
- **Use cases**: Call center analytics, subtitles
- **Key features**: Real-time transcription, custom vocabulary

#### Amazon Translate
- **What it is**: Language translation service
- **Use cases**: Multilingual applications
- **Key features**: Real-time translation, custom terminology

#### Amazon Comprehend
- **What it is**: Natural language processing
- **Use cases**: Sentiment analysis, entity extraction
- **Key features**: Custom entities, topic modeling

#### Amazon Lex
- **What it is**: Conversational AI service
- **Use cases**: Chatbots, voice assistants
- **Key features**: Natural language understanding, speech recognition

#### Amazon Textract
- **What it is**: Document text extraction
- **Use cases**: Form processing, document analysis
- **Key features**: Table extraction, form data extraction

#### AWS DeepLens
- **What it is**: Deep learning camera
- **Use cases**: Computer vision applications
- **Key features**: Pre-trained models, edge computing

#### Amazon Forecast
- **What it is**: Time series forecasting
- **Use cases**: Demand planning, resource planning
- **Key features**: Machine learning algorithms, automatic model selection

#### Amazon Personalize
- **What it is**: Real-time personalization service
- **Use cases**: Recommendation systems
- **Key features**: Real-time recommendations, batch inference

### 9. Internet of Things (IoT)

#### AWS IoT Core
- **What it is**: IoT device connectivity and management
- **Use cases**: Connect IoT devices to cloud
- **Key features**: Device management, message broker

#### AWS IoT Device Management
- **What it is**: Onboard, organize, monitor IoT devices
- **Use cases**: Large-scale device management
- **Key features**: Fleet indexing, remote actions

#### AWS IoT Analytics
- **What it is**: Analytics for IoT data
- **Use cases**: IoT data analysis
- **Key features**: Data processing, machine learning integration

#### AWS IoT Greengrass
- **What it is**: Edge computing for IoT
- **Use cases**: Local compute, messaging, data caching
- **Key features**: Lambda functions at edge, local device shadows

#### AWS IoT Events
- **What it is**: IoT event detection and response
- **Use cases**: Equipment monitoring, fleet management
- **Key features**: State machines, automated actions

#### AWS IoT Things Graph
- **What it is**: Visual IoT application development
- **Use cases**: Connect diverse IoT devices and services
- **Key features**: Visual interface, pre-built models

### 10. Mobile Services

#### AWS Amplify
- **What it is**: Full-stack mobile and web app development
- **Use cases**: Build and deploy mobile/web apps
- **Key features**: Authentication, APIs, hosting

#### Amazon Pinpoint
- **What it is**: Customer engagement service
- **Use cases**: Push notifications, email campaigns
- **Key features**: Analytics, A/B testing

#### AWS Device Farm
- **What it is**: Mobile app testing service
- **Use cases**: Test apps on real devices
- **Key features**: Automated testing, remote access

#### Amazon SNS (Simple Notification Service)
- **What it is**: Messaging service
- **Use cases**: Push notifications, SMS, email
- **Key features**: Fan-out messaging, mobile push

### 11. Application Integration

#### Amazon SQS (Simple Queue Service)
- **What it is**: Message queuing service
- **Use cases**: Decouple application components
- **Key features**: Standard and FIFO queues, dead letter queues

#### Amazon SNS (Simple Notification Service)
- **What it is**: Pub/Sub messaging service
- **Use cases**: Application notifications, mobile push
- **Key features**: Topics, subscriptions, message filtering

#### AWS Step Functions
- **What it is**: Serverless workflow orchestration
- **Use cases**: Coordinate distributed applications
- **Key features**: Visual workflows, error handling

#### Amazon EventBridge
- **What it is**: Event bus service
- **Use cases**: Connect applications using events
- **Key features**: Custom events, third-party integrations

#### Amazon MQ
- **What it is**: Managed message broker
- **Use cases**: Migrate existing message brokers
- **Key features**: Apache ActiveMQ, RabbitMQ support

#### AWS AppSync
- **What it is**: GraphQL API service
- **Use cases**: Real-time and offline mobile apps
- **Key features**: Real-time subscriptions, conflict resolution

### 12. Management & Governance

#### AWS CloudFormation
- **What it is**: Infrastructure as Code
- **Use cases**: Provision AWS resources using templates
- **Key features**: JSON/YAML templates, stack management

#### AWS CloudWatch
- **What it is**: Monitoring and observability
- **Use cases**: Monitor resources, applications, logs
- **Key features**: Metrics, alarms, dashboards, logs

#### AWS CloudTrail
- **What it is**: API call logging
- **Use cases**: Audit and compliance
- **Key features**: Event history, log file encryption

#### AWS Config
- **What it is**: Resource configuration tracking
- **Use cases**: Compliance monitoring
- **Key features**: Configuration history, rules evaluation

#### AWS Systems Manager
- **What it is**: Operational data management
- **Use cases**: Patch management, parameter store
- **Key features**: Session Manager, Run Command

#### AWS Trusted Advisor
- **What it is**: Real-time guidance service
- **Use cases**: Cost optimization, security recommendations
- **Key features**: Best practice recommendations, alerts

#### AWS Organizations
- **What it is**: Multi-account management
- **Use cases**: Centrally manage multiple AWS accounts
- **Key features**: Service Control Policies, consolidated billing

#### AWS Control Tower
- **What it is**: Set up and govern multi-account environment
- **Use cases**: Landing zone setup
- **Key features**: Guardrails, account factory

#### AWS License Manager
- **What it is**: Software license management
- **Use cases**: Track license usage
- **Key features**: License tracking, compliance

#### AWS Service Catalog
- **What it is**: Create and manage catalogs of IT services
- **Use cases**: Standardized deployments
- **Key features**: Product portfolios, launch constraints

#### AWS Well-Architected Tool
- **What it is**: Review workloads against best practices
- **Use cases**: Architecture assessment
- **Key features**: Pillar-based reviews, improvement plans

### 13. Media Services

#### Amazon Elastic Transcoder
- **What it is**: Media transcoding service
- **Use cases**: Convert media files to different formats
- **Key features**: Presets, thumbnails, watermarks

#### AWS Elemental MediaConvert
- **What it is**: File-based video transcoding
- **Use cases**: Broadcast and multiscreen delivery
- **Key features**: Advanced video processing, multiple outputs

#### AWS Elemental MediaLive
- **What it is**: Live video processing
- **Use cases**: Broadcast-grade live video streams
- **Key features**: Real-time encoding, redundancy

#### AWS Elemental MediaPackage
- **What it is**: Video origination and packaging
- **Use cases**: Deliver video streams to viewers
- **Key features**: Just-in-time packaging, content protection

#### AWS Elemental MediaStore
- **What it is**: Media storage service
- **Use cases**: Store and deliver video content
- **Key features**: High performance, low latency

#### Amazon Interactive Video Service (IVS)
- **What it is**: Live streaming service
- **Use cases**: Interactive live video experiences
- **Key features**: Low latency, scalable

## Key Concepts

### 1. Regions and Availability Zones
- **Region**: Geographic area with multiple data centers
- **Availability Zone (AZ)**: Isolated data center within a region
- **Edge Locations**: Cache content closer to users (CloudFront)

### 2. Security and Compliance

#### IAM (Identity and Access Management)
- **Users**: Individual accounts
- **Groups**: Collection of users
- **Roles**: Temporary permissions for AWS resources
- **Policies**: JSON documents defining permissions
- **Principle of Least Privilege**: Give minimum required permissions

### 3. Pricing Models
- **On-Demand**: Pay by the hour/second
- **Reserved Instances**: 1-3 year commitments for discounts
- **Spot Instances**: Bid on unused capacity
- **Dedicated Hosts**: Physical servers for compliance requirements

### 4. Well-Architected Framework
Five pillars:
1. **Operational Excellence**: Running and monitoring systems
2. **Security**: Protecting information and systems
3. **Reliability**: Ensuring systems work consistently
4. **Performance Efficiency**: Using resources effectively
5. **Cost Optimization**: Avoiding unnecessary costs

## Common Interview Questions

### Basic Questions

**Q1: What is cloud computing?**
A: Cloud computing is the delivery of computing services (servers, storage, databases, networking, software) over the internet, allowing for faster innovation, flexible resources, and economies of scale.

**Q2: What are the main cloud service models?**
A: 
- **IaaS** (Infrastructure as a Service): EC2, VPC
- **PaaS** (Platform as a Service): Elastic Beanstalk, Lambda
- **SaaS** (Software as a Service): WorkMail, WorkDocs

**Q3: What is the difference between S3 and EBS?**
A: 
- S3 is object storage accessed via REST API, suitable for static content
- EBS is block storage attached to EC2 instances, suitable for databases and file systems

**Q4: Explain Auto Scaling.**
A: Auto Scaling automatically adjusts the number of EC2 instances based on demand, ensuring optimal performance and cost efficiency.

### Intermediate Questions

**Q5: What is the difference between Security Groups and NACLs?**
A:
- **Security Groups**: Instance-level firewall, stateful, allow rules only
- **NACLs**: Subnet-level firewall, stateless, allow and deny rules

**Q6: Explain Multi-AZ deployment in RDS.**
A: Multi-AZ provides high availability by maintaining a standby replica in a different AZ. Automatic failover occurs if the primary database fails.

**Q7: What is the difference between horizontal and vertical scaling?**
A:
- **Horizontal**: Adding more instances (scale out)
- **Vertical**: Adding more power to existing instances (scale up)

**Q8: How does CloudFront improve performance?**
A: CloudFront caches content at edge locations worldwide, reducing latency by serving content from the nearest location to users.

**Q9: What is the difference between SQS and SNS?**
A:
- **SQS**: Message queuing service for decoupling components (pull-based)
- **SNS**: Notification service for pub/sub messaging (push-based)

**Q10: Explain the difference between ECS and EKS.**
A:
- **ECS**: AWS native container orchestration service
- **EKS**: Managed Kubernetes service for container orchestration

**Q11: What are the different types of Load Balancers?**
A:
- **Application Load Balancer (ALB)**: Layer 7, HTTP/HTTPS traffic
- **Network Load Balancer (NLB)**: Layer 4, TCP/UDP traffic
- **Gateway Load Balancer (GWLB)**: Layer 3, for third-party appliances

**Q12: What is the difference between CloudWatch and CloudTrail?**
A:
- **CloudWatch**: Monitoring service for performance metrics and logs
- **CloudTrail**: Audit service for API calls and user activity

**Q13: Explain AWS Lambda's execution model.**
A: Lambda runs code in response to triggers, automatically manages compute resources, scales automatically, and charges only for compute time consumed.

**Q14: What is the difference between IAM roles and IAM users?**
A:
- **IAM Users**: Permanent identities for people or services
- **IAM Roles**: Temporary permissions that can be assumed by users, services, or applications

**Q15: How does Auto Scaling work with different scaling policies?**
A: Auto Scaling can use target tracking (maintain a metric), step scaling (scale based on CloudWatch alarms), or scheduled scaling (predictable patterns).

### Scenario-Based Questions

**Q16: How would you design a highly available web application?**
A: Use multiple AZs, Auto Scaling Groups, Application Load Balancer, RDS with Multi-AZ, S3 for static content, CloudFront for global distribution, and Route 53 for DNS.

**Q17: How would you secure an AWS environment?**
A: Implement IAM best practices, use Security Groups and NACLs, enable CloudTrail for auditing, encrypt data at rest and in transit, use AWS Config for compliance monitoring, and implement least privilege access.

**Q18: Design a serverless architecture for a photo sharing application.**
A: Use S3 for photo storage, Lambda for image processing, API Gateway for REST APIs, DynamoDB for metadata, CloudFront for content delivery, and Cognito for user authentication.

**Q19: How would you migrate a large database to AWS with minimal downtime?**
A: Use AWS Database Migration Service (DMS) with continuous replication, set up read replicas, perform cutover during low-traffic period, and have rollback plan ready.

**Q20: Design a real-time analytics pipeline.**
A: Use Kinesis Data Streams for ingestion, Kinesis Analytics for real-time processing, Lambda for transformations, and store results in DynamoDB or ElastiCache for low-latency access.

**Q21: How would you handle a sudden traffic spike?**
A: Implement Auto Scaling Groups, use Application Load Balancer, enable CloudFront caching, optimize database with read replicas, and consider using Lambda for elastic compute.

**Q22: Design a disaster recovery strategy.**
A: Implement multi-region deployment, use S3 cross-region replication, set up RDS cross-region read replicas, automate infrastructure with CloudFormation, and regularly test recovery procedures.

**Q23: How would you optimize costs for a development environment?**
A: Use Spot Instances, schedule resources to run only during work hours, use smaller instance types, implement lifecycle policies for S3, and use Reserved Instances for predictable workloads.

### Service-Specific Questions

**Q24: When would you use SQS vs SNS vs EventBridge?**
A:
- **SQS**: Decoupling components with reliable message delivery
- **SNS**: Broadcasting messages to multiple subscribers
- **EventBridge**: Event-driven architectures with routing rules

**Q25: Explain the different S3 storage classes and when to use each.**
A:
- **S3 Standard**: Frequently accessed data
- **S3 Standard-IA**: Infrequently accessed but needs rapid access
- **S3 One Zone-IA**: Lower cost for infrequently accessed data in single AZ
- **S3 Glacier**: Long-term archival with retrieval times
- **S3 Glacier Deep Archive**: Lowest cost for long-term retention

**Q26: What are the benefits of using AWS Lambda?**
A: No server management, automatic scaling, pay per execution, built-in fault tolerance, supports multiple programming languages, integrates with many AWS services.

**Q27: Explain CloudFormation vs Terraform.**
A:
- **CloudFormation**: AWS native, declarative, supports AWS resources natively
- **Terraform**: Multi-cloud, larger community, more flexible templating

**Q28: When would you use ElastiCache and which engine?**
A: Use for session management, database caching, real-time analytics. Choose Redis for advanced data structures and persistence, Memcached for simple caching.

**Q29: What is the difference between Aurora and RDS?**
A: Aurora is AWS's cloud-native database with better performance (5x MySQL, 3x PostgreSQL), automatic scaling, and multiple read replicas. RDS is managed service for traditional databases.

**Q30: How does AWS Direct Connect differ from VPN?**
A:
- **Direct Connect**: Dedicated physical connection, consistent performance, higher bandwidth
- **VPN**: Internet-based connection, variable performance, quick to set up

**Q10: How would you secure an AWS environment?**
A: Implement IAM best practices, use Security Groups and NACLs, enable CloudTrail for auditing, encrypt data at rest and in transit, and use AWS Config for compliance monitoring.

## Hands-on Experience Tips

### Free Tier Resources
- EC2: 750 hours per month of t2.micro instances
- S3: 5 GB of storage
- RDS: 750 hours of db.t2.micro
- Lambda: 1 million requests per month

### Practice Labs
1. **Launch an EC2 instance** and connect via SSH
2. **Create an S3 bucket** and upload files
3. **Set up a VPC** with public and private subnets
4. **Create a Lambda function** triggered by S3 events
5. **Deploy a simple web app** using Elastic Beanstalk

### Certifications to Consider
- **AWS Cloud Practitioner**: Entry-level certification
- **AWS Solutions Architect Associate**: Most popular technical certification
- **AWS Developer Associate**: For developers

## Additional Resources

### Documentation and Learning
- **AWS Documentation**: Official service documentation
- **AWS Whitepapers**: Best practices and architectural guidance
- **AWS Training**: Free digital courses
- **AWS YouTube Channel**: Technical videos and webinars

### Practice Platforms
- **AWS Free Tier**: Hands-on experience with real services
- **AWS Skill Builder**: Interactive learning paths
- **A Cloud Guru/Pluralsight**: Video courses and labs

### Interview Preparation
- Focus on understanding service use cases rather than memorizing features
- Practice explaining concepts in simple terms
- Prepare real-world scenarios where you've used or would use AWS services
- Stay updated with new service announcements

## Final Tips for Interview Success

1. **Understand the fundamentals** before diving into advanced topics
2. **Practice hands-on** with actual AWS services
3. **Know when to use which service** for specific scenarios
4. **Understand cost implications** of different services and configurations
5. **Be honest about your experience level** and show eagerness to learn
6. **Ask clarifying questions** during scenario-based questions
7. **Focus on practical applications** rather than just theoretical knowledge

Remember: AWS is vast, and no one knows everything. Focus on core services and concepts, demonstrate your problem-solving approach, and show enthusiasm for cloud technologies!