# AWS Lamdba - Pluralsight Course

## Understanding Serverless Functions 

### Evolution of Serverless Functions

- Used to have manage everything. Bare metal, to code, and so on.
- Virtualization and Hypervisors - Allowed running of multiple OS's on one machine.
- The Cloud - Amazon EC2 - Logical extension of virtualization. Renting of virtualization. Got to manage virtual machines, rather than physical ones.
- Serverless Functions - You as a developer do not think as much, about the servers running code. 
  - Event driven, responding to some event (file upload, api request, etc.)
  - Code focused (not infastructure). Focus more on writing business logic, not the infastructure.
  - Run on managed machine! 
  - Focus on the code, none of the rest.

## Event Examples

- File Upload (.png gets uploaded to Amazon S3) - Lambda resizes
- Scheduled Time - Triggered from something like a cron job.
- API Request - Comes in from something like AWS API Gateway

### Serverless Benefits
- Cost and Utilization
- Managed machines
- Service integration
- Scaling (up and down)

### Serverless Challenges
- Debugging - Becomes more difficult than something like an EC2 instance
- Lower level control
- Cutting edge quirks (especially in niche cases)

## Why Learn Lambda

### As a Developer
- Managed infastructure!
- Internet of things!
- Growing relevance! Almost 4x more relevant in Google Searches than Azure Functions, Google Cloud Functions, and Open Whisk, combined.
- AWS ecosystem is still about 3x more relevant than the next 2 competitors combineed.

### How is Lambda used?
- Stream data processign
- Easy and scalable APIs
- Photo processing
- Web applications (tons of use cases here!)
- Just a example subsets!

### Serverless Function Providers
- Amazon Web Services - Market leader.
  - Node, Python, Java, C#, Powershell, Ruby, Go, User-provided runtime,
  - Built-in versioning for serverless functions,
  - HTTP endpoints via API Gateway,
  - 15 minute running time limit,
  - 1000 concurrent functions (soft limit)
- Microservice Azure - Catching up rapidly!
  - Node, Python, Java, C#, Powershell, F#, PHP, batch, bash, other executables,
  - Currently no built-in versioning,
  - HTTP endpoints via API Management,
  - 10 minut limit (option for unlimited),
  - 10 concurrent instances
- Iron.io - One of the first to come up the idea that became Serverless Functions. Run time limits of an hour. Main drawback, integrations are lacking.
- Cloudflare - Run JavaScript code close to the users. Respond to API hits closer to the user (AWS has Lambda at Edge).
- OpenFaaS - One of several options to get Serverless function to open source.

## AWS Free Tier - 12 Months of Free

### Some Free Services
- EC2
- SES - Simple Email Service
- Lamdba
- RDS 
- DynamoDB
- S3 - Simple Storage Service

### Free Tier Examples
- EC2 - 750 Hours
- Lambda - 1 million functions per month
- RDS - 20 GB
- DynamoDB - 25 GB
- S3 - 5 GB

### IAM Policies

Sample:

JSON

```
// This is a SUPER broad policy - Good example as simple, but not in practice
{
  "Version": "2012-10-17", // Tracks policy version
  "Statement": [
    {
      "Effect": "Allow", // Allow or Deny
      "Action": "*", // What actions can be taken?
      "Resouce": "*" // On what AWS resource
    }
  ]
}
```

### IAM Best Practices

- Strong Password
- 2FA - Two Factor Authentication
- Principle of Least Priviledge
- Never use your Root Account!

### IAM

- Create Policy 
- Could create your own custom policy.
- But instead, you could use a `AWS Managed Policy`! `Policies` on the left hand side, you can filter.
- Could also use `Policy Generator`. Could Google, or go to [aws policy generator](http://awspolicygen.s3.amazonaws.com/policygen.html) - Makes it a little easier to create your own custom policy.
- `IAM Policy` -> Effect: `Allow` -> `SES` -> Actions: `SendEmail`, `SendRaw` (just as an example!)
- 
