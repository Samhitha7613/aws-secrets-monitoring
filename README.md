# Build a Security Monitoring System

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-monitoring)

**Author:** Samhitha M  
**Email:** samhithamadala74@gmail.com

---

![Image](http://learn.nextwork.org/sparkling_navy_elegant_quail/uploads/aws-security-monitoring_reghtjy)

---

## Introducing Today's Project!

In this project, I will demonstrate how to monitor sensitive activity in AWS by tracking access to secrets and setting up real-time alerts.

I am using AWS Secrets Manager, AWS CloudTrail, and Amazon CloudWatch to build a complete security monitoring system.

In Stage 1, I first create a secret in AWS Secrets Manager. Then I enable CloudTrail to log all API activity in my AWS account. After that, I test whether accessing the secret generates logs.

In Stage 2, I set up a CloudWatch log filter to detect when the secret is accessed. Then I create a CloudWatch alarm and connect it with Amazon SNS to send an email alert whenever such activity occurs. Finally, I test the full system to ensure alerts are triggered correctly.

I am doing this project to learn how real-world cloud security monitoring works, especially how to detect unauthorized access and respond quickly using automated alerts.

### Tools and concepts

In this project, I learned how to use key AWS services like AWS Secrets Manager, AWS CloudTrail, Amazon CloudWatch, and Amazon SNS to build a complete monitoring and alert system.

I understood important concepts such as:

1. Secure storage of sensitive data (secrets management)
2. Logging and auditing of API activities (CloudTrail events)
3. Creating metric filters and alarms for real-time monitoring
4. Setting thresholds and triggering alerts
5. Notification systems using SNS
6. End-to-end security monitoring and troubleshooting

### Project reflection

It took me around 30 minutes to complete this project, including setting up the services, configuring monitoring and alerts, and testing the system end-to-end.

---

## Create a Secret

AWS Secrets Manager is a service provided by AWS that helps you securely store, manage, and retrieve sensitive information such as passwords, API keys, database credentials, and tokens.

You could use Secrets Manager to protect important data in your applications and control who can access it. It also helps automate tasks like rotating passwords and keeping secrets safe without hardcoding them into your code.

The secret I created in AWS Secrets Manager is a sample sentence that contains that I am Spider-Man...

![Image](http://learn.nextwork.org/sparkling_navy_elegant_quail/uploads/aws-security-monitoring_o5p6q7r8)

---

## Set Up CloudTrail

AWS CloudTrail is a service that records all the activities and API calls happening in an AWS account, such as creating resources, modifying settings, or accessing secrets.

A trail is a configuration in CloudTrail that tells it what activities to track and where to store the logs.

I set up a trail to continuously monitor my account’s activity and store the logs in a secure location so I can track when my secret is accessed.

CloudTrail records management events, data events, network activity events, and insights events to help monitor and secure AWS activity.

### Read vs Write Activity

In AWS CloudTrail, Read API activities only retrieve or view data without making changes, while Write API activities create, modify, or delete resources and change the system state.

---

## Verifying CloudTrail

I retrieved my secret in two ways:

First, through the AWS console using AWS CloudTrail and AWS Secrets Manager, where I viewed the secret value directly.

Second, using AWS CloudShell, where I accessed the secret by running commands to retrieve its value.

When I analyzed the events in AWS CloudTrail, I discovered that every time I accessed the secret, an event was recorded with details like the event name (such as GetSecretValue), the time of access, the user identity, and the source of the request.

I also observed that retrieving the secret is logged as a management event, which confirms that CloudTrail is correctly tracking access to sensitive data.

![Image](http://learn.nextwork.org/sparkling_navy_elegant_quail/uploads/aws-security-monitoring_s8t9u0v1)

---

## CloudWatch Metrics

Amazon CloudWatch Logs is a service that collects, stores, and monitors log data from AWS services and applications.

It is important for monitoring because it allows us to analyze logs in real time, create metrics, and set up alerts, so we can quickly detect events like secret access and respond to potential security issues.

The key difference is that AWS CloudTrail stores logs for auditing and history (mainly in S3), while Amazon CloudWatch is used for real-time monitoring, analysis, and triggering alerts based on those logs.

In Amazon CloudWatch:

-> Metric value is the value assigned to a log event when it matches the filter (for example, setting it to 1 to count each secret access event).
-> Default value is used when no matching event is found, ensuring the metric still has a value (usually 0).

![Image](http://learn.nextwork.org/sparkling_navy_elegant_quail/uploads/aws-security-monitoring_a9b0c1d2)

---

## CloudWatch Alarm

In Amazon CloudWatch, the threshold defines the limit at which the alarm is triggered.

In my case, I set the threshold so that if the SecretIsAccessed metric is greater than or equal to 1, the alarm is activated. This means even a single access to the secret will trigger a notification.

We use this setting because accessing a secret is a sensitive action, so even one event should be monitored and alerted immediately.

An Amazon SNS topic is a communication channel used to send messages or notifications to multiple subscribers.

You can use it to deliver alerts via email, SMS, or other endpoints whenever an event (like a CloudWatch alarm) is triggered.

AWS requires email confirmation for Amazon SNS subscriptions to verify that the email address is valid and that the user has given permission to receive notifications.

This helps prevent misuse, spam, and unauthorized subscriptions, ensuring that alerts are only sent to confirmed recipients.

![Image](http://learn.nextwork.org/sparkling_navy_elegant_quail/uploads/aws-security-monitoring_fsdghstt)

---

## Troubleshooting Notification Errors

During my test, when I accessed the secret using AWS Secrets Manager, the event was successfully recorded in AWS CloudTrail.

This triggered the metric in Amazon CloudWatch, which caused the alarm to go into the ALARM state, and I received an email notification through Amazon SNS.

This confirmed that the monitoring and alert system was working correctly end-to-end.

To troubleshoot the issue, I checked the following 5 things:

Verified that Amazon SNS email subscription was confirmed.
Checked if the Amazon CloudWatch alarm was in the correct state (ALARM or OK).
Ensured the metric filter was correctly configured to detect GetSecretValue events.
Verified that AWS CloudTrail was properly logging events and sending them to CloudWatch.
Checked the alarm configuration (threshold, evaluation period, and correct metric selection).

The reason I didn’t receive an email earlier was because my Amazon SNS subscription was not confirmed.

Until the email subscription is confirmed, SNS does not send any notifications, so even though the Amazon CloudWatch alarm was triggered, I didn’t receive any alerts.

---

## Success!

I validated my monitoring system by accessing the secret using AWS Secrets Manager and then checking if the event was recorded in AWS CloudTrail.

After that, I verified that the metric in Amazon CloudWatch increased, the alarm changed to the ALARM state, and I received an email notification through Amazon SNS.

This confirmed that the entire system—from logging to alerting—was working correctly.

![Image](http://learn.nextwork.org/sparkling_navy_elegant_quail/uploads/aws-security-monitoring_ageraergearge)

---

## Comparing CloudWatch with CloudTrail Notifications

I updated my AWS CloudTrail configuration to integrate it with Amazon CloudWatch and Amazon SNS so that the logs can be actively monitored and trigger alerts.

I did this because earlier CloudTrail was only storing logs, but after the update, I can now receive real-time notifications whenever my secret is accessed.

After enabling direct notifications using AWS CloudTrail and Amazon SNS, I received email messages in my inbox whenever an event occurred.



![Image](http://learn.nextwork.org/sparkling_navy_elegant_quail/uploads/aws-security-monitoring_d7e8f9g0)

---

---
