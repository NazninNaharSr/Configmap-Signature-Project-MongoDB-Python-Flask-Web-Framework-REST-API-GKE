# Configmap-Signature-Project-MongoDB-Python-Flask-Web-Framework-REST-API-GKE
   ## Step 1: Create MongoDB using Persistent Volume on GKE
1. Create a Cluster
   
       gcloud container clusters create kubia --num-nodes=1 --machine-type=e2 micro --region=us-west1

   My cluster was already created so I had to delete and create again as the previous one was showing error.

   **Delete the existing cluster:**

       gcloud container clusters delete kubia --region=us-west1
   
   **Create a new cluster with a different name:**

       gcloud container clusters create kubia-new --num-nodes=1 --machine-type=e2-micro --region=us-west1

2. Create a Persistent Volume:

         gcloud compute disks create --size=10GiB --zone=us-west1-a mongodb

3. Create MongoDB Deployment
   
Create mongodb-deployment.yaml and apply it:

         apiVersion: apps/v1
         kind: Deployment
         metadata:
           name: mongodb-deployment
         spec:
           selector:
             matchLabels:
               app: mongodb
           template:
             metadata:
               labels:
                 app: mongodb
             spec:
               containers:
                 - name: mongodb
                   image: mongo
                   ports:
                     - containerPort: 27017
                   volumeMounts:
                     - mountPath: /data/db
                       name: mongodb-persistent-storage
               volumes:
                 - name: mongodb-persistent-storage
                   gcePersistentDisk:
                     pdName: mongodb
                     fsType: ext4
Apply:

      kubectl apply -f mongodb-deployment.yaml
      
4. Check Deployment Pod
   
         kubectl get pods


5. Create Service
   
         nano mongodb-service.yaml
   
In that file copy and paste this -

         apiVersion: v1
         kind: Service
         metadata:
           name: mongodb-service
         spec:
           type: LoadBalancer
           ports:
             - port: 27017
               targetPort: 27017
           selector:
             app: mongodb
   6.

  



       





