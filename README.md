# AWS IAM & S3 Practical Examination

## Student Details

- **Name:** Tanuja Santosh Gade
- **Batch:** 24_June
- **Repository:** AWS-IAM-S3-Practical
- **AWS Region:** Mumbai (`ap-south-1`)

## Objective

To understand AWS Identity and Access Management (IAM), IAM users and groups, managed and customer-managed policies, EC2 IAM roles, and Amazon S3 security, storage management, and access control.

## AWS Services Used

- AWS Identity and Access Management (IAM)
- Amazon Simple Storage Service (S3)
- Amazon Elastic Compute Cloud (EC2)
- AWS Command Line Interface (AWS CLI)

## S3 Bucket Details

| Setting | Value |
|---|---|
| Main bucket | `tanuja-iam-s3-practical-2026` |
| Region | `ap-south-1` |
| Access | Private |
| Block Public Access | Enabled |
| Default encryption | SSE-S3 |
| Versioning | Enabled |
| Lifecycle rule | `BackupTransitionRule` |
| Logging destination bucket | `tanuja-iam-s3-access-logs-2026` |

---

# Section A — AWS IAM

## Task 1: IAM Users and Groups

### Objective
To create IAM users and groups and organize users according to their access requirements.

### Implementation
- Created IAM user `developer-user`.
- Created IAM user `tester-user`.
- Created IAM group `Developers`.
- Created IAM group `Testers`.
- Added `developer-user` to `Developers`.
- Added `tester-user` to `Testers`.

### Result
IAM users and groups were created to manage permissions according to different user requirements.

## Task 2: IAM Groups and AWS-Managed Policies

### Objective
To attach AWS-managed policies to IAM groups and understand the difference between read-only and full S3 access.

### Implementation
- Attached `AmazonS3ReadOnlyAccess` to the `Developers` group.
- Attached `AmazonS3FullAccess` to the `Testers` group.

### Policy Comparison

| Feature | AmazonS3ReadOnlyAccess | AmazonS3FullAccess |
|---|---|---|
| List and view S3 resources | Allowed | Allowed |
| Read and download objects | Allowed | Allowed |
| Upload objects | Not allowed | Allowed |
| Delete objects | Not allowed | Allowed |
| Access level | Read-only | Full S3 access |

Actual permissions can be affected by other applicable IAM policies, resource policies, permission boundaries, and organization-level controls.

### Result
AWS-managed policies were attached to their respective IAM groups.

## Task 3: Custom S3 Read Policy

### Objective
To create a customer-managed IAM policy that permits listing a specific S3 bucket and reading its objects.

### Policy Details
- **Policy Name:** `CustomS3ReadPolicy`
- **Bucket:** `tanuja-iam-s3-practical-2026`
- **Attached To:** `Developers` group

### Permissions
- `s3:ListBucket` — allows listing objects in the specified bucket.
- `s3:GetObject` — allows reading and downloading objects.
- No upload permission is granted by this custom policy.
- No delete permission is granted by this custom policy.

### Security Learning
The principle of least privilege means granting only the permissions needed to perform a task.

**Note:** The `Developers` group also has `AmazonS3ReadOnlyAccess`. Therefore, its effective read access may extend beyond the practical bucket, even though `CustomS3ReadPolicy` itself is bucket-specific.

### Result
A customer-managed policy was created and attached to the `Developers` group.

## Task 4: EC2 IAM Role with S3 Read-Only Access

### Objective
To allow an EC2 instance to access a specific S3 bucket through an IAM role instead of storing long-term AWS access keys on the instance.

### Role Details
- **Role Name:** `EC2-S3-ReadOnly-Role`
- **Trusted Service:** Amazon EC2
- **Policy:** `EC2S3BucketReadPolicy`
- **Bucket:** `tanuja-iam-s3-practical-2026`
- **EC2 Instance:** `i-0613cf2f2eccfaa77`

### Implementation
1. Created an IAM role trusted by the EC2 service.
2. Attached the role to the EC2 instance.
3. Created the customer-managed policy `EC2S3BucketReadPolicy`.
4. Detached the broad `AmazonS3ReadOnlyAccess` policy from the role.
5. Attached the bucket-specific read policy.
6. Used AWS CLI on EC2 to test S3 access.

### Permissions
The bucket-specific policy permits:
- Listing the specified bucket using `s3:ListBucket`.
- Reading and downloading objects using `s3:GetObject`.

It does not grant upload or delete permissions.

### Security Learning
IAM roles provide temporary credentials to authorized AWS services and applications. This avoids storing long-term access keys on the EC2 instance.

### Result
The EC2 instance successfully listed the practical bucket and downloaded an object. An upload attempt was denied during testing.

**Naming note:** The role is named `EC2-S3-ReadOnly-Role`; this differs from the assignment's expected name `EC2S3ReadRole`.

## Task 5: IAM Access Testing

### Objective
To verify that the configured IAM permissions allow intended read operations and deny unauthorized write operations.

### Test Results

| Action | User / Role | Observed result |
|---|---|---|
| List bucket objects | `developer-user` | Bucket and objects were visible |
| Download an object | EC2 IAM role | Successful through AWS CLI |
| Upload an object | `developer-user` | Access denied during testing |
| Upload an object | EC2 IAM role | Access denied during testing |

These results demonstrate the intended separation between read access and write access for the tested operations.

---

# Section B — Amazon S3

## Task 6: S3 Bucket Configuration and Security

### Objective
To configure an S3 bucket and apply basic security settings.

### Implementation
- Used the bucket `tanuja-iam-s3-practical-2026`.
- Confirmed the AWS Region was Mumbai (`ap-south-1`).
- Uploaded sample files to the bucket.
- Kept the bucket private.
- Enabled all four S3 Block Public Access settings.
- Verified default server-side encryption using Amazon S3-managed keys (SSE-S3).

