# Springboot-Kubernetes-Docker-GCP
Springboot-Kubernetes-Docker-GCP

Kubernetes is an open source project which can run in many different environments, from laptops to high-availability multi-node clusters, from public clouds to on-premise deployments, from virtual machines to bare metal.
We Will deploy a simple Java web-based application (using Spring Boot) to Kubernetes running on Container Engine.

Goal is to run your web application with as a replicated application running on Kubernetes. You take code that we have developed on our machine, turn it into a Docker container image, and then run that image on Container Engine.


Screenshot: 

If you don't already have a Google Account (Gmail or Google Apps), you must create one. Sign-in to Google Cloud Platform console (console.cloud.google.com) and create a new project:


`` gcloud auth list ``

`` gcloud config list project ``

`` gcloud config set project <PROJECT_ID> ``


# Use OpenJDK 8

`` sudo update-alternatives --config javac ``

`` sudo update-alternatives --config java ``

# Get the Spring Boot Getting Started Example source code

`` git clone https://github.com/spring-guides/gs-spring-boot.git ``

`` cd gs-spring-boot/complete ``



# Run the Application Locally
`` ./mvnw -DskipTests spring-boot:run ``


# Preview on 8080 


# Package the Java application as a Docker container
`` ./mvnw -DskipTests package ``

# Then, create a Dockerfile:
`` touch Dockerfile ``

# Add the following to Dockerfile using your favorite editor
`` FROM openjdk:8
COPY target/gs-spring-boot-0.1.0.jar /app.jar
EXPOSE 8080/tcp
ENTRYPOINT ["java", "-jar", "/app.jar"] ``



# Save this Dockerfile and build this image by running this command
`` docker build -t gcr.io/PROJECT_ID/hello-java:v1 . `` 



##### Once this completes (it'll take some time to download and extract everything) you can test the image locally with the following command which will run a Docker container as a daemon on port 8080 from your newly-created container image:
`` docker run -ti --rm -p 8080:8080 gcr.io/PROJECT_ID/hello-java:v1 ``

##### Now that the image works as intended you can push it to the Google Container Registry, a private repository for your Docker images accessible from every Google Cloud project (but also from outside Google Cloud Platform) :
`` gcloud docker -- push gcr.io/PROJECT_ID/hello-java:v1 ``

Please note that we need to enable the google container registry before moving forward.





# Create your cluster

`` gcloud container clusters create hello-java-cluster \
  --num-nodes 2 \
  --machine-type n1-standard-1 \
  --zone us-central1-c ``
  
  
 # Deploy your application to Kubernetes 
  `` kubectl run hello-java \
  --image=gcr.io/PROJECT_ID/hello-java:v1 \
  --port=8080 ``
  
  
  # To view the deployment you just created, simply run:
 
  `` kubectl get deployments ``
  
  
  # To view the application instances created by the deployment, run this command:
 `` kubectl get pods ``
  
  
  # Allow external traffic
  
  `` kubectl expose deployment hello-java --type=LoadBalancer ``
  
  
  ### To find the publicly-accessible IP address of the service, simply request kubectl to list all the cluster services:
  
  `` kubectl get services ``
  
  # Access the application 
  `` http://<EXTERNAL_IP>:8080 ``
  
  
  # Scale up your service 

  `` kubectl scale deployment hello-java --replicas=3 ``
  
  `` kubectl get deployment ``
  
  
  # Roll out an upgrade to your service
  
  Navigate to /gs-spring-boot/complete/src/main/java/hello/HelloController.java
  Introduce some change 
  
  ## Then rebuild the application with the latest changes:
  `` ./mvnw -DskipTests package ``
  
  ## Then build a new version of the container image:
  `` docker build -t gcr.io/PROJECT_ID/hello-java:v2 . ``
  
  ## And push the image into the container image registry:
  `` gcloud docker -- push gcr.io/PROJECT_ID/hello-java:v2 ``
  
  #### You can use kubectl set image command to ask Kubernetes to deploy the new version of your application across the entire   cluster one instance at a time with rolling update:
  
  `` kubectl set image deployment/hello-java \
  hello-java=gcr.io/PROJECT_ID/hello-java:v2 ``
  
  
  
  # Roll back
  
  `` kubectl rollout undo deployment/hello-java ``
  
  
  
  
  
  





