**Step-by-Step Guide to Create an NBA Game Day Score Alert System**

Here’s the step-by-step guide to design and implement an alert system that sends real-time notification for NBA game information to subscribed users via SMS or email. The project uses Amazon SNS, AWS Lambda, IAM least privilege, Amazon EventBridge, Python, and NBA APIs.

Step 1: Understand the Requirements
•	Goal: Deliver real-time NBA game day information to subscribed users.
•	Delivery Channels: SMS/Email via Amazon SNS.
•	Data Source: Use an NBA API (e.g., Sportsdata.io, or another sports API) for real-time game data.
•	Cloud Services:
- Amazon SNS: For sending notifications.
- AWS Lambda: To process game data and send alerts.
- Amazon EventBridge: To trigger Lambda functions at regular intervals.
- IAM: For granting least privilege for security purpose.

Step 2: Set Up Prerequisites
1.	AWS Account: Ensure you have an AWS account with admin access.
2.	NBA API Key: Sign up for an NBA API provider and get an API key.
3.	IAM Role with Least Privileges:
- Create an IAM role for Lambda with the following policies:
  -	Custom Policy (copy and paste the policy from the GitHub Repo).
  - AWSLambdaBasicExecutionRole.
- Apply the least privilege principle by restricting resources to only what’s required.

Step 3: Create an SNS Topic
1.	Go to Amazon SNS in the AWS Management Console.
2.	Create an SNS topic:
- Topic type: Standard.
-	Name: NBA_Game_Alerts.
3.	Allow users to subscribe to this topic using their email or phone number. To subscribe;
-	Click on the Topic created
-	On the subscription tab, click Create Subscription
-	Protocol: Email / Phone.
-	Enter Email or Phone number in International Format (e.g. +2348012345678)
-	Click Create Subscription
-	Check your Email to confirm the subscription if Email was selected.

Step 4: Write a Lambda Function
1.	Purpose: Fetch NBA scores, process game day data, and publish updates to the SNS topic.
2.	Steps:
-	Use the NBA API to fetch real-time game data.
-	Parse the response for relevant information (e.g., game score, teams, current period).
-	Publish formatted messages to the SNS topic.
Clone the github repo for the code
https://github.com/Faoziyah/30_days_devops_challenge/tree/main/DevOps_Challenge_Day2/NBA_Game_Day_Notification
1.	Go to Lambda in the AWS Management Console.
2.	Click Create Function.
3.	Choose Author from Scratch
-	Function Name: The Name of the Function.
-	Runtime: Python.
5.	Assign the IAM role created earlier to the function
6.	In the function code tab, copy and paste the content of src/game_day_notification.py to the inline code editor
7.	In the Environment Variables section at the left side of the page, click Add Environment Variable
- SNS_TOPIC_ARN: The ARN of the SNS topic.
-	NBA_API_KEY: Your NBA API key.

Step 5: Configure Amazon EventBridge
1.	Go to Amazon EventBridge in the AWS Management Console.
2.	Create a rule:
-	Name: NBA_Game_Score_Notifier.
-	Event Source: Schedule.
-	Schedule Expression: rate(2 hours) (or another interval based on API rate limits).
-	Target: The Lambda function created earlier.
3.	Ensure the EventBridge rule has permissions to invoke the Lambda function.

Step 6: Test the System
1.	Subscribe to the SNS topic with your email or phone number.
2.	Trigger the Lambda function manually or via EventBridge.
3.	Test the function
-	Go to Lambda function in the AWS Management Console
-	Create a Test event for 
-	Run the function and check CloudWatch Logs for errors
-	The Lambda function fetches game data correctly.
-	Notifications are delivered to subscribers.
-	Verify that SMS notifications are sent to the subscribed users via Phone Message or Email.

Step 7: Future Enhancements
1.	Add user-specific preferences (e.g., favorite teams or games).
2.	Include advanced game statistics in alerts.
3.	Integrate data visualization tools.
4.	Implement a web interface or mobile app for managing subscriptions.
5.	Set up a CI/CD pipeline to deploy updates seamlessly.
