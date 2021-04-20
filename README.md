# aws-lambda-resize

## How to use Lambda

### Create IAM Policy

```javascript
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:PutLogEvents",
                "logs:CreateLogGroup",
                "logs:CreateLogStream"
            ],
            "Resource": "arn:aws:logs:*:*:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::mybucket/*" // change mybucket
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::mybucket-resized/*" // change mybucket
        }
    ]
}         

- skip Tags:
- skip Review:
- Name: AWSLambdaS3Policy
- Create Policy
        
```

### Create the execution role

- Head to the Roles page
- Create a role
  - Trusted entity – Lambda
  - Permissions policy – AWSLambdaS3Policy
  - Role name – lambda-s3-role

### Create The Lambda Function

- Head to AWS Lambda
- Create function
- Name function `CreateThumbnail`
- Set the following settings
  - Runtime: Node.js 14.x
  - Handler: index.handler
  - Upload a deployment package
    - zip file containing your lambda function within a `index.js` file along with your package.json and node modules.

### Create 2 Buckets

- Create 2 buckets:
  - One named `filename`
  - The other should have the same `filename` followed by `resized`
  - example: bucket 1 = `images` bucket 2 = `images-resized`
- Head to your main bucket to the **Event Notifications** section and click **Create event notification** with your newly build lambda function

## Issues I encountered

- There were so many solutions to choose from that were available on google. I eventually found a perfect solution by reconfiguring my search.

- Initially getting used to the GUI. After some time, the process became much easier and I knew exactly where I needed to go and where to go for debugging and testing!

- aws commands in the terminal were not working out. So I figured that I can manually go into the AWS console VIA Browser and create the function and tests there. Worked out fine!

## Results

- I am able to upload images to the main bucket named `jddimage`. A lambda function is triggered upon a new creation of an object which then takes the uploaded image, resizes it to 50px and uploads the newly resized imaged to a DIFFERENT bucket named `jddimage-resized`

![Buckets](./Buckets.png)
![Original](./Original.png)
![Resized](./Resized.png)
