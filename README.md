# aws-cicd-project
# Deployment of end-to-end application in node.js using Jenkins CICD with GitHub Integration

---

| &nbsp; | &nbsp; |
| --- | ----------- |
| **Objective** | - Deploy end-to-end application in node.js using Jenkins CICD with GitHub Integration <br>- Trigger Jenkins pipeline automatically once the code is pushed on GitHub |
| **Approach** | - Using Amazon's Elastic Compute Cloud (EC2) for running application on the Amazon Web Services (AWS) infrastructure <br>- Containerize application by creating Dockerfile<br>- Integrate GitHub with Jenkins using Webhook |
| **Impact** | - Jenkins pipeline triggers automatically once the code is pushed on GitHub</br>- Accomplish faster quality releases by automating CI/CD pipelines |
<br>

**Primary Technology:** Github, Docker, Jenkins, aws EC2 service
<br><br>

![Design Thinking Whiteboard in Yellow Blue Basic Style](https://user-images.githubusercontent.com/102405945/211872655-4ed92a9f-bf03-41bc-ac92-043594863cd8.png)


<br><br>
---

## 1. Creating a Node App
Before we write any CI/CD pipeline we need an application to test and deploy. We are going to build a simple **to-do application** in node.js. Then, create new repository under your GitHub account and name it “node-todo-cicd”.

![Screenshot 2024-12-12 095319](https://github.com/user-attachments/assets/9e3204d5-90cb-4644-9832-9ae86c6de0b6)


## 2. Creating AWS EC2 instance

### 2.1 Creating AWS EC2 instance
After logging into your AWS account, search for EC2
![1c](https://github.com/user-attachments/assets/40b4c88d-5bd7-45d5-93f3-ec48c7f2c4c5)

Select **Instances(running)**
![Iniyavan](https://github.com/user-attachments/assets/646840ed-b163-4d8e-b437-ddbd5a329c3a)

Click **Launch instances**
![Iniyavan1](https://github.com/user-attachments/assets/fee7a20e-0e96-4579-8425-fbdf3dc7d05c)
Enter Name and select **Ubuntu**
![Iniyavan3](https://github.com/user-attachments/assets/097b9b68-5308-4f3f-9d09-12be4c5b61d1)

Select **t2.micro** as Instance type and create new key pair to connect to the server
![Iniyavan4](https://github.com/user-attachments/assets/3bff7792-fc72-4ee5-8b14-24adb2adb0a7)


Enter **key pair name** and select **RSA** as Key pair type and **.pem** as Private key file format. Then, click **Create key pair**
![Iniyavan5](https://github.com/user-attachments/assets/af3cd032-1429-4c92-8e0f-7cf5dc7ab1eb)


Finally, click **Launch instance**
![Iniyavan6](https://github.com/user-attachments/assets/a7ef0534-d77e-41cb-b21e-941cf03391eb)




### 2.2 Connecting AWS EC2 instance
Click **Connect** on the top of the screen
![Iniyavan7](https://github.com/user-attachments/assets/76254530-f4cf-415f-8f50-f7a56c777c5d)

Click **Connect**
![Iniyavan8](https://github.com/user-attachments/assets/2f018ef3-8f14-4ef1-91f7-924e6d0ec2d8)


## 3. Installing Jenkins on AWS

### 3.1 Installing Java
Install Java using following commands: <br>
**sudo apt update**<br>
**sudo apt install openjdk-22-jre**<br>
**java -version**<br>
![Iniyavan9](https://github.com/user-attachments/assets/98596647-a058-4c73-bc3b-4c2c3d0097ae)
![Iniyavan10](https://github.com/user-attachments/assets/444bbf77-ce4c-476a-a316-f764c489ffab)

### 3.2 Installing Jenkins
Install Jenkins using following commands: <br>
**curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee \   /usr/share/keyrings/jenkins-keyring.asc > /dev/null **<br>
**echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \   https://pkg.jenkins.io/debian binary/ | sudo tee \   /etc/apt/sources.list.d/jenkins.list > /dev/null**<br>
**sudo apt-get update**<br>
**sudo apt-get install jenkins**<br>
![Iniyavan11](https://github.com/user-attachments/assets/0d8e1504-19c5-45de-af1c-e2ac488ed9d1)


### 3.3 Start Jenkins
Start jenkins using following commands: <br>
**sudo systemctl enable jenkins**<br>
**sudo systemctl start jenkins**<br>
**sudo systemctl status jenkins**<br>
![Iniyavan12](https://github.com/user-attachments/assets/58c210bd-e29d-434c-a63a-2576d338e9b0)


### 3.4 Open port 8080 from AWS Console
Go to Instances. Click on Security tab
![Iniyavan13](https://github.com/user-attachments/assets/a39ba153-953a-4ae0-96a2-640182a0dd25)
Click on **Edit inbound rules**
![20](https://user-images.githubusercontent.com/102405945/211761553-008189c7-b351-4706-829a-accaf4dd02ec.png)
Click on **Add rule** and add port 8080 and select **My IP** and then click **Save rules**
![21](https://user-images.githubusercontent.com/102405945/211761596-7d06f0a1-f2d7-410e-bb44-9e0f5a987ecc.png)
![22](https://user-images.githubusercontent.com/102405945/211761624-4762a332-f20c-41c2-88a0-23e92a5186c7.png)

### 3.5 Unlock Jenkins
Open port 8080 on new tab
![23](https://user-images.githubusercontent.com/102405945/211763309-270c2fe1-db98-43c6-9b39-032affcf4e38.png)

On console, type the command **sudo cat /var/lib/jenkins/secrets/initialAdminPassword** and copy the password
![24](https://user-images.githubusercontent.com/102405945/211763345-85dcd1a5-78db-4770-a7b5-392248f23909.png)

Paste the password in Administrator password to unlock jenkins
![25](https://user-images.githubusercontent.com/102405945/211763367-b2d215be-ea2b-4a45-bb20-2ec3acef41fe.png)

Install suggested plugins
![26](https://user-images.githubusercontent.com/102405945/211763394-0ea3d5af-8c15-466d-8be2-99487cc1d873.png)
![27](https://user-images.githubusercontent.com/102405945/211763441-25d3a088-6119-43b0-a04e-cde33cfaca52.png)

Create First Admin User
![28](https://user-images.githubusercontent.com/102405945/211763485-8449f84f-313a-4478-ab81-bebdf36a808f.png)

Enter Jenkins URL and click **Save and Finish**
![29](https://user-images.githubusercontent.com/102405945/211763529-e7ac31bd-38b6-45ef-ab43-118ff9b93fa3.png)

Finally, Jenkins is ready. Click on the button **Start using Jenkins**
![30](https://user-images.githubusercontent.com/102405945/211763586-17fdc134-9757-4b82-ad67-5eb57fc4ab9c.png)


### 3.5 Connect GitHub and Jenkins
Create new job by clicking **New item**
![32](https://user-images.githubusercontent.com/102405945/211767102-7ba3199c-a108-4a4f-8ebc-4ba25ef1af38.png)

Enter an item name. Select **Freestyle project**. Add description. Select **GitHub project** and add project url
![33](https://user-images.githubusercontent.com/102405945/211767157-ac02438b-86a8-4f66-8056-87be60df0588.png)

Select **Git** as Code Management and add **Repository URL**. Click on **Add** to add key
![34](https://user-images.githubusercontent.com/102405945/211767199-de9358d1-b081-4f8e-9da8-bead2ff93db3.png)

Generate SSH key on console using following commands: <br>
**ssh-keygen** <br>
**cd .ssh** <br>
**cat id_rsa_pub** <br>
Copy the public key
![35](https://user-images.githubusercontent.com/102405945/211767270-54cf0dbd-2fc5-49c3-9fc4-7983eef0b9bb.png)
![36](https://user-images.githubusercontent.com/102405945/211767284-65ecd0ee-27fe-4db5-b0fc-56be725e85bd.png)
![37](https://user-images.githubusercontent.com/102405945/211767299-10ac5fdb-9812-4f87-aeae-3f774757bd4d.png)

Go to GitHub. Click on **Settings**
![38](https://user-images.githubusercontent.com/102405945/211767336-6f0ac758-7f80-432c-b084-924671fcc8fa.png)

Click on **SSH and GPS keys** on the left pane and click on **Add SSH key**
![39](https://user-images.githubusercontent.com/102405945/211767369-dd05d32c-c87c-4637-9056-6d675a01459f.png)

Paste the SSH key and click **Add SSH key**
![40](https://user-images.githubusercontent.com/102405945/211767410-9b2a26c0-779d-4f64-946f-229ebce031ea.png)
Go to Jenkins, and select **SSH Username with private key** in kind
![41](https://user-images.githubusercontent.com/102405945/211767451-530ca1d9-1ea2-49c9-9637-c82a6a415357.png)
On console, enter the command **cat id_rsa** and copy the private key
![42](https://user-images.githubusercontent.com/102405945/211767482-00352640-a688-4001-8a2f-f5d2c3983efa.png)
Paste the private key in jenkins wizard
![43](https://user-images.githubusercontent.com/102405945/211767525-854c8421-a07e-4c04-b235-45be202d1247.png)

Select **ubuntu(This is for github and jenkins integration)** in credentials
![44](https://user-images.githubusercontent.com/102405945/211767548-5aac0cd6-ca4a-46e6-9d46-b7fd71f23dad.png)

Enter ***/master** in Branch Specifier and click **Save**
![45](https://user-images.githubusercontent.com/102405945/211767592-a9aaf324-878b-4716-a2f6-0de181d67b78.png)


### 3.6 Get code in jenkins
In jenkins, click on **Build Now** on the left pane
![46](https://user-images.githubusercontent.com/102405945/211769407-1f79b77a-8f34-4966-9938-d70121f5305d.png)

Now, click on **#1** and select **Console Output** to view the console
![48](https://user-images.githubusercontent.com/102405945/211769445-03122663-f40d-43fc-9e6a-83d545e87e27.png)
![49](https://user-images.githubusercontent.com/102405945/211769487-4ab1a5c4-2c22-4cdc-9462-1ba62cf0bed4.png)

To check whether we got the code on EC2 instance, go to console, and enter the following commands: <br>
**sudo cd /var/lib/jenkins/workspace/todo-node-app** <br>
**ls** <br>
Clearly, the code is present in this directory
![50](https://user-images.githubusercontent.com/102405945/211769557-f8f0f62e-b426-481d-b209-11b09e6f61f1.png)


### 3.7 Change permission of inbound traffic of port 8000
Click **Add rule** and enter port number 8000 and select **Anywhere IPv4** it can be accessed by anyone. Click **Save rules**
![54](https://user-images.githubusercontent.com/102405945/211807365-5fe644dd-7223-4a2e-8ef7-247517eec3b2.png)
![55](https://user-images.githubusercontent.com/102405945/211807378-983373e8-93d5-4876-89ba-32699362e9ef.png)


### 3.8 Run node.js application
On console, run the following commands: <br>
**sudo apt install nodejs** <br>
**sudo apt install npm** <br>
**npm install** <br>
**node app.js** <br>
![51](https://user-images.githubusercontent.com/102405945/211807429-539b13f6-cc36-4c9a-8d62-c17202247151.png)
![52](https://user-images.githubusercontent.com/102405945/211807441-5f3e28fd-5f4b-491f-809d-b232449c90c6.png)
![53](https://user-images.githubusercontent.com/102405945/211807456-4fe867de-817f-4a4f-afd9-53c7e3546a37.png)
![56](https://user-images.githubusercontent.com/102405945/211807470-1bd37f3f-1e7e-4234-aa6a-f3b24fce6fd2.png)



## 4. Automating using Docker

## 4.1 Install Docker
Remove Dockerfile and install Docker using following command: <br>
**sudo rm Dockerfile** <br>
**sudo apt install docker.io** <br>
![57](https://user-images.githubusercontent.com/102405945/211811699-e22e7e86-837c-425a-9f28-8a07e6242d6c.png)

## 4.2 Create Dockerfile
Edit Dockerfile using the command **sudo vim Dockerfile** and add following commands within it: <br>
**FROM node:12.2.0-alpine** <br>
**WORKDIR app** <br>
**COPY . .** <br>
**RUN npm install** <br>
**EXPOSE 8000** <br>
**CMD ["node", "app.js"]** <br>
![58](https://user-images.githubusercontent.com/102405945/211811726-0638ac1b-94e2-4581-9066-75e6b8e2b2ac.png)

## 4.3 Build Docker
On console, enter the following commands:
**sudo usernod -a -G docker $USER** <br>
**sudo reboot** <br>
To give permission to docker and reboot the system
![59](https://user-images.githubusercontent.com/102405945/211811758-ac46d053-c589-4af8-82ca-ac3ed65b560b.png)
After restarting, enter the following command: <br>
**sudo build . -t todo-node-app** <br>
![63](https://user-images.githubusercontent.com/102405945/211811786-451dd302-58a9-4bd5-bf4c-8741978a738c.png)


## 4.4 Run Docker
After building docker, enter the following command: <br>
**docker run -d --name node-todo-app -p 8000:8000 todo-node-app** <br>
To check status of container **docker ps** <br>
![64](https://user-images.githubusercontent.com/102405945/211811806-f4f2eb1d-d4be-4b0a-910d-acbf60f97d7c.png)
![65](https://user-images.githubusercontent.com/102405945/211811820-1c82be6d-b3f5-4742-aa72-b3be9be4b194.png)



## 5. Automating using Jenkins (CICD pipelines)

### 5.1 Automating commands using Jenkins (Execute shell)
On console, enter the following commands to terminate a container: <br>
**docker ps** <br>
**docker kill <containerid>** <br>
![68](https://user-images.githubusercontent.com/102405945/211852431-49284f24-5471-4ce9-995a-154e5cbf4266.png)
![69](https://user-images.githubusercontent.com/102405945/211852461-b30ef264-d51d-4f83-b728-e2bbfb7813d6.png)

On jenkins, go to dashboard and select the project. For me, it is **todo-node-app**
![71](https://user-images.githubusercontent.com/102405945/211852522-4ade2baa-db4c-4157-9e9c-cfafce28f905.png)

Click on **configure** on the left pane
![72](https://user-images.githubusercontent.com/102405945/211852679-b8e4945c-6b11-49bf-8940-08ea5790c524.png)

Click on **Build Steps** on the left pane and then select **Execute shell** in the Build Steps section
![73](https://user-images.githubusercontent.com/102405945/211852724-4f2d183b-7b4d-4f2e-a6a0-2a5b24f0bfe2.png)

Enter the following commands in the Execute shell to be executed: <br>
**docker build . -t node-app-todo** <br>
**docker run -d --name node-app-container -p 8000:8000 node-app-todo** <br>
Click **Save**
![74](https://user-images.githubusercontent.com/102405945/211852763-e95b9072-7510-4253-8c0b-8df43c5228c3.png)

Click on **Build Now** on the left pane
![75](https://user-images.githubusercontent.com/102405945/211852806-f72ff44c-e13a-47b9-b666-78492c92bc28.png)
![76](https://user-images.githubusercontent.com/102405945/211852837-9de83468-01ec-40b6-859f-43f9585e94f6.png)



## 6. Trigger Jenkins pipelines automatically once the code is pushed on GitHub (Webhooks)

### 6.1 Install plugin
On console, enter the following commands to terminate a container: <br>
**docker ps** <br>
**docker kill <containerid>** <br>
![77](https://user-images.githubusercontent.com/102405945/211855122-2c1e344d-caaf-4a90-8d3f-63216d2ae619.png)
![78](https://user-images.githubusercontent.com/102405945/211855175-367b4652-fd6c-4b95-a0a1-028c6aaea56d.png)

On jenkins, go to dashboard and select the project. For me, it is **todo-node-app**
![79](https://user-images.githubusercontent.com/102405945/211855229-b7feea1c-0524-449e-8402-852b9bc18ba9.png)

Click on **Manage Jenkins** on the left pane and then click on **Manage Plugins**
![80](https://user-images.githubusercontent.com/102405945/211855270-9fd20815-7b72-43a3-8435-440c4fbfb0c8.png)

Search for **GitHub Integration** and select **GitHub Integration**. Finally, click on **Download without restart**
![82](https://user-images.githubusercontent.com/102405945/211855356-d251d724-d4be-4176-a24a-cab09a09676c.png)
![83](https://user-images.githubusercontent.com/102405945/211855373-ced17abb-65c5-47bf-8c7d-532c76cc78fd.png)


### 6.2 Configure inbound rules of port 8080
On aws, go to instances and click on **Security** pane
![88](https://user-images.githubusercontent.com/102405945/211857801-0f09c615-74e5-4c75-8256-6d4a3758f7fe.png)

Edit inbound traffic of port 8080 to **Anywhere IPv4** and click **Save rules**
![89](https://user-images.githubusercontent.com/102405945/211857830-b62f54d5-ca35-46d3-81fd-dd99510873de.png)
![90](https://user-images.githubusercontent.com/102405945/211857845-ab57cbb4-9f9c-48fb-9ef5-b4d968e934fa.png)

### 6.3 Configure webhook in GitHub
Go to GitHub and click **Settings**
![84](https://user-images.githubusercontent.com/102405945/211857936-1888dbab-233c-43f4-a3a9-f9893bdd44a7.png)

Ensure that SSH and GPG keys are present on your GitHub account
![85](https://user-images.githubusercontent.com/102405945/211857997-d5e79b34-a202-4dbd-857e-6bf20511b1de.png)

Now, click on **repository settings**
![86](https://user-images.githubusercontent.com/102405945/211858028-de1dec4b-9da1-4817-98f3-bd2924d1a1ac.png)

Click on **Webhooks** on left pane and click on **Add webhook** to add a webhook
![87](https://user-images.githubusercontent.com/102405945/211858058-6015568f-a09d-4fc2-9d56-da9c43bea8a2.png)

Add Payload URL and select content type as **application/json**
![91](https://user-images.githubusercontent.com/102405945/211858098-29c67833-2c2b-4d36-b72c-40b374a6f50a.png)

Finally, click **Add webhook**
![92](https://user-images.githubusercontent.com/102405945/211858162-dddb3be9-41b3-4b0f-a159-a74162c0ab65.png)
![93](https://user-images.githubusercontent.com/102405945/211858197-72fb7203-8078-4b2c-be51-abf2b6969a3a.png)



### 6.3 Configure Jenkins
Go to Jenkins Job and click on **configure** on left pane
![94](https://user-images.githubusercontent.com/102405945/211859591-c28c92b5-4f6b-4c07-9798-4fa96172f7e6.png)

Click on **Build Triggers** on left pane and enable **GitHub hook trigger for GITScm polling. Finally, click **Save**
![95](https://user-images.githubusercontent.com/102405945/211859617-d48b1e56-8bb8-4b29-9c69-597ca9d9b397.png)

## Testing
Go to GitHub project repository and edit the project. Click **Commit changes**
![96](https://user-images.githubusercontent.com/102405945/211859643-af476c53-d9e6-4413-ae86-7c711ea3f6b3.png)

Clearly, Jenkins pipeline are triggered as the code is pushed on GitHub
![97](https://user-images.githubusercontent.com/102405945/211859669-6eb83c45-2a41-4e63-9c51-db3e8fcabeb7.png)
![98](https://user-images.githubusercontent.com/102405945/211859697-c65ae885-146c-4e9b-86fa-4d52b570837e.png)
<br> <br>
![99](https://user-images.githubusercontent.com/102405945/211859754-cbbf68de-503f-4d5c-b1b7-c543218e07e7.png)



