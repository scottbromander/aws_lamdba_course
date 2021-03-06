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

### Boto3
- Install Boto3, `pip3 install boto3`

### Scripts
Here is a cool set of python scripts to set the values in the Parameter store in SSM.

```python
import boto3

def get_secret(parameter_name):
    """Get a parameter from SSM Parameter store and decrypt it"""
    ssm = boto3.client('ssm')
    parameter = ssm.get_parameter(
        Name=parameter_name,
        WithDecryption=True
    )['Parameter']['Value']
    return parameter


def put_secret(parameter_name, parameter_value):
    """Put a parameter inside SSM Parameter store with encryption"""
    print('Putting a parameter with name of ' + parameter_name + ' into SSM.')
    ssm = boto3.client('ssm')
    ssm.put_parameter(
        Name=parameter_name,
        Value=parameter_value,
        Type='SecureString',
        Overwrite=True
    )
    print("Successfully added a parameter with the name of: " + parameter_name)

# Example of using put_secret() to add your keys
# SECRETS = {
#     "CONSUMER_KEY": "REPLACE_ME",
#     "CONSUMER_SECRET": "REPLACE_ME",
#     "ACCESS_TOKEN_KEY": "REPLACE_ME",
#     "ACCESS_TOKEN_SECRET": "REPLACE_ME"
# }
for parameter_name, parameter_value in SECRETS.items():
    put_secret(parameter_name, parameter_value)
```

Note that you would want to set the values of the `SECRETS` dictonary. But certainly would not want to commit that to GitHub or anything. 
- Run `python3` to enter Python,
- Run line group by line group,
- Import boto3,
- Put the two `def`s in,
- Build the dictonary,
- Run the for loop

### Modules
- `python3 -m venv .venv` - Creates a virtual environment called '.venv',
- This uses a virtual environment, versus a global environment,
- `source .venv/bin/activate` - "Turns on" the virtual environment,
- `pip3 install twython` - Installs trython into the virtual environment,
- `pip3 install boto3` - Installs boto3, for the sources file, although lambda will provide it later,
- `pip3 freeze` - Shows installed packages

### Testing
- Open up `python3` and copy and paste code to try things out!
- Do the imports, the Tywthon secrets code, load all the defs
- Hey! Follow me! `follow_someone("docix")`
- `deactive` turns off the virtual environment.

### Building a Lambda Function Package
- `rm -r .venv`
- `rm -r __pycache__`
- `python3 -m venv .venv`
- `source .venv/bin/activate`
- `mkdir setup`
- `cp sparrow.py setup`
- `cp ssm_secrets.py setup`
- `cd setup`
- `pip install -r ../requirements.txt -t .` - This runs the install on the dependancies listed in the requirements.txt file. Remember boto3 is provided by the Lambda runtime.
- (will be different for PC, but on Mac) - `zip -r ../package.zip ./*`
- `cd ..`
- `rm -rf ./setup`
- `deactivate`
- `rm -rf ./.venv`

### Deploying Lambda Package and Testing
- Over in the AWS Console, select `Lambda`
- Select `Create Function`
- `App from Scratch`
- Pick `python 3.7` runtime
- We are going to need access to AWS SSM Parameter Store, as well as KMS, because of the encrypted params. So we are probably going to need a custom policy.
- Select `Create a new from AWS Templates`
- KMS is preconfigured, SSM is not. Select the KMS policy. (Just kidding, KMS is not in there anymore)
- For now, just name the function (`sparrowFunction`)
- Select `create function`
- From the main console, go to IAM, the `roles`, and find the `sparrowRole`. Click dat.
- Attach Policy -> SSM -> Read Only Policy
- (Note that I made a new policy for KMS, read only access, by creating a policy)
- Back in the Lambda, in the Configuration Section, click on `Add Trigger`
- Select `Event Brudge` - Cloud Watch events trigger
- Add a description
- Schedule expression - `rate(1 day)`
- Click `add`
- Now click on the Lambda,
- Right hand side of the code editor, select `Actions`
- Select `Upload a .zip file`
- Find the package file.
- Select `save`
- Head down to Runtime Settings and click on `Edit`. Update the handler to read `sparrow.handler`, Sparrow being a reference to the file, and Handler being a reference to the function inside of that file. 


### Testing our Function
- We should be all good to go, but let's configure a test event
- From the main Lamdba page, at the top, next to the test, lets click the drop down `Select a Test Event`, then select `Configure Test Events`, or we can click on `Test`, which we will do.
- This opens up a popup window,
- From the event template, select `cloudwatch-scheduled-event`,
- Name it `dailytest`,
- Click `create`
- Click `test`
- Note that if you get an error, its probably because the `put` code in `ssm_secrets` is still active. Make sure to comment out the loop and click `deploy` above the Lambda editor.
