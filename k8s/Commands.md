### Major Kubernetes Commands and Their Purpose

| Category        | Command                                                 | Purpose                                        |                                          |
| --------------- | ------------------------------------------------------- | ---------------------------------------------- | ---------------------------------------- |
| Cluster Info    | `kubectl version`                                       | Check client and server Kubernetes versions    |                                          |
| Cluster Info    | `kubectl cluster-info`                                  | Verify cluster connectivity and core endpoints |                                          |
| Context         | `kubectl config get-contexts`                           | List available clusters/contexts               |                                          |
| Context         | `kubectl config use-context <ctx>`                      | Switch between clusters/environments           |                                          |
| Namespace       | `kubectl get ns`                                        | List namespaces                                |                                          |
| Namespace       | `kubectl create ns <name>`                              | Create a namespace                             |                                          |
| Namespace       | `kubectl config set-context --current --namespace=<ns>` | Set default namespace                          |                                          |
| Discovery       | `kubectl get <resource>`                                | List resources (pods, svc, deploy, etc.)       |                                          |
| Discovery       | `kubectl describe <resource> <name>`                    | Detailed troubleshooting information           |                                          |
| Discovery       | `kubectl explain <resource>`                            | View manifest schema and fields                |                                          |
| Deployment      | `kubectl apply -f <file                                 | dir>`                                          | Create or update resources declaratively |
| Deployment      | `kubectl delete -f <file                                | dir>`                                          | Delete resources defined in manifest     |
| Deployment      | `kubectl rollout status deploy/<name>`                  | Check deployment progress                      |                                          |
| Deployment      | `kubectl rollout history deploy/<name>`                 | View deployment revisions                      |                                          |
| Deployment      | `kubectl rollout undo deploy/<name>`                    | Roll back to previous version                  |                                          |
| Pods            | `kubectl logs <pod>`                                    | View Pod logs                                  |                                          |
| Pods            | `kubectl logs <pod> -c <container>`                     | Logs for specific container                    |                                          |
| Pods            | `kubectl exec -it <pod> -- <cmd>`                       | Execute command inside container               |                                          |
| Pods            | `kubectl attach <pod>`                                  | Attach to running container                    |                                          |
| Scaling         | `kubectl scale deploy/<name> --replicas=N`              | Manually scale Pods                            |                                          |
| Autoscaling     | `kubectl autoscale deploy/<name>`                       | Create Horizontal Pod Autoscaler               |                                          |
| Monitoring      | `kubectl top nodes`                                     | View node CPU/memory usage                     |                                          |
| Monitoring      | `kubectl top pods`                                      | View Pod resource usage                        |                                          |
| Networking      | `kubectl get svc`                                       | List Services                                  |                                          |
| Networking      | `kubectl port-forward pod/<pod> L:C`                    | Local access to Pod                            |                                          |
| Networking      | `kubectl proxy`                                         | Access Kubernetes API locally                  |                                          |
| Config          | `kubectl get configmap`                                 | List ConfigMaps                                |                                          |
| Config          | `kubectl create configmap`                              | Create ConfigMaps                              |                                          |
| Secrets         | `kubectl get secret`                                    | List Secrets                                   |                                          |
| Secrets         | `kubectl create secret`                                 | Create Secrets                                 |                                          |
| Storage         | `kubectl get pv`                                        | List PersistentVolumes                         |                                          |
| Storage         | `kubectl get pvc`                                       | List PersistentVolumeClaims                    |                                          |
| Security        | `kubectl auth can-i <verb> <resource>`                  | Check RBAC permissions                         |                                          |
| Security        | `kubectl get role,rolebinding`                          | Inspect RBAC configuration                     |                                          |
| Troubleshooting | `kubectl get events`                                    | View cluster events                            |                                          |
| Troubleshooting | `kubectl debug <pod>`                                   | Run ephemeral debug container                  |                                          |
| Maintenance     | `kubectl cordon <node>`                                 | Disable scheduling on node                     |                                          |
| Maintenance     | `kubectl uncordon <node>`                               | Re-enable scheduling                           |                                          |
| Maintenance     | `kubectl drain <node>`                                  | Safely evict Pods from node                    |                                          |
| Output          | `kubectl get <res> -o yaml`                             | Export live resource definition                |                                          |
| Output          | `kubectl diff -f <file>`                                | Compare desired vs live state                  |                                          |
