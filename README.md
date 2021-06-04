### Set long term user and policy

#### Connect minio client with server by docker
```bash
docker run --rm -it --entrypoint=/bin/sh minio/mc
mc alias set minio <MINIO_ENVPOINT> <MINIO_ACCESS_KEY> <MINIO_SECRET_KEY>
```

#### Set policy public bucket
1. Write Bucket Policy Following `StorageBucketPolicy.json` file
2. Apply Policy
```bash
mc policy set-json StorageBucketPolicy.json minio/<bucket_name>
```

#### Create User and Group
1. Create User
```bash 
mc admin user add minio <MINIO_USER_ACCESS_KEY> <MINIO_USER_SECRET_KEY>
```

2. Create group and add user to group
```bash
mc admin group add minio <new_group> <new_user>
```

#### Create Policy for user

1. Write Policy following AWS S3 policy
```bash
cat > StorageSITCareersAPIRole.json << EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObjectVersion",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::sit-careers/*",
                "arn:aws:s3:::sit-careers-dev/*"
            ]
        }
    ]
}
EOF
```

2. Create new policy
```bash
mc admin policy add minio <policy_name> <policy_file>
```

3. Apply policy with user or group
```bash
mc admin policy set minio <policy_name> user=<new_user>
or
mc admin policy set minio <policy_name> group=<new_group>
```


Ref:
- Minio Bucket Policy: https://docs.min.io/minio/baremetal/reference/minio-cli/minio-mc/mc-policy.html
- Minio: https://docs.min.io/docs/minio-multi-user-quickstart-guide.html
- How to set policy: https://github.com/minio/minio/issues/5279
