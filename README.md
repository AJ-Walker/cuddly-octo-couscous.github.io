# AWS Systems Manager (SSM) - EC2 Setup Guide

This guide walks you through setting up AWS Systems Manager Session Manager to connect to your EC2 instances without SSH keys or bastion hosts.

## Prerequisites

- AWS CLI installed and configured
- Appropriate AWS permissions to create IAM roles and EC2 instances

## Step 1: Create EC2 IAM Role

1. Navigate to the IAM console in AWS
2. Create a new IAM role for EC2 service
3. Attach the following managed policy:
   - `AmazonSSMManagedInstanceCore`

This policy provides the necessary permissions for the SSM agent to communicate with the Systems Manager service.

## Step 2: Launch EC2 Instance

1. Launch a new EC2 instance
2. In the **Instance Profile** section, select the IAM role created in Step 1
3. Ensure the instance is in a subnet with internet access (for SSM communication)

> **Note**: The SSM agent comes pre-installed on most modern Amazon Linux, Ubuntu, and Windows AMIs.

## Connecting to EC2 Instance

There are two ways to connect to your EC2 instance using SSM:

### Method 1: AWS Console (Web-based)

1. Navigate to the EC2 console
2. Select your instance
3. Click **Connect**
4. Choose **Session Manager** tab
5. Click **Connect**

### Method 2: Local CLI

#### Prerequisites for CLI Method

Before using the CLI method, ensure you have:

1. **AWS CLI installed and configured**
   - Installation guide: [AWS CLI Installation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
   - Configuration guide: [AWS CLI Configuration](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-quickstart.html#getting-started-quickstart-new)

2. **SSM Session Manager Plugin installed**
   - Installation guide: [Session Manager Plugin Installation](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html)

#### Verify SSM Plugin Installation

Run the following command to verify the SSM plugin is installed:

```bash
session-manager-plugin
```

Expected output:
```
The Session Manager plugin is installed successfully. Use the AWS CLI to start a session.
```

#### Connect to Instance

**Basic command:**
```bash
aws ssm start-session --target <instance_id>
```

**With specific region and profile:**
```bash
aws ssm start-session --target <instance_id> --region <aws_region> --profile <profile_name>
```

### Example Usage

```bash
# Connect to instance in default region/profile
aws ssm start-session --target i-1234567890abcdef0

# Connect with specific region and profile
aws ssm start-session --target i-1234567890abcdef0 --region us-west-2 --profile production
```

## Troubleshooting

### Common Issues

1. **Instance not showing in Session Manager**
   - Verify the IAM role is attached to the instance
   - Check that the instance has internet connectivity
   - Ensure the SSM agent is running

2. **Permission denied**
   - Verify your AWS credentials have the necessary SSM permissions
   - Check that the EC2 instance role has the `AmazonSSMManagedInstanceCore` policy

3. **Session Manager plugin not found**
   - Reinstall the Session Manager plugin
   - Verify the plugin is in your system PATH

## Benefits of Using SSM Session Manager

- **No SSH keys required** - No need to manage SSH key pairs
- **No bastion hosts** - Direct connection through AWS infrastructure
- **Audit logging** - All session activity can be logged to CloudTrail
- **Fine-grained access control** - Use IAM policies to control access
- **Cross-platform** - Works with Linux and Windows instances

## Security Best Practices

- Use least privilege IAM policies
- Enable CloudTrail logging for session activity
- Regularly rotate IAM credentials
- Monitor session logs for suspicious activity

---

*Last updated: August 2025*
