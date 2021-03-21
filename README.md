s3cmd role
==========

Install and configure s3cmd for root user

AWS S3 confiuration
-------------------

 * Bucket configuration example (terraform)
 ```terraform
resource "aws_s3_bucket" "devel_backup_n1" { 
   bucket = "devel-backup-n1"
   acl = "private"
   versioning { 
      enabled = true
   } 
   tags = {
     Name = "DEVEL - backup n1.example.com"
     Environment = "devel"
     Comment = "Bucket for backups"
   }
}
 ```

 * Policy example 
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowsToListBackupBuckets",
      "Effect": "Allow",
      "Action": [
        "s3:ListAllMyBuckets"
      ],
      "Resource": [
        "arn:aws:s3:::backup*"
      ]
    },
    {
      "Sid": "AllowsToDoS3sync",
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:PutObject",
        "s3:PutObjectAcl"
      ],
      "Resource": [
        "arn:aws:s3:::backup-env-mybucketname",
        "arn:aws:s3:::backup-env-mybucketname/*"
      ]
    }
  ]
}

```



Role Variables
--------------

A description of the settable variables for this role are available in defaults/vars.yml


Example Playbook
----------------

```yaml
- hosts: cluster
  roles:
  - role: ans-s3cmd
    tags:
    - backup
    vars:
      s3sync_access_key: YYYYXXX
      s3sync_secret_key: YUNKNUIWJ8NWIJKMX
      s3sync_conf:
        bucket_location: eu-central-1
```

License
-------

BSD

Author Information
------------------
msirovy@gmail.com
