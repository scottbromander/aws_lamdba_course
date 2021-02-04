

### Amazon Simple Email Service (SES)
- Inbound Mail
- Outbound Mail

##### Benefits of SES
- High Deliverable
- Sender Statistics
- Control Flow
- Monitoring

### Amazon S3
- Released in 2006
- Text, Audio, Code, many other files
- Stored in Buckets, stored in Objects
- Folder Prefixes

##### Benefits of S3
- Scalability
- Integration (Especially with other AWS Services)
- Simplicity
- Availability

### Creating an S3 Bucket in Command Line
- `aws s3 mb s3://whatever-bucket-name` - Remember, this is a global name, so it needs to be original across the world. Also, baller, I got "whatever-bucket-name"!
- `aws s3 ls` - Lists all buckets across your AWS account
- `aws s3 cp ./templates s3://whatever-bucket-name --recursive` - Hey AWS S3, Copy the Templates folder, to the S3 Bucket, and Copy Recursively. 
- `aws s3 ls whatever-bucket-name` - Show everything that is in the bucket.

### Simple Email Services
- Go to the  `Simple Email Service` from the Console
- Click on `Email Addresses` on the left hand bar
- Click on `Verify a New Email Address`
- Enter Email Address
- Confirm verification in email (Like, actually go to the email client and click on the verification link in the email sent from AWS)
- You will want 3 total for this example.

### General Notes on how things work
- The templates are sitting in the S3 bucket. 
- Boto3 is going to connect to S3 to grab those templates out of the bucket.
- Inside the templates (like `come_to_work.html`) you will see things like `{{first_name}}`. Those will be populated with their related variables. Lines like: `html_email = template.render(first_name = employee_first_name)` handle that.

### IAM Roles
- Go to `IAM`
- Go to `Roles`
- `Create Role`
- Select `Lambda`, then select `Next: Permissions`
- Search `SES`, select the `Full Access Policy`, could go in and make one,
- Search `S3`, select `Full Access`, only for upgrade purposes. `Read only` could be good too.
- Search `Lambda`, then select `AWSLambdaBasicExecutionRole` - You could do specific Lambda Functions, but we have not created them yet. So this is enough for now, and that will give us what we need for CloudWatch.
- Next past tags, 
- Next to reviews.
- Role name: `cuckoo_role`

### Testing and Integrating Cuckoo
- `python3 -m venv .venv`
- `source .venv/bin/activate`
- `pip install Jinja2`
- `pip install boto3` - Remember, just locally
- `python3`
- `import cuckoo`
- (Note, I needed to update the `cuckoo.py` files `print` statements to include `()` wrapping the statements. The import statement errored without them. Which means that I needed to tear down `.venv` and then rebuild with the updates to the code.

```
event = {
  "id": "cdc73f9d-aea9-11e3-9d5a-835b769c0d9c",
  "detail-type": "Scheduled Event",
  "source": "aws.events",
  "account": "123456789012",
  "time": "1970-01-01T00:00:00Z",
  "region": "us-east-2",
  "resources": [
    "arn:aws:events:us-east-2:123456789012:rule/ExampleRule"
  ],
  "detail": {}
}
```
