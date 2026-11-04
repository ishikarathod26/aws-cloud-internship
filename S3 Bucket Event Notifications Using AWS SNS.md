# AWS Task 4: S3 Bucket Actions Using AWS SNS

## 1. Objective

To configure an Amazon S3 bucket so that whenever a new object is uploaded, AWS automatically sends an email notification using Amazon SNS.

### Architecture

```text
User uploads a file
        |
        v
    S3 Bucket
        |
        v
S3 Event Notification
(ObjectCreated:*)
        |
        v
    SNS Topic
        |
        v
 Email Subscription
        |
        v
  Email Inbox
```

## 2. Resources Created

| Resource | Name / Configuration |
|---|---|
| S3 Bucket | `ishika-sns-notification-2026` |
| AWS Region | Asia Pacific (Mumbai) – `ap-south-1` |
| SNS Topic | `s3-upload-notifications` |
| Topic Type | Standard |
| Subscription Protocol | Email |
| Event Notification | `s3-object-created-to-sns` |
| Event Type | All object create events |

---

## 3. Step-by-Step Implementation

### Step 1: Create an S3 Bucket

1. Open the AWS Management Console.
2. Search for **S3**.
3. Open Amazon S3.
4. Click **Create bucket**.
5. Enter the bucket name:

   `ishika-sns-notification-2026`

6. Select the AWS Region:

   `Asia Pacific (Mumbai) – ap-south-1`

7. Keep **Block all public access** enabled.
8. Keep the remaining settings at their defaults.
9. Click **Create bucket**.

**Result:** S3 bucket was successfully created.

### Step 2: Create an SNS Topic

1. Search for **SNS** in the AWS Console.
2. Open Amazon Simple Notification Service.
3. Select **Topics** from the left navigation.
4. Click **Create topic**.
5. Select topic type: **Standard**.
6. Enter the topic name:

   `s3-upload-notifications`

7. Keep other settings at their defaults.
8. Click **Create topic**.

**Result:** SNS topic was successfully created.

### Step 3: Subscribe an Email to the SNS Topic

1. Open the newly created topic:

   `s3-upload-notifications`

2. Scroll to the **Subscriptions** section.
3. Click **Create subscription**.
4. Select protocol: **Email**.
5. Enter your email address in the endpoint field.
6. Click **Create subscription**.

Initially, subscription status will be:

`PendingConfirmation`

7. Open your email inbox.
8. Find the AWS SNS subscription confirmation email.
9. Click the **Confirm subscription** link.

**Important:** Until the email subscription is confirmed, SNS cannot deliver notifications to that email endpoint.

**Result:** Subscription status changed to `Confirmed`.

### Step 4: Configure SNS Topic Permissions

S3 needs permission to publish messages to the SNS topic.

1. Open the SNS topic.
2. Go to the **Access policy** section.
3. Edit the topic access policy.
4. Preserve the existing default policy statement.
5. Add a statement allowing the S3 service to publish messages.

Use the following statement, replacing the account ID if necessary:

```json
{
  "Sid": "AllowS3BucketToPublish",
  "Effect": "Allow",
  "Principal": {
    "Service": "s3.amazonaws.com"
  },
  "Action": "SNS:Publish",
  "Resource": "arn:aws:sns:ap-south-1:YOUR_ACCOUNT_ID:s3-upload-notifications",
  "Condition": {
    "ArnLike": {
      "aws:SourceArn": "arn:aws:s3:::ishika-sns-notification-2026"
    },
    "StringEquals": {
      "aws:SourceAccount": "YOUR_ACCOUNT_ID"
    }
  }
}
```

5. Save the access policy.

**Why is this required?**

By default, S3 does not automatically have permission to publish messages to our SNS topic. This policy authorizes the S3 service to call `SNS:Publish`, restricted to our specified bucket and AWS account.

**Result:** SNS topic policy was successfully updated.

### Step 5: Configure S3 Event Notification

Now we connect the S3 bucket to the SNS topic.

1. Open Amazon S3.
2. Select the bucket:

   `ishika-sns-notification-2026`

3. Open the **Properties** tab.
4. Scroll down to **Event notifications**.
5. Click **Create event notification**.
6. Enter the event name:

   `s3-object-created-to-sns`

7. Under Event types, select:

   **All object create events**

   This represents:

   `s3:ObjectCreated:*`

8. Leave the Prefix and Suffix fields empty.
9. Under Destination, select **SNS Topic**.
10. Choose the existing topic:

    `s3-upload-notifications`

11. Click **Save changes**.

**Result:** S3 event notification was successfully created.

### Step 6: Test the Configuration

Now we test whether the complete notification system works.

1. Open the S3 bucket.
2. Go to the **Objects** tab.
3. Click **Upload**.
4. Click **Add files**.
5. Select a small text file from your computer.
6. Click **Upload**.
7. Wait for the upload to complete.

Our test file:

`sns-test.txt.txt`

Upload status:

- Succeeded: 1 file
- Failed: 0 files
- File size: 28 bytes

### Step 7: Verify Email Notification

1. Open your email inbox.
2. Find the latest AWS SNS notification email.
3. Verify that the email was received after uploading the file.

**Final Result: SUCCESS**

The email was successfully received.

This confirms that the complete flow is working:

`S3 → Event Notification → SNS Topic → Email Subscription → Email Inbox`

---

## 4. Important Concepts Learned

| Concept | Explanation |
|---|---|
| Amazon S3 | Object storage service used to store files. |
| Amazon SNS | Simple Notification Service used to send messages to subscribers. |
| SNS Topic | A communication channel through which messages are published. |
| Subscription | Connects an endpoint, such as an email address, to an SNS topic. |
| Event Notification | Allows S3 to send notifications when specified bucket events occur. |
| ObjectCreated:* | Event type that covers object creation operations. |
| Access Policy | Defines who is allowed to perform actions on an SNS topic. |
| ARN | Amazon Resource Name; a unique identifier for an AWS resource. |

## 5. Important Points to Remember

1. The SNS topic and S3 bucket must be in the same AWS Region for this direct S3-to-SNS notification setup.
2. Email subscriptions must be confirmed before notifications can be delivered.
3. S3 requires permission to publish messages to the SNS topic.
4. `ObjectCreated:*` covers object creation events such as PUT, POST, COPY and multipart upload completion.
5. SNS Standard topics support email subscriptions.
6. One SNS topic can have multiple subscribers.
7. SNS can distribute notifications to multiple subscribers.

## 6. Final Verification Checklist

- [x] S3 bucket created.
- [x] SNS Standard topic created.
- [x] Email subscription created.
- [x] Email subscription confirmed.
- [x] SNS access policy updated.
- [x] S3 event notification configured.
- [x] Test file uploaded successfully.
- [x] Email notification received successfully.

## 7. Conclusion

Successfully implemented an event-driven notification system using Amazon S3 and Amazon SNS.

Whenever a new object is uploaded to the configured S3 bucket, an `ObjectCreated` event triggers an SNS notification, which is delivered to the confirmed email subscriber.

**AWS Task 4 completed successfully.**
