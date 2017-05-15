# Springboot-Kubernetes-Docker-GCP
Springboot-Kubernetes-Docker-GCP


gcloud auth list



gcloud config list project


gcloud config set project <PROJECT_ID>


sudo update-alternatives --config javac


sudo update-alternatives --config java


git clone https://github.com/spring-guides/gs-spring-boot.git


cd gs-spring-boot/complete



Run the Application Locally
./mvnw -DskipTests spring-boot:run


Preview on 8080 


Package the Java application as a Docker container
./mvnw -DskipTests package

touch Dockerfile


FROM openjdk:8
COPY target/gs-spring-boot-0.1.0.jar /app.jar
EXPOSE 8080/tcp
ENTRYPOINT ["java", "-jar", "/app.jar"]




docker build -t gcr.io/PROJECT_ID/hello-java:v1 .




docker run -ti --rm -p 8080:8080 gcr.io/PROJECT_ID/hello-java:v1


gcloud docker -- push gcr.io/PROJECT_ID/hello-java:v1



Create your cluster:

gcloud container clusters create hello-java-cluster \
  --num-nodes 2 \
  --machine-type n1-standard-1 \
  --zone us-central1-c
  
  
  
  kubectl run hello-java \
  --image=gcr.io/PROJECT_ID/hello-java:v1 \
  --port=8080
  
  
  
  kubectl get deployments
  
  
  
  kubectl get pods
  
  
  kubectl expose deployment hello-java --type=LoadBalancer
  
  
  
  kubectl get services
  
  
  http://<EXTERNAL_IP>:8080
  
  
  
  kubectl scale deployment hello-java --replicas=3
  
  
  
  kubectl get deployment
  
  
  Roll out an upgrade to your service
  
  
  Navigate to /gs-spring-boot/complete/src/main/java/hello/HelloController.java
  
  
  ./mvnw -DskipTests package
  
  
  docker build -t gcr.io/PROJECT_ID/hello-java:v2 . 
  
  
  gcloud docker -- push gcr.io/PROJECT_ID/hello-java:v2
  
  
  kubectl set image deployment/hello-java \
  hello-java=gcr.io/PROJECT_ID/hello-java:v2
  
  
  
  # Roll back
  
  kubectl rollout undo deployment/hello-java
  
  
  
  
  
  





