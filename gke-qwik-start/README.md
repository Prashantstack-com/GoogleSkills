# Google Kubernetes Engine: Qwik Start – Hands-On Lab Summary
  
**Platform:** Google Cloud Skills Boost (formerly Qwiklabs)  
**Lab link:** [Google Kubernetes Engine: Qwik Start](https://www.cloudskillsboost.google/focuses/878?parent=catalog)  
**Objective:** Gain practical experience creating a GKE cluster, deploying a sample containerized app, exposing it via a Service, and cleaning up.

This lab introduced core GKE and Kubernetes concepts: provisioning a managed cluster, using `kubectl` to deploy workloads, and exposing applications externally.

## Key Steps & Commands I Executed

### 1. Set up environment (in Cloud Shell)
Configured default zone for consistency:
gcloud config set compute/zone us-east4-c

### 2. Create a GKE cluster
Created a small standard cluster (usually 3 nodes):
gcloud container clusters create my-cluster 
--num-nodes 3 
--machine-type e2-medium   # (or default/small machine type in lab)
- Waited ~3–5 minutes for cluster provisioning.  
- Verified with: `gcloud container clusters list`

### 3. Authenticate kubectl to the cluster
Fetched credentials so `kubectl` could talk to my new cluster:
gcloud container clusters get-credentials my-cluster
- This updated my local kubeconfig file.

### 4. Deploy the sample application
Created a **Deployment** using the pre-built `hello-app` container image:
kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0
- This launched Pod(s) running a simple "Hello, World!" web server.  
- Checked status:
kubectl get deployments
kubectl get pods

### 5. Expose the application with a Service (LoadBalancer)
Created a Kubernetes **Service** of type LoadBalancer to make the app publicly accessible:
kubectl expose deployment hello-server --type LoadBalancer --port 80 --target-port 8080
- Waited for external IP (could take 1–2 minutes):  
kubectl get service
- Copied the EXTERNAL-IP and visited `http://<EXTERNAL-IP>` in browser → saw "Hello, World!" message.

### 6. Clean up
Deleted the cluster to avoid costs (automatic in Qwiklabs, but good practice):
gcloud container clusters delete my-cluster
