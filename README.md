How to Integrate CI CD pipeline in AWS ECR and AWS EKS using Github actions
This document outlines the steps to integrate CI CD pipeline in AWS ECR and AWS EKS using Github actions, allowing you to automate code analysis and quality checks for your repository.
follow this document for more info:- https://towardsaws.com/containerize-node-js-a25b7e9a1e85/"Link"

Step by step user guide https://drive.google.com/file/d/15imowp0o8a4CiGpMJIIFTI2Mbvm60vRS/view?usp=sharing.

Step for integration
Create Node.js application:- 
Step 1: create a folder and initialize the npm in the folder
          .mkdir node-app
          .cd node-app
          .npm init
Step 2: install express js

Step 3: create a server.js file in the node-app 
        touch server.js
        
Create a Dockerfile:-
Step 1:- touch Dockerfile #By the help of this command you create a file.
step 2:- To build the docker image
          docker build .
step 3:- check the images in the
          docker images
          docker run -p 8000:8000 <image_id> # Put the Image id and open the browser and check localhost:8000
          https://drive.google.com/file/d/1DTTprWwUQ7n4T78UQKG36CH9Zuo56gch/view?usp=sharing

Push the image to AWS ECR (Elastic Container Registry):- 
Step by step user guide https://drive.google.com/file/d/1yyKyseGE_N07ogGZcP55pBE7Aod8CgRs/view?usp=sharing

step for integration:-
What you need firt a AWS account.

step 1:-To create security credentials,
step 2:- Goto access keys and click on generate, It will create the Access keys.
step 3:- Go to the application repo settings and add the secrets for the actions to use.


Create an ECR Repo in AWS
step for integration:- 
step 1:- Search for Elastic Container Registry, And click on the create.
step 2:- Add the name of the repo
step 3:- And click on create.
step 4:- Now we have a repo ready, Copy the Image URI to add it ton GitHub action


Add Github Action to build the image and push it to ECR in Code Repo
step for integration:- 
#folder structure
.github
|-workflows
      |- pushtoecr.yaml #In this file replace the REPO url.



Deploy it to the AWS EKS (Elastic Kubernetes Service) cluster
step for integration:- 
step 1:- Install eksctl
// Adds third party repositaries to home brew
brew tap weaveworks/tap
// Installs eksctl
brew install weaveworks/tap/eksctl

step 2:-  Create an EKS cluster using eksctl
eksctl create cluster --name my-cluster --region ap-south-1 --nodegroup-name linux-nodes  --node-type t2.micro --nodes 2
#You can give the any name to the cluster but don't give sampe to more then one.
https://drive.google.com/file/d/1bWsCpLSwjDnYPypMZVtm4mBc0o4Wecih/view?usp=sharing
Step 3:- Configure EKS to kubectl
// To configure
aws eks update-kubeconfig --region ap-south-1 --name my-cluster
// To Check the configuration 
kubectl get nodes
# This will give the the nodes you have in the cluster as output

Step 4:- You deploy the application to the EKS with following commands directly.

kubectl apply -f  kube.yaml service.yaml
https://drive.google.com/file/d/11qsNbkha9YO5T7YQSaMtGp9YdycAczHq/view?usp=sharing
step 5:- Create a GitOps Repo
#folder structure
.github
|-workflows
      |- triggerargocd.yml
|-kube
   |- deployment.yaml
   |- service.yaml
   
   
   
   
   

