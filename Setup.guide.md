Step1. Create a Lambda Function
=
1. Go to AWS Console → Lambda → Create function.

2. Choose:

Author from scratch

Function name: Lambda-canary-Project

Runtime: Python 3.x or Node.js 18

3. Add sample code (example in Python):
 import json

       def lambda_handler(event, context):
       # TODO implement
            return {
               'statusCode': 200,
               'body': json.dumps('This msessage come from Development server')
                  }
   <img width="1908" height="782" alt="image" src="https://github.com/user-attachments/assets/1a8ec965-cd7d-4490-8d5e-8043a79fe91e" />
   Step2. Create an API Gateway
   =
1. Go to Amazon API Gateway → Create API →REST API.

2. Configure:

       API Name: API-Canary-Project

       Integration: Select Lambda Function → Lambda-canary-Project.
3. Click Deploy → Choose Stage: prod.

You now have an endpoint like:

    https://j687im2j4m.execute-api.ap-south-1.amazonaws.com/dev
  <img width="1890" height="747" alt="image" src="https://github.com/user-attachments/assets/f6ab1ebb-4470-437c-9ad6-0fc93f0feede" />
  <img width="1912" height="406" alt="image" src="https://github.com/user-attachments/assets/bb09ae26-ae9b-43fa-8b0d-b4022776b3cc" />

  Step3. Enable Lambda Versions & Aliases
  =
1. Go to Lambda → Lambda-canary-Project.

2. Click Publish new version → Name it v1.

3. Create an Alias (e.g., prod) pointing to version v1.

Step 4. Canary Deployment Setup
=
A Canary deployment allows you to gradually shift traffic from one Lambda version to another.

1. In Lambda → Aliases → prod.

2. Click Edit traffic shifting.

3. Choose:

       Additional version: v2 (publish new version after code update).

       Traffic shifting: e.g., 10% to v2, 90% stays on v1.
4. Save changes.

Now requests hitting API Gateway → Lambda prod alias will be split between v1 and v2.
<img width="1902" height="846" alt="image" src="https://github.com/user-attachments/assets/2ba5b35c-228a-4810-896c-cae5b297c9fa" />

Step 5. Test the Setup
1. Update Lambda with new code, publish as v2.

import json

    def lambda_handler(event, context):
      # TODO implement
         return {
            'statusCode': 200,
                'body': json.dumps('This msessage come from Production server')
                }
2. Invoke API Gateway endpoint:

       curl https://<api-id>.execute-api.<region>.amazonaws.com/prod/
Some responses will show v1.

Some will show v2 (based on canary %).
<img width="1413" height="392" alt="image" src="https://github.com/user-attachments/assets/662c962d-0578-4a82-b90f-31b2958341d5" />



