---
title: "Setting up a Static Website using Terraform and AWS"
seoTitle: "Setting up a Static Website using Terraform and AWS"
datePublished: Fri Aug 18 2023 05:38:13 GMT+0000 (Coordinated Universal Time)
cuid: cllg5t6f5000d09la1je8d6j8
slug: setting-up-a-static-website-using-terraform-and-aws
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1692336980125/f8d78cdb-6f3f-4d9b-9099-01e712f41799.png
tags: aws, projects, devops, iac, aws-s3

---

# Overview

In this step-by-step guide, we'll explore a straightforward DevOps project that combines Terraform and AWS to create a static website. By the end of this tutorial, you'll have a functional S3 bucket with static website hosting, all accomplished through automated processes using Terraform. This project is perfect for those looking to practice their DevOps skills and get hands-on experience with AWS and infrastructure as code. So let's jump right in!

# Implementation

You can find the project on my github: [https://github.com/komal-max/Setting-Static-Website-using-Terraform-and-AWS](https://github.com/komal-max/Setting-Static-Website-using-Terraform-and-AWS)

## Creating an AWS account

If you don't have an AWS account already, follow this official documentation for creating an AWS standalone account: [https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-creating.html](https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-creating.html)

## Install Terraform

To use Terraform we will need to install it. HashiCorp distributes Terraform as a binary package. We can also install Terraform using popular package managers. Install terraform in your machine by following: [https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

I installed Terraform by using Homebrew.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692245361619/4d41bbe9-9da6-4f76-803d-36757c712afd.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692245695777/169b8b63-0982-46c3-8b80-48fd2225d7e1.png align="center")

## Install AWS CLI

We will also need AWS CLI in this project. AWS CLI enables us to interact with AWS services using commands in our command-line shell.

To install AWS CLI make use of official AWS documentation: [https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

Choose the O.S. and follow the provided steps. In my case, I have macOS and I used Command line installer-All users.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692246070016/850b0f82-05bd-49d3-84b9-071d271b96d3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692246226936/b05c124f-cdb4-4f5b-968c-ef5596560d7a.png align="center")

## Connect your AWS account to Terraform

Login to AWS using either your root user or IAM user.

### Creating an IAM user

Once logged in, if you don't have an IAM user, best to create one. Navigate to IAM service &gt; Users &gt; Add users &gt; provide a user name, in my case it is *Jordan &gt;* select *Provide user access to the AWS Management Console* option and then select rest of the options as below:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692256312161/a8767fb6-2d8d-47aa-b0db-5f5d90b95683.png align="left")

After this, click *Next &gt;* Under Permissions options, choose *Attach Policies directly* and select *Administrative Access* permission policy for this user. After setting the policy, click *Next* and then *create user.* Copy the console sign-in URL and paste it in a new browser to sign in using the IAM user

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692300211428/a4d98b8a-fe48-4089-a27d-7b1df8416fea.png align="center")

### Creating the access key

After you login using the IAM user&gt; on the right side under the account info, click on *Security credentials &gt;* it will take you to IAM service, scroll down below and find Access keys section. Access keys are used to send programmatic calls to AWS from the AWS CLI, AWS Tools for PowerShell, AWS SDKs, or direct AWS API calls.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692254813923/a6863142-34e0-495d-b8ad-3385a795a83d.png align="center")

Next, click on *Create access key &gt;* Choose option *CLI &gt;* confirm &gt; Next &gt; add a *Description tag value,* for example: *tf-access-key &gt;* click on *create access key*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692255078056/a6429890-7287-45e7-8868-bfd4cf97b25d.png align="center")

Once the access key is created, save the Access key and Secret access key (you can also download it in a .csv file).

### Authenticate with IAM user credentials

To configure the AWS CLI, we are going to use *aws configure.* For general use, the *aws configure* command is the fastest way to set up AWS CLI installation. Open your local machine's terminal and use *aws configure.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692301569318/4b62e2d5-3fb7-45df-a572-a9333225b035.png align="center")

