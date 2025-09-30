# Documentation of the Project

Project 6 : Minor MLOps project Using Circle CI
In this project we will learn Circle CI (CICD tool) like Jenkins, we will deploy on Google Kubernetes cluster.
Folder Name & Path : MLOps-Beginner to Advanced MLOps on GCP-CICD\_ Kubernetes Jenkins/6_Project

# Highlights of the Project :

1.Use of Circle Ci
2.Making Kubernetes Cluster on Google Kubernetes cluster

# WorkFlow of the project:

Here Data Ingestion part is not explained again, we can implement the same steps as mentioned in the previous projects.
1.Project setup: We define project and folder structure, custom Exception, logging, create virtual environment.  
2.Jupyter Notebook Testing: We will do notebook testing on EDA, Data processing , and all required steps.
3.Data Preprocessing :
4.Model Training :
5.User App building :
6.Training Pipeline :
7.Data and Code Versioning : It is used to track the data at various steps. DVC, can be used when we have huge data. Github, can be used when we have less data. Here we are using Github for both Data and Code versioning.
8.GCP Setup : Here we will create Kubernetes cluster on Google cloud, artifacts repository, service account, creating keys.
9.CI-CD Deployment : We will use Circle CI for deployment. We will use Docker, Google container registry(to store image in GCR), we will deploy the image from GCR to Kubernetes GKE.
10.Done.

Dockerize the project(Image) --> Push the docker image to GCR--> from GCR to GKE , all this happens through Circle CI.

Project is in local PC --> Push the project to SCM(Github) , this will act as source to the Circle CI --> connect Github with Circle CI --> then we create Deployment pipeline.

How deployment happens is, Checkout (Extarct the code form Github to Circle CI) --> it build the docker image --> then push te image to GCR(like Docker Hub) --> from GCR we deploy it to GKE (this is deployment).

How Circle CI is different from Jenkins.

Cricle CI : It is cloud based, we don’t have to do the installation. It is easier than jenkins. It’s highly scalable. Less customization options. It Is paid version. Used by Startups.

Jenkins : Generally not cloud based. It is bit hard as it is manual setup oriented. It requires additional setup for scaling. Has more customization options. It is free open source. Used by big companies.

# Now Starts with Project Code Setup Implementation.

1. Project Setup Implementation :
   Create a folder for the project on Laptop. From this folder path type CMD and enter, then run code . , this will take you to VS Code. Now Go to VS Code, open terminal
   Create virtual environment
   -Python -m venv venv
   -Another way of Creating virtual environment
   Conda create -p venv python==3.10.0 -y
   Activate virtual environment
   -Venv\Scripts\activate
   -Conda activate venv/
   Create a setup.py file.
   Now start with project structure. Create below files/folders. To make any folder as a package, create **init**.py file
   -Create requirements.txt
   -Create setup.py
   -Create src folder. To make this folder as a package create **init**.py file.
   -Create Config folder. To store configurations.
   -Create Notebook folder
   -Static folder
   -Templates folder  
   -Artifacts folder. To store the CSV files, processed files, model.
   -Pipeline folder
   Now src folder should be treated as a package, that means you define some function in one file and import it in another file. For that run “pip install -e .” in terminal.
   Create a folder RAW in artifacts folder and copy the data into it.

2. Jupyter Notebook Testing :
   In VS Code, Extensions install Jupyter Extension. Now Create notebook.ipynb file in Notebook folder. Create a new cell and just run, select the environment. Again run empty cell for installing the Ipykernal.

3. Data Preprocessing :

In Src folder create a data_processing.py file. Add the required code in this file. Define init constructer, load_data, preprocess_data, feature_selection, split_and_scale_data, save_data_and_scaler, run methods.

4.  Model Training :
    In src folder crate a model_training.py file. Create a class ModelTraining in the file. Initialise the parameters. Load_data, train_model, evaluate_model.

5.User App Building:
Now let’s start with User App building. Now create folders static, templates in Project directory. Now create index.html file in templates folder and style.css file in static folder. Add the required code in this files. We can gett index.html and styles.css code from chatgpt.
Create an Application.py file in project directory. Add the required code. Then run python application.py , this will provide an URL for web app.

6.Training Pipeline:
Now in pipeline folder create a file training_pipeline.py.

