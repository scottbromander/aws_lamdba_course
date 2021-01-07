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

