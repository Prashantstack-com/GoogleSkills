## Validation Notes

Below are the commands I used to verify each part of the lab was working correctly. These were run from Google Cloud Shell.

### Cluster Setup

Confirmed the Kubernetes cluster was running and accessible:

```bash
gcloud container clusters list
kubectl cluster-info
kubectl get nodes
```

### NGINX Test Deployment

Deployed a simple NGINX container to confirm the cluster was working:

```bash
kubectl create deployment nginx --image=nginx:1.27.0
kubectl expose deployment nginx --port 80 --type LoadBalancer
kubectl get pods
kubectl get services
```

Tested external access once the external IP was assigned:

```bash
curl http://<EXTERNAL-IP>
```

### Fortune App Pod

Created and inspected the initial fortune app Pod:

```bash
kubectl create -f pods/fortune-app.yaml
kubectl get pods
kubectl describe pod fortune-app
```

### Pod Interaction and Debugging

Used port-forwarding to access the Pod locally:

```bash
kubectl port-forward fortune-app 10080:8080
curl http://127.0.0.1:10080
```

Tested authentication and secure endpoint:

```bash
curl -u user http://127.0.0.1:10080/login
TOKEN=$(curl -u user http://127.0.0.1:10080/login | jq -r '.token')
curl -H "Authorization: Bearer $TOKEN" http://127.0.0.1:10080/secure
```

Checked logs and opened a shell inside the container for troubleshooting:

```bash
kubectl logs fortune-app
kubectl logs -f fortune-app
kubectl exec fortune-app --stdin --tty -- /bin/sh
```

### Secure Service and Labels

Created TLS secrets, config maps, and the secure Pod:

```bash
kubectl create secret generic tls-certs --from-file tls/
kubectl create configmap nginx-proxy-conf --from-file nginx/proxy.conf
kubectl create -f pods/secure-fortune.yaml
```

Created the NodePort service:

```bash
kubectl create -f services/fortune-app.yaml
kubectl get services fortune-app
kubectl describe services fortune-app
```

The service initially had no endpoints. After checking labels, I added the missing label to the Pod:

```bash
kubectl get pods -l app=fortune-app
kubectl get pods -l "app=fortune-app,secure=enabled"
kubectl label pod secure-fortune secure=enabled
kubectl describe services fortune-app | grep Endpoints
```

Once the endpoint appeared, I tested external access:

```bash
gcloud compute instances list
curl -k https://<NODE-TERNAL-IP>:31000
```

### Microservices Deployments

Created the auth, fortune, and frontend Deployments and Services:

```bash
kubectl create -f deployments/auth.yaml
kubectl create -f services/auth.yaml

kubectl create -f deployments/fortune-service.yaml
kubectl create -f services/fortune-service.yaml

kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf
kubectl create -f deployments/frontend.yaml
kubectl create -f services/frontend.yaml
```

Verified everything was running:

```bash
kubectl get deployments
kubectl get pods
kubectl get services frontend
```

Tested the frontend service once the external IP was ready:

```bash
curl -k https://<FRONTEND-EXTERNAL-IP>
```

### Result

All Pods reached a running state, Services resolved correctly using labels, and the frontend successfully routed traffic to the backend auth and fortune services.
