# CICD with Jenkins and AWS
# CICD_jenkins_aws

![My project (10)](https://user-images.githubusercontent.com/98178943/153622078-f8f91b07-a509-4867-befa-0e4596ce3dfd.png)


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