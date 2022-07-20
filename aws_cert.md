# AWS

## IAM

1. A Global service.
2. Create users and assign them with Groups.
3. Can also create user without assigning any group (not best pranctice)
4. Groups only contains users, not groups.
5. A user can be a part of multiple group.
6. Users or Groups can be assigned JSON doucments called policies

`Account Alias` for creating a alias for `AccountID` for ease of login

`inline` policies are the policies attached to user not group.

To give a policy to a user without group select `Attach existing policies` option.

To give admin `IAM Users` the `Billing Management` permission, log in as a root user, on top right username select `My Account`, scroll down to find `IAM User and Role Access to Billing Information` click edit and check `Activate IAM Access`.

### Security

Security - Password or `Multi Factor Authentication (MFA)`

Account Settings -> Change Password Policy

### Setup AWS CLI

1. Install aws cli for the os

```powershell-interactive
$ aws configure
# this command will asks for the following data

AWS Access Key ID [None]:

AWS Secret Access Key [None]:

Default region name [None]:
# check the region code in aws console top right drop down for selecting region

Default output format [None]:
# output format is optional
```

> Dont do the above method inside a ec2 to access anothe aws services like uploading to S3 etc, someone can retrive the details from aws config file inside the .aws dir.
> Instead use `IAM Roles`

To list all the users (only get response if the user has the permission)

```powershell-interactive
$ aws iam list-users
```

### CloudShell

`AWS CloudShell` - An alternative to using terminal to issue commands against terminal.

1. Its not available in all regions.
2. CloudShell provides a terminal in aws cloud
3. All the files in u created will stay even after restart
4. Download and upload also possible (top right `Action` button)

### IAM Roles

IAM Roles will be just like a user but they are intended to be used not by physical people but instead they will be used by `AWS Services`

Some common roles are `EC2 instance Roles, Lambda Function Roles, Roles for CloudFormation etc.`

Inside `Roles -> Create role`
Create a role and attache the policy you want

> Eg: Create a role for ec2 and attach any policy you want. Attach this role into aleady created ec2 instance to provide it with credentials.
> Select the instance, `Actions -> Security -> Modify IAM role`, choose the IAM role you created.

### IAM Security Tools

1. `IAM Credentials Report (account-level)` - a report that lists all your account's users and the status of their various credentials.
   To generate a credentail report
   `Credential report -> Download Report`

2. `IAM Access Advisor (user-level)` - Access advisor shows the service premissions granted to a user and when those services were last accessed. You can use this information to revice your policies.
   `Users -> (select the user) -> Access Advisor`

## EC2 - Elastic Compulte Cloud

It mainly consists in the capability of:

1. Renting virtual machines (EC2 Instances)
2. Storing data on virtual drives (EBS)
3. Distributing load across machines (ELB)
4. Scaling the services using an auto-scaling group (ASG)

### EC2 User Data

`Bootstrapping` means launching commands when a machine starts. The script is only run once at the instance first start.

The EC2 User Data script run with the root user.

To add the script when launching an EC2 instace `Advance Settings -> User data`

If you stop an instance and start later, aws may change the public ipv4 address.

When terminating an instance, by default it deletes EBS volume.

To compare the instance types https://instances.vantage.sh/

### Security Groups (Firewalls)

1. Controll the traffic into or out of the ec2 instances
2. Security groups rules can reference by IP or by securtiy group
3. Can be attached to multiple instances (Also an instace can have multiple security groups too)
4. Locked down to a region / VPC combination (If you switch to another region you have to create a new security group)
5. > It is good to maintain one seperate security group for SSH access
6. By defauld all inbout is blocked and all outbound are allowed
7. "timeout" error means security group issue, "connection refused" error means application error or it's not launched.

### SSH

```powershell-interactive
$ chmod 0400 file.pem
$ ssh -i file.pem user@public_ip
```

> `Ec2 Instace Connect` inside connect option in ec2 instace, will help to connect to the instace without a termianl, it start a terminal in browser.

## EBS (Elastic Block Store)

