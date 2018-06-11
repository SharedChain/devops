# Using EC2 Roles to secure non-AWS credentials

## Create secure bucket

## Create Policy

```{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::<your bucket>/<your file path>",
            "Condition": {
                "IpAddress": {
                    "aws:SourceIp": "172.31.80.0/20"
                }
            }
        }
    ]
}```

## Create Role using Policy

## Attach Role to EC2

## Access S3 resource w/o credentials

