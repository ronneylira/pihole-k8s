# pihole-k8s
privacy and private dns provider for k8s with [pihole](https://pi-hole.net/) and [cloudflared](https://github.com/cloudflare/cloudflared)

## usage
### create the storage class
this is a storage class for the pvc using my own nfs server. you can adjust the file for your own setup
<code>kubectl apply -f storage-class.yaml</code>

### create the namespace
to keep everything tidy, create a namespace for your resources
<code>kubectl apply -f namespace.yaml</code>

### create the secret
pi hole uses a specific environment variable as the password for it's web interface.  you can copy the secrets.env.keep into a secrets.env and edit the password, next run the following command

<code>kubectl create secret generic env-secrets -n pihole --from-env-file .\secrets.env</code>

### create the configmap
the settings that pihole require can be configured using a configmap. 
<code>kubectl apply -f configmap.yaml</code>

### create the persistent volume claim
the PVC is the resource that configures the pod's persistence.
<code>kubectl apply -f storage-nfs.yaml</code>

### create the deployment
the deployment is the desired state of the cluster
<code>kubectl apply -f deployment.yaml</code>

### create the service
the service is how we expose the pods
<code>kubectl apply -f services.yaml</code>

### create the ingress
the ingress is a resource that exposes the service with a friendly url and nice certificates :)
<code>kubectl apply -f ingress.yaml</code>

### create the horizontal pod scaler
the hpa is a mechanism to auto scale the pods based on metrics
<code>kubectl apply -f hpa.yaml</code>