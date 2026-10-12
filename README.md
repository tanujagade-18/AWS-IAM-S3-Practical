# AWS IAM & S3 Practical Examination

## Student Details
- **Name:** Tanuja Santosh Gade
- **Batch:** 24_June
- **Repository:** AWS-IAM-S3-Practical
- **AWS Region:** Mumbai (`ap-south-1`)

## Objective
To understand AWS Identity and Access Management (IAM), IAM users and groups, managed and custom policies, EC2 IAM roles, and Amazon S3 security and access control.

## AWS Services Used
- AWS Identity and Access Management (IAM)
- Amazon Simple Storage Service (S3)
- Amazon Elastic Compute Cloud (EC2)
- AWS Command Line Interface (AWS CLI)

## S3 Bucket Details
- **Bucket Name:** `tanuja-iam-s3-practical-2026`
- **Region:** `ap-south-1`
- **Access:** Private
- **Block Public Access:** Enabled

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
- Added `developer-user` to the `Developers` group.
- Added `tester-user` to the `Testers` group.

### Result
IAM users and groups were created and configured for separate permission requirements.

### Evidence
Screenshots are stored in the `screenshots/` directory.

---

## Task 2: IAM Groups and Managed Policies

### Objective
To attach AWS-managed policies to IAM groups and understand the difference between read-only and full S3 access.

### Implementation
- Attached `AmazonS3ReadOnlyAccess` to the `Developers` group.
- Attached `AmazonS3FullAccess` to the `Testers` group.

### Policy Comparison

| Feature | AmazonS3ReadOnlyAccess | AmazonS3FullAccess |
|---|---|---|
| List and view S3 resources | Allowed, subject to policy scope | Allowed |
| Read and download objects | Allowed | Allowed |
| Upload objects | Not allowed | Allowed |
| Delete objects | Not allowed | Allowed |
| Overall access | Read-only | Full S3 access within the policy's scope |

### Security Learning
Read-only access is appropriate when users only need to view or download data. Full access allows broader S3 operations and should be granted only when required.

Actual access can also be affected by other IAM policies, bucket policies, permission boundaries, and organization-level controls.

### Result
AWS-managed policies were attached to their respective IAM groups.

### Evidence
Screenshots are stored in the `screenshots/` directory.

---

## Task 3: Custom S3 Read Policy

### Objective
To create a customer-managed IAM policy that allows listing a specific S3 bucket and reading its objects without granting upload or delete permissions.

### Policy Details
- **Policy Name:** `CustomS3ReadPolicy`
- **S3 Bucket:** `tanuja-iam-s3-practical-2026`
- **Attached To:** `Developers` group

### Policy Permissions
- `s3:ListBucket` — allows listing objects in the specified bucket.
- `s3:GetObject` — allows reading and downloading objects from the specified bucket.

The custom policy does not grant:
- `s3:PutObject` — uploading or overwriting objects.
- `s3:DeleteObject` — deleting objects.

### Security Best Practices
- Keep S3 buckets private unless public access is explicitly required.
- Follow the principle of least privilege.
- Grant only the permissions required for a user's tasks.
- Never upload AWS passwords, access keys, or secret keys to GitHub.

### Result
A customer-managed policy was created to provide bucket-specific read permissions.

**Note:** The `Developers` group also has `AmazonS3ReadOnlyAccess`, so its effective read access may extend beyond this one bucket. The custom policy itself is restricted to the practical bucket.

### Evidence
Policy details and JSON are maintained in the `policies/` directory.

---

## Task 4: EC2 IAM Role with S3 Read-Only Access

### Objective
To allow an EC2 instance to access a specific S3 bucket securely through an IAM role instead of storing long-term AWS access keys on the instance.

### Role Details
- **Actual Role Name:** `EC2-S3-ReadOnly-Role`
- **Trusted Service:** EC2 (`ec2.amazonaws.com`)
- **Bucket-Specific Policy:** `EC2S3BucketReadPolicy`
- **S3 Bucket:** `tanuja-iam-s3-practical-2026`

### Implementation
1. Created an IAM role trusted by the EC2 service.
2. Attached the role to the practice EC2 instance.
3. Initially used `AmazonS3ReadOnlyAccess` for testing.
4. Created the customer-managed policy `EC2S3BucketReadPolicy`.
5. Detached `AmazonS3ReadOnlyAccess` and attached `EC2S3BucketReadPolicy` to the role.
6. Used AWS CLI on EC2 to test access to S3.

