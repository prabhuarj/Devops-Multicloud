## Getting started with AWS Solutions Architect Associate - SAA-C03

#### Author : Prabhu Arjunan

## Identity & Access Management (IAM)

# **Task 1: Create an IAM User and Attach Policies** 

Logged into the Management console and clicked the IAM section as it is already been chosen as favourites it showing in top panel so clicked it

![IAM Dashboard](../assets/.labImages/IAM/iam_dashboard.png)

Click the users in the left panel and add user enter name and if you want to provide the user with console access click it and give the password for the user and also click(user must create a password while loggin in) a good feature for the useer to change password and click next

![IAM Dashboard](../assets/.labImages/IAM/create_user.png)

In set Permission tab choose “Attach Policies directly” and choose any policy you want to attach to the user . so I choose AWS EC2 Full access and S3 Full access ad click next

![IAM Dashboard](../assets/.labImages/IAM/review_create.png)

Click create user and the user gets created.

![IAM Dashboard](../assets/.labImages/IAM/retrieve_password.png)

![IAM Dashboard](../assets/.labImages/IAM/user_summary.png)

You can also enable Access key for the programmatic access by clicking the Create Access key

After Key creation

![IAM Dashboard](../assets/.labImages/IAM/user_access_keys.png)

Later we can also attach a custom policy to user by choosing policy in left panela nd click create a new policy

Created a policy with RDS Full access

![IAM Dashboard](../assets/.labImages/IAM/policy_details.png)
![IAM Dashboard](../assets/.labImages/IAM/policy_created.png)

Now after creating the custom policy(customer managed) you can attach it to the user

Follow the same steps above go to users choose your user and click add permission attach policies directly and search your policy and once shown click it

![IAM Dashboard](../assets/.labImages/IAM/add_permissions.png)

Now click Next and choose Add permission and Now you see the policy gets attached to the user

![IAM Dashboard](../assets/.labImages/IAM/user_permissions.png)

**Task 1 Done:**

Created a user and gave console access and also the user needs to change password during their login for security   
Attached 2 policies EC2 and S3 full access during user creation and user has been created  
After creating the user gave Programmatic access by generating access key and secret key so user can access the resource via cli or programmatically

Also created a custom policy using policies tab and created a RDS Full access policy and once done attached the same to the user.

Logged in using the awstest user to check whether the user has necessary permission that we attached using policy

![IAM Dashboard](../assets/.labImages/IAM/access_denied.png)

As i dont have permission for EKS showing inaccessible

![IAM Dashboard](../assets/.labImages/IAM/eks_access_error.png)

But foe EC2 S3 and RDS we can perform action

![IAM Dashboard](../assets/.labImages/IAM/ec2_dashboard.png)

Also tested with cli access. Created a vm and installed aws cli and setup the keys and trued below commands to check the access via CLI. Working fine Able to access s3 but not EKS as it has no permission

![IAM Dashboard](../assets/.labImages/IAM/cli_eks_error.png)

# **Task 2: Create an IAM Group and Attach Policies**

# 

 To create a new group in aws go to left panel and click User groups and create group

![IAM Dashboard](../assets/.labImages/IAM/create_user_group.png)

Next add a permission ECS Full access to the group and click create.  
Now a group with a single policy is created below

![IAM Dashboard](../assets/.labImages/IAM/group_permissions.png)

Next in same page go to users and add the awstest user we created before to this “awstestgroup”. Now the user has been added to the group

![IAM Dashboard](../assets/.labImages/IAM/group_user_added.png)

Now to understand All the users part of this group will have ECS Full access. And one more thing to remember is the user “awstestuser” has already EC2 and S3 full access which we added during user creation and along with we have RDS Full access which we added as a custom policy after user creation. Now since we have added this user to group he must also inherit the ECS Full access from the group. You can also see the column attached via to see from where this policy is attached either via Directly or Via Group

To confirm find the screenshot below

![IAM Dashboard](../assets/.labImages/IAM/user_group_inherited_permissions.png)

Now lets verify whether this user has ECS access both via management console and via cli. From this it confirms this user has access to ECS which was inherited from group

![IAM Dashboard](../assets/.labImages/IAM/ecs_console.png)

![IAM Dashboard](../assets/.labImages/IAM/ecs_cli.png)


If we remember earlier cli screenshot and console screenshot and we knew rhe user dont have access to EKS now lets add EKS policy to group and see whether the user can access the EKS Cluster

EKS Cluster policy is attached to group

