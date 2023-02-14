# Week 0 — Billing and Architecture

## Create an Admin AWS User
### why do we need another user
We will have the root user when we created the aws account. But it's not a good practice to use root user to do the tasks because root user allow full access to all resources for all AWS services, including the billing information. So instead, we will create other users to perform the tasks.

### how to create other users using console

1. Search and choose IAM service
2. In the navigation pane, choose Users and then choose Add user.
3. Type in the user name.
4. Set permission to the user. If the user needs an admin role, we can add the user to the admin group.
5. We need to set up the password for the user to log in to the console.
6. We also need to get the access key and secret key to use in AWS CLI.

## Set Budget
### why do we need to do this
We can set up budget and alert, so that if cost exceeds, we can get alert from aws.

### how to set up budget
#### set up a dollar budget
1. Search and choose Budget service from console.
2. Choose the Zero spend budget
3. Give it a name and enter the email you want to receive the alert
4. Save it and you can see a new budget in the console.

#### set up a credit budget
1. Search and choose Budget service from console.
2. Choose to create a new budget
3. Choose Customize (advanced) option in the Budget setup.
4. Choose Cost budget for Budget Types
5. Give it a name like credit budget
6. Choose monthly as the period and choose recurring budget so you don't need to do it next month.
7. Enter the amount you want to set up, like 10.00 a month.
8. Choose only the Credits type in the Budget Scope area.
9. For the alert, set up the trigger percentage, like 60%, so you will receive the alert email when the cost exceeds 60% of your monthly budget.
10. Enter the email you want to receive the alert.
11. Save it and you will see the budget in the console.

## Use CloudShell
### What is CloudShell
Using AWS CloudShell, a browser-based shell, you can quickly run scripts with the AWS Command Line Interface (CLI), experiment with service APIs using the AWS CLI, and use other tools to increase your productivity. The CloudShell icon appears in AWS Regions where CloudShell is available.

### how to use a cloudshell
In the console, you can see an icon next to the bell on the right opt of the navigation bar. Click it and wait for some time, you will see a terminal page in your browser, and you can use it like a normal terminal and perform AWS cli commands in the terminal.

## Installed AWS CLI On GitPod

1. Authorize GitPod to access your GitHub.
2. Choose your repo you want to set up the workspace.
3. Open it and choose VS code in browser and you will see the workspace.
4. To install the AWS CLI on the workspace, you will need to get the awscli zip and install it. Perform the next commands in the terminal:
`
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

`
5. Also perform the following command to add aws symlink: 
`sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update`

6. If you type aws, you will see some prompts
7. Next, we need to add our aws account information to the workplace. We can export our credentials to the workplace environment. Issue the following commands:
'
export AWS_ACCESS_KEY_ID="YOUR_AWS_ACCESS_KEY_ID"
export AWS_SECRET_ACCESS_KEY="YOUR_AWS_SECRET_ACCESS_KEY"
export AWS_DEFAULT_REGION="YOUR_AWS_DEFAULT_REGION"
'
Then, type in the next commands and you will be able see your environment credentials back.
`
gp env AWS_ACCESS_KEY_ID="YOUR_AWS_ACCESS_KEY_ID"
gp env  AWS_SECRET_ACCESS_KEY="YOUR_AWS_SECRET_ACCESS_KEY"
gp env AWS_DEFAULT_REGION="YOUR_AWS_DEFAULT_REGION"
`
8. Go to your settings of the GitPod account, you should see the credentials on your corresponding repo. 

## Conceptual Lucid Chart
https://lucid.app/lucidchart/faace3f0-4a2b-4ecb-9554-11e26297446a/edit?invitationId=inv_9a80abb2-8ef7-47e7-92a3-e82dcff82825&page=0_0#