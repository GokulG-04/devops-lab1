# Dev Lab 8 - Manage Kubernetes Resources Using CLI

## Practical Record (Write this in your exam record)

### 1. Aim
To install Docker, kubectl and Minikube, and to manage basic Kubernetes resources such as Pods, Deployments and Services using the kubectl CLI.

### 2. Procedure
1. Open PowerShell as Administrator.
2. Verify Docker installation using the command `docker --version`.
3. Verify kubectl installation using the command `kubectl version --client`.
4. Start the Minikube cluster using the command `minikube start --driver=docker`.
5. Check the cluster status using `minikube status` and `kubectl get nodes`.
6. Create a YAML file named `nginx-pod.yaml` containing a Pod definition for the nginx container with label `app: nginx` and container port 80.
7. Create the pod using the command `kubectl apply -f nginx-pod.yaml`.
8. Check the pod status using `kubectl get pods` and view detailed information using `kubectl describe pod nginx-pod`.
9. Forward port 8080 to port 80 of the pod using `kubectl port-forward pod/nginx-pod 8080:80` and open `http://localhost:8080` in the browser.
10. Delete the pod using `kubectl delete pod nginx-pod`.
11. Create a deployment using `kubectl create deployment my-nginx --image=nginx`.
12. Check deployments and pods using `kubectl get deployments` and `kubectl get pods -l app=my-nginx`.
13. Scale the deployment to 3 replicas using `kubectl scale deployment my-nginx --replicas=3` and verify using `kubectl get pods`.
14. Update the image using `kubectl set image deployment/my-nginx nginx=nginx:1.25` and watch the rollout using `kubectl rollout status deployment/my-nginx`.
15. Undo the rollout using `kubectl rollout undo deployment/my-nginx`.
16. Expose the deployment as a NodePort service using `kubectl expose deployment my-nginx --type=NodePort --port=80`.
17. View the service using `kubectl get svc` and get the URL using `minikube service my-nginx --url`.
18. Open the displayed URL in the browser.
19. Clean up using `kubectl delete deployment my-nginx`, `kubectl delete svc my-nginx` and `kubectl delete all --all -n default`.
20. Stop the cluster using `minikube stop`.

### 3. Output
- `kubectl get pods` shows the pod `nginx-pod` with status `Running 1/1`.
- `kubectl get deployments` shows `my-nginx` with `READY 3/3` after scaling.
- `kubectl rollout status` prints `successfully rolled out`.
- `kubectl get svc` shows the service `my-nginx` with type `NodePort` and a ClusterIP.
- `minikube service my-nginx --url` prints a URL like `http://127.0.0.1:<PORT>`.
- Browser at `http://localhost:8080` (port-forward step) and at the minikube service URL shows the nginx welcome page ("Welcome to nginx!").

### 4. Result
Thus, Kubernetes resources such as Pods, Deployments and Services were successfully created, scaled, exposed and cleaned up using the kubectl CLI.

---

## Execution Guide (VS Code)

> Pre-check (do once, before the lab): Docker Desktop must be installed and running. If `kubectl` or `minikube` is missing, install them with:
> `choco install kubernetes-cli` and `choco install minikube` (in an **Admin** PowerShell).

1. Open the `Dev Lab 8` folder in VS Code. Open the integrated terminal (`Terminal > New Terminal`).
2. Verify Docker, kubectl and Minikube:

```
docker --version
kubectl version --client
minikube status
```

   Expected: Docker/kubectl version printed; minikube shows `Running`. If the cluster is stopped, start it (next step).

3. Start the cluster (skip if minikube status already shows Running):

```
minikube start --driver=docker
```

   Expected: `Done! kubectl is now configured to use "minikube"`.

4. Confirm the node is Ready:

```
kubectl get nodes
```

   Expected: node `minikube` with `STATUS Ready`.

5. Apply the pod manifest (file `nginx-pod.yaml` is already in this folder):

```
kubectl apply -f nginx-pod.yaml
```

6. Check pod status and details:

```
kubectl get pods
kubectl describe pod nginx-pod
```

   Expected: `nginx-pod` with `1/1 Running`.

7. Keep port-forward running in the terminal (do not close it), then open `http://localhost:8080` in your browser:

```
kubectl port-forward pod/nginx-pod 8080:80
```

   Expected: `Forwarding from 127.0.0.1:8080 -> 80`. Browser shows the nginx welcome page.
   Press `Ctrl + C` to stop it when done.

8. Delete the pod:

```
kubectl delete pod nginx-pod
```

9. Create and check the deployment:

```
kubectl create deployment my-nginx --image=nginx
kubectl get deployments
kubectl get pods -l app=my-nginx
```

   Expected: deployment `my-nginx` READY `1/1`.

10. Scale to 3 replicas:

```
kubectl scale deployment my-nginx --replicas=3
kubectl get pods
```

   Expected: 3 pods for `my-nginx`, all `Running`.

11. Rolling update:

```
kubectl set image deployment/my-nginx nginx=nginx:1.25
kubectl rollout status deployment/my-nginx
```

   Expected: `deployment.apps/my-nginx image updated` then `deployment "my-nginx" successfully rolled out`.

12. Rollback (demo):

```
kubectl rollout undo deployment/my-nginx
```

13. Expose the deployment as a NodePort service:

```
kubectl expose deployment my-nginx --type=NodePort --port=80
kubectl get svc
```

14. Get the service URL and open it in the browser:

```
minikube service my-nginx --url
```

   Copy the printed URL and open it in your browser. Show the nginx page to the invigilator.

15. Cleanup:

```
kubectl delete deployment my-nginx
kubectl delete svc my-nginx
kubectl delete all --all -n default
```

16. Stop the cluster (optional):

```
minikube stop
```

## Troubleshooting

- **`minikube start` hangs or fails**: Ensure Docker Desktop is running first, then run `minikube delete` followed by `minikube start --driver=docker`.
- **`kubectl get nodes` shows NotReady**: Wait 1–2 minutes and run it again; the node takes some time to become Ready.
- **Port-forward says pod not found**: Check the pod exists with `kubectl get pods`; if you deleted it, reapply `kubectl apply -f nginx-pod.yaml`.
- **`minikube service` URL does not open**: Close and reopen the browser, or use `kubectl get svc` and browse to `http://<minikube-ip>:<node-port>` obtained from `minikube ip` and the NodePort column.
