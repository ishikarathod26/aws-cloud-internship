# S3 Cross-Region Replication (CRR)

## 1. Objective

To configure Amazon S3 Cross-Region Replication (CRR) between two S3 buckets located in different AWS Regions and verify automatic object replication.

## 2. Architecture

| Component | Configuration |
|---|---|
| Source Bucket | `ishika-crr-source-2026` |
| Source Region | Mumbai (`ap-south-1`) |
| Destination Bucket | `ishika-crr-destination-2026` |
| Destination Region | Singapore (`ap-southeast-1`) |
| Replication Type | Cross-Region Replication |

**Replication Flow:**

Mumbai Source Bucket → S3 Replication Rule → Singapore Destination Bucket

## 3. Prerequisites

- AWS account access.
- Two S3 general-purpose buckets in different AWS Regions.
- Versioning must be enabled on both buckets.

## 4. Implementation Steps

### Step 1: Create Source Bucket

Created an S3 bucket with the following configuration:

- Bucket Name: `ishika-crr-source-2026`
- Region: Asia Pacific (Mumbai) – `ap-south-1`
- Block Public Access: Enabled
- Default Encryption: SSE-S3
- Bucket Versioning: Enabled

### Step 2: Create Destination Bucket

Created another S3 bucket:

- Bucket Name: `ishika-crr-destination-2026`
- Region: Asia Pacific (Singapore) – `ap-southeast-1`
- Block Public Access: Enabled
- Default Encryption: SSE-S3
- Bucket Versioning: Enabled

### Step 3: Configure Cross-Region Replication

Opened the source bucket and navigated to:

**Management → Replication Rules → Create Replication Rule**

Configured the following settings:

| Setting | Value |
|---|---|
| Rule Name | Mumbai-to-Singapore-CRR |
| Status | Enabled |
| Priority | 0 |
| Rule Scope | Apply to all objects in the bucket |
| Destination | ishika-crr-destination-2026 |
| Destination Region | Singapore |
| IAM Role | Create new role |

### Step 4: Configure Additional Options

- KMS Replication: Disabled
- Change Destination Storage Class: Disabled
- Replication Time Control (RTC): Disabled
- Replication Metrics: Disabled
- Delete Marker Replication: Disabled
- Replica Modification Sync: Disabled

Default storage class and SSE-S3 encryption were retained.

### Step 5: Configure Existing Objects Replication

AWS prompted whether to replicate existing objects.

Selected:

**No, do not replicate existing objects.**

This avoided creating a one-time Batch Operations replication job. The practical focused on testing new object replication after enabling the rule.

### Step 6: Configure IAM Role

Selected **Create new role**.

AWS created an IAM role to provide the permissions required for S3 to read objects from the source bucket and replicate them to the destination bucket.

## 5. Live Replication Testing

Created a text file on the local desktop:

**File Name:** `crr-live-test.txt`

Uploaded the file to the source bucket:

`ishika-crr-source-2026`

Upload status: **Succeeded**

Then opened the destination bucket:

`ishika-crr-destination-2026`

The file appeared automatically in the Singapore bucket without manually uploading it there.

## 6. Verification

Opened the replicated object in the destination bucket and checked its Properties.

| Property | Result |
|---|---|
| Object Name | crr-live-test.txt |
| AWS Region | Singapore (`ap-southeast-1`) |
| Bucket Versioning | Enabled |
| Replication Status | REPLICA |
| Storage Class | Standard |
| Encryption | SSE-S3 |

**Result:** The `REPLICA` status confirmed that the object was successfully replicated from the source bucket.

## 7. Important Concepts Learned

### Cross-Region Replication (CRR)
CRR automatically and asynchronously replicates objects between S3 buckets located in different AWS Regions.

### Versioning
Versioning maintains multiple versions of an object. It must be enabled on both source and destination buckets for S3 replication.

### IAM Role
Provides the permissions required by S3 to perform replication operations.

### Replication Rule
Defines which objects should be replicated and the destination bucket.

### Live Replication
Replicates eligible new objects uploaded after the replication rule is enabled. Replication is asynchronous, not necessarily instantaneous.

### Batch Replication
Used to replicate existing objects through a one-time Batch Operations job.

### Replication Status
- PENDING: Replication is in progress.
- COMPLETED: Replication completed successfully on the source object.
- FAILED: Replication failed.
- REPLICA: Object is a replicated copy in the destination bucket.

## 8. CRR vs SRR

| CRR | SRR |
|---|---|
| Cross-Region Replication | Same-Region Replication |
| Replicates objects to a different AWS Region | Replicates objects within the same AWS Region |

## 9. Cost Considerations

Potential charges include:

- Destination bucket storage.
- Cross-Region data transfer.
- Replication requests.
- Additional charges for optional features such as Replication Time Control and CloudWatch metrics.

Optional paid features were kept disabled during this practical.

## 10. Final Outcome

Successfully configured and tested Amazon S3 Cross-Region Replication between Mumbai and Singapore.

Verified that a newly uploaded object was automatically replicated to the destination bucket and displayed the `REPLICA` replication status.

**Task Status: Completed Successfully.**
