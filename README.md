# AWS IAM & S3 Practical Examination

## Student Details
- **Name:** Tanuja Santosh Gade
- **Batch:** [24_June]
- **Repository:** AWS-IAM-S3-Practical

## Objective
To learn AWS IAM identity and permission management and Amazon S3 storage, security, versioning, lifecycle management, logging, and encryption.

## Practical Tasks

### Section A — IAM
1. Create IAM users and groups.
2. Attach managed IAM policies.
3. Create a custom S3 read policy.
4. Create an EC2 IAM role.
5. Verify IAM permissions.

### Section B — S3
6. Create and secure an S3 bucket.
7. Organize objects and enable versioning.
8. Configure an S3 bucket policy.
9. Configure a lifecycle rule.
10. Configure server access logging.
11. Explain encryption and access controls.

## Test Results
Results will be recorded after each practical is completed and tested.

## Conclusion
This practical will help me understand AWS identity management, permissions, secure object storage, object versioning, lifecycle rules, access logging, and encryption.

## Evidence
Screenshots are organized in the `screenshots/` directory. Policy JSON files are stored in the `policies/` directory.

# AWS IAM and S3 Practical

## Task 1: IAM Users and Groups

### Task 1A: Create IAM User
- User created: `developer-user`
- Permissions: No group or policy assigned yet
- Status: Completed

## Task 2: Amazon S3 Bucket and IAM Policies

### Objective
To create an Amazon S3 bucket and configure IAM policies to control access to S3 resources based on user permissions.

### AWS Services Used
- Amazon S3
- AWS IAM

### Implementation
1. Created an S3 bucket named `tanuja-iam-s3-practical-2026` in the Mumbai (`ap-south-1`) region.
2. Kept the bucket private by enabling Block Public Access.
3. Configured bucket ownership with ACLs disabled.
4. Created a custom IAM policy named `CustomS3ReadPolicy`.
5. Configured the policy to allow listing the specified S3 bucket and reading its objects.
6. Attached the required S3 permissions to the appropriate IAM group.
7. Reviewed the IAM group permissions to understand how AWS controls access to S3 resources.

### Custom Policy Permissions
The custom policy grants these actions on the project bucket:
- `s3:ListBucket` — allows listing objects in the bucket.
- `s3:GetObject` — allows reading and downloading objects.

### Security Best Practices
- Keep S3 buckets private unless public access is explicitly required.
- Follow the principle of least privilege by granting only the permissions users need.
- Use IAM policies to control access to AWS resources.
- Avoid sharing AWS access keys, secret keys, or sensitive information in GitHub.

### Learning Outcome
Learned how Amazon S3 stores objects and how IAM policies control access to buckets and objects. Understood the difference between listing a bucket, reading an object, and granting broader S3 permissions.


## Task 3: EC2 IAM Role with S3 Read-Only Access

### Objective
To securely access Amazon S3 from an EC2 instance using an IAM Role without storing AWS access keys on the instance.

### AWS Services Used
- Amazon EC2
- AWS IAM
- Amazon S3
- AWS CLI

### Implementation
1. Created an IAM Role named `EC2-S3-ReadOnly-Role`.
2. Configured EC2 as the trusted service.
3. Attached the `AmazonS3ReadOnlyAccess` policy.
4. Attached the IAM Role to the `PRACTICE` EC2 instance.
5. Installed and configured AWS CLI using the instance role.
6. Listed S3 buckets and objects from the EC2 terminal.
7. Downloaded an image from the S3 bucket successfully.
8. Attempted to upload a file and received `AccessDenied`, as expected for read-only permissions.

### Test Results
- **List S3 objects:** Successful
- **Download S3 object:** Successful
- **Upload S3 object:** Denied as expected

### Security Learning
IAM Roles allow EC2 instances to access AWS services using temporary credentials. Read-only permissions allow listing and downloading objects without granting upload permissions.


## Task 4: EC2 IAM Role with S3 Read-Only Access

### Objective
Create an IAM role for EC2 that allows read-only access to the practical S3 bucket.

### Role Details
- **Role Name:** EC2-S3-ReadOnly-Role
- **Trusted Entity:** EC2 (`ec2.amazonaws.com`)
- **Policy Attached:** EC2S3BucketReadPolicy
- **S3 Bucket:** `tanuja-iam-s3-practical-2026`

### Permissions
- List objects in the specified S3 bucket.
- Read and download objects from the specified bucket.
- Upload and delete permissions are not granted by this policy.
- No long-term AWS access keys are required for EC2 role access.

### Evidence
Screenshots are available in `screenshots/Task-4-EC2-IAM-Role/`.

Policy JSON: `policies/EC2S3BucketReadPolicy.json`
