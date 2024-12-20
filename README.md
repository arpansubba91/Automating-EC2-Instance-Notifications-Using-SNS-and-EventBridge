How to Set Up Notifications for EC2 Instance State Changes Using SNS and EventBridge
This guide will help you create a system to send notifications to an email or phone number when an EC2 instance changes its state (like starting, stopping, or terminating). Follow these simple step-by-step instructions.
---
Step 1: Create an Amazon SNS Topic
1. Open the Amazon SNS Console:
   - Go to the Amazon SNS console at https://console.aws.amazon.com/sns/.
2. Create a Topic:
   - Click on the “Create topic” button.
   - Under Type, choose “Standard”.
   - In the Name field, type a name for your topic. For example: EC2StateChangeTopic.
   - Click on the “Create topic” button at the bottom of the page.
3. Create a Subscription:
   - After creating the topic, go to the “Subscriptions” tab.
   - Click on the “Create subscription” button.
   - Under Topic ARN, select the topic you just created (e.g., EC2StateChangeTopic).
   - In the Protocol dropdown, choose how you want to receive notifications:
       - For email: Choose “Email”.
       - For phone: Choose “SMS”.
   - In the Endpoint field, type the email address or phone number where you want to receive notifications.
   - Click on “Create subscription”.
   - Verify the Subscription:
       - If you chose email: Check your email inbox for a confirmation message. Click on the link inside the email to confirm the subscription.
       - If you chose SMS: You will receive a confirmation message on your phone. Follow the instructions to confirm.
---
Step 2: Create an EventBridge Rule
1. Open the EventBridge Console:
   - Go to the Amazon EventBridge console at https://console.aws.amazon.com/events/.
2. Create a Rule:
   - Click on the “Create rule” button.
   - In the Name field, type a name for your rule. For example: EC2StateChangeRule.
   - Leave the Event bus as the default value.
3. Define Event Source:
   - Under Event source, choose “AWS services”.
   - In the Service name dropdown, select “EC2”.
   - In the Event type dropdown, select “EC2 Instance State-change Notification”.
4. Add Event Pattern:
   - Click on “Edit” under Event pattern.
   - Copy and paste the following JSON to monitor specific state changes:


    {
      "source": ["aws.ec2"],
      "detail-type": ["EC2 Instance State-change Notification"],
      "detail": {
        "state": ["pending", "running", "stopping", "stopped", "shutting-down", "terminated"]
      }
    }

   - Click “Save”.
5. Set a Target:
   - Under Target, click on “Add target”.
   - In the Target type dropdown, choose “SNS topic”.
   - In the Topic dropdown, select the SNS topic you created earlier (e.g., EC2StateChangeTopic).
6. Review and Save:
   - Scroll down and click on “Create” to save the rule.
---
Step 3: Test Your Setup
1. Perform an EC2 Action:
   - Go to the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
   - Start, stop, or terminate an EC2 instance.
2. Check for Notifications:
   - If you subscribed via email: Check your inbox for a notification.
   - If you subscribed via SMS: Check your phone for a message.