![IAM Dashboard](../assets/.labImages/IAM/group_policies.png)
Confirmed in user  
![IAM Dashboard](../assets/.labImages/IAM/user_all_permissions.png)

Now lets retry in console and cli For Eks Cluster access. When i tried not able to get the cluster access. Got below error

![IAM Dashboard](../assets/.labImages/IAM/cli_eks_error_repeat.png)

So on investigation 

**Who is the Policy For?**

* **The Intent:** This policy is designed for the **EKS Cluster itself** (the control plane). It gives the EKS service permission to create network interfaces, manage logs, and create resources in your account.  
* **The Reality:** When you attach it to a human user like `awstestuser`, AWS looks at the "Actions" inside that policy. Most of those actions are for internal service tasks, not for a user to "List" or "Describe" clusters from a terminal.

So for a user to get those list or delete cluster we need to create a Inline policy by going to Add permission and Add Inline policy and then choose  service EKS and choose necessary actions and then create a policy

![IAM Dashboard](../assets/.labImages/IAM/eks_policy_editor.png)

I followed the same and attched a “eksviewpolicy” to the user 

![IAM Dashboard](../assets/.labImages/IAM/user_inline_policy.png)

Now after this while testing got the access

![IAM Dashboard](../assets/.labImages/IAM/eks_cli_success.png)

![IAM Dashboard](../assets/.labImages/IAM/eks_console_empty.png)

**Task 2 Done:**

 Created a group with some policy attached directly and created. After creation add user to the group and check whether the user has inherited the permission from group and alos verify the user has necessary permission to access the Resources. Tested and working as expected.

# **Task 3: Create a Custom IAM Policy** 

To create a policy go to left pane and choose policies

Before creating policy am going to attach SNS policy and create a new policy. So here we check for the awstest user to see whether we have SNS access for the user.

We confirmed that the user dosent have SNS access

![IAM Dashboard](../assets/.labImages/IAM/sns_access_error.png)

So now we create a custom policy and attach it to a user

![IAM Dashboard](../assets/.labImages/IAM/sns_policy_created.png)

![IAM Dashboard](../assets/.labImages/IAM/sns_policy_list.png)

Now policy is created we are going to add it to the user “awstestuser”. Now the user has this policy attached.

![IAM Dashboard](../assets/.labImages/IAM/user_permissions_final.png)

Lets test the user can access SNS

Now we confirmed the user has SNS access. Also confirmed via cli

![IAM Dashboard](../assets/.labImages/IAM/sns_console.png)

![IAM Dashboard](../assets/.labImages/IAM/sns_cli.png)

**Task 3 done:**

Now we have created a custom policy named “snspolicy attach” and added sns policy and the policy has been attached to the user. Once the policy has been added user is now able to list the sns topics and access them.

# **Task 4: Create an IAM Role and Attach Policies** 

Create a new Role from the left pane and create Role and from the Trusted entity type choose

AWS Service and choose EC2

![IAM Dashboard](../assets/.labImages/IAM/create_role.png)

And in next add the necessary permission i have added s3 & rds Full access and sns(custom policy) to it 

Name the role and view permission and also added tags

![IAM Dashboard](../assets/.labImages/IAM/role_review.png)

![IAM Dashboard](../assets/.labImages/IAM/role_tags.png)


Now a role has been created for EC2 Instances and attached the necessary permission

![IAM Dashboard](../assets/.labImages/IAM/role_summary.png)

![IAM Dashboard](../assets/.labImages/IAM/role_permissions.png)

Once the role has been created. We need to test it so for that reason i have created a EC2 Instance named “dash-awsvmte-uVNq” and installed aws cli and did “aws s3 ls” to see if it is working . It dint work

![IAM Dashboard](../assets/.labImages/IAM/aws_cli_error.png)

Now I have attached the role we ctraeted to this EC2 Instance where it has policy like s3 RDS and SNS (custom) Full access. In below screenshot we can see role has been attached to a ec2 instance  
![IAM Dashboard](../assets/.labImages/IAM/attach_iam_role.png)

Now we will execute the commands in cli to see if the necessary actions can be made , The test is successful we are able to access s3 sns and rds. Find in below screenshot

![IAM Dashboard](../assets/.labImages/IAM/aws_cli_success.png)

**Task 4 Done:**

Created an IAM role for EC2 with permission added like s3 rds and sns policy attached to the role with tags addes.

For testing we have create an instance and checked to see befor attaching the role and also after attaching the role, After attaching the role we are able to access the necessary policies attached to the role,

Screenshots are attached.
