---
layout: post
title:  "Save Money with AWS Organizations & the CloudTrail Free Tier"
date:   2024-08-16 20:30:01 -0500
categories: cloud
---

In this blog post, I explain how combining AWS Organizations with the Cloudtrail free tier helps you simplify your AWS audit logs infrastructure, enable automated enrollment of new AWS accounts and regions to Cloudtrail, implement a critical security control required by current security standards and more importantly, save money.

Something your trusted advisor won't tell you, and an optimization often overlooked by Infrastructure engineers. 

Whether you are still fighting for a (decent) security budget or succeeded to get properly funded, costs shall always be optimized and money spent wisely. 

I'll provide you with Terraform Infrastructure as Code (IaC) snippets to help you get started.

### AWS Organizations - One account to rule them all

AWS Organizations allows you to centrally manage and govern your environment as you grow and scale your AWS resources. By consolidating multiple AWS accounts into a parent AWS Organization account, you can consolidate billing, apply organization level policies for security and compliance, and centralize access management across your different AWS tenants.

#### Key Benefits:

1. **Centralized Management**: Manage multiple AWS accounts from a single location.
2. **Simplified Billing**: Consolidate billing to receive a single bill for all accounts.
3. **Policy-based Management**: Apply Service Control Policies (SCPs) to control AWS services usage.
4. **Cost Savings**: Optimize resources and receive volume pricing discounts.

If you only use one AWS account, in which you run staging and production, time to consider transitioning to AWS Organizations. Terraform remains your best friend here to nullify additional burden of deploying in multiple environments.

#### Admin Delegation

AWS Organizations gives you to possibility to choose which account to use for administering a given feature. Depending on the size of your organization, you may have one account dedicated to "Security Toolings", or "Logging", as recommended by the Security Architecture Framework. Such accounts are great candidates for Cloudtrail admin delegation.

### CloudTrail

AWS CloudTrail is a service that enables governance, compliance, and operational auditing of your AWS account. With CloudTrail, you'll gain visibility over events across your AWS infrastructure, every single action is recorded. That includes, user accounts, groups, security groups events.

Cloudtrail can be configured at the region level, there is an option available to activate it in all regions.

#### CloudTrail's Free Tier

1. **Free for Management Events**: The first copy of management events within each region is free.
2. **Improved Security**: Monitor and log all account activity for security analysis and operational troubleshooting.
3. **Enhanced Compliance**: Ensure compliance with regulatory and internal requirements by maintaining an audit trail.

### Organization CloudTrail

An Organization CloudTrail is a centralized logging solution provided by AWS that allows you to monitor and log activities across ALL accounts in an AWS organization. By creating a single CloudTrail for the entire organization, you can capture API activity and management events from all member accounts and store them in a designated S3 bucket. This centralized approach simplifies monitoring, enhances security compliance

#### Leveraging the Free Tier Across All Accounts

Provided there are no other trails configured in the individual accounts, by using an organization CloudTrail, you can benefit from the free tier of CloudTrail across all your AWS accounts. This means that the first copy of management events from each account in your organization is free, provided you do not have other trails configured in those accounts. This setup allows you to monitor and log activities from multiple accounts without incurring additional costs, making it a highly cost-effective solution.

If you are working in an Infrastructure you did not build, here is a script that will audit your various accounts and regions looking for existing Cloudtrails. Once your organization trail is active, you may want to pause them, then wait until your retention period for audit logs expires to delete the trail once for all. I am using Okta to programmatically authenticate to AWS, depending on how you do that in your infrastructure, you'll want to modify the wrapper.

#### Auto-Discovery

One of the key benefits of Organization CloudTrail is its auto-discovery feature. When you create an Organization CloudTrail, it automatically includes all existing and newly created accounts within your AWS organization. This ensures that any new accounts added to your organization are immediately covered by the centralized logging configuration without requiring additional setup. 

### Setting Up AWS Organizations and CloudTrail with Terraform

Let's dive into the Terraform code that will help you set up an AWS Organization and enable CloudTrail for centralized logging.

#### Step 1: Setting Up AWS Organizations

First, ensure that AWS Organizations is enabled. Here’s a Terraform snippet to create an organization:

{% highlight ruby %}
provider "aws" {
  region = "us-east-1"
}

resource "aws_organizations_organization" "org" {
  feature_set = "ALL"
}
{% endhighlight %}

#### Step 2: Creating Organizational Units (OUs) and Accounts
Next, create organizational units and accounts. Below is a basic example:

{% highlight ruby %}
resource "aws_organizations_organizational_unit" "security_ou" {
  name      = "Security"
  parent_id = aws_organizations_organization.org.roots[0].id
}

resource "aws_organizations_account" "security_account" {
  name      = "SecurityAccount"
  email     = "security@example.com"
  role_name = "OrganizationAccountAccessRole"
  parent_id = aws_organizations_organizational_unit.security_ou.id
}
{% endhighlight %}

#### Step 3: Setting Up CloudTrail
Now, enable CloudTrail for your organization to capture all activities. The following Terraform code snippet sets up an organization trail:

{% highlight ruby %}
resource "aws_cloudtrail" "organization_trail" {
  name                          = "OrganizationTrail"
  s3_bucket_name                = aws_s3_bucket.trail_bucket.bucket
  include_global_service_events = true
  is_multi_region_trail         = true
  enable_logging                = true

  event_selector {
    read_write_type           = "All"
    include_management_events = true

    data_resource {
      type   = "AWS::S3::Object"
      values = ["arn:aws:s3:::"]
    }
  }

  organization_enabled = true
}

resource "aws_s3_bucket" "trail_bucket" {
  bucket = "my-org-cloudtrail-bucket"

  versioning {
    enabled = true
  }

  server_side_encryption_configuration {
    rule {
      apply_server_side_encryption_by_default {
        sse_algorithm = "AES256"
      }
    }
  }

  lifecycle_rule {
    enabled = true
    expiration {
      days = 365
    }
  }
}
{% endhighlight %}

#### Step 4: Applying Policies and Configurations
Finally, apply necessary policies and configurations to ensure security and compliance.

{% highlight ruby %}
resource "aws_s3_bucket_policy" "trail_bucket_policy" {
  bucket = aws_s3_bucket.trail_bucket.bucket

  policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AWSCloudTrailAclCheck20150319",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudtrail.amazonaws.com"
      },
      "Action": "s3:GetBucketAcl",
      "Resource": "arn:aws:s3:::${aws_s3_bucket.trail_bucket.bucket}"
    },
    {
      "Sid": "AWSCloudTrailWrite20150319",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudtrail.amazonaws.com"
      },
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::${aws_s3_bucket.trail_bucket.bucket}/AWSLogs/*",
      "Condition": {
        "StringEquals": {
          "s3:x-amz-acl": "bucket-owner-full-control"
        }
      }
    }
  ]
}
EOF
}
{% endhighlight %}

### Conclusion

Leveraging AWS Organizations and the free tier of AWS CloudTrail can significantly streamline your cloud setup and save costs. By following the above steps and using Terraform for deployment, you can establish a secure, compliant, and cost-effective cloud infrastructure. Remember, using an organization CloudTrail allows you to benefit from the free tier across all accounts, provided you do not have other trails configured. Hope that helped.