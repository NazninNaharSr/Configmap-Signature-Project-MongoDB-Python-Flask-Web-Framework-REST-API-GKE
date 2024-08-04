# Configmap-Signature-Project-MongoDB-Python-Flask-Web-Framework-REST-API-GKE
# Step 1: Create MongoDB using Persistent Volume on GKE
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

  



       





