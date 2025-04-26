# Workout based on helm installing and uninstalling

```
➜  helm git:(main) helm install dev-nginx nginx-helm -n dev-ngnix-ns --create-namespace  
NAME: dev-nginx
LAST DEPLOYED: Sat Apr 26 22:48:50 2025
NAMESPACE: dev-ngnix-ns
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace dev-ngnix-ns -l "app.kubernetes.io/name=nginx-helm,app.kubernetes.io/instance=dev-nginx" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace dev-ngnix-ns $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace dev-ngnix-ns port-forward $POD_NAME 8080:$CONTAINER_PORT
➜  helm git:(main) kubectl get all -n dev-nginx-ns
No resources found in dev-nginx-ns namespace.
➜  helm git:(main) kubectl get all -n dev-ngnix-ns
NAME                                        READY   STATUS    RESTARTS   AGE
pod/dev-nginx-nginx-helm-67cc4685d8-6rpz2   1/1     Running   0          26s
pod/dev-nginx-nginx-helm-67cc4685d8-mkq6s   1/1     Running   0          26s

NAME                           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
service/dev-nginx-nginx-helm   ClusterIP   10.96.111.241   <none>        80/TCP    26s

NAME                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/dev-nginx-nginx-helm   2/2     2            2           26s

NAME                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/dev-nginx-nginx-helm-67cc4685d8   2         2         2       26s
➜  helm git:(main) helm install prod-nginx nginx-helm -n prod-nginx-ns --create-namespace
NAME: prod-nginx
LAST DEPLOYED: Sat Apr 26 22:50:19 2025
NAMESPACE: prod-nginx-ns
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace prod-nginx-ns -l "app.kubernetes.io/name=nginx-helm,app.kubernetes.io/instance=prod-nginx" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace prod-nginx-ns $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace prod-nginx-ns port-forward $POD_NAME 8080:$CONTAINER_PORT
➜  helm git:(main) kubectl get all -n prod-nginx-ns
NAME                                         READY   STATUS    RESTARTS   AGE
pod/prod-nginx-nginx-helm-598fb99c9b-2ltsm   1/1     Running   0          14s
pod/prod-nginx-nginx-helm-598fb99c9b-kdkqj   1/1     Running   0          14s

NAME                            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
service/prod-nginx-nginx-helm   ClusterIP   10.96.158.123   <none>        80/TCP    14s

NAME                                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/prod-nginx-nginx-helm   2/2     2            2           14s

NAME                                               DESIRED   CURRENT   READY   AGE
replicaset.apps/prod-nginx-nginx-helm-598fb99c9b   2         2         2       14s

```
