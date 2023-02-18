## ITI DevOps Final CI-CD Project Application

## By Abdelrahman Mahmoud Mahmoud Anany

This repository is dependent on the Infrastrucre Repo: https://github.com/AbdelrahmanAnany/ITI-Final-CI-CD-Project-Infrastructure 

## 5th Part: Build CI/CD Pipeline using Jenkins

#### Once a commit is made Jenkins will:
- Build an image from Dockerfile
- Push the image to DockerHub
- Apply deployment for the app based on the image
- Apply LoadBalancer service for the app

### 1. Add Credentials in Jenkins
- #### DockerHub Credentials
> Add your DockerHub Credentials `(Username and Password)` and save the id with this value `DockerHub-Credential`.

- #### Service Account Credentials
> Go to GCP Console and navigate to  `Service accounts` from the `IAM & Admin` page.

> Click on your `Service accounts` then click on the `KEYS` Tab then `Add Key` then `Create new key`, for `Key type` Select `JSON`

> Now go to Jenkins and Make a New credential, select `Secret file` for `credentials kind` then upload the Service Account you just downloaded.
> for `Secret ID` enter `gcpCredential`.

![jenkins-credentials](https://github.com/AbdelrahmanAnany/ITI-Final-CI-CD-Project-Jenkins/blob/main/screenshots/dockerhub-and-gcp-serviceacc-creds.png)

### 2. Create CI Pipeline:
- Pull Code from GitHub
- Build the Application image using Docker
- Push Image to DockerHub
- Trigger CD Pipeline to Run

### 3. Create CD Pipeline:
- Deploy Application in GKE

## 6th Part: Check the Application
- get application external ip using the following command in the VM

```
kubectl get all -n application
```
- Finally accessing the application
![helloworld-application](https://github.com/AbdelrahmanAnany/ITI-Final-CI-CD-Project-Jenkins/blob/main/screenshots/deployed-helloworld-application.png)
