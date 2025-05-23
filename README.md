# AWS Certified Solutions Architect – Associate (SAA-C03) Study Guide

# 📘 AWS Simple Storage Service

## 💡 1. What is S3?

**Amazon S3 (Simple Storage Service)** is a highly scalable, secure, and durable **object storage service** provided by AWS.  
As an **object storage system**, S3 is designed to store data as discrete units called *objects*, rather than blocks or files like traditional storage systems.

It allows users to store and retrieve **any amount of data**, from **anywhere on the web**, at **any time**.

S3 stores data as objects within *buckets*. Each object consists of:

- The data itself
- metadata
- A globally unique identifier (key).

Unlike block storage and file storage, you **cannot modify parts of an object in-place**.  
To "edit" a file, you must **upload a new version** of the object and either delete or archive the old version (or use versioning to keep both).

This makes S3 ideal for storing static files, backups, logs, media, and other unstructured data — but not suitable for hosting databases or operating systems.

**🗂️ Comparison of Block Storage, File Storage and Object Storage**

|  Aspect         | 🧱 **Block Storage**                          | 📁 **File Storage**                                 | 🧊 **Object Storage**                                  |
|--------------------------|----------------------------------------------|----------------------------------------------------|--------------------------------------------------------|
| **Unit of transaction**  | Blocks                                       | Files                                              | Objects (data + metadata + unique ID)                 |
| **How you can update**   | Directly update parts of the file            | Directly update file contents                      | Cannot modify directly; upload a new version instead  |
| **Protocols**            | SCSI, Fiber Channel, SATA                    | SMB, NFS, CIFS                                     | REST, SOAP over HTTP/HTTPS                            |
| **Support for metadata** | ❌ No custom metadata (only file system attrs) | ❌ No custom metadata (only file system attrs)      | ✅ Supports custom metadata                            |
| **Typical usage**        | OS volumes, databases, transactional data    | Shared network drives, team collaboration          | Cloud storage for media, logs, backups, static files  |
| **Strength**             | High performance and low latency             | Simple access and file-level sharing               | Highly scalable, accessible globally via internet     |
| **Weakness**             | Tied to a data center, limited flexibility   | Limited scalability, still tied to data centers     | Cannot update in place, not suitable for DB or OS     |
| **Can you format it?**   | ✅ Yes – full disk-level control              | ❌ No – only manage files, not the underlying FS    | ❌ No – only manage objects, not storage medium        |
| **Can install OS?**      | ✅ Yes, used as boot/root volume              | ❌ No – not for OS installation                     | ❌ No – not designed for OS or applications            |
| **Pricing model**        | Based on **allocated capacity**, even if not full | Based on **actual data stored**                    | Based on **actual data stored**                       |
| **AWS Example**          | Amazon **EBS** (Elastic Block Store)         | Amazon **EFS** (Elastic File System)               | Amazon **S3** (Simple Storage Service)                |

**📌 Detailed Explanation of Each Storage Type**

🧱 Block Storage (e.g., EBS)

- Works like a raw hard drive. You can format, partition, and use it just like a local disk.

- Used for high-performance tasks like databases, boot volumes, and applications.

- Supports random access, low latency, and high IOPS.

📁 File Storage (e.g., EFS)

- Provides a shared file system over a network, similar to a NAS.

- Supports file-level permissions and is good for multi-user environments.

- Ideal for content management systems, home directories, and shared data repositories.

🧊 Object Storage (e.g., S3)

- Designed for massive scalability and global access.

- Each object is immutable; to change content, you must replace the whole object.

- Suitable for backups, media files, logs, website assets, and archive data.

- Supports custom metadata, versioning, lifecycle rules, and event-driven triggers.

**🧠 Use This When**

- Use EBS when you need to install an operating system or host a database.

- Use EFS when you need multiple servers to share access to files.

- Use S3 when storing large volumes of static or semi-static content over the long term.

## ✅ 2. Advantages of Amazon S3

| Feature         | Description |
|----------------|-------------|
| **🧭 Simple** | Easy to use with web console, mobile app, REST APIs, and SDKs. |
| **📈 Scalable** | Infinitely scalable — no capacity planning required. Store petabytes of data easily. |
| **🛡️ Durable** | 99.999999999% durability (11 nines) by storing data redundantly across multiple facilities. |
| **🔒 Secure** | Supports encryption (at rest & in transit), IAM policies, and SSL for secure access. |
| **🚀 High Performance** | Multipart uploads, region selection, and CloudFront integration for fast delivery. |
| **📶 Highly Available** | 99.99% availability SLA — minimal downtime annually. |
| **💰 Cost-effective** | Pay-as-you-go pricing with volume discounts and tiered storage options (e.g., Glacier, IA). |
| **🛠️ Easy to Manage** | Storage management tools, lifecycle policies, and metadata-driven organization. |
| **🔗 Easy Integration** | Works seamlessly with other AWS services and third-party tools. |

