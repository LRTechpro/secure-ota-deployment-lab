# Partner Image Transfer and Kubernetes Notes

The Kubernetes exercise used a partner-provided Docker image archive rather than editable website source.

## Load the archive into Docker

```powershell
docker load -i .\docker-compose-images.tar
docker image ls
```

## Why `ErrImageNeverPull` occurred

Docker Desktop's Kubernetes node uses its own containerd image store. An image visible to the normal Docker CLI is not always available to the Kubernetes node. Because the manifest uses `imagePullPolicy: Never`, the pod correctly refuses to contact an external registry.

## Import into Docker Desktop Kubernetes

From Command Prompt or PowerShell:

```powershell
cmd /c "docker exec -i desktop-control-plane ctr --namespace=k8s.io images import --all-platforms - < docker-compose-images.tar"
```

## Deploy and verify

```powershell
kubectl apply -f k8s\secure-ota-k8s.yaml
kubectl get pods
kubectl get service secure-ota-service
kubectl port-forward service/secure-ota-service 8082:8080
```

Open `http://localhost:8082` and verify the original and tampered-update paths.