### Permissions
The bucket-specific policy allows:
- Listing the specified bucket through `s3:ListBucket`.
- Reading and downloading objects through `s3:GetObject`.

The policy does not grant upload or delete permissions.

### Security Learning
EC2 IAM roles provide temporary credentials to applications and AWS CLI on the instance. This avoids the need to store long-term access keys on EC2.

### Result
The EC2 role successfully listed the practical bucket and downloaded an object. An upload attempt was denied during testing.

### Evidence
- Role screenshots: `screenshots/Task-3-EC2-IAM-Role/`
- Bucket-specific policy screenshots: `screenshots/Task-4-EC2-IAM-Role/`
- Policy JSON: `policies/EC2S3BucketReadPolicy.json`

**Naming note:** The assignment specifies `EC2S3ReadRole`, but the role currently documented is named `EC2-S3-ReadOnly-Role`. These names are not identical.

---

## Task 5: IAM Access Testing

### Objective
To verify that the configured IAM permissions allow intended read operations and deny unauthorized write operations.

### Test Environment
- **IAM User:** `developer-user`
- **EC2 IAM Role:** `EC2-S3-ReadOnly-Role`
- **S3 Bucket:** `tanuja-iam-s3-practical-2026`

### Test Results

| No. | Action Tested | IAM User / Role | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|
| 1 | List S3 bucket objects | `developer-user` | Allowed | Bucket opened and objects were visible | PASS |
| 2 | Download an object | `EC2-S3-ReadOnly-Role` | Allowed | Object downloaded successfully through AWS CLI | PASS |
| 3 | Upload an object | `developer-user` | Denied | Access Denied | PASS |
| 4 | Upload an object | `EC2-S3-ReadOnly-Role` | Denied | Access Denied during EC2 testing | PASS |
| 5 | Delete an object | `developer-user` | Denied | Record the actual result after confirming the test | PENDING |

### Conclusion
The `developer-user` successfully listed the S3 bucket and was denied upload access. The EC2 IAM role successfully listed and downloaded an object, while an upload attempt was denied. Delete access for `developer-user` must be marked complete only after the result has been confirmed.

### Evidence
Screenshots are stored in:
- `screenshots/Task-3-EC2-IAM-Role/`
- `screenshots/Task-4-EC2-IAM-Role/`

---

# Section B — Amazon S3

The following tasks are part of the practical examination and should be documented after implementation and testing.

## Task 6: Create and Secure an S3 Bucket
**Status:** Existing practical bucket created.

- Bucket: `tanuja-iam-s3-practical-2026`
- Region: `ap-south-1`
- Block Public Access: Enabled
- Bucket access: Private

Additional security settings should be documented if configured.

## Task 7: Organize Objects and Enable Versioning
**Status:** Pending implementation and verification.

Planned activities:
- Organize objects using meaningful object keys or prefixes.
- Enable S3 bucket versioning.
- Upload and update a test object to verify object versions.

## Task 8: Configure an S3 Bucket Policy
**Status:** Pending implementation and verification.

Planned activities:
- Create a bucket policy according to the practical requirements.
- Verify that the policy grants only the intended access.
- Test access and record the results.

## Task 9: Configure an S3 Lifecycle Rule
**Status:** Pending implementation and verification.

Planned activities:
- Create a lifecycle rule based on the instructor's requirements.
- Document the selected actions and transition or expiration settings.
- Verify the rule configuration.

## Task 10: Configure Server Access Logging
**Status:** Pending implementation and verification.

Planned activities:
- Configure server access logging if required by the assignment.
- Select an appropriate destination bucket.
- Verify the logging configuration.

## Task 11: Explain Encryption and Access Controls
**Status:** Pending completion.

Topics to document:
- Encryption at rest.
- Encryption in transit using HTTPS/TLS.
- S3 Block Public Access.
- IAM policies and bucket policies.
- Least-privilege access.
- The difference between authentication and authorization.

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
    └── Task-4-EC2-IAM-Role/
```

Additional screenshot folders and files may be added as the remaining practical tasks are completed.

# Overall Conclusion

This practical demonstrates AWS IAM user and group management, managed and customer-managed policies, EC2 IAM roles, and S3 read-only access testing. It also provides a plan for completing the remaining S3 practical tasks involving versioning, bucket policies, lifecycle management, logging, and encryption.

All test results should reflect the actions actually performed, and all screenshots and policy files should be maintained in the repository as evidence.
