# Cloud-security-using-AWS-IAM
AWS Cloud Security with IAM: Managing User Groups and EC2 Instances for Development and Production environments

## Project Description

This project demonstrates how to create and manage EC2 instances in different environments (production and development) using AWS, and how to implement Identity and Access Management (IAM) policies to control access to those resources.

## Project Steps

### 1. Launch EC2 Instances with Tags

- Two EC2 instances were launched: one for production and one for development.
![image alt]( https://github.com/ris21/Cloud-security-using-AWS-IAM/blob/main/EC2%20Instances.PNG)
- Tags were added to categorize the environments (`production` and `development`).
- Instances were created using free-tier eligible options to minimize costs.

### 2. Create an IAM Policy

- A custom IAM policy was created to allow EC2 instance operations (e.g., starting, stopping) for instances tagged as `development`.
- The policy denies the creation or deletion of tags on any instances.
- JSON Policy used:

    ```json
    {    
      "Version": "2012-10-17",    
      "Statement": [        
        {            
          "Effect": "Allow",            
          "Action": "ec2:*",            
          "Resource": "*",            
          "Condition": {                
            "StringEquals": {                    
              "ec2:ResourceTag/Env": "development"                
            }            
          }        
        },        
        {            
          "Effect": "Allow",            
          "Action": "ec2:Describe*",            
          "Resource": "*"        
        },        
        {            
          "Effect": "Deny",            
          "Action": [                
            "ec2:DeleteTags",                
            "ec2:CreateTags"            
          ],            
          "Resource": "*"        
        }    
      ] 
    }
    ```

### 3. Create an AWS Account Alias

- An account alias `alias-rispar` was created to simplify the AWS login URL for new users.

### 4. Create IAM Users and User Groups

- A user group `dev-group` was created and the custom policy `developmentpolicy` was attached.
- A new user `dev-rispar` was created and added to the group.

### 5. Test User Access

- The new user attempted to stop the production EC2 instance but was denied due to the policy restrictions.
- The policy worked as expected by allowing actions only on development instances.

