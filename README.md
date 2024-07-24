#### Create Metadata Token and set expiration. v2
```
curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 600"
```
#### Sent request to get metadata v2
```
curl -H "X-aws-ec2-metadata-token: AQAAAHtmIhZm3BBISjnfQrZkUqq29KOmQZhy7L9j7GjYYjtYFwihow==" -v http://169.254.169.254/latest/meta-data/
```
#### Access Metadata v1
```
curl http://169.254.169.254/latest/meta-data/instance-id
```
#### Get Security keys from role
```
curl -H "X-aws-ec2-metadata-token: AQAAAGOFIDlf1xs" -v http://169.254.169.254/latest/meta-data/iam/security-credentials/ec2-s3-role
```
#### Role assumed for ec2 service
```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Principal": {
				"Service": "ec2.amazonaws.com"
			},
			"Action": "sts:AssumeRole"
		}
	]
}
```
#### Role assumed for ec2 user and IAM user
```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Principal": {
				"Service": "ec2.amazonaws.com",
				"AWS": "arn:aws:iam::082897113543:user/blue"
			},
			"Action": "sts:AssumeRole"
		}
	]
}

```
#### Role assumed for just IAM user
```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Principal": {
				"AWS": "arn:aws:iam::fujis-account-id:user/blue"
			},
			"Action": "sts:AssumeRole"
		}
	]
}
```
#### Inline policy for user to assume the role
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::082897113543:role/S3FullAccessRole"
        }
    ]
}

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::anittas-account-id:role/S3FullAccessRole"
        }
    ]
}
```
#### Get access keys
```
aws sts assume-role --role-arn arn:aws:iam::082897113543:role/S3FullAccessRole --role-session-name mysession --profile=blue
```
#### Edit aws cli config credentials to use the new role
```
[s3assumerole]

source_profile = blue
role_arn = arn:aws:iam::082897113543:role/S3FullAccessRole

```
#### list using new role
```
aws s3 ls --profile=s3assumerole
```
