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
