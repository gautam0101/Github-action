## How to Integrate CI CD pipeline in AWS ECR and AWS EKS using Github actions

This document outlines the steps to integrate CI CD pipeline in AWS ECR and AWS EKS using Github actions, allowing you to automate code analysis and quality checks for your repository.

follow this document for more info:- [Link](https://towardsaws.com/containerize-node-js-a25b7e9a1e85)

Step by step user guide [Link](https://drive.google.com/file/d/15imowp0o8a4CiGpMJIIFTI2Mbvm60vRS/view?usp=sharing).

## Prerequisite:
 
 Create Node.js application
 
 install express js
 

# Step for integration
## Create Node.js application:- 
Step 1: create a folder and initialize the npm in the folder
          .mkdir node-app
          .cd node-app
          .npm init
Step 2: install express js

Step 3: create a server.js file in the node-app 
        touch server.js
        
# Create a Dockerfile:-

Step 1:- touch Dockerfile #By the help of this command you create a file.

step 2:- To build the docker image
          docker build .

step 3:- check the images in the
          
          `docker images`
          
          `docker run -p 8000:8000 <image_id>` # Put the Image id and open the browser and check localhost:8000
 ![Screenshot from 2023-05-25 09-46-05](https://github.com/gautam0101/Github-action/assets/101164301/39b86115-15a5-488f-9409-3b95c19b4b49)

          Push the image to AWS ECR (Elastic Container Registry):- 

          Step by step user guide:-
 [Video](https://drive.google.com/file/d/1yyKyseGE_N07ogGZcP55pBE7Aod8CgRs/view?usp=sharing)

# step for integration:-
## What you need firt a AWS account.

step 1:-To create security credentials,
step 2:- Goto access keys and click on generate, It will create the Access keys.
step 3:- Go to the application repo settings and add the secrets for the actions to use.


## Create an ECR Repo in AWS
# step for integration:- 

step 1:- Search for Elastic Container Registry, And click on the create.

step 2:- Add the name of the repo

step 3:- And click on create.

step 4:- Now we have a repo ready, Copy the Image URI to add it ton GitHub action


## Add Github Action to build the image and push it to ECR in Code Repo

# step for integration:- 

folder structure

.github

|-workflows
      
      |- pushtoecr.yaml  #In this file replace the REPO url.



# Deploy it to the AWS EKS (Elastic Kubernetes Service) cluster

# step for integration:- 

step 1:- Install eksctl

// Adds third party repositaries to home brew

brew tap weaveworks/tap

// Installs eksctl

brew install weaveworks/tap/eksctl

step 2:-  Create an EKS cluster using eksctl

`eksctl create cluster --name my-cluster --region ap-south-1 --nodegroup-name linux-nodes  --node-type t2.micro --nodes 2`

#You can give the any name to the cluster but don't give sampe to more then one.

![Screenshot from 2023-05-26 16-22-27](https://github.com/gautam0101/Github-action/assets/101164301/9704902f-00dd-4843-8382-41c7831f1988)

Step 3:- Configure EKS to kubectl

// To configure

`aws eks update-kubeconfig --region ap-south-1 --name my-cluster`
// To Check the configuration 

`kubectl get nodes`

# This will give the the nodes you have in the cluster as output

Step 4:- You deploy the application to the EKS with following commands directly.

`kubectl apply -f  kube.yaml service.yaml`![Screenshot from 2023-05-30 12-03-29](https://github.com/gautam0101/Github-action/assets/101164301/49225bce-7754-4fd9-a5ef-5f1ca156c7b1)

step 5:- Create a GitOps Repo

#folder structure

.github

|-workflows

|- triggerargocd.yml

|-kube
   
   |- deployment.yaml
   
   |- service.yaml
   
   
   
   
   