1. It's a network store you can attach to your instances while they run
2. It allows instance to persist data, even after their termination, (by default root EBS volume deletes when termination, but not the other attached volumes)
3. > Only be mounted to one instace at a time, (`multi-attach`) features are available for some EBS
4. They are bounted to a specific `availability zone`. To move a volume across, you first need to `snapshot` it.

To Access - Select EC2 instanc, under the `Storage` tab, select `Block Devices`.
or under `Elastic Block Store` section selects `Volumes`

To Create - Select `Volumes` under `Elastic Block Store` section, click `Create volume` button. When selecting the `Availability zone`, should choose the same one where the ec2 instance is.

To Attach - Select the volume and select `Actions` -> `Attach volume`, then select the instance.

### EBS Snapshot (Backup)

1. Not necessary to detach volume to do snapshot, but recommended
2. Can copy snapshot across AZ or Region
3. Features: EBS Snapshot Archive, Recycle Bin for EBS Snapshots (1day to 1year)

To Access - `Elastic Block Store` -> `Snapshots`

To Create a Snapshot - select `Volumes` under `Elastic Block Store`, select the volume and select `Actions` -> `Create snapshot`

To Copy a snapshot to another region, right click on the snapshot and select `Copy snapshot` option (also available in `Actions`).

To create a volume from snapshot - `Actions` -> `Create volume from snapshot`

To Setup `Recycle Bin` (top right of `Snapshots` section) - `Create retention rule`, select `Resource type` then click create button.
Under the `Resources` section you can see the deleted snapshots, (you can `Recover` it from here)

### EBS Volume Types

(check the course slide, lecture: 51)

### EBS Multi-Attach - io1/io2 family

1. Only for io1/io2 family (EBS Volume type)
2. Attach the same EBS volume to multiple EC2 instance in the `same AZ`
3. Each instance has full read & write permissions to the volume
4. Use case:
   1. Archieve `higher application availability` in clustered Linux applications (ex: Teradata)
   2. Applications must manage concurrent write operations
5. Must use a file system that's cluster-aware (not XFS, EX4, etc...)

## AMI (Amazon Machine Image)

1. AMI are a customization of an EC2 instance (you add your own software, configuration, os, monitoring, etc)
2. AMI are built for an `specific region` (and can be copied across regions)
3. You can launch EC2 instances from:
   1. A Public AMI: Aws provided
   2. Your own AMI: You make and maintain then yourself
   3. An AWS Marketplace AMI: an AMI someone else made (and potentially sells)
4. Creating an AMI from an EC3 instance
   1. Starat an EC2 instance and customize it (install the basic softwares etc)
   2. Stop the instance (for data integrity)
   3. Build an AMI - this will also create EBS snapshots (right click the instance -> `Image and templates` -> `Create image` , to see the created AMI, left side -> `image` -> `AMIs`)
   4. Launch instances from other AMIs (either `AMIs` page -> `Launch instance from AMI` or from `instaces` -> `Launch instance` -> when selecting the AMI select `My AMIs` tab and choose the created AMI )

## EC2 Instance Store

1. EBS volumes are `network drives` with goot but limited performance
2. If you need a high-performance hardware disk, use EC2 Instance Store
3. Better I/O performance
4. EC2 Instance Store lose their storage if they're stopped (`ephemeral`)
5. Good for buffer / cache / scratch data / temporary content
6. Risk of data loss if hardware fails
7. Backups and Replication are your responsibilty

## EFS (Elastic File System)

1. Managed NFS (network file system) that can be mounted on many EC2
2. EFS works with EC2 instances in `multi-AZ`
3. Highly available, scalable, expensive (3 * gp2), pay per use
4. EFS file system are surrounded by `Security Group` (Uses security gruop to control access to EFS).
5. Use cases: content management, web serving, data sharing, Wordpress
6. Uses NFSv4.1 protocol
7. > Compatible with Linux based AMI (not Windows)
8. POSIX file system (~Linux) that has a standard file API
9. File system scales automatically, pay-per-use, no capacity planning

### EFS Performance & Storage Classes
