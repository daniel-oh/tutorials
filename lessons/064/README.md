# How to Create GKE Cluster from Scratch?

[Step by Step Tutorial](https://youtu.be/<id>)

# Create Shared VPC

- Add `Compute Shared VPC Admin` role to your user
- Create 2 projects: `host-staging` and `k8s-staging`
- Enable `Compute Engine API` and remove `default` VPC from both projects
- Enable `Kubernetes Engine API` in both projects
- Create `main` VPC with the following subnets
  - `private` - 10.5.0.0/20
  - `pod-ip-range` - 10.0.0.0/14
  - `services-ip-range` - 10.4.0.0/19
- Create NAT Gateway
- Set up Shared VPC from the `host-staging` project
- Create Service Account `k8s-staging` for Kubernetes in `k8s-staging`
- Create Kubernetes in `k8s-staging` project
  - Cluster name - `gke`
  - Node pool name - `general`
  - Control plane IP range 172.16.0.0/28
  - Authorized network - 0.0.0.0/0
  - Enable `Enable Workload Identity`
- Install `gcloud`
- Authenticate gcloud client `gcloud auth login`
- Connect to GKE
- Test with `kubectl get svc` and `kubectl get nodes`

# Clean Up

## Delete gcloud

1. Locate your installation directory
```
gcloud info --format='value(installation.sdk_root)'
gcloud info --format='value(config.paths.global_config_dir)'
```
2. Delete both these directories
3. Additionally, remove lines sourcing completion.bash.inc and path.bash.inc in your .bashrc or equivalent shell init file, if you added them during installation.

## Remove Role
Remove `Compute Shared VPC Admin` role from your users that allows


0-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80



1-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: default
spec:
  type: LoadBalancer  
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  selector:
    app: nginx