7.Data and Code Versioning:
Now let’s start with Data and Code versioning. Here we will use Github for both data and code versioning. Since our dataset is very small we will use Github for Data versioning also.
Now in .gitignore file, add all the folders which are not required, add venv, logs, tests, plugins, MLOps_Project_3.egg-info. We will not add Artifacts folder in .gitignore file because we want this for data versioning.
Now make sure git is installed. Check git --version.
Now initialize the git repository. Run below commande: - git init - Now login to github and create a new repository.  
 Using github account credential , rajasekharvardhi@gmail.com, RaJuArJuN$89.
Created MLOps_Udemy_Project_2 Repository. - git branch -M main

- git remote add origin https://github.com/RajaSekhar899/MLOps_Udemy_Project_6.git
- git add .
- git commit -m “pushing code to git”
- git push origin main
- Code is pushed to Github. Just refresh the page in Github.
  Data and Code versioning is done.

  8.GCP and CI-CD Setup :
  Important step in the project.  
  Install Google cloud CLI on computer (https://cloud.google.com/sdk/docs/install)
  -Once installed, to verify , close and open VS Code, and then in terminal run “gcloud – version “

Logged into GCP using rajasekharvardhi@gmail.com mail. Once logged in we need to enable some API’s as mentioned below ,go to API Library

- Kubernetes Engine API
- Google Container Registry API
- Compute Engine API
- Cloud Build
- Cloud Storage
- IAM
  Creating a Cluster
  In the Gcp console --> Kuberenetes Engine --> Cluster --> Create --> Provide a name for the cluster --> Control Panel Access, select Access using DNS, Access using IPV4 networks --> click on Create cluster.
  Create a Service account
  Steps :
  1.Create a service account in GCP
  -To create a service account, go to IAMservice accountsProvide a name for Service account --> create role as Owner,, Storage object viewer,, Storage object admin ,, Artifact registry Administrator,, Artifact registry Writer.
  Created service account
  -Then again go to service account click on 3 dots, under Key ID , there are no keys, for that go to manage keys add key create a new key --> select Json key and download.
  -Now copy the downloaded service account key into the VS Code directory. Rename it to gcp-key.json for conveience.
  -Add this file(gcp-key.json) into .gitignore file, we don’t want this to push to Github.
  -After making these changes push the code again to Github.
  -Then , go to storage bucket file click on 3 dots, and edit access Add principal select the service account and add the roles same as above.

Create Artifact Registry
Now go to Artifact Registry --> Create a Registry --> Provide a name (mlops-app) , choose as Docker,, choose region,, --> Click on Create.  
We have created this artifact registry Is for whatever the project docker image we create will be stored here.

Now create a Dockerfile file in VS Code project directory.  
Now create a kubernetes-deployment.yaml file in VS Code project directory. Create the file. In the file provide the metadata name from the artifact that is create above from Artifact Registry.
metadata:
  name: mlops-app

Provide the container port from application.py file.
Provide the image full path from artifact repository that is create above Artifact Registry. Note : Copy the image path after the deployment is done.
spec:
      containers:
        - name: mlops-app-container
          image: us-central1-docker.pkg.dev/mlops-new-447207/mlops-app/mlops-app:latest

# project name/ image name

ports:
            - containerPort: 5000 # Replace with the port your app listens on

Now comes the important part Circle CI - Create a folder .circleci in VS Code project directory. In this folder create config.yaml file. Here in this file we will write all CI-CD deployment code. Circle CI will automatically detects this folder. This file is same as Jenkins file.
version: 2.1 # Circle CI version

executors:
  docker-executor: ## we run all the jobs in Docker , Running the app in Docker
    docker:
      - image: google/cloud-sdk:latest ## Here we can install python also, but it needs lots of dependencies.
working_directory: ~/repo

jobs: ## This is to extract the code from Github to Docker Circle CI
  checkout_code:
    executor: docker-executor
    steps:
      - checkout
  build_docker_image: ## Once checkout is done, build the docker image
    executor: docker-executor
    steps:
      - checkout
      - setup_remote_docker
      - run: ## We are configuring , authenticating google account
          name: Authenticate with google cloud
          command: |
            echo "$GCLOUD_SERVICE_KEY" | base64 --decode > gcp-key.json  ## We will be decoding the Gcp-key from different format
            gcloud auth activate-service-account --key-file=gcp-key.json
            gcloud auth configure-docker us-central1-docker.pkg.dev || gcloud auth configure-docker ## This Is artifact repository,, 
      - run:  ## Here we will build image and push
          name: Build and Push Image
          command: |
            docker build -t us-central1-docker.pkg.dev/$GOOGLE_PROJECT_ID/mlops-app/mlops-app:latest . ## image full path from artifact repository
            docker push us-central1-docker.pkg.dev/$GOOGLE_PROJECT_ID/mlops-app/mlops-app:latest ## Pushing the image to GCR
  deploy_to_gke:  ##  Here we will deploy image to GKE
    executor: docker-executor
    steps:
      - checkout
      - setup_remote_docker
      - run: ##  Authenticating Google account
          name: Authenticate with google cloud
          command: |
            echo "$GCLOUD_SERVICE_KEY" | base64 --decode > gcp-key.json
            gcloud auth activate-service-account --key-file=gcp-key.json
            gcloud auth configure-docker us-central1-docker.pkg.dev || gcloud auth configure-docker
      - run: ## Configure with GKE
          name: Configure GKE
          command: |
            gcloud container clusters get-credentials $GKE_CLUSTER --region $GOOGLE_COMPUTE_REGION --project $GOOGLE_PROJECT_ID ## Here GKE_CLUSTER is cluster name
      - run: # Ceploy to Google Kubernetes Engine
          name: Deploy to GKE
          command: |
            kubectl apply -f kubernetes-deployment.yaml
workflows: ## De
  version: 2
  deploy_pipeline:
    jobs:
      - checkout_code
      - build_docker_image:
          requires:
            - checkout_code
      - deploy_to_gke:
          requires:
            - build_docker_image

We are done with coding.
Now push all the code Github.
Now go to browser , search for Circle Ci, Sign up if you don’t have account. We can login using google or Github. In the Circle Ci dashboard --> Projects --> Create a project --> Choose Build, test and deploy software applications --> Provide a name for the project(Mlops-project6) --> Click on Next : setup a pipeline --> Click on Next: choose a repo --> Here authorize Github account and choose the project Repo --> Click on Next : Set up your config --> It will automatically detects the circle ci config file. --> Now click on Next : set up triggers --> Here we are selecting when to run the pipeline, by default it is set to All pushes I.e everytime you push code , it will trigger pipeline. --> Now click on Next: Review and finish setup --> click on Finish setup.

Add Environment Variables
Now click on the created project --> project settings --> Environment variables --> click on Add environment variables. (Here we are adding all the environment variables that we have defined in the .circleci/config.yaml file. We have defined environment variables like $GCLOUD_SERVICE_KEY. How will do this is we need to convert json file into base64 format, Circle Ci doesn’t understand json format, so that’s why we need to convert it into base64 format. for that in VS Code terminal , Git bash , Run as below - cat gcp-key.json | base64 -w 0 - This will provide a encoded data, Copy the data)
--> Add environment variables --> Provide Name as : GCLOUD_SERVICE_KEY and in Value segment paste the copied encoded data from above line --> click on Add this environment variable.
Now we will do the same for other environment variables $GOOGLE_PROJECT_ID, for that repeat the same process as above ,, --> click on Add environment variables --> Provide Name as : GOOGLE_PROJECT_ID and in value segment provide the google project id --> click on Add environment variable.
Now we will do the same for other environment variables $GKE_CLUSTER, for that repeat the same process as above ,, --> click on Add environment variables --> Provide Name as : GKE_CLUSTER and in value segment provide the google kubernetes cluster name --> click on Add environment variable.
Now we will do the same for other environment variables $GOOGLE_COMPUTE_ZONE, for that repeat the same process as above ,, --> click on Add environment variables --> Provide Name as : GOOGLE_COMPUTE_REGION and in value segment provide the google kubernetes cluster region --> click on Add environment variable.
We are done with setup.

Now we need to trigger the pipeline. For that in Circle CI dashboard --> Projects --> Choose the project --> trigger pipeline --> choose pipeline --> Select the brach as main --> Click on Run pipeline. This will take sometime for creating pipeline.

Now with this we have deployed our Project. Now wait for 2-3 minutes.
Now go to Gcp Kubernetes Engine console , we can see the cluster status as success . Go to Workloads --> click on the available workload --> Here if you come to end , we can see Endpoint where the app is running.

With this we have succesfully deployed our application in Google Kubernetes Engine through Circle CI.

9.Done
