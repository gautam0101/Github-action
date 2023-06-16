## How to Integrate CI CD pipeline in AWS ECR and AWS EKS using Github actions

This document outlines the steps to integrate the CI CD pipeline in AWS ECR and AWS EKS using Github actions, allowing you to automate code analysis and quality checks for your repository.

follow this document for more info:- [Link](https://towardsaws.com/containerize-node-js-a25b7e9a1e85)

Step-by-step user guide [Link](https://drive.google.com/file/d/15imowp0o8a4CiGpMJIIFTI2Mbvm60vRS/view?usp=sharing).

## Prerequisite:
 
 1. Now about how to create node app. 

# Step for integration
1. Clone the app ``` Git clone https://github.com/gautam0101/CICD_pipeline_AWS_ECR_and_AWS_EKS_using_Github_actions.git ```

## Create Node.js application:- 

Step 1: Create a folder and initialize the npm in the folder
```
mkdir node-app
          
cd node-app
          
npm init
```
Step 2: Install Express js

``` 
npm i express

```

Step 3: Create a server.js file in the node-app 
        
```
touch server.js

```
        
## Create a Dockerfile:-

Step 1:- 

``` 
touch Dockerfile  #By the help of this command you create a file.

```

step 2:- 

```
docker build . #To build the docker image

```

step 3:- check the images in the
          
  ``` 
   docker images
          
   docker run -p 8000:8000 <image_id>` # Put the Image id and open the browser and check localhost:8000 
          
  ``` 
          
 ![Screenshot from 2023-05-25 09-46-05](https://github.com/gautam0101/Github-action/assets/101164301/39b86115-15a5-488f-9409-3b95c19b4b49)


 Step-by-step user guide:- [Video](https://drive.google.com/file/d/1yyKyseGE_N07ogGZcP55pBE7Aod8CgRs/view?usp=sharing)


## Push the image to AWS ECR (Elastic Container Registry)

Log in to AWS and create a security credential to add it to the Github secrets to use in the Github actions.

step 1:- Go create security credentials.

step 2:- Go to access keys and click on generate, It will create the Access keys. # If you have already so use that key

![image](https://github.com/gautam0101/Github-action/assets/101164301/f6f8881f-acdf-4a2e-a0fb-1bab994e3cf6)


step 3:- Go to the application repo settings and add the secrets for the actions to use.
         
         1. Go to the repo setting.
         
         2. Click on the secrets and variables
         
         3. Go to the actions
         
         4. Go to the new repository secret
         
         5. AWS_ACCESS_KEY_ID Add this and save
           
  ![Screenshot from 2023-06-09 13-35-18](https://github.com/gautam0101/Github-action/assets/101164301/9075ef95-5056-487a-8c84-d400a8b91a7d)



## Create an ECR Repo in AWS

step 1:- Search for Elastic Container Registry, And click on Create.
 
 ![Screenshot from 2023-06-09 13-37-06](https://github.com/gautam0101/Github-action/assets/101164301/6ca0e1cf-403b-4f13-9424-de40c9aac3b1)


step 2:- Add the name of the repo

![Screenshot from 2023-06-09 13-37-17](https://github.com/gautam0101/Github-action/assets/101164301/63bcc7ab-155e-4bc1-8d31-5d72e26c30ee)


step 3:- And click on Create.

![Screenshot from 2023-06-09 13-37-28](https://github.com/gautam0101/Github-action/assets/101164301/4d6c39cc-1a5c-42b5-afab-ee169daa1b13)


step 4:- Now we have a repo ready, Copy the Image URL to add it to pushtoecr.yaml at line 32

![Screenshot from 2023-06-09 13-39-21](https://github.com/gautam0101/Github-action/assets/101164301/a438d014-fb6e-4605-a9c1-b2a39fc9d0a6)



## Add Github Action to build the image and push it to ECR in Code Repo

Create an action in the gitops repo with the name pushtoecr.yaml and the following code. In the below code, The GitHub action triggers to push and pull_request in the repo. The action will log in to the AWS account and build the image with the latest code and push it to the ECR.

```
#folder structure
.github
|-workflows
      |- pushtoecr.yaml

```

# Deploy it to the AWS EKS (Elastic Kubernetes Service) cluster

step 1:- Install eksctl

```
brew tap weaveworks/tap  # Adds third party repositaries to home brew

brew install weaveworks/tap/eksctl  # Installs eksctl

```

step 2:-  Create an EKS cluster using eksctl

```
eksctl create cluster --name my-cluster --region ap-south-1 --nodegroup-name linux-nodes  --node-type t2.micro --nodes 2

```

#You can give any name to the cluster but don't give simple to more than one.

![Screenshot from 2023-05-26 16-22-27](https://github.com/gautam0101/Github-action/assets/101164301/9704902f-00dd-4843-8382-41c7831f1988)

Step 3:- Configure EKS to kubectl

```
aws eks update-kubeconfig --region ap-south-1 --name my-cluster  #To configure

kubectl get nodes  # To Check the configuration 

```

# This will give the nodes you have in the cluster as output

Step 4:- You deploy the application to the EKS with the following commands directly.

`kubectl apply -f  kube.yaml service.yaml`![Screenshot from 2023-05-30 12-03-29](https://github.com/gautam0101/Github-action/assets/101164301/49225bce-7754-4fd9-a5ef-5f1ca156c7b1)

step 5:- Create a GitOps Repo with this folder structure.

# steps for creating a git repo
   
   1. go to the GitHub account
   2. go to the repositories
   3. click new
   4. add repo name
   5. click on add readme.md file
   6. create.
   7. then clone the repo
   8. after clone make this folder structure in it and add the code to the files (we cloned this repo so we don't need the make new repo to start this)

```
#folder structure
.github
|-workflows
      |- triggerargocd.yml
|-kube
   |- deployment.yaml
   |- service.yaml

```
   
   
   
   
   
