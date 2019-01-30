---
title: AWS IAM Access Key Age Integration
keywords:
tags: [integrations]
sidebar: doc_sidebar
permalink: integrations_aws_key_age.html
summary: Send AWS Key Age data to Wavefront.
---

AWS Identity and Access Management (IAM) allows administrators of different AWS services to manage access to those AWS services and resources securely.

## Why Access Key Age Metrics?

An AWS Identity and Access Management (IAM) user represents a person or application that interacts with AWS. The user has a name and an access key. The access key consists of an access key ID and a secret key and can be used when accessing AWS programmatically.

Wavefront supports a built-in integration that allows Wavefront to collect metrics about the access key age of all IAM users in a AWS profile. If you establish a trust relationship with a Wavefront AWS integration and follow the setup steps in this guide you can:
* Monitor access key age metric related to IAM users by a profile and region.
* Set up alerts so you know when access keys are about to expire. For example, you can configure an alert target to notify when the access key age of user is about to reach the MAX_KEY_AGE (180 days).

## Prerequisites

To perform the AWS IAM Access Key Age integration setup, your environment must meet these prerequisites:
* Access to Amazon Web Services
* EC2 instance with SSH enabled and Python and Python-pip installed.
* AWS SDK for Python setup complete. You can follow the [Quick Start Guide of Boto 3](https://github.com/boto/boto3)
* Wavefront AWS integration.
* For each user you want to monitor from one of the profiles in the integration, you need the IAM user access_key_id and secret_access_key.

## Sending IAM Access Key Age Data to Wavefront

After you've set up your environment to meet the prerequisites, follow these steps to send the access key age data to Wavefront:
1. Connect to the EC2 instance with SSH.
2. Update the `~/.aws/credentials` file with an access_key_id and secret_key for each profile. The following example illustrates this:
   ```
   [default]
   aws_access_key_id =
   aws_secret_access_key =
   [aws_profile1]
   aws_access_key_id =
   aws_secret_access_key =
   [aws_profile2]
   aws_access_key_id =
   aws_secret_access_key =
   ```
   If this file doesn't exists create it.
3. Download the `access_key_check.py` file into the EC2 instance.
   ```
   wget https://github.com/wavefrontHQ/integrations/blob/master/aws/scripts/access_key_check.py
   ```
4. Open the `access_key_check.py` file for edit and add the following information:
   ```
   # The AWS_PROFILES must be same as in your ~/.aws/credentials file
   AWS_PROFILES = [] # List of AWS profiles, to fetch IAM Users' Access Key Age per profile
   PROFILE = ''
   MAX_KEY_AGE = 180 # max allowed access key age
   WAVEFRONT_API_TOKEN = '<wavefront api token>'
   WAVEFRONT_URL = '<wavefront url>' # the cluster where we'll push the access key age metrics
   REPORT_URL = WAVEFRONT_URL + '/report'
   WRITE_INFO_LOG = 'true'
   ```
5. Run the script:
   ```
   python access_key_check.py
   ```
   The output looks similar to the following snippet:
   ```
   INFO:__main__:Sending metric to wavefront : aws.iam.accessKey 209 source="AWS" name="user1" key="ABCDE" status="Active" profile="aws-profile" region="us-west-2"
   INFO:__main__:202
   INFO:__main__:Sending metric to wavefront : aws.iam.accessKey 38 source="AWS" name="user1" key="FGHIJ" status="Active" profile="aws-profile" region="us-west-2"
   INFO:__main__:202
   INFO:__main__:Sending metric to wavefront : aws.iam.accessKey 55 source="AWS" name="user2" key="KLMNO" status="Inactive" profile="aws-profile" region="us-west-2"
   INFO:__main__:202
   INFO:__main__:Sending metric to wavefront : aws.iam.accessKey 189 source="AWS" name="user3" key="PQRST" status="Active" profile="aws-profile" region="us-west-2"
   INFO:__main__:202
   ```
6. You can now view the access key age metrics on the dashboard:
   ![aws key age dashboard](images/AWS_KeyAge_dashboard.png)