# AWS-Certified-Solutions-Architect-Associate-Study-Guide

## 📘 AWS Simple Storage Service

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
