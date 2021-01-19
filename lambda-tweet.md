## Lamdba Tweets

### API Key Management Options
- AWS Systems Manager Parameter Store
- KMS - AWS Key Management Service ($$$)

### AWS Systems Manager
- From console, go to AWS Systems Manager
- `Parameter Store` on the left hand toolbar
- Check yo region!
- Click on `Create Parameter`
- Name and Description - `CONSUMER_KEY` - `Twitter Consumer Key`
- Select `SecureString` - KMS will encrypt the string to make it more secure.
- You could select another account, but lets select `My current account` and select the KMS Keys ID associated with `alias/aws/ssm`.
- Paste in your Consumer API Key into the `Value`.
- Click on `Create Parameter`.

### Sparrow
The link for this project is below. It is used for the course:

[Github Link to Project](https://github.com/scottbromander/sparrow)

### IAM 
- Create a new IAM group.
- I called mine `cliadmin`.
- We are going to grant this new group the `AdministratorAccess` policy.

_(Note that the course is now covering the best practice of not using the root user)_
- Going over to Users, and going to create a new user. Also called `cliadmin`.
- In this case, we will grant this user both `programmatic access` as well as `AWS Management Console Access`
- I am going to use a custom password, but that is up to you.
- Since we are creating this user for ourselves, we are going to uncheck the requirement to do a password reset.
- Next, we will associate the user ot the above policy we created. 
- `Next`, `Next`, and then `Create User`

### AWS CLI
- In terminal, `aws configure`
- It will ask you for the access key you created, then your secret
- Select your region (for me: `us-east-2`)
- Go ahead and leave default output to `none`
