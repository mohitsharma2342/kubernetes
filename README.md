# kubernetes On Google Cloud


How To create Docker Image
1.Create file Dockerfile and put this configuration

FROM openjdk:8
EXPOSE 8090
ADD target/ibm-cloud-demo-8.jar ibm-cloud-demo-8.jar
ENTRYPOINT ["java","-jar","/ibm-cloud-demo-8.jar"]


To build the image we can run:

$ docker build . --tag demo .

Then we can test it:
$ docker run -it -p8080:8080 demo:latest

After Building  Docker image ,push that image to container Registery So GKE Can pull that whenever is required
First Step To Pushing We need create tag
1.docker tag demo gcr.io/qwiklabs-gcp-04-64db622d43a8/demo:v1
Then push Command
2.docker — push gcr.io/qwiklabs-gcp-04-64db622d43a8/demo:v1

Create Cluster on GKE 
gcloud container clusters create spring-boot-cluster --zone us-central1-a --num-nodes 2

For using that cluster we need to login on that cluster so here is the Command
gcloud container clusters get-credentials spring-boot-cluster --zone us-central1-a

To Create yml file 
https://static.brandonpotter.com/kubernetes/DeploymentBuilder.html

Deployment.yml
------------------------------------------------------------------------------------------------------------------------------------------------------------
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: demo
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: demo
    spec:
      containers:
        - name: demo
          image: 'gcr.io/qwiklabs-gcp-04-64db622d43a8/demo:v1'
          ports:
            - containerPort: 8080

---
apiVersion: v1
kind: Service
metadata:
  name: demo-service
  labels:
    name: demo-service
spec:
  ports:
    - port: 80
      targetPort: 8080
      protocol: TCP
  selector:
    app: demo
  type: LoadBalancer
----------------------------------------------------------------------------------------------------------------------------------------------------------


For Deploying the yml File

kubectl apply -f Deployment.yml

To see the pods 
kubectl get pods

To see the service 
kubectl get service

To see the logs 
kubectl logs pod_name

