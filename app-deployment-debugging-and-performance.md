# App Development, Debugging, and Performance

## Implement continuous integration and delivery for reliable release

### Continuous Integration

- Code
    - Cloud Source Repositories
    - Github
    - BitBucket
- Build
    - Cloud Build
    - Jenkins
    - Circle CI
- Deploy
    - Deployment Manager
    - Spinnaker
    - Chef
    - Puppet 
    - Ansible
    - Terraform
- Test

### Continuous Delivery
- Code
    - Cloud Source Repositories
    - Github
    - BitBucket
- Build
    - Cloud Build
    - Jenkins
    - Circle CI
- Deploy
    - Deployment Manager
    - Spinnaker
    - Chef
    - Puppet 
    - Ansible
    - Terraform
- Test
- Release
    - Deployment Manager
    - Spinnaker
    - Chef
    - Puppet 
    - Ansible
    - Terraform
- Monitor
    - Stackdriver

## Containers

- Consistency
    - Across development, testing, and production environments
- Loose Coupling 
    - Beween application and operating system layers
- Workload Migration
    - Simplified between on-premises and cloud environments
- Agility
    - Agile development and operations

## Deploying the Node.js Application into Kubernetes Engine

### Clone source code in Cloud Shell

In Cloud Shell, clone the repository for the class.

```
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```

Create a soft link as a shortcut to your working directory:

```
ln -s ~/training-data-analyst/courses/developingapps/v1.2/nodejs/containerengine ~/containerengine
```

### Configure the case study application and review code

Change the working directory.

```
cd ~/containerengine/start
```

Configure the Quiz application.

```
. prepare_environment.sh
```

```
This script file:

Creates a Google App Engine application.
Exports environment variables GCLOUD_PROJECT and GCLOUD_BUCKET.
Runs npm install.
Creates entities in Google Cloud Datastore.
Creates a Google Cloud Pub/Sub topic.
Creates a Cloud Spanner Instance, Database, and Table.
Prints out the Google Cloud Platform Project ID.
```

In Cloud Shell, click Launch code editor.

Navigate to containerengine/start.

```
The folder structure for the Quiz application changes to reflect how it is deployed in Kubernetes Engine.

The web application is in a folder called frontend.

The worker application code that subscribes to Cloud Pub/Sub and processes messages is in a folder called backend.

There are configuration files for Docker (a Dockerfile in the frontend and backend folder) and Kubernetes Engine (\*.yaml).
```

### Create a Kubernetes Engine Cluster

In the Cloud Platform Console, on the Navigation menu, click APIs & Services.

Scroll down in the list of enabled APIs and confirm that both of these APIs are enabled:

- Kubernetes Engine API

- Container Registry API

If either API is missing, click + ENABLE APIS AND SERVICES at the top. Search for the above APIs by name and enable each for your current project. (You noted the name of your GCP project above.)

In the Cloud Platform Console, on the Navigation menu, click Kubernetes Engine.
Click Create cluster.
Configure the cluster using the following table:

Name: my-cluster

Zone: us-central1-b

In the Node pools area, click More options

In the Security > Access scopes area, Select Allow full access to all Cloud APIs

Click Save.

Click Create.

### Connect to the cluster

Paste the command into Cloud Shell and press ENTER.

```
gcloud container clusters get-credentials my-cluster --zone us-central1-b --project <Project-ID>
```

### Create the Dockerfile for the frontend and backend

In the Cloud Shell code editor, open ``frontend/Dockerfile``.

After the existing text, enter the Dockerfile commands to initialize the creation of a custom Docker image using Google's NodeJS App Engine image as the starting point.

```
The image you are going to use is: gcr.io/google_appengine/nodejs
```

Add the Dockerfile command to copy the contents from the current folder to a destination folder in the image /app/.

Add the Dockerfile command to execute npm install -g npm@6.11.3 as part of the build process to ensure that the container runs a compatible version of npm for the application.

Add the Dockerfile command to execute npm update.

Complete the Dockerfile by entering the statement, npm start, which executes when the container runs.

...frontend/Dockerfile
```dockerfile
FROM gcr.io/google_appengine/nodejs
RUN /usr/local/bin/install_node '>=0.12.7'
COPY . /app/
RUN npm install -g npm@6.11.3 --unsafe-perm || \
  ((if [ -f npm-debug.log ]; then \
      cat npm-debug.log; \
    fi) && false)
RUN npm update    
CMD npm start
```

Save the Dockerfile.


Repeat the previous steps for the backend/Dockerfile file.

```dockerfile
...backend/Dockerfile
FROM gcr.io/google_appengine/nodejs
RUN /usr/local/bin/install_node '>=0.12.7'
COPY . /app/
RUN npm install -g npm@6.11.3 --unsafe-perm || \
  ((if [ -f npm-debug.log ]; then \
      cat npm-debug.log; \
    fi) && false)
RUN npm update
CMD npm start
```

Save the second Dockerfile.

### Build Docker images with Cloud Build


In Cloud Shell, build the frontend Docker image.

```
gcloud builds submit -t gcr.io/$DEVSHELL_PROJECT_ID/quiz-frontend ./frontend/
```

The files is staged into Cloud Storage, and a Docker image is built and stored in the Container Registry. This takes a few minutes.

Build the backend Docker image.

```
gcloud builds submit -t gcr.io/$DEVSHELL_PROJECT_ID/quiz-backend ./backend/
```

In the Cloud Platform Console, on the Navigation menu, click Container Registry.

You should see two items: quiz-frontend and quiz-backend.

Click quiz-frontend.

You should see the image name, tags (latest), and size.

### Create a Kubernetes Deployment file

In the Cloud Shell code editor, open the frontend-deployment.yaml file.

The file skeleton has been created for you. Your job is to replace placeholders with values specific to your project.

Replace the placeholders in the frontend-deployment.yaml file using the following values:

[GCLOUD_PROJECT]

GCP Project ID (Display the Project ID by enteringecho $GCLOUD_PROJECT in Cloud Shell)

[GCLOUD_BUCKET]

Cloud Storage bucket name for the media bucket in your project (Display the bucket name by enteringecho $GCLOUD_BUCKET in Cloud Shell)

[FRONTEND_IMAGE_IDENTIFIER]

The frontend image identified in the form gcr.io/[Project_ID]/quiz-frontend

The quiz-frontend deployment provisions three replicas of the frontend Docker image in Kubernetes pods, which are distributed across the three nodes of the Kubernetes Engine cluster.

Save the file.

Replace the placeholders in the backend-deployment.yaml file using the following values:

[GCLOUD_PROJECT]

GCP Project ID

[GCLOUD_BUCKET]

Cloud Storage bucket ID for the media bucket in your project

[BACKEND_IMAGE_IDENTIFIER]

The backend image identified in the form gcr.io/[Project_ID]/quiz-backend

The quiz-backend deployment provisions two replicas of the backend Docker image in Kubernetes pods, which are distributed across two of the three nodes of the Kubernetes Engine cluster.

Save the file.

Review the contents of the frontend-service.yaml file.

The service exposes the frontend deployment using a load balancer. The load balancer will send requests from clients to all three replicas of the frontend pod.

### Execute the Deployment and Service Files

In Cloud Shell, provision the quiz frontend Deployment.

```
kubectl create -f ./frontend-deployment.yaml
```

Provision the quiz backend Deployment.

```
kubectl create -f ./backend-deployment.yaml
```

Provision the quiz frontend Service.

```
kubectl create -f ./frontend-service.yaml
```