There are two other ways to configure the AWS CLI- *Importing access keys via .CSV file, Directly editing the config and credentials files,* we simply opted for *aws configure*. If you are curious and want to find out more about the other two procedures, follow: [https://docs.aws.amazon.com/cli/latest/userguide/cli-authentication-user.html](https://docs.aws.amazon.com/cli/latest/userguide/cli-authentication-user.html)

To check if the connection has been successfully established, run any command as listed below:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692302905868/aa7f6d18-eed8-46fd-893f-6b9e10d05140.png align="center")

## Configuration using Terraform

### Adding Provider and creating a bucket

Create a new folder using your terminal and open that folder in Visual Studio Code.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692250195825/5a0d4caa-64e7-4930-b547-643f521a513d.png align="center")

Whenever we need to work with AWS in Terraform we need to create a provider. Create a *provider.tf* file. We will make use of Terraform's documentation as much as possible. Search for AWS Provider in terraform documentation and copy the configuration as provided here: [https://registry.terraform.io/providers/hashicorp/aws/latest/docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692251304240/08d00965-b52b-4a4b-b498-47398d7c88d5.png align="center")

You can now initialize terraform using *terraform init,* it will help download all the required dependencies or files for the provider.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692251755736/a030410b-b906-4721-80da-384d7a8d3ad9.png align="center")

Now, let's create a new file under our folder *my-static-web* and name that file *main.tf.* We will use this file to define our bucket configuration. Going back to Terraform documentation, we will look for S3 bucket configuration: [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3\_bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket)

Make sure that you have a globally unique name of the bucket. In our case, we will define the name of the bucket in the form of a variable. For this, we will create a separate file called *variables.tf*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692253378965/5b53d5ef-4542-4edd-af81-27fdbd011ac2.png align="center")

Once done, run *terraform plan* and then *terraform apply -auto-approve* (use -auto-approve only if you want to skip typing *yes* to apply) and then check if the bucket is getting created successfully in AWS

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692299572341/db590412-b777-4701-843f-5f84d158adfb.png align="center")

login into your AWS account &gt; S3 &gt; you will find the newly created bucket.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692303269227/38d6ca7c-ac77-48fb-b770-a42d6b0119ac.png align="center")

### Manage S3 Bucket Ownership

We can make use of Terraform documentation here to define bucket ownership: [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3\_bucket\_ownership\_controls](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_ownership_controls)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692304754098/29206660-1734-46c9-9347-d78cc02e8abe.png align="center")

This will ensure that everything in this bucket is owned by the bucket owner.

### Making the bucket Public and add ACL resource

Using the documentation, we will now make our S3 bucket public: [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3\_bucket\_public\_access\_block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_public_access_block)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692305161932/ecaef62c-61d9-405b-8ca0-5f14738f9b21.png align="center")

To add ACL resource use: [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3\_bucket\_acl](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_acl)

ACL is a set of permissions associated with an S3 bucket. It defines who can access the objects within the bucket and the type of access they have. It helps to manage public access to objects within the bucket. By setting permissions in the ACL, we can make objects within a bucket either publicly accessible or restricted.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692306527279/25ec1e77-9d4d-4333-a738-46dda3a7105c.png align="center")

Now run terraform plan and terraform apply to apply the changes to the bucket.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692306784365/5fcaff0f-80e7-4c37-afdc-a85266cd07bc.png align="center")

To check the changes on AWS, navigate to bucket &gt; Permissions

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692307622825/b087158b-ceb1-427c-9d78-120bc6007c47.png align="center")

### Enable Static website hosting

To set up S3 Static website, we need to enable static website hosting, for that, we need to make use of website configuration resource: [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3\_bucket\_website\_configuration](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_website_configuration).

To use this, we need to first create *index.html* and *error.html* and add those in our S3 bucket. In VSC, under our main folder *my\_static\_web,* create two files- *index.html* and *error.html.*

*Index.html-* serves as the default landing page of our website. This file typically contains the main content of our website, such as text, images, links. It's the page visitors first see when they access a website.

