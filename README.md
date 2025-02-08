# Build an AI-Powered Chatbot with Amazon Lex, Bedrock, S3, and RAG

## Step 1: Create an IAM User
1. Sign in to AWS Management Console.
2. Navigate to **IAM** (Identity and Access Management).
3. Click on **Users** > **Add users**.
4. Set a username (e.g., `chatbot-user`).
5. Select **Access key - Programmatic access**.
6. Click **Next: Permissions**.
7. Attach the following policies:
   - `AmazonLexFullAccess`
   - `AmazonS3FullAccess`
   - `AWSBedrockFullAccess`
8. Click **Next** until you finish creating the user.
9. Save the **Access Key ID** and **Secret Access Key**.

## Step 2: Create an Amazon S3 Bucket
1. Go to **S3** service.
2. Click **Create bucket**.
3. Set a unique **Bucket name** (e.g., `chatbot-storage-123`).
4. Choose a region (same as Lex and Bedrock).
5. Disable **Block all public access** if required.
6. Click **Create bucket**.

## Step 3: Set Up Amazon Bedrock
1. Navigate to **Amazon Bedrock** in AWS.
2. Enable **Foundational Models** (if not enabled).
3. Create a model using **Anthropic Claude** or **Amazon Titan**.
4. Deploy the model and note down the API endpoint.

## Step 4: Create an Amazon Lex Chatbot
1. Go to **Amazon Lex**.
2. Click **Create bot**.
3. Choose **Custom bot**.
4. Name your bot (e.g., `AIChatbot`).
5. Set IAM role to **Lex Service Role**.
6. Configure intents, slots, and utterances.
7. Integrate Bedrock by calling its API inside Lambda.

## Step 5: Deploy Amazon Lambda for RAG Integration
1. Navigate to **AWS Lambda**.
2. Click **Create function**.
3. Select **Author from scratch**.
4. Name the function (e.g., `Lex-Bedrock-RAG-Handler`).
5. Choose **Python 3.x** as runtime.
6. Attach IAM permissions for Bedrock, S3, and Lex.
7. Add the following code:
   ```python
   import json
   import boto3

   bedrock = boto3.client('bedrock-runtime')

   def lambda_handler(event, context):
       user_input = event['inputTranscript']
       response = bedrock.invoke_model(
           modelId='anthropic.claude-v1',
           contentType='application/json',
           accept='application/json',
           body=json.dumps({"prompt": user_input})
       )
       return {
           'messages': [{'contentType': 'PlainText', 'content': response['body']}]
       }
   ```
8. Save and deploy.

## Step 6: Integrate Lex with Lambda
1. Go to **Amazon Lex** > Your chatbot.
2. Select **Lambda initialization and validation**.
3. Choose the Lambda function created earlier.
4. Save and rebuild the bot.

## Step 7: Test the Chatbot
1. Go to **Amazon Lex Console**.
2. Select **Test Chatbot**.
3. Enter sample inputs and verify responses.

## Step 8: Deploy on a Web Interface (Optional)
- Use AWS Amplify or an S3-hosted static website to integrate the chatbot with a frontend.

## Upload Screenshots
![S3 Bucket steup](https://github.com/user-attachments/assets/db21312a-cc2a-499d-9535-bb3b11e63bca)
![Bedrock knowledge base stepup after synced](https://github.com/user-attachments/assets/c8af5ba5-e80d-4a89-b429-86d6675dc487)
![Bot without Generative Ai](https://github.com/user-attachments/assets/0610910d-8fc4-44f8-8fe5-e22c73bf3e7a)
![Bot with Generative Ai ](https://github.com/user-attachments/assets/478a1dd1-1a46-497c-a119-68fc024aaaee)