### Result
The bucket was configured with private access, Block Public Access, sample objects, and default encryption.

### Evidence
Folder: `screenshots/Task-6-S3-Bucket/`

## Task 7: S3 Versioning and Object Organization

### Objective
To organize S3 objects and understand how versioning preserves multiple versions of an object.

### Implementation
- Enabled versioning on the main S3 bucket.
- Organized objects using the prefixes `documents/`, `images/`, and `backups/`.
- Uploaded and updated `report.txt` to create multiple object versions.
- Viewed the versions of `report.txt`.
- Downloaded an earlier version and uploaded its content again under the same object key to restore the earlier content as a new current version.

### Result
S3 versioning was enabled, and multiple versions of `report.txt` were created. Restoring older content by uploading it again creates a new version; it does not erase the historical versions.

### Evidence
Folder: `screenshots/Task-7-S3-Versioning/`

## Task 8: S3 Bucket Policy

### Objective
To configure a bucket policy that grants a specific IAM user read access to the practical bucket.

### Implementation
Configured a bucket policy for `developer-user` with these permissions:

- `s3:ListBucket` on `tanuja-iam-s3-practical-2026`.
- `s3:GetObject` on objects inside the bucket.

The policy does not grant public or anonymous access. Block Public Access remains enabled.

### Security Learning
A bucket policy is a resource-based policy attached to an S3 bucket. It specifies which principals can perform which actions on the bucket or its objects.

The final effective permissions depend on all applicable policies and access controls.

### Evidence
Folder: `screenshots/Task-8-S3-Bucket-Policy/`

## Task 9: S3 Lifecycle Rule

### Objective
To configure an S3 lifecycle rule for transitioning objects to a lower-cost storage class.

### Rule Details

| Setting | Value |
|---|---|
| Rule name | `BackupTransitionRule` |
| Filter prefix | `backups/` |
| Action | Transition current object versions |
| Destination storage class | S3 Standard-IA |
| Transition period | 30 days |
| Status | Enabled |

### Result
The lifecycle rule was configured for objects under the `backups/` prefix to transition to S3 Standard-Infrequent Access after 30 days.

The transition is not immediate. Actual transition timing and eligibility depend on S3 lifecycle rules and applicable object-size and storage-class constraints.

### Evidence
Folder: `screenshots/Task-9-S3-Lifecycle/`

## Task 10: S3 Server Access Logging

### Objective
To configure server access logging for the main S3 bucket.

### Bucket Details
- **Source bucket:** `tanuja-iam-s3-practical-2026`
- **Destination bucket:** `tanuja-iam-s3-access-logs-2026`
- **Log prefix:** `access-logs/`

### Implementation
- Created the destination bucket in the Mumbai region.
- Kept the destination bucket private.
- Enabled server access logging on the main bucket.
- Selected the destination bucket and configured the `access-logs/` prefix.

### Result
Server access logging was configured to deliver logs to the designated destination bucket. Log delivery can take time; configuration being enabled does not by itself confirm that log files have arrived.

### Evidence
Folder: `screenshots/Task-10-S3-Access-Logging/`

## Task 11: S3 Security and Encryption

### Objective
To understand how S3 encryption and access controls help protect stored objects.

### Implementation and Security Concepts

**1. Encryption at rest — SSE-S3**

Verified that `report.txt` used server-side encryption with Amazon S3-managed keys (SSE-S3). This protects stored object data through server-side encryption.

**2. Encryption in transit — HTTPS/TLS**

HTTPS/TLS protects data while it travels between a client and AWS. Use secure HTTPS endpoints when accessing S3.

**3. Block Public Access**

All four S3 Block Public Access settings were enabled on the main bucket to help prevent accidental public exposure.

**4. IAM policies**

IAM policies grant or restrict permissions for IAM users, groups, and roles.

**5. Bucket policies**

Bucket policies are resource-based policies that control access to a particular S3 bucket and its objects.

**6. Least privilege**

Grant only the permissions required for a task, and avoid unnecessary broad access.

**7. Authentication vs. authorization**

- **Authentication:** Determines who or what is making a request.
- **Authorization:** Determines whether that identity is permitted to perform the requested action.

### Result
The practical covered S3 encryption, private bucket configuration, public access prevention, IAM permissions, and bucket-level access control.

### Evidence
Folder: `screenshots/Task-11-S3-Security/`

---

# Repository Structure

```text
AWS-IAM-S3-Practical/
├── README.md
├── policies/
│   ├── CustomS3ReadPolicy
│   └── EC2S3BucketReadPolicy.json
└── screenshots/
    ├── Task-3-EC2-IAM-Role/
    ├── Task-4-EC2-IAM-Role/
    ├── Task-6-S3-Bucket/
    ├── Task-7-S3-Versioning/
    ├── Task-8-S3-Bucket-Policy/
    ├── Task-9-S3-Lifecycle/
    ├── Task-10-S3-Access-Logging/
    └── Task-11-S3-Security/
```

The screenshot directory names above represent the intended evidence organization. Keep the README aligned with the folders and files actually present in the repository.

# Overall Conclusion

This practical provided hands-on experience with AWS IAM users and groups, AWS-managed and customer-managed policies, EC2 IAM roles, and Amazon S3 configuration and security.

The exercises covered read-only access, access-denied testing, bucket versioning, object organization, lifecycle transitions, server access logging, SSE-S3 encryption, and Block Public Access.

The practical demonstrates how IAM and S3 security controls can be combined to manage access and protect stored data while following the principle of least privilege.
