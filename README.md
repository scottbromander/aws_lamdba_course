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
- `IAM Policy` -> Effect: `Allow` -> `SES` -> Actions: `SendEmail`, `SendRaw` -> Include an ARN as well. `*` works. Add statement, then `Generate Policy` (just as an example!) - Cranks out the JSON.

## Considerations and Limitations

- Runtime - Not just the lanuage, but the version of the language.
- AWS Managed Languages - Node, Python, Ruby, Java, Go, .NET Core

### Size Limitations

- < 250 mb, uncompressed code and dependancies
- < 50 mb, compressed function package
- < 75 gb, total function packages within a region

### Workarounds

- Smaller libraries
- Microservice Architecture
- Other runtime options, maybe another language/runtime might be smaller. Java tends to have higher resource loads.

### Resource Limitations

- Ephemeral storage < 512 mb
- Maximum execution duration of < 900 seconds
- 1000 concurrent languages

### Workarounds

- Load and store files in S3
- Chain functions together
- Ask AWS about increasing limits

### Memory, CPU, and VPC

- Memory - 128 mb - 3 gb
- CPU scales with Memory
- Consider Virtual Private Cloud

### AWS Lambda Considerations

- Event Driven Code
- Code and Size Limitations
- Lamdba as a component
- Performance limitations
- Long running workloads

## Lamdba Prerequisites

### Gather Dependancies

- Libraries - Additional dependancies will need to be bundled up with your Lamdba
- Other Files - Same as above, needs to be bundled together
- Other AWS Services - Does it require interaction with the API Gateway - Permissions may need to be granted
- API Keys - Does it have all the API and Configurations needed for your Function

- Can write locally
- Can write in AWS Lamdba editor

### Creating a Function Package

- ZIP archive, of all your function information, uploaded to the AWS console, or using the AWS CLI

### Debugging

- Use logging with CloudWatch
- Be aware of Environment Differences
- Search for your issues
- Consider 3rd Party Tooling

### Creating a Lanbda with a Blueprint

- Go to Lambda, `Create Function`, `Use a Blueprint`, Search `Canary`, Select `LambdaCanary` with Python 3.7, Click `Configure`
- Now we will start the process of creating and configuring the function. Name it something like `lambda-canary`, and go ahead and leave the default of Create a New Role with Basic Lambda Permissions. The default does allow to send logs to CloudWatch, which is what we want.
- Because it is a Blueprint, it already knows that we are working with a CloudWatch events trigger.
- In the dropdown for the Rule, select `Create a New Rule` - Rule name: "Canary" - Frequency: 5 mins.
- Let's use rate expression, `rate(5 minutes)`
- Note: Event patterns are neat, you can trigger events that come from other services like EC2, RDS, S3, or Step Functions or Health events.
- Don't enable quiet yet. Wait until everything is set up first.
- Go ahead and check out the code already created, but no need to adjust at this point. 
- Check a specific site, let's do `https://www.aftercamp.io/` and for `EXPECTED` to some text it expects to see. In this case, let's do `DAILY GROWTH`.
- Click `Create Function`
- File Name >>> `lambda_function.lambda_handler` <<< Function inside the File

### Running a Test

- Click on the `Test` button in the upper right of the page,
- Because we set up this from a blueprint with a CloudWatch event, all of that information is prepopulated,
- Name the event, `testevent1` (or whatever)
- Click on `Create`, 
- Then click `Test` again!
- Note that `DAILY GROWTH` failed, but we updated the environmental value to be `Aftercamp`, and it passed.
