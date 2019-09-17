# aws2fa
Write a 2fa sts session token for awscli to ~/.aws/credentials. 

Works with [this IAM Policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_examples_aws_my-sec-creds-self-manage.html).

## Getting Started 
Instructions are for Mac OSX and assumes you have homebrew.

This tool only supports Virtual MFA Devices like Duo Mobile and Google Authenticator

### Installation
```
brew install python
pip3 install awscli boto3
cp aws2fa /usr/local/bin/
chmod +x /usr/local/bin/aws2fa
```

### Usage
```
$ aws2fa --help
usage: aws2fa [-h] [-p PROFILE] [--mfa MFA] [--mfa_code MFA_CODE]

optional arguments:
  -h, --help            show this help message and exit
  -p PROFILE, --profile PROFILE
                        non-STS profile (default is default)
  --mfa MFA             Virtual MFA device arn, omit for a device list
  --mfa_code MFA_CODE   Code from MFA device
```

Get a list of assigned MFA devices
```
$ aws2fa --profile username
No saved MFA device found
Assigned virtual MFA devices. Pass one of these to aws2fa with the --mfa option
['arn:aws:iam::1234567890:mfa/username']
```
Store the MFA arn
```
$ aws2fa --profile username --mfa arn:aws:iam::1234567890:mfa/username
MFA device is assigned but no code provided.
Pass the code from your MFA device with --mfa_code
```
Store an STS session in ~/.aws/credentials for use with awscli
```
$ aws2fa --profile username --mfa_code 123456
Session token written successfully to profile username_sts for 43200 seconds
$ aws s3 ls --profile username_sts
```