
 
<!-- AWS Managed Microsoft Active Directory -->
## AWS Managed Microsoft Active Directory
In this project, I created a AWS Managed Microsoft Active Directory and also created a user in this AD. I was able to create a Windows 10 Workspace for the user using Amazon Workspaces. I gave the user permissions so that they are able to log into the AWS portal in the same account using their Microsoft AD credentials. I also created a Single Sign On URL for the user to be able to access a different AWS account. 

## Architecture

## Prerequisites

You will need to have an AWS organization with at least two accounts.

## Set Up

#### 1. Create a Directory 
  - Go to the Directory Service 
  - Create a Directory by clicking Set up Directory 
#### 2. Create IAM Role – Used by the EC2 Instance to join the domain
  - Use case: Select EC2
  - Permissions: select AmazonSSMManagedInstanceCore  and AmazonSSMDirectoryServiceAccess 
  ![ebsapp](https://github.com/jazminchannel/images/blob/main/GetImage.png)

#### 3. Create IAM Role – Used for Delegation 
  - Use case: Select Directory Service (the Directory Service will be able to assume this role) 
  - Permissions: select PowerUserAccess (allows you to provide power user access to the user we will create in the directory) 
  ![ebsapp](https://github.com/jazminchannel/images/blob/main/GetImage%20(1).png)
#### 4. Go to Directory Service after the Directory is created, click on your directory, click on the Application Management tab, and create an application access URL  
  ![ebsapp](https://github.com/jazminchannel/images/blob/main/GetImage%20(2).png)
#### 5. Scroll down and enable AWS Management Console access 
  ![ebsapp](https://github.com/jazminchannel/images/blob/main/GetImage%20(3).png)
#### 6. On the Scale and Share tab you will see that two domain controllers have been created   
#### 7. Launch EC2 instance and connect it to the domain 
  - Go to EC2 management console and click launch instances 
  - Select Windows Server 2019 base 
  ![ebsapp](https://github.com/jazminchannel/images/blob/main/GetImage%20(4).png)
  - Instance Type: Select t2.micro 
  - Select your directory as the Domain join directory and  the IAM role that was created for the domain join 
  ![ebsapp](https://github.com/jazminchannel/images/blob/main/GetImage%20(5).png)
  - Network settings: select allow RDP traffic from custom CIDR 
  ![ebsapp](https://github.com/jazminchannel/images/blob/main/GetImage%20(6).png)
  - Create keypair 
  - Launch Instance 
#### 8. RDP into the instance (Used Microsoft Remote Desktop connection tool) 
  - Login using directoryname\admin and password created when setting up the directory 
  - Open command prompt and type in whoami to verify you are logged in as admin
  - You can type set command for more details and to verify that the computer is joined to the domain 
  - Go to Server Manager 
  
  ![ebsapp](https://github.com/jazminchannel/images/blob/main/GetImage%20(7).png)
  - Add Roles and Features
  
  
  ![ebsapp](https://github.com/jazminchannel/images/blob/main/GetImage%20(8).png)
  - Keep all the default settings except when you get to Features. Select the following: 
  
  
  ![ebsapp](https://github.com/jazminchannel/images/blob/main/GetImage%20(9).png)
  - You should now have the following tools on your computer 
  
  
  ![ebsapp](https://github.com/jazminchannel/images/blob/main/GetImage%20(10).png)
  - Open Active Directory Users and Computers
    * If you go into the organizational unit named after the domain > users, you can create users 
    * Create a User and an email address for that user 
#### 9. Go into AWS Directory Service in console and delegate the user permissions 
  - Go to the Application Management tab > click on the iam role 
  
  
  ![ebsapp](https://github.com/jazminchannel/images/blob/main/GetImage%20(12).png)
  - Add the user to this role

## Creating an Amazon Workspace


#### 10. Create an Amazon Workspace and assign it to the user
  - Go to Amazon Workspaces Application Manager in the console 
  - Register your directory 
    * Directories > select Directory > go to Actions > click Register
    * Select two subnets > click Register 
  - Create a workspace 
    * Go to the Workspace tab
    * Select the directory 
    * Select the user 
    * Select Standard Windows 10 Bundle 
    * Launch 
#### 11. Log into Wokspace as user and access the management console
  - Download software to your computer from https://clients.amazonworkspaces.com/ to install and connect to the workspace 
  
  
  ![ebsapp](https://github.com/jazminchannel/images/blob/main/GetImage%20(13).png)
  - Copy the registration code for the user from Workspaces Application Manager in the console and pasted into the workspaces app on your computer 
  - Login as user with Microsoft AD password 
  - Go to the AWS Directory Service and find the URL for the AWS Management Console  
  - Paste the URL in Firefox in your workspace
  - Login as user using AD credentials 


## Creating a Single Sign On URL

#### 12. Change Identity Source in the AWS account to Active Directory and assign the user access
  - Go to IAM Identity Center > Settings > Identity Source > Actions > Change Identity Source 
  - Choose identity source: Active Directory 
  - Choose your active directory and confirm change 
  - Set up AD sync and choose the user 
  - go to Permission sets tab and create a permission set 
  - Go to AWS accounts tab and select the account you want to give the user access to and add that user and permission 
  - User can now log into Workspace , use the AWS Identity Center URL found in Directory Service under Application Management Tab to sign into the other account 


<!-- ACKNOWLEDGMENTS -->
## Acknowledgments
Project Source: Solution Architect Professional Course by Neal Davis
