# CICD with Jenkins and AWS
# CICD_jenkins_aws

![My project (11)](https://user-images.githubusercontent.com/98178943/153639945-52662638-4ad1-46dc-8af4-55826c368a53.png)

[Read on how to create a CI/CD pipeline from scratch using Jenkins!](#creating-a-cicd-pipeline-from-scratch-using-jenkins)

### Note: Before we start cloning repos if we get permission denied, we can enter the commands below to fix it:

<img src="https://user-images.githubusercontent.com/98178943/153861943-9236b5f2-712e-4245-894f-2942ba86d5d0.png" width="250">

# Create a SSH connection from localhost to GitHub

## Generate SSH key on localhost
- cd into `~/.ssh`
- enter the command `ssh-keygen -t rsa -b 4096 -C "email@email.com"`
- Follow the instructions as seen in the image below:

<img src="https://user-images.githubusercontent.com/98178943/153604417-ac45743e-60a7-43f0-a787-8a8ba6990840.png" width="500">

## Put a lock on GitHub with ssh – copy the public SSH to GitHub
- Enter cat `103a.pub` to view your ssh key. Copy and paste all of it onto github as shown on the image below:

<img src="https://user-images.githubusercontent.com/98178943/153604861-27931b8f-aad1-4a29-af7f-6ae4753f3be8.png" width="500">
<img src="https://user-images.githubusercontent.com/98178943/153605001-ff057744-075a-40b8-800b-114e53517bad.png" width="500">

## Creating a new repo for CICD on GitHub

- Save the key and create a new repo and select 'SSH' after creating it, as seen below:
<img src="https://user-images.githubusercontent.com/98178943/153605288-a7766873-3141-4788-9df3-4a9c8c838356.png" width="500">

Go back to your terminal and create a new directory and add a README.md:

<img src="https://user-images.githubusercontent.com/98178943/153605514-b540d7e3-b004-44ae-8c70-4f985c4fc248.png" width="350">

<img src="https://user-images.githubusercontent.com/98178943/153605579-90088655-b8f6-4b7e-b14f-31513fb74cb5.png" width="350">

Go back to your repo and enter the instructions into your terminal:

![image](https://user-images.githubusercontent.com/98178943/153605707-8f1ed1aa-a9fc-402b-b318-f26d4079de7a.png)

After entering all the commands, if you get an error that says that you don't have permissions, enter the code below:

<img src="https://user-images.githubusercontent.com/98178943/153606359-471962d3-1087-419f-80e2-7ff6b6d4a473.png" width="500">

GitHub documentation on how this works can be read [here.](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

# Starting with Jenkins
Open your Jenkins link and login using the details you have been given. 

<img src="https://user-images.githubusercontent.com/98178943/153607505-66cd4cd8-50ee-480b-865c-6f434c1f0ad5.png" width="250">

On the homepage select new item or build and then enter an item name:
<img src="https://user-images.githubusercontent.com/98178943/153607663-fa7369b2-716f-4e9b-bba8-a37af3dcd895.png" width="500">

In our case we are building two items, one to start a test build to see the date and time of the build and the other build is the post build, for the first test.

Build 1 is called yacob_jenkins_test: it's configure contents are as seen below:
![image](https://user-images.githubusercontent.com/98178943/153608158-fc71f529-bcd8-43b8-95ef-442c909364d3.png)

This to call the date and time:
![image](https://user-images.githubusercontent.com/98178943/153608215-09d5f5aa-f055-4d25-aaae-2cf4e94d48fd.png)

This is to start the test_2 post build:
![image](https://user-images.githubusercontent.com/98178943/153608346-948e914a-b274-429a-946c-8cf84603c47f.png)

Build 2: 
![image](https://user-images.githubusercontent.com/98178943/153608456-771fa95f-0568-4e47-9c7b-fe2c3869d90b.png)

The `ps aux` command is a tool to monitor processes running on your Linux system:
![image](https://user-images.githubusercontent.com/98178943/153608583-932ba358-0048-40c8-8c46-8eefef2fe990.png)

build time and date from yacob_jenkins_test:

<img src="https://user-images.githubusercontent.com/98178943/153608715-560a45f0-ed3b-47ce-a2df-ef69306222e6.png" width="250">

Select `console output` under the build number and you'll see a separate page saying the build was a success and what the post build is as seen below:

<img src="https://user-images.githubusercontent.com/98178943/153608932-979d4886-1f1f-420f-814f-925dbc290e11.png" width="500">

Click on test_2 and click on it's `console output` and you'll see that the `ps aux` command was successfully executed:

<img src="https://user-images.githubusercontent.com/98178943/153609418-626854dc-104f-4176-b647-6162f3b8b779.png" width="500">

# Setting up Jenkins to create a CI/CD pipeline

## Generate a new key of cloned repo you want to push
-----
- `ssh-keygen -t rsa -b 4096 -C "email@email.com"`
- enter `eval "$(ssh-agent -s)"` and `ssh-add ~/.ssh/103a` to give yourself permissions to commit the cloned repo.

- do `cat yourkey.pub` to obtain the public key which we'll put on github.

## Copying the key to github
----
- Enter your cloned repo, select the settings tab INSIDE the repo.
- Deploy keys > Add deploy key
- Copy the public key you copied and enter it into the key field, with an appropriate title. 

## Connecting the repo to Jenkins
------
{Awaiting screenshot but AWS hosting jenkins is down...} 

## Setting up webhook
- Go to the settings tab in your cloned repo on github > create new github
- Enter the payload url of your jenkins server. E.g. `69.594.384.22:8080/github-webhook/
- Content type `application/json`
- Leave secret blank
- `Send me everything` for events triggered


## Creating three jenkins jobs to create the CI/CD pipeline. 

### First job to pull and push dev builds
- Create new item
- Discard old builds, set 3 for max builds to keep
- Select GitHub project, enter the HTTP link to your repo
- Under `office 365 connector`, select `Restrict where this project can be run` and type `sparta-ubuntu-node` as we want to run it from this node. 
- Under `source code management` select the `git` option.
- Enter your SSH repository link under `Repository URL`
- Under credentials, we need to add the private SSH key, we can obtain the private key from our terminal. Paste that under credentials by adding a new key
- Under `Branches to build` enter `*/dev` as we want to push the dev builds
- Under `Build Triggers` select `GitHub hook trigger for GITScm polling`, this is to ensure that jenkins is notified from the webhook.
- Under `Build Environment` select `Provide Node & npm bin/folder to PATH`
- - Select `Add post-build action` and click `Build other projects`, enter the name of your second job (once you have made it)

## Second job to merge dev branch to main branch
-----------
- Create new item
- Discard old builds, set 3 for max builds to keep
- Select GitHub project, enter the HTTP link to your repo
- Under `office 365 connector`, select `Restrict where this project can be run` and type `sparta-ubuntu-node` as we want to run it from this node. 
- Under `source code management` select the `git` option.
- Enter your SSH repository link under `Repository URL`
- Under credentials, we can enter the SSH private key that we already have.
- Under `Build Environment` select `Provide Node & npm bin/folder to PATH`
- Under `Branches to build` enter `*/dev`, we will push to main branch in the next few steps.
- Under the build option, select `Execute Shell` and enter:
```
git checkout main
git merge origin/dev
```
### Post-build actions
- Select `Add post-build action` > `Git Publisher`
- Click `Push Only If Build Succeeds` - this action self-explanatory
- Select `Add post-build action` again and click `Build other projects`, enter the name of your third job (once you have made it)
- Ensure that `Trigger only if build is stable` is selected.

## Creating EC2 instance in AWS
- A very straightforward process, from the VPCs and its containers, create a new instance, ensuring the ports and inbound rules are open for your own IP and for the jenkins ip.

## Creating the third job
- Create new item
- Discard old builds, set 3 for max builds to keep
- Select GitHub project, enter the HTTP link to your repo
- Under `office 365 connector`, select `Restrict where this project can be run` and type `sparta-ubuntu-node` as we want to run it from this node. 
- Under `source code management` select the `git` option.
- Enter your SSH repository link under `Repository URL`
- Under credentials, we can enter the SSH private key that we already have.
- Under `Build Environment` select `Provide Node & npm bin/folder to PATH`
- Under `Branches to build` enter `*/main`
- Enter your file.pem to be able to enter the EC2 instance
- Under `build`, we can enter:

```
scp -o "StrictHostKeyChecking=no" -r app ubuntu@IPfromaws:~
```
# Creating a CI/CD pipeline from scratch using Jenkins!
![My project (11)](https://user-images.githubusercontent.com/98178943/153639945-52662638-4ad1-46dc-8af4-55826c368a53.png)

In the following steps, I will show the steps to create a CI/CD pipeline using jenkins.

In short the steps are:

1. Create a master node and agent node instance on AWS. Create an AMI of the agent node
2. Install jenkins on the master node and the required plugins
3. Create webhooks and link github repo to jenkins using private and public ssh keys
4. Connect AWS credentials to jenkins and set configuration to create agent node instance from AMI to launch on AWS
5. Create jobs to automate builds and tests.

## Step 1
-----------------
Create a master agent where the jenkins server will be built. This is built using an EC2 instance on AWS.

Create an instance on EC2, choose your subnets and so on. Auto assign Public ip. For ports, follow as shown below. Ensure you remember the security group names as you'll need it later. 

![image](https://user-images.githubusercontent.com/98178943/154802767-1b5edf61-d636-40fc-8648-8efefdde442f.png)

At the same time, you'll want to create another EC2 instance for the agent node. You'll take an AMI of this after you're done. The ports will be as seen below:

![image](https://user-images.githubusercontent.com/98178943/154802950-1843c9a3-4059-47c0-8516-5c0e3739ef48.png)

## **For the agent node:**
- SSH into it
- run `sudo apt update && sudo apt upgrade -y && sudo apt install default-jre -y` to update/upgrade and install java
- create an AMI, you'll need this later.

**For the master node:**
- SSH into it
- run `sudo apt update && sudo apt upgrade -y && sudo apt install default-jre -y` to update/upgrade and install java
- `curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null` to add they key to our system
- `echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null`
- `sudo apt-get update`
- `sudo apt-get install jenkins`
- `sudo systemctl start jenkins`

Open jenkins in your browser on port 8080:


![image](https://user-images.githubusercontent.com/98178943/154803220-a98dc7ff-ecfb-435e-b578-26dddfa67529.png)

- Enter `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` in your terminal to obtain the password. 
- Enter the password into the terminal

- Choose what option suits you best, the first option will take longer:

![image](https://user-images.githubusercontent.com/98178943/154803330-62455839-1a72-4919-8f8c-a06447d95655.png)

![image](https://user-images.githubusercontent.com/98178943/154803355-4e9429a8-8d27-4730-b131-3909f4b994a7.png)

Enter your login details

![image](https://user-images.githubusercontent.com/98178943/154803411-9e5fabbd-110b-4535-ac53-2f6261178490.png)

Once you're in, go to plugins:

![image](https://user-images.githubusercontent.com/98178943/154803457-e9282a9d-abeb-46b4-972b-8389a579440f.png)

Go to the 'available' tab and ensure you tick all the plugins you'll need, such as AWS EC2, SSH Agent, Office 365 connector, Github, node.js etc.

Once that is done, click download and restart at the bottom. 

Restart Jenkins:

![image](https://user-images.githubusercontent.com/98178943/154803597-93759541-384b-432f-a503-566c0b0cc2c5.png)

![image](https://user-images.githubusercontent.com/98178943/154803682-681e5142-1772-4547-96d8-93e2b584bc2e.png)

After it restarts:

Once your jenkins restarts, go to Manage Jenkins > Manage Nodes and Clouds

Go to configure clouds 
![image](https://user-images.githubusercontent.com/98178943/154803965-7c229323-7748-46d2-9d7b-a6f349fbb963.png)

Add new cloud > Amazon EC2

![image](https://user-images.githubusercontent.com/98178943/154804087-f09a72c5-7781-4f3f-a639-86923f049983.png)

Configure your cloud, add your access and secret keys:

![image](https://user-images.githubusercontent.com/98178943/154804117-f5194c4a-5580-4d09-a05a-af5377ff8524.png)

Enter the key from your pem file

![image](https://user-images.githubusercontent.com/98178943/154804322-7702a71d-71ea-4123-9114-3fd2e51207b9.png)

Test connection to ensure it is connected and set the correct region.

![image](https://user-images.githubusercontent.com/98178943/154804445-9cdd5af4-c5cb-47ee-80ee-bec3423e9d24.png)

Next, add AMIs to add your agent nodes:

![image](https://user-images.githubusercontent.com/98178943/154804480-02902691-edc4-49dd-b89d-8d7545ad7832.png)

In the description section, enter the name of your agent node that you want to see as your EC2 instance name and enter the AMI id of the agent node that you took a snapshot of:

![image](https://user-images.githubusercontent.com/98178943/154804561-63ab6b2a-32e8-4e5c-9697-199a6145770c.png)

- Set instance type to T2micro
- Enter the security group name of your master node
- set /home/ubuntu/ for Remote FS root
- set ubuntu for Remote user
- under AMI Type set sudo for Root command prefix
- set 22 for Remote ssh port
- For labels enter the name of your agent (e.g. jenkins-agent)
- click on Advanced below Init script
- set 1 for Number of Executors
- add a Tag with Name for Name and jenkins-agent for Value
- **Apply** and **save**

Launch your agent node to see if it works:

![1f839bb8e9729b64006d06895419a577_AdobeCreativeCloudExpress](https://user-images.githubusercontent.com/98178943/154805064-eb9ebb58-254b-4058-b0d7-7eda8a17fda1.gif)

Go back to your EC2 dashboard and see if the instance has been launched:

![image](https://user-images.githubusercontent.com/98178943/154805105-7d7940f4-0f1d-4330-8776-8c9f0d9eb184.png)

Check the agent node logs to see if it has launched in the instance:

![2fb7578ffbf7edccc9086461411f8e21_AdobeCreativeCloudExpress](https://user-images.githubusercontent.com/98178943/154805244-2d21dd0c-41f2-4546-84c3-88632af99c95.gif)

Set number of executors in the built in node to 0, so only the agent nodes are running the tests:

![9a4dd94c8354637a7fde95b4164eb9ae_AdobeCreativeCloudExpress](https://user-images.githubusercontent.com/98178943/154805411-ef212854-e474-4735-9cd4-fa79847b88f8.gif)

## Step 2 - Setting up webhook
------------------
Go to the github repo where you want to push your changes from.

Go to Settings from your repo > Wehbooks > Add webhook and add your webhook:


![image](https://user-images.githubusercontent.com/98178943/154807066-53b45ead-bec1-4b55-9400-f81bb3a2f25e.png)

Add the public SSH key from your local host to the repo:

![image](https://user-images.githubusercontent.com/98178943/154807274-6e8c55a7-2757-4d38-bcd7-0a5df0d912be.png)

## Step 3: Making the jobs
-------------
### Job 1
Build item > Freestyle project > OK

![image](https://user-images.githubusercontent.com/98178943/154808861-86674f37-664d-4eb0-8889-e07b523333c3.png)
![image](https://user-images.githubusercontent.com/98178943/154808875-d9ab35dd-cc36-4f05-97e3-f97d5bdb9944.png)

![image](https://user-images.githubusercontent.com/98178943/154808891-1233a93e-bf08-4b60-81c0-da269a3e37b4.png)

![image](https://user-images.githubusercontent.com/98178943/154808917-764d8dc1-f1ca-4143-871e-5fa1ca523382.png)

![image](https://user-images.githubusercontent.com/98178943/154808945-79f4eab8-21b2-401d-a54a-4b99a9f49f09.png)

### Job 2

![image](https://user-images.githubusercontent.com/98178943/154808973-28f577de-9227-47a6-bfdd-02e37794ab6a.png)

![image](https://user-images.githubusercontent.com/98178943/154808985-803d3df6-a985-464e-8390-f4a49b65e4c9.png)

![image](https://user-images.githubusercontent.com/98178943/154809000-b7f3b878-a0c1-4c40-a745-5ee031ec4da5.png)

### Job 3

![image](https://user-images.githubusercontent.com/98178943/154809014-bb71eab0-9c2a-4aac-9539-8dce1837879a.png)

![image](https://user-images.githubusercontent.com/98178943/154809028-40fea0a2-c71e-4fae-81c9-264c50179cb6.png)

![image](https://user-images.githubusercontent.com/98178943/154809038-c3cdce9f-ecac-4054-9200-d8748f459a25.png)

For job 3, enter the ip of your instance where you want to host your app.

## Testing your pipeline

I have written this readme alongside creating the pipeline from scratch so it should work once you push a change from your localhost > repo > jenkins > deployed to AWS.


Sources:

https://pkg.jenkins.io/debian-stable/

https://www.kisphp.com/linux/setup-jenkins-server-on-aws-ec2-with-slave-agents