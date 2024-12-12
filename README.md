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