## 🌍 3. Real-Life Use Cases of Amazon S3

Amazon S3 is a versatile service used in a wide range of real-world scenarios across industries. Here are the most common use cases:

### Backup
- S3 is widely used to store backup files due to its **99.999999999% durability**.  
- Data is replicated across **multiple Availability Zones**, reducing risk from disasters.  
- Versioning protects against accidental deletion or human error.

### Tape Replacement
- Organizations are increasingly replacing old **magnetic tape systems** with S3.  
- This eliminates the need for physical tape infrastructure while gaining better scalability and accessibility.

### Static Website Hosting
- S3 allows hosting static websites (HTML, CSS, JS) without setting up web servers.  
- It automatically scales to handle any level of traffic with **low cost and zero maintenance**.

### Application Hosting
- Used to host **mobile apps** or web applications that require **high availability** and **global access**.  
- S3 integrates well with other AWS services for seamless deployments.

### Disaster Recovery
- With **cross-region replication**, S3 enables automatic backup of data to another region — ideal for disaster recovery strategies.

### Content Distribution
- S3 is commonly used to distribute digital content (e.g., software, documents, media files).  
- It scales automatically and can be used with **Amazon CloudFront** for global content delivery.

### Data Lake for Big Data
- S3 serves as a **centralized data lake** to store raw, semi-processed, or processed data.  
- Widely used in big data ecosystems, integrated with services like:
- **Amazon EMR**
- **Amazon Redshift & Redshift Spectrum**
- **Athena**, **Glue**, **QuickSight**

- Use cases include storing data for analytics, financial reports, research, multimedia, and more.

### Private Repository
- You can use S3 to build a **private package or code repository**, similar to:
    - Git (code)
    - Yum (Linux packages)
    - Maven (Java libraries)

- This provides a secure, scalable backend for internal development teams.

## **📦 Basic Concepts**

### **Buckets**
- **Bucket** is a container in Amazon S3 used to store objects (files). You can think of a **bucket as a folder** on a computer.
- Each bucket:
    - Must have a **globally unique name** (no duplicates allowed across AWS).
    - Exists in **one specific AWS Region**:
    - All objects inside inherit the same region.
    - Data **does not leave the region** unless:
        - You manually transfer it.
        - You enable **Cross-Region Replication (CRR)**.
- Purpose:
  - Organize namespace  
    → Each bucket has a globally unique name and helps manage object identifiers.
  - Associate with AWS account (billing, permissions)  
    → Buckets are tied to specific AWS accounts for cost tracking and access control.
  - Control access and reporting  
    → Buckets enable setting permissions, logging access, and auditing usage.

- 📝Example   
The object `ringtone.mp3` stored in a bucket named `newringtones` can be accessed via:  
`http://newringtones.s3.amazonaws.com/ringtone.mp3`

### **Objects**
- Fundamental storage unit in S3
- Stored **inside a specific bucket (and therefore a specific region)**
- Consists of
  - **Data**: the actual file content (image, video, document…)
  - **Metadata**:
    - A set of name-value pairs describing the object
    - Include
      - System-defined (e.g. LastModified, Content-Type)
      - User-defined (custom key-value pairs)
- Identified by
  - **Key**: unique name within a bucket (like a path)
  - **Bucket**: container where object resides
  - **Version ID**: if versioning is enabled
  - Example
    - Object URL: https://my-bucket.s3.amazonaws.com/photos/img1.jpg
    - Key: photos/img1.jpg
- Object can be
  - Downloaded (GET)
  - Uploaded/Overwritten (PUT)
  - Deleted (DELETE)
- Object Size Limits
  - Max size per object: **5 TB**
  - Max size for single PUT: **5 GB**
  - Use **Multipart Upload** when
    - File > 100 MB (recommended)
    - File > 5 GB (required)
  - Multipart Upload
    - Upload parts in parallel
    - Each part ≥ 5 MB (except last)

### **Regions**
- A bucket is created in **one region**
- All objects in the bucket reside in that region
- Choose region based on:
  - **Latency**
  - **Data compliance**
  - **Cost optimization**
- Objects stay in region unless
  - Moved manually
  - Cross-region replication enabled

### **Access Methods**
- You can interact with S3 using
  - **REST APIs** (preferred): use standard HTTP verbs (GET, PUT, POST, DELETE)
  - **AWS SDKs**: available in Python, Java, Node.js, etc.
  - **AWS CLI**: command-line tool to manage S3 (e.g. `aws s3 mb`, `aws s3 rb`)
- Use **HTTPS** for secure communication
- Suitable for scripting, app development, automation
