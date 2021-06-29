
# Sample Website

This Repository defines the necessary resources to deploy a sample website.
It provides a cloudformation template to build the resources.
The website is powered by ECS cluster running on fargate behind an application loadbalancer.

## Prerequisites

To be able to deploy the template few requisites need to exist prior to deployment.

1. A certificate to use with the HTTPS listener for the application loadbalancer
2. A docker image in dockerhub or an ECR in the account. In the template I used `modamod/sample-website` docker image. For the sake of simplicity, I included the dockerfile and the necessary files to build the image yourself.
3. Make sure when filling the parameters to choose a vpc CIDR that is not currently in use.

### Pushing docker image to ecr

These steps are required only when pushing an image to a private repository on AWS.

To successfully run a docker image hosted on ECR the following steps are required.

* To push docker image to ecr first create a repository

* To authenticate docker against ecr run the following command

* To build the docker image and push it to ecr, get placed on the folder where the Dockerfile is and run the following commands. Depends on how fast your internet is and what type of image you are using this may take few minutes. For slow internet, I suggest using a cloud9 environment or ec2 instance with access to ecr and build the docker image there.

Use the following script to perform the necessary actions.

```bash
# Build a repository
aws ecr create-repository --repository-name sample-website

# authenticate docker against ecr repository
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account_id>.dkr.ecr.<region>.amazonaws.com

# Build the docker image from Dockerfiles
docker build -t sample-website .

# Tags the image with the latest tag
docker tag sample-website:latest <account_id>.dkr.ecr.<region>.amazonaws.com/sample-website:latest

# Push the image to ecr repository
docker push <account_id>.dkr.ecr.<region>.amazonaws.com/sample-website:latest

```

## Deployment

Once the prerequisites are met on the targeted region proceed to deploy the template using the console

## Deletion

If for what ever reason you need to delete the deployment simply delete the cloudformation template.
If you used an ECR image make sure to delete it to save storage cost.