*Error.html-* This file comes into play when visitors encounter errors while navigating our website. These errors could be due to broken links, mistyped URLs, or other issues that prevent a page from loading correctly.

You can simply visit my github and use both files as is: [https://github.com/komal-max/Setting-Static-Website-using-Terraform-and-AWS](https://github.com/komal-max/Setting-Static-Website-using-Terraform-and-AWS), you will just need to tweak it a little which I will explain below.

For adding a profile picture in *index.html,* search for any photo you want to use from the internet and add the photo in your main folder, which in my case is *my\_static\_web.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692309053586/0766234a-cc61-40f5-b9d3-7022d0c3a722.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692309270677/248f6001-1764-4ae7-80b5-d30cdb31644d.png align="center")

This file is now visible in our VSC, let's first add this to our S3 bucket and then we can use the object URL to make changes to *index.html* file. To put objects to S3, we can make use of documentation: [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/s3\_object](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/s3_object)

In our main.tf, define resources for all three objects.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692310189151/fc64a973-6b0e-426d-94ae-1f07cf2de44c.png align="center")

Now, do a terraform plan and terraform apply. After that, check using AWS console that the objects have been added to our S3 bucket.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692311411777/6c299f04-d11c-4a81-8317-a69a8bdbd579.png align="center")

Let's edit index.html to add profile photo, navigate to AWS &gt; S3&gt; your bucket &gt; my-profile.jpeg &gt; copy Object URL

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692312629310/4e60e21a-0359-46e4-8af5-1bc3ef7a55ac.png align="center")

After copying object URL, go to VSC &gt; index.html and add object URL to *img src.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692312508623/b88c12ac-c2f9-4c83-be83-c39d0fc1fb8f.png align="center")

The *index.html* file contains around many occurrences of my name, you can replace it with your name.

To configure website in terraform, let's make use of the documentation: [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3\_bucket\_website\_configuration](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_website_configuration)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692311853676/30043bc4-614b-480a-9f75-06f2fde29836.png align="center")

After adding website configuration resource, run terraform plan and apply

Navigate to AWS&gt; S3 bucket &gt; your bucket name &gt; Properties &gt; static web hosting

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692312089575/1b2ee882-7fc5-4917-b488-49a1d71291fa.png align="center")

If we click on this link, we can see the portfolio!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692334882145/b19b40b9-2d24-455d-8926-cd486a93e842.png align="center")

Feel free to make changes to *index.html* file and customise it as you please.

We can also create *outputs.tf* file so that we can get endpoint right on our terminal. By creating this output variable, we enable Terraform to provide us with the URL endpoint of our static website after the infrastructure is created.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692328179274/918b7c2f-9114-47d1-a751-33c903d67be0.png align="center")

That's it! I hope you enjoyed working on this short and pretty interesting project where you got to learn about AWS S3 and Infrastructure as Code using Terraform. If you feel anything can be made better in this project or you find any issues or run into any problems, please feel free to let me know. I am also adding a list of references from which I took inspiration and guidance for this project 👏.

Happy Learning!

## References

1. [https://www.youtube.com/watch?v=wFWg0Y68Owo&t=1385s](https://www.youtube.com/watch?v=wFWg0Y68Owo&t=1385s)
    
2. [https://github.com/komal-max/Setting-Static-Website-using-Terraform-and-AWS](https://github.com/komal-max/Setting-Static-Website-using-Terraform-and-AWS)
    
3. [https://docs.aws.amazon.com/cli/latest/userguide/cli-authentication-user.html](https://docs.aws.amazon.com/cli/latest/userguide/cli-authentication-user.html)
    
4. [https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-creating.html](https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-creating.html)
    
5. [https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
    
6. [https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)
    
7. [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3\_bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket)
    
8. [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3\_bucket\_public\_access\_block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_public_access_block)
    
9. [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3\_bucket\_acl](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_acl)
    
10. [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3\_bucket\_website\_configuration](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_website_configuration)
    
11. [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/s3\_object](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/s3_object)