# code-server-k8s
Deploy code-server on a Kubernetes cluster.

## Create a secret for the code-server password
Replace <password> in the following command with your desired password and then run.

```bash
kubectl create secret generic secret-code-server --from-literal=PASSWORD='<password>'
```

## Create a PersistentVolumeClaim
Using persitent volumes will allow the code-server config and the code you develop to persist pod failures.
The pvc manifest file will:
- Assume that a default storage class is defined.
- Set persistent volumes to 1Gi

```bash
kubectl apply -f code-server-pvc.yaml
```

## Deploy code-server on your kubernetes cluster
The deployment manifest file will:
- Create a service with a loadbalancer ip and expose code-server on port 80 using HTTP.
- Deploy code-server pinned to version 'v3.4.0-ls46'
- Set a password as defined by the kubernetes secret named "secret-code-server" using the PASSWORD value.


```bash
kubectl apply -f code-server-deployment.yaml
```

## Access code-server using your browser
You can now access code-server using the service ip.

To get the IP address to access code-server.

```bash
kubectl get svc code-server -o=jsonpath={.status.loadBalancer.ingress[].ip}
```
